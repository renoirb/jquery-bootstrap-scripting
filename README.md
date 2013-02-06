Contains some nice [jQuery](http://jquery.com) plugins to make working with the facilities provided by the 
[bootstrap](http://twitter.github.com/bootstrap) CSS framework more fun.

# Dialog Two, a jQuery dialog plugin based on the bootstrap modal dialog

`jquery.dialog2.js` uses the modal dialog provided by bootstrap and provides a controlable dialog functionality for web pages as a [jQuery](http://jquery.com) plugin. 

## Features

* Ajax content
* Ajax links and form submission in the dialog
* Loading indicator
* Localization
* No dependencies to jQuery.UI
* Control via [`html` markup](#let-the-markup-rule) possible

## Dependencies

* [jQuery.form](http://jquery.malsup.com/form/) plugin >= 2.8 for ajax form submit 
* [jQuery.controls](https://github.com/Nikku/jquery-controls) plugin  >= 0.9 for ajax link binding support
* [Bootstrap styles](http://twitter.github.com/bootstrap) 2.x to look nice

## Migrating from earlier versions
* Migration from 1.x:
    * You will need bootstrap 2.x on your page
    * Change your css markup for forms according to the bootstrap 2.0 (buttons always require `.btn`, `.primary` to `.btn-primary`, `.actions` to `.form-actions` and so forth. 

## Controlling a dialog with JavaScript

* `$(selector | element | text).dialog2(options)` creates a dialog with the given `options` from an element or selector. If the selected element exists, it will be turned into the body of the newly created dialog.
* `$(".selector").dialog2("method", argument1, argument2, ...)` executes an API method (see below)

### Options

The `dialog2()` method accepts an *options object*:

```javascript
{
  id: "my-dialog", // id which (if specified) will be added to the dialog to make it accessible later 
  autoOpen: true | false, // Should the dialog be automatically opened?
  title: "Title of the dialog", 
  buttons: {
    button1Label: callback | object, 
    button2Label: callback, 
    ...
  }, 
  closeOnOverlayClick: true | false, // Should the dialog be closed on overlay click?
  closeOnEscape: true | false, // Should the dialog be closed if [ESCAPE] key is pressed?
  removeOnClose: true | false, // Should the dialog be removed from the document when it is closed?
  showCloseHandle: true | false, // Should a close handle be shown?
  initialLoadText: "" // Text to be displayed when the dialogs contents are loaded
}
```

When adding buttons to a dialog, a *button options object* can be passed to it instead of a callback:

```javascript
{
  click: function() { }, // callback to execute on button click (has this bound to the dialog)
  primary: true | false, // if this button is the primary button (will be styled accordingly)
  type: "info" // basically additional classes to be attached to the button
}
```

### API

* `open()`: Opens the dialog (essentially shows it if it is still hidden)
* `close()`: Closes the dialog and removes it from the document (if configured)
* `options(options)`: Applies the given options to the dialog
* `addButton(label, options)`: Adds a button with the given `label` to the dialog. If `options` is a function binds the click or "enter" action of the button to it. Accepts a *button options object* too to customize the look and feel of the button.
* `removeButton(label)`: Removes the button with the specified `label` from the list of buttons

## Let the markup rule

The dialog has some distinct features which make it totally controllable via `html` markup. 

* When the [jquery-controls](https://github.com/Nikku/jquery-controls) plugin is added to a website, `a.open-dialog` links will open a dialog which shows in page or ajax content.
* If the dialog content updates, a dialog will change its title to the contents of the first `h1` element found in it. Additionally its buttons are updated based on the buttons found inside the `.actions` element of the dialog content. 
* `.ajax` forms will be submitted asynchronously and the result will be displayed in the dialog.

## Changing the UI

### Dialog width

Override the boostrap styles for `.modal` which are (as of version 1.3)

```css
.modal {
    ...
    width: 560px;
    top: 50%;
    margin: -250px 0 0 -250px;
}
````

with your new styles for wide dialogs 

```css
.wide-modal {
    ...
    top: 20%;
    width: 800px;
    margin: 0 0 0 -400px; /* -1 * (width / 2) */ 
}
```

and add the class `wide-modal` to big dialogs. 

## Extending the dialog

A user may plug into the dialog behaviour by listening to events, namely 

```
dialog2.ajax-start: fired right before an ajax request is started, 
dialog2.ajax-complete: fired when an ajax request was executed successfully, 
dialog2.content-update: when the dialogs contents are updated (after ajax-complete)
```

The preferred way to register handlers on these events is [$().delegate](http://api.jquery.com/delegate/). 
The code snipped shown below illustrates how this behaviour can be used to implement a *auto-close* ability for a dialog (see issue #19).

```javascript

// If a a.auto-close is contained in the dialogs content, 
// the dialog will close itself automatically and (optionally)
// redirect to a new page
$(document).delegate(".modal", "dialog2.content-update", function() { 
     // got the dialog as this object. Do something with it!
     
    var e = $(this);
    
    var autoclose = e.find("a.auto-close");
    if (autoclose.length > 0) {
        e.dialog("close");
        
        var href = autoclose.attr('href');
        if (href) {
            window.location.href = href;
        }
    }
});

```

## Check out some examples

Go to [the plugins web page](http://nikku.github.com/jquery-bootstrap-scripting/) to check out a number of examples on usage.
