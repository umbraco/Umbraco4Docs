---
versionFrom: 7.0.0
meta.Title: "jQuery coding guidelines"
meta.Description: "A guide to coding in jQuery in Umbraco."
---

# jQuery coding guidelines

_Ensure that you have read the [JavaScript Guidelines](js-guidelines.md) document before continuing. As specified in the [JavaScript Guidelines](js-guidelines.md) document, method names are named in "camelCase" and therefore jQuery plugins (since they are methods) are named as "camelCase"._

Like with other JavaScript in the Umbraco backoffice, you need to wrap your class in the jQuery self-executing function if you want to use the dollar ($) operator.

## jQuery plugins
jQuery plugins don't require an internal class to perform the functionality and therefore do not expose or return an API. These could be vertically aligning something:

```javascript
(function($) {
    $.fn.verticalAlign = function(opts) {
        // we are not using opts (options) for this plugin
        // but you could!
        return this.each(function() {
            var top = (($(this).parent().height() - $(this).height()) / 2);
            $(this).css('margin-top', top);
        });
    };
})(jQuery);
```

## Standard jQuery plugins
Most jQuery plugins will expose an API or a way in which a developer can interact with the plugin, not instantiating it. To do this we need to create a class that does the work of the plugin and then expose that class via a different jQuery plugin.

### Naming Conventions
There are many different ways to expose an API for a jQuery plugin, in Umbraco the standard will be:

* `$("#myId").myFirstJQueryPlugin();` - to instantiate the plugin
* `var pluginApi = $("#myId").myFirstJQueryPluginApi();` - to retrieve the plugin API for that selector

So essentially, we'll be creating 2 plugins, one to instantiate it and one to retrieve the API. The naming conventions are obvious, create your plugin name and then append the term *Api* to create your API plugin name.

### Creating the plugins

```javascript
// using the same vertical align concept but we'll expose an API for it
// ( not that this is very useful :) )

Umbraco.Sys.registerNamespace("MyProject.MyNamespace");

    (function($) {

        // create the standard jQuery plugin

        $.fn.verticalAlign = function(opts) {
            // we are not using opts (options) for this plugin
            // but you could!
            return this.each(function() {
                // create the aligner for the current element
                var aligner = new MyProject.MyNamespace.VerticalAligner($(this));
            });
        };

        // create the Api retriever plugin

        $.fn.verticalAlignApi = function () {
            // ensure there is only one
            if ($(this).length != 1) {
                throw "Requesting the API can only match one element";
            }
            // ensure this has a vertical aligner applied to it
            if ($(this).data("api") == null) {
                throw "The matching element had not been bound to a VerticalAligner ";
            }
            return $(this).data("api");
        }

        // create a JavaScript class to support the plugin

        MyProject.MyNamespace.verticalAligner = function(elem) {
            // the jQuery selector
            var _e = elem;
            var api = {
            align: function() {
                var top = ((_e.parent().height() - _e.height()) / 2);
                _e.css('margin-top', top);
            }
            }
            // store the api object in the jQuery data object for
            // the current selector
            _e.data("api", api);

            // return the api object
            return api;
        }

    })(jQuery);
  ```

### Consuming the plugins

To use the plugin and API requires a few lines of code, see example:

```javascript
$("#myId").verticalAlign();
// now to get the api and do the alignment
$("#myId").verticalAlignApi().align();
```
