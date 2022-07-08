---
versionFrom: 7.0.0
meta.Title: "Coding Standards"
meta.Description: "A guide to Javascript coding standards in Umbraco."
---

# JavaScript coding standards and guidelines

_All JavaScript in the Umbraco core should adhere to these guidelines. The legacy JavaScript code in the core doesn't adhere to these guidelines but should be refactored so that it does._

**All JavaScript in the backoffice needs to be in a namespace and defined in a class.**

## Namespaces
To declare a namespace for your JavaScript class you can use the following command (as an example to create a namespace called 'Umbraco.Controls'):

```javascript
Umbraco.Sys.registerNamespace("Umbraco.Controls");
```

The above code will require a reference to the NamespaceManager.js file which should generally be included by default in all pages in Umbraco.

## jQuery
If you are going to use jQuery and its dollar ($) operator, you will need to wrap your code in a self-executing function, this is to ensure that your code will still work with jQuery.noConflict() turned on. Example:

```javascript
(function($) {
    // your code goes here
    alert($);
})(jQuery);
```

To create jQuery plugins, see the [jQuery Plugin Guidelines](jquery-guidelines.md)

## Creating classes

There are quite a few different ways to create classes in JavaScript. For Umbraco we have opted to use the 3rd party, classical inheritance library, [Base2](https://code.google.com/archive/p/base2/) to make class declarations extendable:

```javascript
Umbraco.Sys.registerNamespace("MyProject.MyNamespace");

MyProject.MyNamespace.NamePrinter = base2.Base.extend({

    // in order to make private methods/variables accessible
    // to derived types, everything actually has to be public
    // so to identify private variables, prefix with an underscore

    // private methods/variables

    _isDebug: true,
    _timer: 100,
    _currIndex: 0,

    _log: function (p) {
        // this is a private method that can only be
        // accessed inside of this class
        if (this._isDebug) {
            console.dir(p);
        }
    }

    // public methods/variables

    name: ctorParams,
    start: function() {
        this._log("start method called");

        // need to create a closure so we have a reference to our
        // current this object in the interval function
        var _this = this;

        // this will write the name out to the console one letter
        // at a time every _timer interval
        setInterval(function() {
            if (_this._currIndex < _this.name.length) {
            console.info(_this.name[_this._currIndex]);
            _this._currIndex++;
            }
        }, _this._timer);
    }

})
```

You can then use the class, as shown in example:

```javascript
var printer = new NamePrinter("Shannon");
printer.start();

// or since we exposed the name property publicly,
// we can set it after the constructor
var printer2 = new NamePrinter();
printer2.name = "Shannon";
printer2.start();
```

## Singleton classes

Sometimes it's useful to have a class that can only be instantiated once and shared, rather than have multiple unsynchronised instances floating around. In those circumstances a Singleton pattern should be used.

Define a singleton class:

```javascript
Umbraco.Sys.registerNamespace("MyProject.MyNamespace");

MyProject.MyNamespace.NamePrinterManager = base2.Base.extend({

    // in order to make private methods/variables accessible
    // to derived types, everything actually has to be public
    // so to identify private variables, prefix with an underscore

    // private methods/variables

    _registeredPrinters: [],

    // public methods/variables

    registerPrinter: function(printer) {
        this._registeredPrinters[printer.name] = printer;
    },
    getPrinter: function(name) {
        return this._registeredPrinters[printer.name];
    }

}, { // Static members

    // private methods/variables
    _instance: null,

    // Singleton accessor
    getInstance: function () {
        if(this._instance == null)
            this._instance = new MyProject.MyNamespace.NamePrinterManager();
        return this._instance;
    }

});
```

Defining a singleton is the same as defining a regular class, except that we also define a static "getInstance" accessor for accessing the entity in a controlled manner. By providing the static accessor we can ensure only one instance of the class is created per request. See example:


```javascript
var printer = new NamePrinter("Shannon");
MyProject.MyNamespace.NamePrinterManager.getInstance().registerPrinter(printer);
```

## Static classes

Sometimes it's useful to have static classes that require no constructor. Before you make one of these, definitely make sure that you won't require different instances of one.

An example of static classes:

```javascript
Umbraco.Sys.registerNamespace("MyProject.MyNamespace");

MyProject.MyNamespace.Utility = base2.Base.extend(null, {

    showMsg: function(msg) {
        alert(msg);
    }
})
```

Using the class/method requires no instantiation but you can therefore have no separate instances of Utility:

```javascript
MyProject.MyNamespace.Utility.showMsg("hello");
```

## The difference between a Singleton class and Static class

Both singleton and static classes allow you to access methods directly without having to create an entity of your own. The main difference between the two, and what should govern when to use one over the other, is one of state.

A singleton class can hold information which can be manipulated and retrieved via its public methods. It will be stored between method calls, whereas static methods should only manipulate and return values which it can gather from its parameters and should not be persisted between individual method calls.

A good example of a Singleton is the one highlighted above, "NamePrinterManager". Here printers can be registered using the registerPrinter method for storage, later retrieved using the getPrinter method. Here a singleton is used as you will only want one central repository of printers.

A good example use of a Static class is for helper methods, where each method will perform a single self-contained task based upon the parameters passed in and will return an immediate response.
