---
versionFrom: 8.0.0
---

# FileService
The FileService acts as a "gateway" to Umbraco data for operations which are related to Scripts, Stylesheets and Templates.

[Browse the API documentation for IFileService](https://our.umbraco.com/apidocs/v8/csharp/api/Umbraco.Core.Services.IFileService.html).

 * **Namespace:** `Umbraco.Core.Services`
 * **Assembly:** `Umbraco.Core.dll`

All samples in this document will require references to the following dll:

* Umbraco.Core.dll

All samples in this document will require the following using statements:

```csharp
using Umbraco.Core;
using Umbraco.Core.Models;
using Umbraco.Core.Services;
```

## Getting the service

### Services property
If you wish to use use the file service in a class that inherits from one of the Umbraco base classes (eg. SurfaceController, UmbracoApiController or UmbracoAuthorizedApiController), you can access the file service through a local Services property:

```csharp
IFileService fileService = Services.FileService;
```

### Dependency Injection
In other cases, you may be able to use Dependency Injection. For instance if you have registered your own class in Umbraco's dependency injection, you can specify the IFileService interface in your constructor:

```csharp
public class MyClass
{

    private IFileService _fileService;
    
    public MyClass(IFileService fileService)
    {
        _fileService = fileService;
    }

}
```

### Static accessor
If neither a Services property or Dependency Injection is available, you can also reference the static Current class directly:

```csharp
IFileService fileService = Umbraco.Core.Composing.Current.Services.FileService;
```
