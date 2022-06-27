# Important Caveats

## CSS Consideration
The web component has a dropdown which is `position:absolute` to the navbar container. If you are using [sgds](https://www.designsystem.tech.gov.sg/), it will be relative to `sgds-navbar` class. If you are not using [sgds](https://www.designsystem.tech.gov.sg/) then you will have to set your custom navbar container as `position:relative` for the dev-console-widget dropdown to behave properly.

## Responsivity Consideration
dev-console-widget can be used as a `mobile variant`, `desktop variant` or `both` depending on how you want to integrate it. In general, if you want to nest the widget with other nav items, you should use desktop and mobile variant together like below : 

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
?> The instances of widget components communicate and sync with each other across the dom through an event controller.


The components will appear and disappear according to your responsive media queries for `mobile variant` and `desktop variant`. The reason is because not every app will have the same breakpoint and it will be difficult for us to predetermine them, therefore we leave it up to the hosts to determine their own breakpoints.

If no `variant` prop is passed in, widget will default to `both` variant which follows its own breakpoints. This means that it will have to be separate from other nav items like shown below :

```html
  <div class="sgds-container">
      <div class="sgds-navbar-brand">
          <a class="sgds-navbar-item" href="/">
              <img src="/assets/img/logo_color.svg" alt="Home">
          </a>
          <div class="sgds-navbar-item has-dropdown is-hoverable is-mega">
              <dev-console-widget 
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
            ...
          </div>
      </div>
  </div>
```