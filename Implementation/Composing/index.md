---
keywords: composing composers components runtime boot booting v8 version8 events registering
versionFrom: 8.0.0
---

# Composing
Customising the behaviour of an Umbraco Application at 'start up'. eg adding, removing or replacing the core functionality of Umbraco or registering custom code to subscribe to events. 

## Overview
An Umbraco application is a `Composition` made of many different 'collections' of specific functionality/implementation logic/components (eg. UrlProviders, ContentFinders - see below for a full list). These collections are populated when the Umbraco Application starts up. 

'Composing' is the term used to describe the process of curating which pieces of functionality should be included in a particular collection. The code that implements these choices at start up is called a `Composer`.

A `Component`, is a generic wrapper for writing custom code during composition, it has two methods: `Initialize()` and `Terminate()` and these are executed when the Umbraco Application starts up, and when it shuts down, respectively. Typically a `Component` may be used to wire up custom code to handle a particular event in Umbraco. (see content saving example below). 

How are the collections populated? - Either by scanning the codebase for c# classes that inherit from a particular base class or implement a particular interface (typed scanned) or by being explictly registered via a `Composer`.

Umbraco ships with a set of `ICoreComposer`'s  that pull together the default set of components and collections that deliver the core 'out of the box' Umbraco behaviour. These default collections and components can be removed, reordered, replaced, etc by implementing `IUserComposer`'s and `IComponent`s to customise and extend Umbraco's behaviour. 

### Example - Explicity Registering a new custom OEmbedProvider
This example shows a custom 'Spotify' OEmbed Provider which will allow Spotify URLs to be used via the 'embed' button in the Grid and Rich Text Editors. As the collection for OEmbedProviders is not 'typed scanned', we need to explicitly register the provider in the collection of OEmbedProviders. We create a c# class which implements `IUserComposer` and append our new Spotify OEmbedProvider to the OEmbedProviders() collection:

```csharp
using System.Collections.Generic;
using Umbraco.Web.Media.EmbedProviders;

namespace My.Website
{
    public class Spotify : EmbedProviderBase
    {
        public override string ApiEndpoint => "https://embed.spotify.com/oembed/";

        // Playlist
        // https://open.spotify.com/user/spotify/playlist/37i9dQZF1E4sNI4jZloSZr?si=cueBooBfTnqCGriSa4N_Kg
        // spotify:user:spotify:playlist:37i9dQZF1E4sNI4jZloSZr

        // Artist
        // https://open.spotify.com/artist/0iirUbtgwt9jEkc2Grin8C?si=TLeUR2cHR-KPRJJhW6YiVg
        // spotify:artist:0iirUbtgwt9jEkc2Grin8C

        // Album
        // https://open.spotify.com/album/0lvtdqkqIln6uDBBUT7DHL?si=XTVJIEmnS_OVv9l6ktPFiw
        // spotify:album:0lvtdqkqIln6uDBBUT7DHL

        // Track
        // https://open.spotify.com/track/7aCk4XfXIEJM2MecU6Gmf2?si=vESDzI0xTNeA9FQ_dvf1eQ
        // spotify:track:7aCk4XfXIEJM2MecU6Gmf2

        public override string[] UrlSchemeRegex => new string[]
        {
            @".*.spotify.com/.*",
            @"spotify:.*"
        };

        public override Dictionary<string, string> RequestParams => new Dictionary<string, string>();

        public override string GetMarkup(string url, int maxWidth = 0, int maxHeight = 0)
        {
            var requestUrl = base.GetEmbedProviderUrl(url, maxWidth, maxHeight);
            var oembed = base.GetJsonResponse<OEmbedResponse>(requestUrl);

            return oembed.GetHtml();
        }
    }
}
```

```csharp
using Umbraco.Core.Composing;
using Umbraco.Web;

namespace My.Website
{
    public class CustomOEmbedComposer : IUserComposer
    {
        public void Compose(Composition composition)
        {
            // Change the OEmbedProviders collection
            // by adding our new EmbedProvider for Spotify
            composition.OEmbedProviders().Append<Spotify>();
        }
    }
}
```
See a list of collections below to determine which are 'type scanned' and which require explicit registration.

### Example - Creating a Component to listen for ContentService.Saving events

This example shows how to create a component to listen and respond to  `ContentService.Saving` events, (perhaps to check for explicit for words, or some custom business logic that needs to run before the content item is saved in Umbraco).

We create a new c# class which implements `IUserComposer` and use it to add our new `IComponent`  to the collection of Components - when Umbraco starts up the `Initialize()` method of the component will be called and the ContentService Saving event will be subscribed to.

