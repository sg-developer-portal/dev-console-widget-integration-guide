# Getting Started

Dev console widget is a web component built with [litjs](https://lit.dev/). See widget [live](https://docs.developer.tech.gov.sg/) on doc portal.

<!-- ![Figma](./assets/dev-console-widget.png ":size=100%")

![Figma Mobile](./assets/dev-console-widget-mobile.png ':size=50%') -->

## How it Works

To give a brief breakdown of how the bundle scripts work together : <br/>

![Gateway Script Flow](./assets/gateway-script-flow.png)

Your application will inject the gateway script which will subsequently inject the latest version of actual widget web component script.

To find out more about bundle scripts, attributes you can pass in to the scripts and the different script environments, read more in [`JS Bundle Breakdown`](./content/js-bundles-breakdown.md). Also, checkout [`Framework Integration`](./content/integration.md) to learn how dev console widget can be integrated with popular javascript frameworks.

Widget is a web component and thus you will use it as a html tag as such : 
```html
  <dev-console-widget 
  variant="mobile" 
  iconColor="black" 
  iconWidth="24px" 
  iconHeight="24px"
  ></dev-console-widget>
```
To learn more about available props, find out more in [`Widget Props`](./content/widget-props.md) and to learn more about styling, check out [`Style Customisation`](./content/style-customisation.md).

Lastly for important caveats to take note when implementing, read this [section](./content/important-caveats.md).
## Quick Start

Inject the script below in your application :

```html
<script type="module" id="dev-console-gateway" src="https://assets.developer.tech.gov.sg/bundled-scripts/dev-console-gateway.bundle.js"></script> 
```

Then, use the widget in your application as such :

```html
  <div class="sgds-container">
      <div class="sgds-navbar-brand">
          <a class="sgds-navbar-item" href="/">
              <img src="/assets/img/logo_color.svg" alt="Home">
          </a>
          <div class="sgds-navbar-item has-dropdown is-hoverable is-mega is-hidden-desktop">
              <dev-console-widget 
              variant="mobile" 
              iconColor="black" 
              iconWidth="24px" 
              iconHeight="24px"
              ></dev-console-widget>
          </div>
          <div class="sgds-navbar-burger" data-target="sgds-navbar-main">
              <span></span>
              <span></span>
              <span></span>
          </div>
      </div>
      <div id="sgds-navbar-main" class="sgds-navbar-menu">
          <div class="sgds-navbar-start">
            <div>Navbar Item 1</div>
            <div>Navbar Item 2</div>
            <div>Navbar Item 3</div>
          </div>
          <div class="sgds-navbar-end">
            <dev-console-widget
              variant="desktop"
              class="sgds-navbar-item is-mega is-hidden-touch"
              iconColor="black"
              iconWidth="28px"
              iconHeight="28px"
            ></dev-console-widget>
          </div>
      </div>
  </div>
```

?> You may have noticed `is-hidden-touch` and `is-hidden-desktop` in the class list. Find out more in [`Important Caveats`](./content/important-caveats.md).

## Widget Placement Guide

Our guideline is to place the widget on the `far right in desktop view` and `on the left of hamburger in mobile view`. An example is shown below.

**Desktop** <br/>
![Desktop Widget Placement](assets/desktop-widget-placement.png)

**Mobile** <br/>
![Mobile Widget Placement](assets/mobile-widget-placement.png)

## Resources

- [Technical Documentation](https://confluence.ship.gov.sg/display/DEV/Developer+Console+Widget?src=contextnavpagetreemode)
- [Self Publishing Repository](https://github.com/GovTechSG/dev-console-products-info)
- [Telegram channel](https://t.me/+k87OuBm9MARkYjk1)