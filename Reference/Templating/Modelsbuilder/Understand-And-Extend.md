---
versionFrom: 8.5.0
---

# Understand and Extend

Models are generated as partial classes. In its most basic form, a model for content type `TextPage` ends up in a `TextPage.generated.cs` file and looks like:

```csharp
[PublishedContentModel("textPage")]
public partial class TextPage : PublishedContentModel
{
  public new const string ModelTypeAlias = "textPage";
  public new const PublishedItemType ModelItemType = PublishedItemType.Content;

  public TextPage(IPublishedContent content)
    : base(content)
  { }

  public static PublishedContentType GetModelContentType()
  {
    return PublishedContentType.Get(ModelItemType, ModelTypeAlias);
  }

  public static PublishedPropertyType GetModelPropertyType<TValue>(Expression<Func<Doc1, TValue>> selector)
  {
    return PublishedContentModelUtility.GetModelPropertyType(GetModelContentType(), selector);
  }

  [ImplementPropertyType("header")]
  public string Header { get { return this.GetPropertyValue<string>("header"); } }
}
```

What is important is the `Header` property. The rest is (a) a constructor and (b) some static helpers to get the `PublishedContentType` and the `PublishedPropertyType` objects:

```csharp
var contentType = TextPage.GetModelContentType(); // is a PublishedContentType
var propertyType = TextPage.GetModelPropertyType(x => x.Header); // is a PublishedPropertyType
```

## Composition and Inheritance

Content type *composition* consists in having content types "inherit" properties from other content types. Contrary to C#, where a class can only inherit from one other class, Umbraco content types can be composed of several other content types.

The `TextPage` content type could be composed of the `MetaInfo` content type (and thus inherit properties `Author` and `Keywords`) and of the `PageInfo` content type (and thus inherit properties `Title` and `MainImage`).

Each content type that is involved in a composition is generated both as a class and as an interface, and so the `MetaInfo` content type would be generated as (some code has been removed for simplicity's sake):

```csharp
// The composition interface
public partial interface IMetaInfo : IPublishedContent
{
  public string Author { get; }
  public IEnumerable<string> Keywords { get; }
}

// The composition class
public partial class MetaInfo : PublishedContentModel
{
  // the "static mixin getter" for the property
  public static string GetAuthor(IMetaInfo that)
  {
    return that.GetPropertyValue<string>("author");
  }

  public string Author { get { return MetaInfo.GetAuthor(this); } }
}
```

And the `TextPage` model would be generated as (some code has been removed for simplicity's sake):

```csharp
public partial class TextPage : PublishedContentModel, IMetaInfo
{
  // get the property value from the "static mixin getter"
  public string Author { get { return MetaInfo.GetAuthor(this); } }
}
```

A content type *parent* is a tree-related concept: In the Umbraco backoffice, a content type appears underneath its parent, if any. By convention, a content type is always **composed of its parent** and therefore inherits its properties. However, the parent content type is treated differently, and the child content type *directly inherits* (as in C# inheritance) from the parent class.

Therefore, assuming that the `AboutPage` content type is a direct child of `TextPage`, it would be generated as:

```csharp
// Note: Inherits from TextPage
public partial class AboutPage : TextPage
{
  ...
}
```

## Extending

Because a model is generated as a partial class, it is possible to extend it. That could be by adding a property, by dropping the following code in a `TextPage.cs` file:

```csharp
public partial class TextPage
{
  public string WrappedHeader { get { return "[" + Header + "]"; } }
}
```

In PureLive mode where models are built within the site any `*.cs` file in the `~/App_Data/Models` directory that is *not* a `*.generated.cs` file, is preserved and compiled alongside the models. If models are built outside the site, e.g. in Visual Studio, remember to include the files in the compilation.

If the custom partial class provides a **constructor** that has the same signature as the generated one, it will be detected and no constructor will be generated, as that would be redundant and would not compile.

If the custom partial class **inherits** from a base class, it will be detected and the generated model will *not* inherit from anything, as that would be redundant and would not compile. The base class *must* inherit (directly or indirectly) from `PublishedContentModel` in order for the model to be valid, though.

If the custom partial class **implements** a generated property, it will *not* be detected and will cause a compilation error. Models Builder needs to be explicitly notified about the situation. See [Control Models Generation](Control-Generation.md).

## Best Practices

Extending models should be used to add stateless, local features to models, and *not* to transform *content* models into view models or manage trees of content.

### Example of good practice

A customer has "posts" that has two "release date" properties. One is a true date picker property and is used to specify an actual date and to order the posts. The other is a string that is used to specify dates such as "Summer 2015" or "Q1 2016". Alongside the title of the post, the customer wants to display the text date, if present, else the actual date. If none of those are present, the Umbraco update date should be used. Of course, each view can contain code to deal with the situation, but it is much more efficient to extend the `Post` model:

```csharp
public partial class Post
{
  public string DisplayDate
  {
    get
    {
    if (!string.IsNullOrEmpty(this.TextDate)) return this.TextDate;
    if (this.ActualDate != DateTime.MinValue) return this.ActualDate.ToString();
    return this.UpdateDate;
    }
  }
}
```

And to simplify the view as:

```csharp
<div class="title">
  <div class="title-text">@Model.Title</div>
  <div class="title-date">@Model.DisplayDate</div>
</div>
```

### Example of bad practice

Because, by default, the content object is passed to views, one can be tempted to add view-related properties to the model. Some properties that do *not* belong to a *content* model would be:

* A `HomePage` property that would walk up the tree and return the "home page" content item
* A `Menu` property that would list the content items to display in a top menu
* Etc.

Generally speaking, anything that is tied to the current request, or that depends on more than the modeled content, is a bad idea. There are much cleaner solutions, such as using true *view model* classes that would be populated by a true controller and look like:

```csharp
public class TextPageViewModel
{
  public TextPage Content; // The content model
  public HomePage HomePage; // The home page content model
  public IEnumerable<MenuItem> Menu; // The menu content models
}
```

One can also extend Umbraco's views to provide a special view helper that would give access to important elements of the website, so that views could contain code such as:

```csharp
<a href="@MySite.HomePage.Url">@MySite.HomePage.Title</a>
```

### Example of ugly practice

The scope and life-cycle of a model is *not specified*. In other words, you don't know whether the model exists only for you and for the context of the current request, or if it is cached by Umbraco and shared by all requests.

As a consequence, the following code has a major issue: the `TextPage` model "caches" an instance of the `HomePageDocument` model that will never be updated if the home page is re-published.

```csharp
public partial class TextPage
{
  public TextPage(IPublishedContent content)
    : base(content)
  {
    HomePage = content.AncestorOrSelf(1) as HomePageDocument;
  }

  public HomePageDocument HomePage { get; private set; }
}
```

As a rule of thumb, models should never, *ever* reference and cache other models.