---
versionFrom: 7.0.0
---

# (Obsolete) Dropdown list multiple, publish keys

`Returns: Comma Separated String`

Displays a list of preset values as a list where multiple values can be selected. The value saved is a comma separated string of the prevalue ids.

## Data Type Definition Example

![Dropdown list multiple Data Type Definition](../images/wip.png)

## Content Example

![Dropdown list multiple Content](../images/wip.png)

## MVC View Example

### Typed

```csharp
@{
    if (Model.Content.HasValue("superHeros"))
    {
        // this conditional is required because of a bug where a single item is returned as value instead of id
        var superHeros = Model.Content.GetPropertyValue<string>("superHeros").Split(",");
        <ul>
            @if (superHeros.Count() == 1)
            {
                <li>@Model.Content.GetPropertyValue("superHeros")</li>
            }
            else
            {
                foreach (var item in superHeros.Select(int.Parse))
                {
                    <li>@Umbraco.GetPreValueAsString(item)</li>
                }
            }
        </ul>
    }
}
```

### Dynamic

:::warning
See [Common pitfalls](https://our.umbraco.com/documentation/reference/Common-Pitfalls/#dynamics) for more information about why the dynamic approach is obsolete.
:::

```csharp
@{
    if (CurrentPage.HasValue("superHeros"))
    {
        // this conditional is required because of a bug where a single item is returned as value instead of id
        var superHeros = ((string)CurrentPage.superHeros).Split(',');
        <ul>
            @if (superHeros.Count() == 1)
            {
                <li>@CurrentPage.superHeros</li>
            }
            else
            {
                foreach (var item in superHeros.Select(int.Parse))
                {
                    <li>@Umbraco.GetPreValueAsString(item)</li>
                }
            }
        </ul>
    }
}
```
