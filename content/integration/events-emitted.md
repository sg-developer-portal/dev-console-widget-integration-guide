# Events Emitted

This portion lists some dom events that are emitted by the widget that may be useful for host applications to listen to and execute callback functions.

## devConsoleWidgetToggle

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