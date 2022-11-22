---
versionFrom: 9.0.0
---

# Media Picker

`Alias: Umbraco.MediaPicker3`

`Returns: IEnumerable<MediaWithCrops>` or `MediaWithCrops`

This property editors returns a single `MediaWithCrops` item if the "Pick multiple items" data type setting is disabled or a collection if it is enabled.

## Data Type Definition Example

![Media Picker Data Type Definition](images/MediaPicker-DataType-v10.png)

### Accepted types

Use setting to limit the picker to only select Media Items of these types.

### Pick multiple items

Use this setting to enable the property to contain multiple items. When this is enabled the property editor returns an `IEnumerable<MediaWithCrops>`.

You can still set the maximum amount to 1. Do so when you want to retrieve a collection but only allow the Content Editors to select one Media Item.

### Amount

Use this setting to enforce a minimum and/or maximum amount of selected Media Items. It is not possible to set a maximum when the "Pick multiple items" feature is disabled.

### Start node

This setting is used to limit the Media Picker to certain parts of the Media Tree.

### Ignorer user start nodes

Use this setting to overrule user permissions, to enable any user of this property to pick any Media Item of the chosen Start node.

When this setting is enabled, a user who doesn't normally have access to the media selected as "Start Node" (/Design in this case), can access the media when using this particular Media Picker. If no Start node has been defined for this property any content can be viewed and selected of this property.

### Enable Focal Point

Enable the focal point setter, do only enable this if the focal point is used or if you have Image crops defined.

### Image Crops

Define local image crops. Local image crop data is stored on the document in this property. This means it can differentiate between documents.

This is different from Global crops as they are defined on the Media Item, making the crops shared between all usage of that Media Item.

Global crops are configured on the Image Cropper property of the Image Media Type

[Read about the Image Cropper here](../Image-Cropper/index.md)

## Content Example

![Media Picker Content](images/Media-Picker3-Content.jpg)

## MVC View Example

### Multiple enabled without Modelsbuilder

```csharp
@using Umbraco.Cms.Core.Models
@{
    var typedMultiMediaPicker = Model.Value<IEnumerable<MediaWithCrops>>("medias");
    foreach (var entry in typedMultiMediaPicker)
    {
        <img src="@entry.MediaUrl()" style="width:200px" />
    }
}
```

### Multiple enabled with Modelsbuilder

```csharp
@{
    var typedMultiMediaPicker = Model.Medias;
    foreach (var entry in typedMultiMediaPicker)
    {
        <img src="@entry.MediaUrl()" style="width:200px" />
    }
}
```

### Multiple disabled without Modelsbuilder

```csharp
@using Umbraco.Cms.Core.Models
@{
    var typedMediaPickerSingle = Model.Value<MediaWithCrops>("media");
    if (typedMediaPickerSingle != null)
    {
        <img src="@typedMediaPickerSingle.MediaUrl()" style="width:200px" alt="@typedMediaPickerSingle.Value("alt")" />
    }
}
```

### Multiple disabled with Modelsbuilder

```csharp
@using Umbraco.Cms.Core.Models
@{
    var typedMediaPickerSingle = Model.Media;
    if (typedMediaPickerSingle is MediaWithCrops mediaEntry)
    {
        <img src="@mediaEntry.MediaUrl()" style="width:200px"/>
    }
}
```

## Using local crops

Local image crops are stored and retrieved differently than global crops. Below are two examples of how to retrieve local crops.

### Using GetLocalCropUrl

```csharp
@{
    foreach (var entry in Model.Medias)
    {
        <img src="@entry.GetLocalCropUrl("cropAlias")"/>
    }
}
```

### Using UrlHelper

```csharp
@{
    foreach (var entry in Model.Medias)
    {
        <img src="@Url.GetCropUrl(entry.LocalCrops, "cropAlias")"/>
    }
}
```

## Use global crops

Global image croppings are crops stored on the Media Item, by the Property Editor `Image Cropper`, making it shared between all usages of the media Item.

The global crops are configured on the DataType of the `umbracoFile` property on the Media Type `Image`

[Read about the Image Cropper here](../Image-Cropper/index.md)

### Using GetCropUrl

```csharp
@{
    foreach (var entry in Model.Medias)
    {
        <img src="@entry.GetCropUrl("cropAlias")" />
    }
}
```

### Using UrlHelper

```csharp
@{
    foreach (var entry in Model.Images)
    {
        <img src="@Url.GetCropUrl(entry, "cropAlias")" />
    }
}

```

### Add values programmatically

See the example below to see how a value can be added or changed programmatically. To update a value of a property editor you need the [Content Service](../../../../../Reference/Management/Services/ContentService/index.md).

The following sample will update a single image in a Media Picker.

```csharp
@using Umbraco.Cms.Core;
@using Umbraco.Cms.Core.Services;
@inject IContentService Services;
@{
    // Get access to ContentService
    var contentService = Services;

    // Create a variable for the GUID of the page you want to update
    var guid = Guid.Parse("32e60db4-1283-4caa-9645-f2153f9888ef");

    // Get the page using the GUID you've defined
    var content = contentService.GetById(guid); // ID of your page

    // Get the media you want to assign to the media picker 
    var media = Umbraco.Media("bca8d5fa-de0a-4f2b-9520-02118d8329a8");

    // Create an Udi of the media
    var udi = Udi.Create(Constants.UdiEntityType.Media, media.Key);

    // Set the value of the property with alias 'featuredBanner'. 
    content.SetValue("featuredBanner", udi.ToString());

    // Save the change
    contentService.Save(content);
}
```

Although the use of a GUID is preferable, you can also use the numeric ID to get the page:

```csharp
@{
    // Get the page using it's id
    var content = contentService.GetById(1234); 
}
```

If Modelsbuilder is enabled you can get the alias of the desired property without using a magic string:

```csharp
@using Umbraco.Cms.Core.PublishedCache;
@inject IPublishedSnapshotAccessor _publishedSnapshotAccessor;
@{
    // Set the value of the property with alias 'featuredBanner'
    content.SetValue(Home.GetModelPropertyType(_publishedSnapshotAccessor, x => x.FeaturedBanner).Alias, udi.ToString());
}
```
