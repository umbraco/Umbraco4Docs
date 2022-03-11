---
versionFrom: 8.18.0
meta.Title: "Serverside Sanitizing"
meta.Description: "This section describes how to sanitize the Rich Text Editor serverside"
---

# Sanitizing the Rich Text Editor

The rich text editor is sanitized on the frontend by default, however, you may want to do this serverside as well.
The libraries that are out there tend to have very strict, and therefore, problematic dependencies, so we'll leave it up to you how you want to sanitize the HTML.

## Implementing your own IHtmlSanitizer

To make this task as easy as possible we've added an abstraction called `IHtmlSanitizer`, by default this doesn't do anything, but you can overwrite it with your own implementation to handle sanitization how you see fit.
This interface only has a single method `string Sanitize(string html)`, the output of this method is what will be stored in the database when you save a RichText editor.

To add your own sanitizer you must first create a class that implements the interface:

```c#
using Umbraco.Core.Security;

namespace MySite.HtmlSanitization
{
    public class MySanitizer : IHtmlSanitizer
    {
        public string Sanitize(string html)
        {
            // Sanitize the html parameter here
            return "<h1>Sanitized HTML</h1>";
        }
    }
}
```

As you can see this specific implementation doesn't do a whole lot, but the `Sanitize` method is where you can use a library, or even your own sanitizer implementation, to sanitize the RichText editor input.

Now that you've added your own custom `IHtmlSanitizer` you must register it in the container to replace the existing NoOp sanitizer using a Composer:

```c#
using Umbraco.Core;
using Umbraco.Core.Composing;
using Umbraco.Core.Security;

namespace MySite.HtmlSanitization
{
    public class SanitizerComposer : IComposer
    {
        public void Compose(Composition composition)
        {
            composition.RegisterUnique<IHtmlSanitizer, MySanitizer>();
        }
    }
}
```

If you've followed along you'll now see that no matter what you type in a Rich Text Editor when you save it, it'll always only contain a heading that says "Sanitized HTML", this is of course isn't that helpful, but it shows that everything is working as expected and that whatever your sanitizer returns are what will be saved.

