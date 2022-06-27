# Application Bundles Breakdown

To give a brief breakdown of how the bundle scripts work together : <br/>

![Gateway Script Flow](../assets/gateway-script-flow.png)

Your application will inject the gateway script which will subsequently inject the latest version of actual widget web component script.

## Gateway Script

https://assets.developer.tech.gov.sg/bundled-scripts/dev-console-gateway.bundle.js
## Web Component Scripts
- https://assets.developer.tech.gov.sg/bundled-scripts/dev-console-widget.bundle.js
- https://assets.developer.tech.gov.sg/bundled-scripts/dev-console-widget.es5.bundle.js

Application host will inject the gateway script in the header which will in turn inject the relevant web component scripts. Example:

```html
<script type="module" id="dev-console-gateway" src="https://assets.developer.tech.gov.sg/bundled-scripts/dev-console-gateway.bundle.js" es5 polyfills></script> 
```
Add in attributes ```es5``` and ```polyfills``` to inject the ```es5``` version of bundled script and relevant polyfills. They are **optional**.

We recommend not including them if your application do not plan to support older browsers.

Web component can be used in this manner:

```html
  <dev-console-widget
    iconColor="black" 
    iconWidth="24px"  
    iconHeight="24px" 
    activeColor="blue" 
    variant="mobile"
  ></dev-console-widget>
```

|  props          | required | description  |  
|---              |---           |---
|  iconColor      | No. <br /> default : white   | icon color <br/> type: string <br />            |  
|  iconWidth      | No. <br /> default : 32px    | icon width <br/> type: string                           |
|  iconHeight     | No. <br /> default : 32px    | icon height <br/> type: string                          |
|  activeColor    | No. <br /> default : #0161AF | active and hover color for icon <br /> type: string     |
|  variant        | No. <br /> default : both  | component can be used as mobile, desktop or both <br /> type: string <br /> **more info in caveats** |