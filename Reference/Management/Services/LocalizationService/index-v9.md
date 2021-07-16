---
versionFrom: 9.0.0
verified-against: beta-3
state: complete
updated-links: true
---

# LocalizationService

The LocalizationService acts as a "gateway" to Umbraco data for operations which are related to Dictionary items and Languages.

[Browse the API documentation for ILocalizationService](https://apidocs.umbraco.com/v9/csharp/api/Umbraco.Cms.Core.Services.ILocalizationService.html).

 * **Namespace:** `Umbraco.Cms.Core.Services`
 * **Assembly:** `Umbraco.Core.dll`

All samples in this document will require references to the following dll:

* Umbraco.Core.dll

All samples in this document will require the following using statements:

```csharp
using Umbraco.Cms.Core.Services;
```

For Razor views:
```csharp
@using Umbraco.Cms.Core.Services
```

## Getting the service

### Dependency Injection

If you wish to use the localization service in a class, you need to specify the `ILocalizationService` interface in your constructor:

```c#
public class MyClass
{
    private ILocalizationService _localizationService;
    
    public MyClass(ILocalizationService localizationService)
    {
        _localizationService = localizationService;
    }
}
```

In Razor views, you can access the localization service through the `@inject` directive:

```csharp
@inject ILocalizationService LocalizationService
```

## Samples

* [**Retrieving languages**](Retrieving-languages-v9.md)<br />See examples on how to retrieve languages via the localization service - either individually or as a collection.