---
description: >-
  Depending on which version of Umbraco CMS you are using, we distinguish
  between two package types, NuGet packages, and Package ZIP files.
---

# Package Types

There are two main ways to install packages in Umbraco CMS: Package zip files and NuGet packages.

{% hint style="warning" %}
The zip file package type is only available for Umbraco version 8 and earlier versions. Learn more about it in the [Package zip Files](types-of-packages.md#package-zip-files) section of this article.
{% endhint %}

When developing a package you need to consider the Umbraco CMS version you're using before determining the package type.

* Modern Umbraco (Umbraco 10+): Providing a NuGet package is the only option.
* Legacy Umbraco (Umbraco 8 and earlier versions): It is recommended to provide both a zip file and a NuGet package.

## NuGet Packages

A NuGet package is a standard way of delivering compiled code and configuration to a .NET project. NuGet packages contain dll-files and other files required for the solution. For more information on NuGet packages see Microsoft's [An introduction to NuGet](https://docs.microsoft.com/en-us/nuget/what-is-nuget) documentation.

NuGet packages can be installed using via the command line or through Visual Studio using either the [Package Manager Console](https://docs.microsoft.com/en-us/nuget/consume-packages/install-use-packages-powershell) or the [NuGet Package Manager](https://learn.microsoft.com/en-us/nuget/consume-packages/install-use-packages-visual-studio).

See below, for an example of installing a package using the Package Manager Console in Visual Studio:

```powershell
PM> Install-Package MyPackage -version 1.2
```

NuGet packages can include any solution files and can be configured to run PowerShell scripts after installation.

As NuGet packages are installed outside of the Umbraco website they cannot directly manipulate any of the Umbraco settings or content during their installation.

When adding or changing configuration of Umbraco as part of a NuGet package you need to develop code to run as part of a [Migration](../database.md). The Migration will run the first time the Umbraco site starts after the package is installed, applying the correct configuration.

## Package zip files

{% hint style="warning" %}
Zip file packages are only available for Umbraco CMS 8 and earlier versions.

Refer to the [NuGet Packages](types-of-packages.md#nuget-packages) section above, if your website is using Umbraco 10 or a later version.
{% endhint %}

A package zip file can be installed directly through the Umbraco backoffice.

![Zip packages can be installed via the Umbraco backoffice package section](images/backoffice-package-section.png)

Packages zip files can contain:

* Content
* Solutions files (`dll`s, `App_Plugins` files, etc)
* Document Types
* Templates
* Stylesheets
* Macros
* Languages
* Dictionary Items
* Data Types
* Media

{% hint style="info" %}
If you want to include media in your package, you must select it in both the "Media" section and the "Package Files" section under "Path to file"
{% endhint %}
