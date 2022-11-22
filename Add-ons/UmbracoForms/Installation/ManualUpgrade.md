---
versionFrom: 9.0.0
versionTo: 10.0.0
---

# Manual upgrade of Umbraco Forms

This article shows how to manually upgrade Umbraco Forms to run the latest version.

## Get the latest version of Umbraco Forms

To get the latest version of Umbraco Forms, you can upgrade using:

- [NuGet](#nuget)
- [Visual Studio](#visual-studio)

### NuGet

- NuGet installs the latest version of the package when you use the `dotnet add package Umbraco.Forms` command unless you specify a package version:

  `dotnet add package Umbraco.Forms --version <VERSION>`

- After you have added a package reference to your project by executing the `dotnet add package Umbraco.Forms` command in the directory that contains your project file, run `dotnet restore` to install the package.

### Visual Studio

- Go to `Tools` -> `NuGet Package Manager` -> `Manage NuGet Packages for Solution...` in Visual Studio, to upgrade your Forms:
- Select **Umbraco.Forms**.
- Select the latest version from the **Version** drop-down and click **Install**.

  ![NuGet Package Manager](images/Manage_packages_v10.png)

- When the command completes, open the **<project-name>.csproj** file to make sure the package reference is updated:

  ```xml
  <ItemGroup>
    <PackageReference Include="Umbraco.Forms" Version="10.x.x" />
  </ItemGroup>
  ```
