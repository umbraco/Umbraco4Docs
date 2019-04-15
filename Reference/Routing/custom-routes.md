---
keywords: customizing external source integration
versionFrom: 8.0.0
---
    
# Custom MVC Routes

_Documentation about how to setup your own custom controllers and routes that need to exist alongside the Umbraco pipeline_

## Where to put your routing logic?

In Umbraco the best place to put your routing logic is in the Initialize() method of an `Umbraco.Core.Composing.IComponent` implementation. There you can add any custom routing logic you like and Umbraco will add the routes during it's start up.

## User defined routes

Umbraco doesn't interfere with any user defined routes that you wish to have. Your custom routes to your own custom controllers will work perfectly and seamlessly alongside Umbraco's routes.

## Custom routes within the Umbraco pipeline

You can specify your own custom MVC routes to work within the Umbraco pipeline. This requires you to use an implementation of `Umbraco.Web.Mvc.UmbracoVirtualNodeRouteHandler` with your custom route, essentially to provide the `IPublishedContent` context of the custom route through the Umbraco request pipeline.

As an example:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;
using System.Web.Routing;
using Umbraco.Core.Composing;
using Umbraco.Web;
using Umbraco.Web.Mvc;

namespace Umbraco8.Components
{

    public class RegisterCustomRouteComposer : ComponentComposer<RegisterCustomRouteComponent>
    {

    }

    public class RegisterCustomRouteComponent : IComponent
    {
        public void Initialize()
        {
    // Custom route to MyProductController which will use a node with a specific ID as the
    // IPublishedContent for the current rendering page
            RouteTable.Routes.MapUmbracoRoute("ProductCustomRoute", "products/{action}/{id}", new
            {
                controller = "MyProduct",
                id = UrlParameter.Optional
            }, new ProductsRouteHandler(1105));
        }

        public void Terminate()
        {
            throw new NotImplementedException();
        }
    }
}
```

This is using a extension method: `MapUmbracoRoute` which takes in the normal routing parameters (you can also include constraints, namespaces, etc….) but notice the instance of `UmbracoVirtualNodeRouteHandler` which is required.

The instance of `UmbracoVirtualNodeRouteHandler` is responsible for associating an `IPublishedContent` with this route. It has one abstract method which must be implemented:

```csharp
IPublishedContent FindContent(RequestContext requestContext, UmbracoContext umbracoContext)
```

It has another virtual method that can be overridden which will allow you to manipulate the PublishedContentRequest however you’d like:

```csharp
PreparePublishedContentRequest(PublishedRequest request)
```

So how do you find content to associate with the route? Well that’s up to you, one way (as seen above) would be to specify a node Id. In the example `ProductsRouteHandler` is inheriting from `UmbracoVirtualNodeByIdRouteHandler` which accepts an id of a content item to use as the IPublishedContent associated with the route:

```csharp
namespace Umbraco.Web.Mvc
{
    public class UmbracoVirtualNodeByIdRouteHandler : UmbracoVirtualNodeRouteHandler
    {
        public UmbracoVirtualNodeByIdRouteHandler(int realNodeId);

        protected sealed override IPublishedContent FindContent(RequestContext requestContext, UmbracoContext umbracoContext);
        protected virtual IPublishedContent FindContent(RequestContext requestContext, UmbracoContext umbracoContext, IPublishedContent baseContent);
    }
}
```

So based on all this information provided in these methods, you can associate whichever IPublishedContent item you want / feels most appropriate to a request.

## Virtual Content
This implementation expects **any** instance of `IPublishedContent`, so this means you can create your own virtual nodes with any custom properties you want. Generally speaking you’ll probably have a real Umbraco `IPublishedContent` instance as a reference point, so you could create your own virtual `IPublishedContent` item based on `PublishedContentWrapped`, pass in this real node and then just override whatever properties you want, like the page Name, etc..

Whatever instance of `IPublishedContent` returned in the `FindContent` method will be converted to a `ContentModel` for use in your controllers.

## Controllers
Controllers are straight forward and work like any other routed controller except that the Action will have an instance of ContentModel mapped to it’s parameter.

```csharp
public class MyProductController : RenderMvcController
{
    public ActionResult Product(ContentModel model, string id)
    {
        // in my case, the IPublishedContent attached to this
        // model will be my products node in Umbraco which i
        // can now use to traverse to display the product list
        // or lookup the product by sku / id

        if (string.IsNullOrEmpty(id))
        {
            // render the products list if no sku/id
            return RenderProductsList(model);
        }
        else
        {
            return RenderProduct(model, id);
        }
    }
}
```
