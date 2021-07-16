---
versionFrom: 7.0.0
needsV8Update: "true"
---

:::note
**This article has not yet been verified against Umbraco 8.**

The concepts and code examples might not work if you are running Umbraco 8.0 or a later version. If you are using Umbraco 7, this article is perfect for you!

You are more than welcome to report any issues found on the [Documentation Issue Tracker](https://github.com/umbraco/UmbracoDocs/issues).
:::

# Understanding Umbraco the BootManager

After IIS started the W3 process, Umbraco will begin launching.  There is a bootstrapper for the `Umbraco Application` which initializes all objects.

The responsible objects for the startup are the [CoreBootManager](https://our.umbraco.com/apidocs/v7/csharp/api/Umbraco.Core.CoreBootManager.html) and [WebBootManager](https://our.umbraco.com/apidocs/v7/csharp/api/Umbraco.Web.WebBootManager.html) where the latter includes the Web portion of the application.

The boot managers initialize the [UmbracoApplication](https://our.umbraco.com/apidocs/v7/csharp/api/Umbraco.Web.UmbracoApplication.html) (the global.asax) object.  After it has initialized the UmbracoApplication, it will initialize the ApplicationContext.

The bootmanager will initialize the ApplicationContext with: the database context, services context, profiling and logger. It will also register the Application Startup handlers which will execute later using the [ApplicationEventsResolver](https://our.umbraco.com/apidocs/v7/csharp/api/Umbraco.Core.ObjectResolution.ApplicationEventsResolver.html).

For those wondering: **Examine doesn't do anything on startup - 'if the indexes are built'**

### IBootManager (EXPERT)

In some cases you may be using a custom `IBootManager` which has the following methods: `Initialize`, `Startup`, `Complete`. This sequence of events and the logic that should be performed in these methods is exactly the same as the methods mentioned above in this order:
* `Initialize` --> `ApplicationInitialized`
* `Startup` --> `ApplicationStarting`
* `Complete` --> `ApplicationStarted`

## Related Links
* [Troubleshooting Slow Startup](Troubleshooting-Slow-Startup.md)
* [Adding startup logic and plugin on c# events](Application-Startup.md) (EXPERT)
* [Overriding UmbracoApplication](Extending-UmbracoApplication.md) (EXPERT)
