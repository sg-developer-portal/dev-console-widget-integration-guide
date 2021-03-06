# Getting Started

## How it Works

To give a brief breakdown of how the bundle scripts work together : <br/>

![Gateway Script Flow](../../assets/gateway-script-flow.png)

Your application will inject the gateway script which will subsequently inject the latest version of actual widget web component script.

To find out more about bundle scripts, attributes you can pass in to the scripts and the different script environments, read more in `Javascript Bundles` subsection. Also, checkout `Framework Integration` to learn how dev console widget can be integrated with popular javascript frameworks.

Widget is a web component and thus you will use it as a html tag as such : 
```html
  <dev-console-widget 
  variant="mobile" 
  iconColor="black" 
  iconWidth="24px" 
  iconHeight="24px"
  ></dev-console-widget>
```
To learn more about available props, find out more in `Widget Props` and to learn more about styling, check out `Style Customisation`.

Lastly for important caveats to take note when implementing, read the `Important Caveats` subsection.
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

?> You may have noticed `is-hidden-touch` and `is-hidden-desktop` in the class list. Find out more in `Important Caveats`.

## Widget Placement Guide

Our guideline is to place the widget on the `far right in desktop view` and `on the left of hamburger in mobile view`. An example is shown below.

**Desktop** <br/>
![Desktop Widget Placement](../../assets/desktop-widget-placement.png)

**Mobile** <br/>
![Mobile Widget Placement](../../assets/mobile-widget-placement.png)