```csharp
using System.Linq;
using Umbraco.Core;
using Umbraco.Core.Composing;
using Umbraco.Core.Events;
using Umbraco.Core.Services;
using Umbraco.Core.Services.Implement;

namespace My.Website
{
    [RuntimeLevel(MinLevel = RuntimeLevel.Run)]
    public class SubscribeToContentServiceSavingComposer : IUserComposer
    {
        public void Compose(Composition composition)
        {
            // Append our component to the collection of Components
            // It will be the last one to be run
            composition.Components().Append<SubscribeToContentServiceSavingComponent>();
        }
    }

    public class SubscribeToContentServiceSavingComponent : IComponent
    {
        // initialize: runs once when Umbraco starts
        public void Initialize()
        {
            ContentService.Saving += ContentService_Saving;
        }

        // terminate: runs once when Umbraco stops
        public void Terminate()
        {
        }

        private void ContentService_Saving(IContentService sender, ContentSavingEventArgs e)
        {
            foreach (var content in e.SavedEntities
                //Check if the content item type has a specific alias
                .Where(c => c.ContentType.Alias.InvariantEquals("MyContentType")))
            {
                //Do something if the content is using the MyContentType doctype
            }
        }
    }
}
```
## Types of Composers
Composers are responsible for drawing together the different components an Umbraco application is composed of. They implement the following Composer interfaces which group together the composers and control the order in which they are run:<br/>
`IRuntimeComposer`<br/>
There should only ever been one IRuntimeComposer in an application and it belongs to Umbraco, this Composer runs first before all other composers, do not mess with the IRuntimeComposer.

`ICoreComposer`<br/>
After the IRuntimeComposer has 'composed' - all ICoreComposer instances also 'compose'. They compose all the default elements that Umbraco needs to run.

`IUserComposer`<br/>
Finally, all IUserComposer instances 'compose'. These types of composers are for developers/implementors to use to customise and extend Umbraco or use in package development.

:::warning
Ordering of composers is important, the last one added can override a previously added composer! Make sure, when overriding, that your composer that is doing the overriding, is 'composing', after the composer has 'composed' the element you wish to override!
:::

### ComponentComposer&lt;T&gt;
Is an implementation of IUserComposer, that provides a quicker way to add a custom Component to the Component's collection. Creating a c# class that inhertits from ComponentComposer&lt;YourComponentType&gt; will automatically add YourComponentType to the collection of Components. In the example above, the SubscribeToContentServiceSavingComposer for the SubscribeToContentServiceSavingComponent could have been written more conveniently as:

```csharp
        [RuntimeLevel(MinLevel = RuntimeLevel.Run)]
        public class SubscribeToContentServiceSavingComposer : ComponentComposer<SubscribeToContentServiceSavingComponent>
        {       
        }
```
## Collections
>"Collections of elements", for example the ContentFinders collection. - Collections are another concept that Umbraco uses to make things simpler, on top of DI. A collection builder builds a collection, allowing users to add and remove types before anything is actually registered into DI.

Below is a list of collections with their corresponding 'collection type' and how items for this collection 'out of the box' are registered.


| Collection                | Type      | Registration                                                      |
| ---                       | ---       | ---                                                               |
| CacheRefreshers           | Lazy      | Type scanned for `ICacheRefresher`                                |
| Components                | Ordered   | Explicit Registration                                             |
| ContentApps               | Ordered   | Package.manifest & Explicit Registration                          |
| ContentFinders            | Ordered   | Explicit Registration                                             |
| Dashboards                | Weighted  | Package.manifest & Type scanned for `IDashboard`                  |
| DataEditors               | Lazy      | Type scanned for `IDataEditor`                                    |
| FilteredControllerFactory | Ordered   | Explicit Registration                                             |
| HealthChecks              | Lazy      | Type scanned for `HealthCheck`                                    |
| OEmbedProviders           | Ordered   | Explicit Registration                                             |
| PackageActions            | Lazy      | Type scanned for `IPackageAction`                                 |
| PropertyValueConvertors   | Ordered   | Type scanned for `IPropertyValueConverter`                        |
| SearchableTrees           | Lazy      | Type scanned for `ISearchableTree`                                |
| Sections                  | Ordered   | Package.manifest & Explicit Registration                          |
| TourFilters               | Base      | Empty collection                                                  |
| Trees                     | Base      | Type scanned. Must inherit `TreeControllerBase` & use `[Tree]`    |
| UrlProviders              | Ordered   | Explicit Registration                                             |
| UrlSegmentProviders       | Ordered   | Explicit Registration                                             |
| Validators                | Lazy      | Explicit Registration                                             |


### Types of Collections
Ordered<br/>
`OrderedCollectionBuilderBase` - The base class for collection builders that order their items explicitly.

Weighted<br/>
`WeightedCollectionBuilder` - The base class for collection builders that order their items by the `[Weight]` attribute.

Lazy<br/>
`LazyCollectionBuilderBase` - The base class for collection builders that resolve the types at the last moment, only when the collection is required.


### Example - Modifying Collections

This example shows how to control which Healthchecks are available to run in the Umbraco backoffice. Create a c# class which implements IUserComposer, the Compose method gives access to the HealthChecks collection of the Umbraco Composition - first we clear all HealthChecks from the collection, then add back in the ones we want to keep:

