# Collapsible Controls
A script to make any control collapsible

https://github.com/stadium-software/collapse-controls/assets/2085324/96ed1b20-a77f-4cd1-bd2b-abd77eb6217d

## Version
1.1 - fixed "layout control did not collapse" CSS bug (copy collapsible-control.css file into the application to fix)

1.2 Added 'CollapseOnClickAway' parameter

1.3 Integrated CSS into script

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "CollapseControl"
2. Add the input parameters below to the Global Script
   1. ControlClassName
   2. HeaderTitle
   3. StartCollapsed
   4. CollapseOnClickAway
3. Drag a *JavaScript* action into the script
4. Add the Javascript below unchanged into the JavaScript code property
```javascript
/* Script Version 1.3 https: //github.com/stadium-software/collapse-controls */
let controlClass = ~.Parameters.Input.ControlClassName;
let controlTitle = ~.Parameters.Input.HeaderTitle;
let startCollapsed = ~.Parameters.Input.StartCollapsed;
let collapseOnClickAway = ~.Parameters.Input.CollapseOnClickAway;

loadCSS();
let collapsibleControls = document.querySelectorAll("." + controlClass);
for (let i = 0; i < collapsibleControls.length; i++) {
    let wrapper = document.createElement("div");
    let innerContainer = document.createElement("div");
    let headerContainer = document.createElement("div");

    innerContainer.classList.add("collapsible-control-inner");
    wrapper.classList.add("collapsible-control-wrapper");
    if (startCollapsed) {
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
if (collapseOnClickAway) {
    document.body.addEventListener("click", function (e) {
        let el = document.querySelector(".collapsible-control-wrapper:has(." + controlClass + ")");
        if (!e.target.closest(".collapsible-control-wrapper:has(." + controlClass + ")") && el) el.classList.add("control-collapsed");
    });
}
function loadCSS() {
    let moduleID = "stadium-collapsible-control";
    if (!document.getElementById(moduleID)) {
        let cssMain = document.createElement("style");
        cssMain.id = moduleID;
        cssMain.type = "text/css";
        cssMain.textContent = `
/* CSS Version 1.1 https: //github.com/stadium-software/collapse-controls */
.collapsible-control-wrapper {
    display: inline-block;
    border: 0.1rem solid var(--collapsible-control-border-color, var(--GENERAL-BORDER-COLOR));
    margin-top: 1rem;
    margin-right: 1.6rem;
    vertical-align: top;
    background-color: var(--collapsible-control-background-color, var(--BODY-BACKGROUND-COLOR));

    .collapsible-control-header {
        cursor: pointer;
        user-select: none;
        position: relative;
        display: flex;
        align-items: center;
        font-size: var(--collapsible-control-header-font-size, var(--FONT-SIZE-LARGE));
        color: var(--collapsible-control-header-font-color, var(--BODY-FONT-COLOR));
        padding: var(--collapsible-control-header-topbottom-padding, 0.6rem) var(--collapsible-control-header-rightleft-padding, 1.2rem);
        padding-right: max(3.6rem, var(--collapsible-control-header-rightleft-padding, 1.2rem));
        min-height: var(--collapsible-control-header-collapse-image-size, 2rem);
        background-color: var(--collapsible-control-header-background-color, var(--DARKER-GREY));
    }

    .collapsible-control-header:before {
        content: "";
        position: absolute;
        width: var(--collapsible-control-header-collapse-image-size, 2rem);
        height: 100%;
        top: 0;
        right: var(--collapsible-control-header-rightleft-padding, 1.2rem);
        background-image: var(--collapsible-control-header-collapse-image, url('data: image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxZW0iIGhlaWdodD0iMWVtIiB2aWV3Qm94PSIwIDAgNTEyIDUxMiI+PHBhdGggZmlsbD0iIzg4ODg4OCIgZD0ibTY0IDE0NGwxOTIgMjI0bDE5Mi0yMjRINjR6Ii8+PC9zdmc+'));
        background-repeat: no-repeat;
        background-position: center;
        background-size: var(--collapsible-control-header-collapse-image-size, 2rem);
        transition: transform var(--collapsible-control-expansion-speed, 500ms);
        transform: rotate(180deg);
    }

    .collapsible-control-inner {
        display: grid;
        grid-template-rows: 1fr;
        transition: grid-template-rows var(--collapsible-control-expansion-speed, 500ms), padding var(--collapsible-control-expansion-speed, 500ms);
        padding: 0 var(--collapsible-control-rightleft-padding, 1.2rem);

        > div {
            overflow: hidden;
            padding-right: 0;
            margin-top: 0;
        }

        > .control-container > *:first-child {
            padding-top: 1rem;
        }
    }

}
.control-collapsed .collapsible-control-inner {
    grid-template-rows: 0fr;
}
.control-collapsed .collapsible-control-header:before {
    transform: rotate(0deg);
}
html {
    min-height: 100%;
    font-size: 62.5%;
}        
        `;
        document.head.appendChild(cssMain);
    }
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
   3. StartCollapsed: A boolean (true / false) indicating whether the control should initially be shown as collapsed. The default is 'false'
   4. CollapseOnClickAway: A boolean (true / false) indicating whether the control should collapse when user click outside of it. The default is 'false'

## CSS
Variables exposed in the [*collapsible-control-variables.css*](collapsible-control-variables.css) file can be [customised](#customising-css).

### Customising CSS
1. Open the CSS file called [*collapsible-control-variables.css*](collapsible-control-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Stadium 6.12+ users can comment out any variable they do **not** want to customise
4. Add the [*collapsible-control-variables.css*](collapsible-control-variables.css) to the "CSS" folder in the EmbeddedFiles (overwrite)
5. Paste the link tag below into the *head* property of your application (if you don't already have it there)
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/collapsible-control-variables.css">
``` 
6. Add the file to the "CSS" inside of your Embedded Files in your application

## Upgrading Stadium Repos
Stadium Repos are not static. They change as additional features are added and bugs are fixed. Using the right method to work with Stadium Repos allows for upgrading them in a controlled manner. 

How to use and update application repos is described here: [Working with Stadium Repos](https://github.com/stadium-software/samples-upgrading)