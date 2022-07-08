---
versionFrom: 9.0.0
versionTo: 10.0.0
---

# Controller & Action Selection

When you make a page request to the MVC application, a controller is responsible for returning the response to that request. The controller can perform one or more actions. The controller action can return different types of action results based on the request.

## Default Controller Action

By default, Umbraco will execute every request via it's built-in default controller: `Umbraco.Cms.Web.Common.Controllers.RenderController`. Umbraco site automatically routes all the front-end requests via the `Index` action of the `RenderController`.

```csharp
using Umbraco.Cms.Web.Common.Controllers;
using Microsoft.Extensions.Logging;
using Microsoft.AspNetCore.Mvc.ViewEngines;
using Umbraco.Cms.Core.Web;
using Microsoft.AspNetCore.Mvc;

namespace UmbracoProject.Controller
{
    public class HomePageController : RenderController
    {

        public HomePageController(ILogger<RenderController> logger, ICompositeViewEngine compositeViewEngine, IUmbracoContextAccessor umbracoContextAccessor)
        : base(logger, compositeViewEngine, umbracoContextAccessor)
        {
        }
        public override IActionResult Index()
        {
            return CurrentTemplate(CurrentPage);
        }

        public IActionResult HomePage()
        {
            return CurrentTemplate(CurrentPage);
        }
    }
}

```

## Change the Default Controllers

It is possible to implement a custom Controller to replace the default implementation to give complete control during the Umbraco request pipeline execution. You can configure Umbraco to use your implementation in the `ConfigureServices` method in the `Startup.cs` class, for example:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddUmbraco(_env, _config)
        .AddBackOffice()             
        .AddWebsite()
        .AddComposers()
        .Build();

    // Configure Umbraco Render Controller Type
    services.Configure<UmbracoRenderingDefaultsOptions>(c =>
    {
        c.DefaultControllerType = typeof(MyRenderController);
    });
}
```

Ensure that the controller inherits from the base controller `Umbraco.Cms.Web.Common.Controllers.RenderController`. You can override the `Index` method to perform any customizations of your choice.

## Custom Controller Selection

You can create Custom controllers for different Document Types and Templates. This is termed 'Hijacking Umbraco Routes'. For details on how this process works, see the [Custom MVC Controllers (Umbraco Route Hijacking)](../../../Reference/Routing/custom-controllers.md) article.