```csharp
using Umbraco.Core;
using Umbraco.Core.Composing;
using Umbraco.Web;
using Umbraco.Web.HealthCheck.Checks.Permissions;
using Umbraco.Web.HealthCheck.Checks.Security;

namespace My.Website
{
    [RuntimeLevel(MinLevel = RuntimeLevel.Run)]
    public class MyComposer : IUserComposer
    {
        public void Compose(Composition composition)
        {
            //Remove all HealthChecks
            composition.HealthChecks().Clear();

            //Explictly add back the ones we want to use
            composition.HealthChecks().Add<FolderAndFilePermissionsCheck>();
            composition.HealthChecks().Add<ExcessiveHeadersCheck>();

        }
    }
}
```


## Attributes
Umbraco has some useful C# attributes to decorate your composer classes or Types used in collections, to give you further control on how and when your Composers will 'compose'.

### `[ComposeBefore]` and `[ComposeAfter]`
A finer-grain mechanism can then be used to refine the order of composition. Each composer can specify that it should compose before or after another composer, using the ComposeBefore and ComposeAfter attributes. For instance:

```csharp
[ComposeBefore(typeof(ThatOtherComposer))]
public class ThisComposer : IUserComposer
{
    public void Compose(Composition composition)
    {
    }
}
```

ThisComposer will 'compose' before ThatOtherComposer.

:::warning
If you create a circular dependancy then Umbraco will fail to boot and will report the conflicting/circular dependancy.
:::

### [Weight]
This attribute is used only for `WeightedCollectionBuilders` (see list above) - and specifies an integer ordinal value for each item to be added to the weighted collection which controls their sort order - (note the weighting attribute is not applied to the Composers).

```csharp
using System;
using Umbraco.Core;
using Umbraco.Core.Composing;
using Umbraco.Core.Dashboards;

namespace Umbraco.Web.Dashboards
{
    [Weight(10)]
    public class FormsDashboard : IDashboard
    {
        public string Alias => "formsInstall";
        public string[] Sections => new [] { Constants.Applications.Forms };
        public string View => "views/dashboard/forms/formsdashboardintro.html";
        public IAccessRule[] AccessRules => Array.Empty<IAccessRule>();
    }
}
```

### [HideFromTypeFinder]
This is used to hide a type from being auto scanned/added to a collection as in some cases certain items/types may need to be added to a collection manually. For example, a Search package may make it optional whether to replace the 'backoffice search' with an ISearchableTree implementation, type scanning would make this change automatically at start up if the custom implementation was detected via type scanning - this attribute could hide the class from the scanner.

### [DisableComposer] & [Disable]
These attributes allows you to disable a particular implementation of a composer or class - Let's say Umbraco ships with two different ways of doing "something" (for instance, two front-end caches). Each way has its own composer, which registers all the relevant elements. Of course, if both composers are detected, there will be some sort of collision. Ideally, we want to disable one of them. That can be achieved with the Disable attribute:

```csharp
[Disable]
public void Way2Composer : IComposer
{ ... }
```

When used without arguments, these attributes apply to the composer they are marking. But, and this is where it becomes interesting, they can be used with an argument to act on another component. Therefore, should a user want to replace our "something" with hers, she would write the following code:

```csharp
[Disable(typeof(Way1Composer))]
public void MyComposer : IComposer
{
    public void Compose(Composition composition)
    { ... }
}
```

But maybe she just wants to swap our two "something" implementations? In this case, assembly-level attributes can be used:
```csharp
[assembly:DisableComposer(typeof(Way1Composer))]
[assembly:EnableComposer(typeof(Way2Composer))]
```

:::tip
Note that Umbraco also has a `[Enable]` & `[EnableComposer]` attributes but all composers are enabled by default.
:::

### [RuntimeLevel]
The most common usecase for this is to set this attribute on your own composers and to set the minimum level to Run. Which will mean this composer will not be invoked until Umbraco is fully booted and is running. So if an upgrade or Umbraco is still booting your own custom composer code won't run until everything is all setup and good.

```csharp
using Umbraco.Core;
using Umbraco.Core.Composing;

namespace My.Website
{
    [RuntimeLevel(MinLevel = RuntimeLevel.Run)]
    public class MyComposer : IUserComposer
    {
        public void Compose(Composition composition)
        {
        }
    }
}
```

:::tip
If you wish to see the order of components when Umbraco boots, then you can see this information in the logs of Umbraco
:::

## Runtime Levels

The `Umbraco.Core.RuntimeLevel` enum contains the following values:<br/>
`BootFailed`<br/>
The runtime has failed to boot and cannot run.

`Unknown`<br/>
The level is unknown.

`Boot`<br/>
The runtime is booting.

`Install`<br/>
The runtime has detected that Umbraco is not installed at all, ie there is no database, and is currently installing Umbraco.

`Upgrade`<br/>
The runtime has detected an Umbraco install which needed to be upgraded, and is currently upgrading Umbraco.

`Run`<br/>
The runtime has detected an up-to-date Umbraco install and is running.


