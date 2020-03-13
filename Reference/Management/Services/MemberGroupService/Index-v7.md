---
versionFrom: 6.2.0
---

# MemberGroupService

:::note
Applies to Umbraco 6.2 and 7.1 and newer
:::

The MemberGroupService acts as a "gateway" to Umbraco data for operations which are related to Member groups, which are also known as Member Roles.

[Browse the API documentation for MemberGroupService](https://our.umbraco.com/apidocs/v7/csharp/api/Umbraco.Core.Services.MemberGroupService.html).

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
The MemberGroupService is available through the `ApplicationContext`, but the if you are using a `SurfaceController` or the `UmbracoUserControl` then the MemberGroupService is available through a local `Services` property.

```csharp
Services.MemberGroupService
```

Getting the service through the `ApplicationContext`:

```csharp
ApplicationContext.Current.Services.MemberGroupService
```
