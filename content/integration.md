# Integration

Currently this widget can be consumed by host application through a script tag and below are the guides for the integration.

## Static Html

Utilising with static html is relatively straight forward. Just add gateway script tag into file that is used everywhere within the application e.g layout.html etc.

This will also work if you're utilising static html or server side rendered html together with a frontend javascript framework such as React or Vue. For details on client side script injection, refer to the portions below.

## React Application

<!-- For SPA's, the gateway script tag should be injected in the ```useEffect``` method of ```navbar``` component or components of similar nature. -->

<!-- ```js
useEffect(() => {
  const script = document.createElement('script');
  script.async = true;
  script.id = `dev-console-gateway`;
  script.type = `module`;
  // script.setAttribute('es5', '');
  // script.setAttribute('polyfills', '');
  script.src =
    'https://stg.assets.developer.tech.gov.sg/bundled-scripts/dev-console-gateway.bundle.js';
  document.head.appendChild(script);

  return () => {
    document.head.removeChild(script);
  };
}, []);
``` -->
For SPA's, gateway script should be injected into ```index.html``` in ```public``` folder or whereever the entry file is located at.

?> If you're using typescript, you will need to declare type in your respective *.d.ts file.

Example below is declared in global.d.ts file.

```ts
declare namespace JSX {
  interface IntrinsicElements {
    'dev-console-widget': any;
  }
}
```

You can then use the web component as a normal html tag like so : 

```html
<div className="sgds-masthead">
  <a href="https://www.gov.sg" target="_blank">
    <span className="sgds-icon sgds-icon-sg-crest"></span>
    <span className="is-text">A Singapore Government Agency Website</span>
  </a>
</div>
<nav className="sgds-navbar" role="navigation">
  <div className="sgds-navbar-brand">
    <span className="sgds-navbar-item">
      <Link id="techpay-logo" to="/">
        <img src={TechBizLogo} alt="Design System" />
      </Link>
    </span>
    <div className="sgds-navbar-item has-dropdown is-hoverable is-mega">
      <dev-console-widget
        iconColor="black"
        iconWidth="24px"
        iconHeight="24px"
      ></dev-console-widget>
    </div>
    <div
      className={`sgds-navbar-burger ${
        isBurgerActive ? 'is-active' : ''
      }`}
      data-target="mainnav-1"
      onClick={() => setIsBurgerActive(!isBurgerActive)}
    >
      <span></span>
      <span></span>
      <span></span>
    </div>
  </div>
```
<!-- You could also consider extracting this logic out into its own [hook](https://reactjs.org/docs/hooks-intro.html). -->


## Angular Application

Inject gateway script in ```index.html``` or relevant html files.

You will need to add ```CUSTOM_ELEMENTS_SCHEMA``` into the relevant application module to enable web components. For more information, refer [here](https://vaadin.com/learn/tutorials/using-web-components-in-angular). Example:

```js
@NgModule({
    declarations: [
        ...
    ],
    imports: [
       ...
    ],
    providers: [
        {
         ...
        }
    ],
    schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
```
You can then use the web component as per normal.

## Vue Application

Web components work well with Vue. You just need to inject the script tag in entry file(s).


## Events Emitted

### **devConsoleWidgetToggle**

A custom event emitted into the dom when widget is opened and closed. Returns `true` when open and `false` when closed.

CustomEvent breakdown:

```js
    const options = {
      detail: { isWidgetOpen: updatedIsOpen },
      bubbles: true,
      composed: true,
    };
    document.dispatchEvent(new CustomEvent("devConsoleWidgetToggle", options));
```

Consume event in your JS file as such:

```js
 document.addEventListener("devConsoleWidgetToggle", function (event) {
      const widgetState = event.detail.isWidgetOpen;
      // Widget is open
      if (widgetState) {
        document
          .querySelector(".sgds-navbar-burger")
          .classList.remove("is-active");
        document
          .querySelector(".sgds-navbar-menu")
          .classList.remove("is-active");
      }
  });
```

It can be used to close other opened dropdown or modals. For e.g in above code, hamburger menu is closed when widget is opened.
<!-- If you're building a SPA with Vue, the script tag will have to injected during runtime similar to React. This could be added in the ```created()``` lifecycle hook and cleanedup in ```beforeDestroy()``` hook. -->

<!-- !> I have not personally tried this out. -->

<!-- ```js
created(){
  const script = document.createElement('script');
  script.async = true;
  script.id = `dev-console-gateway`;
  script.type = `module`;
  // script.setAttribute('es5', '');
  // script.setAttribute('polyfills', '');
  script.src =
    'https://stg.assets.developer.tech.gov.sg/bundled-scripts/dev-console-gateway.bundle.js';
  document.head.appendChild(script);
}
...
beforeDestroy(){
  document.head.removeChild(script);
}
```

You can also consider extracting this logic out into a composable using [composition api](https://vuejs.org/guide/extras/composition-api-faq.html). -->