# Version specific upgrades

*This document covers specific upgrade steps if a version requires them, most versions do not require specific upgrade steps and most of the time you will be able to upgrade directly from your current version to the latest version*

Follow the steps in the [general upgrade guide](general.md), then these additional instructions for the specific versions. (Remember that new config files are not mentioned because they are already covered in the general upgrade guide.)

## Version 7.7.0

Version 7.7.0 introduces User Groups and a better user management and security facilities. This means that anything to do with "User Types" no longer exist including several APIs that work with User Types. If your code or any package's code that you use makes reference to "User Type" APIs, you may need to make changes to your code. In many cases we've created backward compatibility shims for these scenarios and obsoleted APIs that should no longer be used but in some cases this was not possible. 

For a full list of breaking changes see: [the list on the issue tracker](http://issues.umbraco.org/issues/U4?q=Due+in+version%3A+7.7.0+Backwards+compatible%3F%3A+No+) 

Version 7.7.2 no longer ships with the `CookComputing.XmlRpcV2` assembly so if you reference this assembly or have a package that requires this assembly, you may need to copy it back into your website from the backup you've taken before you began the 7.7.2 upgrade.

This version also ships with far less client files (i.e. js, css, images) that were only relevant for very old versions of Umbraco (i.e. < 7.0.0). There might be some packages that were referencing these old client files so if you seen missing image references you may need to contact the vendor of the package in question to update their references.

## Version 7.6.3
In short:

In Umbraco version 7.6.2 we made a mistake in the Property Value Converts (PVCs) which was corrected 2 days later in version 7.6.3. If you were having problems with querying the following datatypes in the frontend, then make sure to upgrade to 7.6.3:

 * Multi Node Tree Picker
 * Related Links
 * Member Picker

Depending on if you tried to fix problem with those datatypes you will might need to fix them again after you upgrade to 7.6.3.

## Property Value Converters?

Umbraco stores data for datatypes in different ways, for a lot of pickers it will store (for example) 1072 or 1083,1283. These numbers refer to the identifier of the item in Umbraco. In the past, when building your templates you would manually have to take that value and find the content item it belongs to and then get the data you wanted from there, for example:

		@{
			IPublishedContent contactPage;
			var contactPageId = Model.Content.GetPropertyValue<int>("contactPagePicker");
			if(contactPageId > 0) {
				 contactPage = Umbraco.TypedContent(contactPageId);
			}
		}

		<p>
		  <a href="@contactPage.Url">@contactPage.Name</a>
		</p>


Wouldn't it be nice if instead of that you could "just" do:

		<p>
			<a href="@Model.Content.ContactPagePicker.Url">@Model.ContactPagePicker.Name</a>
		</p>
		
This is possible since 7.6.0 using Models Builder and through the inclusion of [core property value converters](https://our.umbraco.org/projects/developer-tools/umbraco-core-property-value-converters/), a brilliant package by Jeavon.

In order to not break everybody's sites (the results of queries are different when PVCs are enabled) we disabled these PVCs by default. 

Umbraco 7.6.0 also came with new pickers that store their data as a [UDI (Umbraco Identifier)](https://our.umbraco.org/Documentation/Reference/Querying/Udi). We wanted to make it easy to use these new pickers so by default we wanted PVCs to always be enabled for those pickers.

Unfortunately we noticed that some new pickers also got their PVCs disabled when the configuration setting was set to false (`<EnablePropertyValueConverters>false</EnablePropertyValueConverters>`) - yet the content picker ignored this setting.

In order to make everything consistent, we made sure that the UDI pickers would always use PVCs in 7.6.2... or so we thought! By accident we actually reversed the behavior. So when PVCs were enabled, the property would NOT be converted and when PVCs were disabled, the property would be converted after all. This is the exact opposite behavior of 7.6.2. Oops!
So we have fixed this now in 7.6.3.

This issue only affects:

 * Multi Node Tree Picker
 * Related Links
 * Member Picker

If you have already upgraded to 7.6.2 and fixed some of your queries for those three datatypes then you might have to fix them again in 7.6.3. We promise it's the last time you need to update them! We're sorry for the inconvenience.

## Version 7.6.0

There are a few breaking changes in 7.6.0 be sure to **[read about them here](760-breaking-changes.md)** and [here's the list of these items on the tracker](http://issues.umbraco.org/issues/U4?q=Due+in+version%3A+7.6.0+Backwards+compatible%3F%3A+No+)

The three most important things to note are:

 1. In web.config do NOT change `useLegacyEncoding` to `false` if it is currently set to `true` - changing the password encoding will cause you not being able to log in any more
 2. In umbracoSettings.config leave `EnablePropertyValueConverters` set to `false` - this will help your existing content queries to still work
 3. In tinyMceConfig.config make sure to remove `<plugin loadOnFrontend="true">umbracolink</plugin>` so that the rich text editor works as it should

#### Upgrading via Nuget

This is an important one and there was unfortunately not a perfect solution to this. We have removed the UrlRewriting dependency and no longer ship with it, however if you are using it we didn't want to have Nuget delete all of your rewrites. So the good news is that if you are using it, the Nuget upgrade will not delete your rewrite file and everything should just continue to work (though you should really be using IIS rewrites!). 

However, if you are not using it, **you will get a YSOD after upgrading, here's how to fix it**

Since you aren't using UrlRewriting you will have probably never edited the UrlRewriting file and in which case Nuget will detect that and remove it. However you will need to manually remove these UrlRewriting references from your web.config:

* `<section name="urlrewritingnet" restartOnExternalChanges="true" requirePermission="false" type="UrlRewritingNet.Configuration.UrlRewriteSection, UrlRewritingNet.UrlRewriter" />`
* `<urlrewritingnet configSource="config\UrlRewriting.config" />`
* And the following http modules

	    <system.web>
		<httpModules>
		    <add name="UrlRewriteModule" type="UrlRewritingNet.Web.UrlRewriteModule, UrlRewritingNet.UrlRewriter"/>
		    ...
		</httpModules>
	    <system.web>

	    ...

	    <system.webServer>
	       <modules>
		   <remove name="UrlRewriteModule"/>
		   <add name="UrlRewriteModule" type="UrlRewritingNet.Web.UrlRewriteModule, UrlRewritingNet.UrlRewriter"/>
		    ...
	       </modules>
	    </system.webServer>



#### Forms

Umbraco Forms 6.0.0 has been released to be compatible with Umbraco 7.6, it is a new major version release of Forms primarily due to the strict dependency on 7.6+. If you are using Forms, you will need to update it to version 6.0.0

There is **[important Forms upgrade documentation that you will need to read the here](https://our.umbraco.org/documentation/Add-ons/UmbracoForms/Installation/Version-Specific#version-4-to-version-6)**

#### Courier

Umbraco Courier 3.1.0 has been released to be compatible with Umbraco 7.6. If you are using Courier, you will need to update it to version 3.1.0

## Version 7.4.0
For manual upgrades: 

* Copy the new folder `~/App_Plugins/ModelsBuilder` into the site
* Do not forget to merge `~/Config/trees.config` and `~/Config/Dashboard.config` - they contain new and updated entries that are required to be there
  * If you forget `trees.config` you will either not be able to browse the Developer section or you will be logged out immediately when trying to go to the developer section
* You may experience an error saying `Invalid object name 'umbracoUser'` - this can be fixed by [clearing your cookies on localhost](http://issues.umbraco.org/issue/U4-8031)

## Version 7.3.0
Make sure to manually clear your cookies after updating all the files, otherwise you might an error relating to `Umbraco.Core.Security.UmbracoBackOfficeIdentity.AddUserDataClaims()`. The error looks like: `Value cannot be null. Parameter name: value`.

NuGet will do the following for you but if you're upgrading manually:

* Delete `bin/Microsoft.Web.Helpers.dll`
* Delete `bin/Microsoft.Web.Mvc.FixedDisplayModes.dll`
* Delete `bin/System.Net.Http.dll`
* Delete `bin/System.Net.Http.*.dll` (all dll files starting with `System.Net.Http`) **EXCEPT** for `System.Net.Http.Formatting.dll`
* Delete `bin/umbraco.XmlSerializers.dll`
* In your `web.config` file, add this in the `appSetting` section: `<add key="owin:appStartup" value="UmbracoDefaultOwinStartup" />`

Other considerations:

* WebApi has been updated, normally you don’t have to do anything unless you have custom webapi configuration:
  * See this article if you are using `WebApiConfig.Register`: [http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2](http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2) 
  * You need to update your `web.config` file to have the correct WebApi version references - this should be done by doing a compare/merge of your `~/web.config` file with the `~/web.config` file in the release
* MVC has been updated to MVC5
  * You need to update your `web.config` file to have the correct MVC version references - this should be done by doing a compare/merge of your `~/web.config` file with the `~/web.config` file in the release
  * The upgrader will take care of updating all other web.config’s (in all other folders, for example, the `Views` and `App_Plugins` folders) to have the correct settings
  * For general ASP.Net MVC 5 upgrade details see: [http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2](http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2) 
* It is not required that you merge the changes for the Examine index paths in the ExamineIndex.config file. However, if you do, your indexes will be rebuilt on startup because Examine will detect that they don’t exist at the new location.
* It's highly recommended to clear browser cache - the ClientDependency version is automatically bumped during install which should force browser cache to refresh, however in some edge cases this might not be enough.

## Version 7.2.0
* Copy in the /Views/Partials/Grid (contains Grid rendering views)

## Version 7.1.0
* Remove the /Install folder.

## Version 7.0.1 to 7.0.2

* There was an update to the /umbraco/config/create/ui.xml which needs to be manually updated, the original element had this text:

		<nodeType alias="users">
			<header>User</header>
			<usercontrol>/create/simple.ascx</usercontrol>
			<tasks>
			  <create assembly="umbraco" type="userTasks" />
			  <delete assembly="umbraco" type="userTasks" />
			</tasks>
		</nodeType>

	* The &lt;usercontrol&gt; value has changed to: **/create/user.ascx**, this is a required change otherwise creating a new user will not work.
* There is a breaking change to be aware of, full details can be found [here](https://umbraco.com/blog/heads-up-breaking-change-coming-in-702-and-62/).

## Version 7.0.0 to 7.0.1
* Remove all uGoLive dlls from /bin
 * These are not compatible with V7
* Move appSettings/connectionStrings back to web.config
 * If you are on 7.0.0 you should migrate these settings into the web.config instead of having them in separate files in /config/
 * The keys in config/AppSettings.config need to be moved back to the web.config <appSettings> section and similarly, the config/ConnectionStrings.config holds the Umbraco database connections in v7.0.0 and they should be moved back to the web.config <connectionStrings> section.
 * /config/AppSettings.config and /config/ConnectionString.config can be removed after the contents have been moved back to web.config. (Make backups just in case)
* Delete all files in ~/App_Data/TEMP/Razor/*
 * Related to issues with razor macros

## Version 6 to 7.0.0
Read and follow [the full v7 upgrade guide](upgrading-to-v7.md)

## Version 4.10.x/4.11.x to 6.0.0
* If your site was ever a version between 4.10.0 and 4.11.4 and you have just upgraded to 6.0.0 install the [fixup package](http://our.umbraco.org/projects/developer-tools/path-fixup) and run it after the upgrade process is finished.
* The DocType Mixins package is **NOT** compatible with v6+ and will cause problems in your document types.

## Version 4.10.x to 4.11.x
* If your site was ever a version between 4.10.0 and 4.11.4 install the [fixup package](http://our.umbraco.org/projects/developer-tools/path-fixup) and run it after the upgrade process is finished.

## Version 4.8.0 to 4.10.0
* Delete the bin/umbraco.linq.core.dll file
* Copy the new files and folders from the zip file into your site's folder
 * /App_Plugins
 * /Views
 * global.asax
* Remove the Config/formHandlers.config file

## Version 4.7.2 to 4.8.0
* Delete the bin/App_Browsers.dll file
* Delete the bin/App_global.asax.dll file
* Delete the bin/Fizzler.Systems.HtmlAgilityPack.dll file
* For people using uComponents 3.1.2 or below, 4.8.0 breaks support for it. Either upgrade to a newer version beforehand or follow the workaround [posted here](http://our.umbraco.org/projects/backoffice-extensions/ucomponents/questionssuggestions/33021-Upgrading-to-Umbraco-48-breaks-support-for-uComponents)

## Version 4.7.1.1 to 4.7.2
* Delete the bin/umbraco.MacroEngines.Legacy.dll file

## Version 4.6.1 to 4.7.1.1
* Delete bin/Iron*.dll (all dll files starting with "Iron")
* Delete bin/RazorEngine*.dll (all dll files starting with "RazorEngine")
* Delete bin/umbraco.MacroEngines.Legacy.dll
* Delete bin/Microsoft.Scripting.Debugging.dll
* Delete bin/Microsoft.Dynamic.dll
