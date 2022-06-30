# Publishing Bundles

For host apps to consume and use the widget, they will have to inject a bundled javascript file through a script tag. We are bundling our widget with [rollup](https://rollupjs.org/guide/en/).

In our rollup.config.js we specify the different bundle type we want. The more important ones are ecma 2017 and es5 .

```js
	input: "./transpiled/src/dev-console-widget.js",
    output: [
      {
        // Modern bundle with ecma 2017
        file: "./build/dev-console-widget.bundle.js",
        format: "esm",
      },
      {
        file: "./build/dev-console-widget.iife.bundle.js",
        format: "iife",
        name: "devConsoleWidget",
        sourcemap: false,
      },
      {
        // Bundle with babel config for JS backward compatability
        file: "./build/dev-console-widget.es5.bundle.js",
        format: "esm",
        plugins: [
          getBabelOutputPlugin({
            configFile: path.resolve(__dirname, "babel.config.cjs"),
          }),
        ],
      },
    ],
```

All of the bundle scripts are stored in s3 bucket and served through our Cloudfront distributions. There are 3 environments the scripts are being served from namely: dev,stg and prod.

The purpose of dev is to test out new experimental changes, stg is to make sure it will work in prod and prod is just prod. We recommend consuming teams to mirror the environment they are injecting to the environment their app is running on.

E.g Doc Portal's dev env app injects widget bundled script served from its dev env.