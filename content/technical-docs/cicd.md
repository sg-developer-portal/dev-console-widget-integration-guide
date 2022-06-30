# CICD

Our workflow is as such :

> Local — push to gitlab → GitLab — deploys to aws through cdk → AWS

We are using [aws cdk](https://aws.amazon.com/cdk/) to speed up our deployment to aws. Depending on the branch the code is pushed to in gitlab, it'll be pushed to either dev, stg or prod i.e if you push to dev branch, gitlab runner will deploy using cdk to dev env in AWS.

Gitlab runner reads the deployment instructions we have set in gitlab-ci.yml . Find out more about deployment [here](https://docs.gitlab.com/ee/ci/quick_start/).

Code pushed to dev and stg are pushed to OO AWS env whereas code pushed to prod are pushed to GCC 1.0. The main difference is that in GCC 1.0, there's more restriction on IAM Roles creation and it will be discussed further below.

## Pushing to Prod and GCC 1.0 Deployment

To start to explain the difference, we first need to understand how cdk works. Whenever a new push is made to dev or stg branch in gitlab, gitlab runner will run cdk bootstrap and cdk deploy. Find out more [here](https://docs.aws.amazon.com/cdk/v2/guide/cli.html).

CDK assumes that the user has admin access to all of the resources and they use this assumption to create IAM roles behind the scenes while bootstrapping and deploying. This is not an issue when deploying in dev and stg since we do give gitlab-runner admin access in these environments.

However for GCC 1.0, which is where our prod branch will deploy to, a [permissions boundary](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) is set to every user or roles such that any subsequent roles or users they create must have that same exact permission boundary i.e the permission boundary dictates that if I were to create a new role, that new role must be assigned the same permission boundary.

This causes issue with our cdk bootstrap and deploy since the roles created behind the scenes by cdk are not being assigned the permission boundary, which in turn fails both bootstrap and deploy.

### Fix for CDK Deploy

Since cdk is creating roles behind the scenes, a logical approach will be to configure our code to assign permission boundary to any roles being created. We achieved this through [AWS Aspects](https://docs.aws.amazon.com/cdk/v2/guide/aspects.html).

```js
import * as cdk from "aws-cdk-lib";
import { IConstruct } from "constructs";

export class PermissionsBoundary implements cdk.IAspect {
  private readonly permissionsBoundaryArn: string;

  constructor(permissionBoundaryArn: string) {
    this.permissionsBoundaryArn = permissionBoundaryArn;
  }

  public visit(node: IConstruct): void {
    if (
      cdk.CfnResource.isCfnResource(node) &&
      node.cfnResourceType === "AWS::IAM::Role"
    ) {
      node.addPropertyOverride(
        "PermissionsBoundary",
        this.permissionsBoundaryArn
      );
    }
  }
}
```

More in detail within the code base in `/infra` folder of the [GitLab Repository](https://gitlab-in.ship.gov.sg/developer-portal/dev-console-mvp/dev-console-systems).

### Fix for CDK Bootstrap

Unfortunately the same programatic approach could not be used to fix the issue for bootstrap. We managed to find a great blog [here](https://medium.com/@imageryan/bootstrapping-aws-cdk-in-a-secure-environment-9bc778ea6d94) from someone that faced the same issue.

TLDR we had to create a CDK Bootstrap .yml template and edit the content inside to manually assign the permission boundary to the roles within the .yml template. An example code will be below :

```yml
  FilePublishingRole:
    Type: AWS::IAM::Role
    Properties:
      // Manually added permission boundary
      PermissionsBoundary:
        Fn::If:
          - AttachGCCRoleBoundary
          - {
              "Fn::Sub": "arn:aws:iam::${AWS::AccountId}:policy/GCCIAccountBoundary",
            }
          - Ref: AWS::NoValue
```

The actual template code is in prod-bootstrap.yml in the project's root folder. We need to deploy this template using [AWS CLI](https://aws.amazon.com/cli/) in our CICD deployment. The prod deploy script is in `scripts/prod-cdk-bootstrap.sh` of the [GitLab Repository](https://gitlab-in.ship.gov.sg/developer-portal/dev-console-mvp/dev-console-systems) :

```bash
#!/bin/bash

# Variables
STACK_ACTION='update-stack'
STACK_NAME='CDKToolkit'
CAPABILITY_NAMED_IAM='CAPABILITY_NAMED_IAM'
TEMPLATE_BODY='file://prod-bootstrap.yml'
PARAM_KEYS=("FileAssetsBucketKmsKeyId" "CloudFormationExecutionPolicies")
PARAM_VALUES=("AWS_MANAGED_KEY" "arn:aws:iam::aws:policy/AdministratorAccess")

for i in ${!PARAM_KEYS[@]}; do
  PARAMS+="ParameterKey=${PARAM_KEYS[$i]},ParameterValue=${PARAM_VALUES[$i]} "
done

outputStr="aws cloudformation $STACK_ACTION --stack-name $STACK_NAME --capabilities $CAPABILITY_NAMED_IAM --template-body $TEMPLATE_BODY --parameters $PARAMS"

output=$($outputStr 2>&1)
RESULT=$?

if [ $RESULT -eq 0 ]; then
  echo "$output"
else
  if [[ "$output" == *"No updates are to be performed"* ]]; then
    echo "No cloudformation updates are to be performed."
    exit 0
  else
    echo "$output"
    exit $RESULT
  fi
fi
```

As imagined, this works but is not the best. Since we are manually creating the bootstrap template, if we later add on more resources to the infra stack that we may need, we will need to regenerate the bootstrap the template and manually assign the permission boundaries again.

This however is the only way we are aware of to deploy to GCC 1.0 using cdk. The steps to modify the bootstrap template .yml file will be documented in a .md file within the code repository itself.

Luckily there is hope as GCC 2.0 may not have the same permission boundary rules, which will mean we can remove this fix and revert back to the standard cdk bootstrap and deploy commands once we move to GCC 2.0.