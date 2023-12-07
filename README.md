# Collapsible Controls
A script to make any control collapsible

https://github.com/stadium-software/collapse-controls/assets/2085324/96ed1b20-a77f-4cd1-bd2b-abd77eb6217d

## Sample Application
This repo contains a sample application created in Stadium v 6.7
[CollapseControls.sapz](Stadium6/CollapseControls.sapz?raw=true)

## Version
1.0

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "CollapseControl"
2. Add the input parameters below to the Global Script
   1. ControlClassName
   2. HeaderTitle
   3. StartCollapsed
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/*Stadium Script Version 1.0*/
let controlClass = ~.Parameters.Input.ControlClassName;
let controlTitle = ~.Parameters.Input.HeaderTitle;
let startCollpsed = ~.Parameters.Input.StartCollapsed;

let collapsibleControls = document.querySelectorAll("." + controlClass);
for (let i = 0; i < collapsibleControls.length; i++) {
    let wrapper = document.createElement("div");
    let innerContainer = document.createElement("div");
    let headerContainer = document.createElement("div");

    innerContainer.classList.add("collapsible-control-inner");
    wrapper.classList.add("collapsible-control-wrapper");
    if (startCollpsed) {
        wrapper.classList.add("control-collapsed");
    }
    headerContainer.classList.add("collapsible-control-header");
    headerContainer.addEventListener("click", function (e) {
        e.target.closest(".collapsible-control-wrapper").classList.toggle("control-collapsed");
    });
    if (controlTitle) {
        headerContainer.textContent = controlTitle;
    }
    wrapper.appendChild(headerContainer);
    wrapper.appendChild(innerContainer);
    collapsibleControls[i].before(wrapper);
    innerContainer.appendChild(collapsibleControls[i]);
}
```

## Page Setup
1. Drag any control to a page
2. Assign a classname to the control (e.g. collapse-control)

## Page.Load Setup
1. Drag the "CollapseControl" script into the Page.Load event handler (or another event handler that should cause the control to become collapsible)
2. Complete the input parameters
   1. ControlClassName: The classname you assigned to the control  (e.g. collapse-control)
   2. HeaderTitle: A title that will be shown above the collapsible control (optional)
   3. StartCollapsed: A boolean (true / false) indicating whether the control should initially be shown as collapsed (by default it will be expanded)

# Styling
Various elements in this module can be styled using the two CSS files in this repo

## Applying the CSS

**Stadium 6.6 or higher**
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*collapsible-control-variables.css*](collapsible-control-variables.css) and [*collapsible-control.css*](collapsible-control.css) into that folder
3. Paste the link tags below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/collapsible-control.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/collapsible-control-variables.css">
``` 

![](images/ApplicationHeadProp.png)

**Versions lower than 6.6**
1. Copy the CSS from the two css files into the Stylesheet in your application

## Customising CSS
1. Open the CSS file called [*collapsible-control-variables.css*](collapsible-control-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Overwrite the file in the CSS folder of your application with the customised file

## CSS Upgrading
To upgrade the CSS in this module, follow the [steps outlined in this repo](https://github.com/stadium-software/samples-upgrading)
