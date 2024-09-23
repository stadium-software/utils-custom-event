# Custom Events <!-- omit in toc -->

Stadium controls support the most frequently needed events. Sometimes we may want to listen to events that are not natively supported by a control. This module provides a way to attach events to a select set of controls. 

Video / screenshot

# Version
Initial 1.0

# Setup

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script
1. Create a Global Script called "CustomEvent"
2. Add the input parameters below to the Global Script
   1. ClassName
   2. Callback
   3. Event
   4. RunOnce
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script v1.0 https://github.com/stadium-software/utils-custom-event */
let scope = this,
    classname = ~.Parameters.Input.ClassName,
    callback = ~.Parameters.Input.Callback,
    ev = ~.Parameters.Input.Event,
    once = ~.Parameters.Input.RunOnce,
    controls = [
        { parentClass: 'button-container', childEl: 'button' },
        { parentClass: 'link-container', childEl: 'a' },
        { parentClass: 'label-container', childEl: 'span' },
        { parentClass: 'image-container', childEl: 'img' },
        { parentClass: 'text-box-container', childEl: 'input[type="text"]' },
        { parentClass: 'check-box-container', childEl: 'label' },
        { parentClass: 'date-picker-container', childEl: 'input[type="text"], button' },
        { parentClass: 'drop-down-container', childEl: 'select' },
        { parentClass: 'radio-button-list-container', childEl: 'label' },
        { parentClass: 'check-box-list-container', childEl: 'label' },
        { parentClass: 'upload-file-container', childEl: 'label' }
    ],
    els;
let elParent = document.querySelector("." + classname);
let callScript = (e) => {
    scope[callback](e.type);
};
if (elParent) {
    for (let i = 0; i < controls.length; i++) {
        if (elParent.classList.contains(controls[i].parentClass)) { 
            els = elParent.querySelectorAll(controls[i].childEl);
            for (let j = 0; j < els.length; j++) {
                attach(els[j]);
            }
        }
    }
}
function attach(el) { 
    el.addEventListener(ev, callScript, { once: once });
}
```

## Page
1. Drag any of the controls below to the page
   1. Button
   2. Link
   3. Label
   4. Image
   5. TextBox
   6. CheckBox
   7. DatePicker
   8. DropDown
   9. RadioButtonList
   10. CheckBoxList
   11. UploadFile
2.  Add a class that uniquely identifies the control on this page (e.g. custom-text-box-blur-event)

## Custom Event Handler
1. Create a script under the page and name it anything you like (e.g. CustomEventHandler)
2. Add an input parameter to the script called "Type"
3. Add actions to the script to process the event

## Event Handler Setup
1. Drag the "CustomEvent" to the Page.Load or any other event handler or script
2. Complete the input parameters
   1. ClassName: The unique classname you added to the control (e.g. custom-text-box-blur-event)
   2. Callback: The custom event handler script you created above (e.g. CustomEventHandler)
   3. Event: Enter the [event](https://www.w3schools.com/jsref/dom_obj_event.asp). This event will be attrached to the control. Not every event works with every control. Here is a [tutorial on events](https://www.w3schools.com/js/js_events.asp). 
   4. RunOnce: When this value is true, the event will only fire one time. 

## Working with Stadium Repos
Stadium Repos are not static. They change as additional features are added and bugs are fixed. Using the right method to work with Stadium Repos allows for upgrading them in a controlled manner. How to use and update application repos is described here 

[Working with Stadium Repos](https://github.com/stadium-software/samples-upgrading)