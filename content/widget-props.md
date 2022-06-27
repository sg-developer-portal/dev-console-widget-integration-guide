# Widget Props

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