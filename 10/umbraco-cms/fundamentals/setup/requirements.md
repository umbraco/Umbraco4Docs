# Requirements

## Browsers

The Umbraco UI should work in all modern browsers:

* Chrome (Latest)
* Edge (Chromium)
* Firefox (Latest)
* Safari (Latest)

## Local Development

{% tabs %}
{% tab title="Umbraco 10" %}
* Either OS:
  * Microsoft Windows 7 SP1, 8.1, 10 and 11
  * MacOS High Sierra 10.13
  * Linux (Ubuntu, Alpine, CentOS, Debian, Fedora, openSUSE and other major distributions)
* One of the following .NET Tools or Editors:
  * [Visual Studio Code](https://code.visualstudio.com/) with the [IISExpress extension](https://marketplace.visualstudio.com/items?itemName=warren-buckley.iis-express)
  * [Microsoft Visual Studio](https://www.visualstudio.com/) 2022
  * [JetBrains Rider](https://www.jetbrains.com/rider) **version 2020.3 and higher**
  * .NET Core CLI
* .NET 6.0.5+
* SQL connection string (SQL Server)
{% endtab %}

{% tab title="Umbraco 9" %}
* Either OS:
  * Microsoft Windows 7 SP1, 8.1, 10 and 11
  * MacOS High Sierra 10.13
  * Linux (Ubuntu, Alpine, CentOS, Debian, Fedora, openSUSE and other major distributions)
* One of the following .NET Tools or Editors:
  * [Visual Studio Code](https://code.visualstudio.com/) with the [IISExpress extension](https://marketplace.visualstudio.com/items?itemName=warren-buckley.iis-express)
  * [Microsoft Visual Studio](https://www.visualstudio.com/) 2019 **version 16.8 and higher**
  * [JetBrains Rider](https://www.jetbrains.com/rider) **version 2020.3 and higher**
  * .NET Core CLI
* .NET 5.0
* SQL connection string (SQL Server)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
When using Visual Studio as your primary Integrated Development Environment (IDE) we recommend [finding and downloading the Software Development Kits (SDKs) for Visual Studio](https://dotnet.microsoft.com/en-us/download/visual-studio-sdks).
{% endhint %}

## Hosting

### Recommendation

For the best experience, we would recommend that you ensure your hosting environment has the following to run Umbraco CMS:

{% tabs %}
{% tab title="Umbraco 10" %}
* Windows Server 2019 and higher
* IIS 10 and higher
* SQL Server 2019 and higher
* .NET 6.0.5+
* Ability to set file permissions to include create/read/write (or better) for the user that "owns" the Application Pool for your site (NETWORK SERVICE, typically)
{% endtab %}

{% tab title="Umbraco 9" %}
* Windows Server 2019 and higher
* IIS 10 and higher
* SQL Server 2019 and higher
* .NET 5.0
* Ability to set file permissions to include create/read/write (or better) for the user that "owns" the Application Pool for your site (NETWORK SERVICE, typically)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You can use Umbraco Cloud to manage the hosting infrastructure. All Umbraco Cloud plans are hosted on Microsoft Azure, which gives your site a proven and solid foundation.
{% endhint %}

### Miminium requirements to run Umbraco

Umbraco can run on an environment with the minimum requirements stated below:

{% tabs %}
{% tab title="Umbraco 10" %}
* Windows Server 2012 R2 and higher
* IIS 8.5 and higher
* SQL Server 2012 and higher
* .NET 6.0.5+
* Ability to set file permissions to include create/read/write (or better) for the user that "owns" the Application Pool for your site (NETWORK SERVICE, typically)
{% endtab %}

{% tab title="Umbraco 9" %}
* Windows Server 2012 R2 and higher
* IIS 8.5 and higher
* SQL Server 2012 and higher
* .NET 5.0
* Ability to set file permissions to include create/read/write (or better) for the user that "owns" the Application Pool for your site (NETWORK SERVICE, typically)
{% endtab %}
{% endtabs %}

_For more information, see the_ [_Host and deploy ASP.NET Core applications_](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/?view=aspnetcore-6.0) _article in the Microsoft documentation._

## Database Account Roles

The database account used in the connection string will need permissions to read and write from tables as well as create schema during installs and upgrades:

* The `db_owner` role has full permissions on the database.
* To use an account with more restricted permissions, the `db_datareader` and `db_datawriter` roles will be needed for normal use to read from and write to the database. The `db_ddladmin` role, which can modify the database schema, is required for installs and upgrades of the CMS and/or any packages that create database tables.

For more information on the Database-level roles, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/database-level-roles?view=sql-server-ver16#fixed-database-roles).
