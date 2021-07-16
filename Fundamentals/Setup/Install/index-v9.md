---
versionFrom: 9.0.0
verified-against: alpha-4
state: partial
updated-links: true
meta.Title: "Installing Umbraco"
meta.Description: "Instructions on installing Umbraco on various platforms using various tools."
---

# Installation

The easiest way to get the latest version of Umbraco up and running is with VS Code.

1. **Download and install [Visual Studio Code](https://code.visualstudio.com/)**
1. **Download and install [IIS Express](https://www.microsoft.com/en-us/download/details.aspx?id=48264) (Optional as IIS Express for VSCode will install it)**
1. **Download and unzip [Umbraco](https://our.umbraco.com/download)**
1. **Install the [IIS Express Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=warren-buckley.iis-express)**
1. **Open the unzipped root folder in VS Code**
1. **Run the website with the IIS Express Extension (CTRL+F5)**
1. **Follow instructions on installer**

If you have never used VS Code before, you can check out a more detailed guide below that shows these steps more in depth to run a local instance of Umbraco.
Below you'll find some in-depth tutorials on the different ways to install Umbraco.

## [VS Code installation](install-umbraco-with-vs-code-v9.md)

Visual Studio Code is an editor with an embedded webserver (through the IIS Express extension). A fast way to get you up and running with Umbraco.

### [Run Umbraco on IIS](iis-v9.md)

Learn how to run an already installed local installation of Umbraco 9.

## [.NET CLI installation](install-umbraco-with-templates.md)

.NET CLI, included with the .NET SDK, can be used to install or uninstall .NET templates from NuGet using the `dotnet new` command on any OS. The underlying Template Engine enables the creation of custom templates which make new project bootstrapping much faster. With a few steps you can have an Umbraco project running without the need for a code editor.

## [Visual Studio installation](visual-studio.md)

Visual Studio is used to write native code and managed code supported by .NET and many others.
Its built-in tools provide the ability to develop and execute applications for any platform. Developers will be able to install Umbraco without ever having to leave Visual Studio.

## [Install Umbraco unattended](Unattended-Install-v9.md)

Use the Unattended installs when spinning up Umbraco instances on something like Azure Web Apps to avoid having to run through the installation wizard.
