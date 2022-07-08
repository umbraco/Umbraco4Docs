---
versionFrom: 9.0.0
versionTo: 10.0.0
meta.Title: "Backoffice Search"
meta.Description: "A guide to customization of Backoffice Search"
---

# Backoffice Search

The search facility of the Umbraco Backoffice allows the searching 'across sections' of different types of Umbraco entities, eg Content, Media, Members. However 'by default' only a small subset of standard fields are searched:

| Node Type    | Propagated Fields      |
| ------------ | ---------------------- |
| All Nodes    | Id, _NodeId_ and _Key_ |
| Media Nodes  | UmbracoFileFieldName   |
| Member Nodes | email, loginName       |

An Umbraco implementation might have additional custom properties that it would be useful to include in a Backoffice Search. For example: an 'Organisation Name' property on a Member Type, or a 'Product Code' field for a 'Product' content item.

## Adding custom properties to backoffice search

To add custom properties, it is required to register a custom implementation of `IUmbracoTreeSearcherFields`. We recommend to override the existing `UmbracoTreeSearcherFields`.

Your custom implementation needs to be registered in the container. E.g. in the `Startup.ConfigureServices` method or in a composer, as an alternative.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddUmbraco(_env, _config)
    ...

    services.AddUnique<IUmbracoTreeSearcherFields, CustomUmbracoTreeSearcherFields>();
}
```

or

```csharp
using Umbraco.Cms.Core.Composing;
using Umbraco.Cms.Core.DependencyInjection;
using Umbraco.Cms.Infrastructure.Examine;
using Umbraco.Extensions;

namespace Umbraco.Docs.Samples.Web.BackofficeSearch
{
    public class BackofficeSearchComposer : IComposer
    {
        public void Compose(IUmbracoBuilder builder)
        {
            builder.Services.AddUnique<IUmbracoTreeSearcherFields, CustomUmbracoTreeSearcherFields>();
        }
    }
}
```

```csharp
using System.Collections.Generic;
using Umbraco.Cms.Core.Services;
using Umbraco.Cms.Infrastructure.Examine;
using Umbraco.Cms.Infrastructure.Search;

//https://our.umbraco.com/documentation/Extending/Backoffice-Search/

namespace Umbraco.Docs.Samples.Web.BackofficeSearch
{
    public class CustomUmbracoTreeSearcherFields : UmbracoTreeSearcherFields, IUmbracoTreeSearcherFields
    {
        public CustomUmbracoTreeSearcherFields(ILocalizationService localizationService) : base(localizationService)
        {
        }

        //Adding custom field to search in all nodes
        public new IEnumerable<string> GetBackOfficeFields()
        {
            return new List<string>(base.GetBackOfficeFields()) { "parentID" };
        }

        //Adding custom field to search in document types
        public new IEnumerable<string> GetBackOfficeDocumentFields()
        {
            return new List<string>(base.GetBackOfficeDocumentFields()) { "parentID" };
        }

        //Adding custom field to search in media
        public new IEnumerable<string> GetBackOfficeMediaFields()
        {
            return new List<string>(base.GetBackOfficeMediaFields()) { "parentID" };
        }

        //Adding custom field to search in members
        public new IEnumerable<string> GetBackOfficeMembersFields()
        {
            return new List<string>(base.GetBackOfficeMembersFields()) { "parentID" };
        }
    }
}
```

## More advanced extensions

For further extensibility of the Umbraco Backoffice search implementation check [ISearchableTree](../Section-Trees/Searchable-Trees/index.md "https://our.umbraco.com/Documentation/Extending/Section-Trees/Searchable-Trees/index.md")
