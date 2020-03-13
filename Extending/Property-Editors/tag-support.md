---
versionFrom: 7.0.0
needsV8Update: "true"
---

# Tag support for property editors

## What is tag support?

This document will explain how to add tagging support to property editors and what tagging support means.

When a property editor with tag support enabled is published (or saved in the case of media / members) its tags will be saved into the tag table in the database and associated with the corresponding document property. This then allows you to query or render content based on tags on the front-end.

*NOTE: The documentation for querying tag data is coming soon....*

## Enabling tag support

This will explain both ways to enable tag support for property editors: both in C# or by the manifest.

### CSharp

Like creating a normal C# property editor you start by declaring a class with the PropertyEditor attribute:

```csharp
[PropertyEditor("tags", "Tags", "tags")]
public class TagsPropertyEditor : PropertyEditor
{
}
```

To add tag support, we can add the SupportsTags attribute:

```csharp
[SupportsTags]
[PropertyEditor(Constants.PropertyEditors.TagsAlias, "Tags", "readonlyvalue")]
public class TagsPropertyEditor : PropertyEditor
{
}
```

There are a few options for the `SupportsTags` attribute but normally the default options will suffice.
The default options are:

* Requires that the property editor value is a comma delimited string value
* Will replace the current property's tags in the tags database table with the ones in the value
* Will add all tags as part of the 'default' tag group in the database table

These options can all be changed on the attribute, you can specify a different delimiter, you can specify whether to replace or append the tags associated with the property and you can specify a different tag group.

There is one last option that can be set which is the `TagValueType` enum, the values can be:

* `FromDelimitedValue` - this is the default
* `CustomTagList` - if this is used then it is expected that the property editor's value (returned from the method `ConvertEditorToDb`) is an `IEnumerable<string>`
    * This setting might be handy if you need to dynamically (in C#) generate the tags list
