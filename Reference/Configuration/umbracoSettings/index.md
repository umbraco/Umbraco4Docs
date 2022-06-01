---
versionFrom: 8.0.0
versionTo: 8.17.0
meta.Title: "umbracoSettings.config options in Umbraco"
meta.Description: "Reference on umbracoSettings.config options in Umbraco"
---

# umbracoSettings

Here you will be able to find documentation on all the options you can modify in the umbracoSettings.config file.

## Backoffice

Below you can see the settings that affect the Umbraco backoffice.

### Tours

The section is used for controlling the backoffice tours functionality.

```xml
<backOffice>
    <tours enable="true"></tours>
</backOffice>
```

There is only one supported attribute on the tours element:

**`enable`**
By default this is set to true. Set it to false to turn off [backoffice tours](../../../Extending/Backoffice-Tours/index.md)

## Content

Below you can see settings that affects content in Umbraco.

### Cleanup policy

The global settings for the scheduled job which cleans up historic content versions. These settings can be overridden per document type.

Current draft and published versions will never be removed, nor will individual content versions which have been marked as "preventCleanup".

See [Content Version Cleanup](../../../Fundamentals/Data/Content-Version-Cleanup/index.md) for more details on overriding configuration and preventing cleanup of specific versions.

```json
<?xml version="1.0" encoding="utf-8"?>
<settings>
  <content>
    <!-- Add contentVersionCleanupPolicyGlobalSettings in the content section -->
    <contentVersionCleanupPolicyGlobalSettings
      enable="true"
      keepAllVersionsNewerThanDays="7"
      keepLatestVersionPerDayForDays="90" />
  </content>
<settings>
```

If you don't wish to retain any content versions except for the current draft and currently published you can set both of the "keep" settings values to 0, after which the next time the scheduled job runs (hourly) all non-current versions (except those marked "prevent cleanup") will be removed.

### Obsolete data types

This section is used for controlling whether or not the data types marked as obsolete should be visible in the dropdown when creating new data types.

```xml
<showDeprecatedPropertyEditors>false</showDeprecatedPropertyEditors>
```

**`enable`**
By default this is set to false. To make the obsolete data types visible in the dropdown change the value to **true**.

### Imaging

This section is used for managing thumbnail creation and which properties of an image that should be automatically updated on upload.

```xml
<imaging>
    <!-- what file extension that should cause Umbraco to create thumbnails -->
    <imageFileTypes>jpeg,jpg,gif,bmp,png,tiff,tif</imageFileTypes>
    <!-- automatically updates dimension, file size and extension attributes on upload -->
    <autoFillImageProperties>
        <uploadField alias="umbracoFile">
            <widthFieldAlias>umbracoWidth</widthFieldAlias>
            <heightFieldAlias>umbracoHeight</heightFieldAlias>
            <lengthFieldAlias>umbracoBytes</lengthFieldAlias>
            <extensionFieldAlias>umbracoExtension</extensionFieldAlias>
        </uploadField>
    </autoFillImageProperties>
</imaging>
```

Let's break it down.

**`<imageFileTypes>`**
As the comment above states, this is a comma separated list of accepted image formats, which Umbraco can create a thumbnail of the image from.

**`<autoFillImageProperties>`**
As the comment above states, you can define what properties should be automatically updated when an image is being uploaded. This means that if you decide to rename the default **umbracoWidth** and **umbracoHeight** properties to **width** and **height** then the values in **`<widthFieldAlias>`** and **`<heightFieldAlias>`** need to be updated with the new property aliases. This needs to happen in order to automatically populate the values when the image is being uploaded.

If you need to create a custom media document type to handle images called something like "Custom Image" width an alias of **customImage** then you need to add another
**`<uploadField>`** element where the **alias** is set to **customImage**. Like below. Note that the width and height attributes has also been changed in this example.

```xml
<imaging>
    <!-- what file extension that should cause Umbraco to create thumbnails -->
    <imageFileTypes>jpeg,jpg,gif,bmp,png,tiff,tif</imageFileTypes>
    <!-- automatically updates dimension, file size and extension attributes on upload -->
    <autoFillImageProperties>
        <uploadField alias="umbracoFile">
            <widthFieldAlias>umbracoWidth</widthFieldAlias>
            <heightFieldAlias>umbracoHeight</heightFieldAlias>
            <lengthFieldAlias>umbracoBytes</lengthFieldAlias>
            <extensionFieldAlias>umbracoExtension</extensionFieldAlias>
        </uploadField>
        **<uploadField alias="customImage">**
            **<widthFieldAlias>width</widthFieldAlias>**
            **<heightFieldAlias>height</heightFieldAlias>**
            <lengthFieldAlias>umbracoBytes</lengthFieldAlias>
            <extensionFieldAlias>umbracoExtension</extensionFieldAlias>
        </uploadField>
    </autoFillImageProperties>
</imaging>
```

### Errors

In case of a 404 error (page not found), Umbraco can return a default page instead. You can also set a different error page, based on the current culture so a 404 page can be returned in the correct language.

You can customize the error pages using the `Error404Collection` section in the `appsettings.json` file. For more information, see the [Implementing Custom Error Pages](../../../Tutorials/Custom-Error-Pages/index.md#404-errors) article.

#### Configuration for Multiple Sites with different Cultures

If you have multiple sites, with different cultures, setup in your tree then you need to update the `Error404Collection` section in the `appsettings.json` file like below:

```json
{
    "Umbraco": {
        "CMS": {
            "Content": {
                "Error404Collection": [
                {
                    "ContentXPath": "//rootNode//errorPages[@nodeName='Page Not Found']",
                    "Culture": "default"
                },
                {
                    "ContentXPath": "//rootNode//errorPages[@nodeName='English Page Not Found']",
                    "Culture": "en-US"
                },
                {
                    "ContentXPath": "//rootNode//errorPages[@nodeName='Danish Page Not Found']",
                    "Culture": "da-DK"
                }
                ]
            }
        }
    }
}
```

If you have more than two sites and did not update the `Error404Collection` section with a 404 page and a culture, then the **default** page will act as a fallback. It acts the same if you haven't defined a hostname on a site.

#### Proxying through IIS on Umbraco Cloud

For environment specific transforms, include a `web.{ENVIRONMENT}.config` file for each environment requiring a `web.config` transformation such as `web.Development.config`, `web.Staging.config`, or `web.Production.config`.

To set the appropriate status code, update your **web.Production.config** for the *Production* environment:

```html
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <httpErrors errorMode="Custom" existingResponse="Replace">
        <remove statusCode="404"/>
            <error statusCode="404" path="~/ErrorPages/404.html" responseMode="File"/>
      </httpErrors>
    </system.webServer>
  </location>
</configuration>
```

#### Validation Errors

The error pages won’t capture every exception you’re likely to encounter in your application.

For example, if the user entered or pasted the wrong URL in the address window, an invalid request message, or a malformed syntax request etc. These request will produce a 400 (Bad Request) response so you can either add a specific error page to handle this request or set up a `defaultRedirect` as such:

```html
<customErrors mode="Off" redirectMode="ResponseRewrite" defaultRedirect="~/404.aspx">
    <error statusCode="404" redirect="~/404.aspx"/>
</customErrors>
```

#### Errors and IIS7+

You may find that your custom error page doesn't show, and instead IIS handles the error. To resolve this add the following key to your web.config right before the closing tag of the system.webServer section.

```xml
<httpErrors existingResponse="PassThrough" />
```

Now IIS will ignore httpErrors and allow Umbraco to handle them.

#### Handling multiple sites with the same culture

If you have multiple sites with the same culture then you can't use the above error settings. Then you will need to have a look at the [uComponents Multi-Site Not Found handler](http://ucomponents.codeplex.com/wikipage?title=MultiSitePageNotFoundHandler).
The benefit of using this handler is that you can choose the error page to be shown within the Umbraco backoffice.

### Notifications

Umbraco can send out email notifications, set the sender email address for the notifications emails here. To set the SMTP server used to send the emails, edit the standard `<mailSettings/>` section in the web.config file.

```xml
<notifications>
    <!-- the email that should be used as from mail when Umbraco sends a notification -->
    <email>your@email.here</email>
    <disableHtmlEmail>false</disableHtmlEmail>
</notifications>
```

### `<PreviewBadge>`

This allows you to customize the preview badge being shown when you're previewing a node.

```xml
<PreviewBadge><![CDATA[<a id="umbracoPreviewBadge" style="position: absolute; top: 0; right: 0; border: 0; width: 149px; height: 149px; background: url('{1}/preview/previewModeBadge.png') no-repeat;" href="{0}/endPreview.aspx?redir={2}"><span style="display:none;">In Preview Mode - click to end</span></a>]]></PreviewBadge>
```

### `<ResolveUrlsFromTextString>`

This setting is used when you're running Umbraco in virtual directories.

```xml
<!-- Url Resolving ensures that all links works if you run Umbraco in virtual directories -->
<!-- Setting this to true can increase render time for pages with a large number of links -->
<!-- If running Umbraco in virtual directory this *must* be set to true! -->
<ResolveUrlsFromTextString>false</ResolveUrlsFromTextString>
```

### `<DisallowedUploadFiles>`

This setting consists of a list of file extensions that editors shouldn't be allowed to upload via the backoffice.

```xml
<!-- These file types will not be allowed to be uploaded via the upload control for media and content -->
<disallowedUploadFiles>ashx,aspx,ascx,config,cshtml,vbhtml,asmx,air,axd,swf,xml,xhtml,html,htm,svg,php,htaccess</disallowedUploadFiles>
```

### `<AllowedUploadFiles>`

If greater control is required than available from the above, this setting can be used to store a list of file extensions. If provided, only files with these extensions can be uploaded via the backoffice.

```xml
<!-- If completed, only the file extensions listed below will be allowed to be uploaded.  If empty, disallowedUploadFiles will apply to prevent upload of specific file extensions. -->
<allowedUploadFiles></allowedUploadFiles>
```

### `<loginBackgroundImage>`

You can specify your own background image for the login screen here. The image will automatically get an overlay to match backoffice colors. This path is relative to the `~/umbraco` path. The default location is: `/umbraco/assets/img/installer.jpg`.

```xml
<loginBackgroundImage>../App_Plugins/Backgrounds/login.png</loginBackgroundImage>
```

## Security

In the security section you have the following options: **`<keepUserLoggedIn>`**, **`<usernameIsEmail>`**, **`<hideDisabledUsersInBackoffice>`**, **`<allowPasswordReset>`**, **`<authCookieName>`** and **`<authCookieDomain>`**. These settings deal with backoffice users and settings for the backoffice authentication cookies.

```xml
<security>
    <!-- set to true to auto update login interval
    (and there by disabling the lock screen) -->
    <keepUserLoggedIn>true</keepUserLoggedIn>

    <!-- by default this is true and if not specified in config will be true.
    Set to false to always show a separate username field in the backoffice user editor -->
    <usernameIsEmail>true</usernameIsEmail>

    <!-- If you prefer not to display them set this to true -->
    <hideDisabledUsersInBackoffice>false</hideDisabledUsersInBackoffice>

    <!-- set to true to enable the UI and API to allow backoffice users to reset their passwords -->
    <allowPasswordReset>true</allowPasswordReset>

    <!-- set to a different value if you require the authentication cookie for backoffice users to be renamed -->
    <authCookieName>UMB_UCONTEXT</authCookieName>

    <!-- set to a different value if you require the authentication cookie
    for backoffice users to be set against a different domain -->
    <authCookieDomain></authCookieDomain>

</security>
```

**`<keepUserLoggedIn>`**
Keep this setting to "true" to avoid the lock screen introduced in earlier version of Umbraco. If you like the lock screen feel free to set this
option to "false" and thereby enabling it.

**`<usernameIsEmail>`**
This setting specifies whether the username and email address are separate fields in the backoffice editor. When set to "false", you can specify an email address and username, only the username can be used to log on. When set the "true" (the default value) the username is hidden and always the same as the email address.

**`<hideDisabledUsersInBackoffice>`**
If this is set to "true" it's not possible to see disabled users, which means it's
not possible to re-enable their access to the backoffice again. It also means you can't create an identical username if the user was disabled by a mistake.

**`<allowPasswordReset>`**
The feature to allow users to reset their passwords if they have forgotten them. The feature is based on [a method provided by ASP.NET Identity](https://www.asp.net/identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity). By default, this is enabled but if you'd prefer to not allow users to do this it can be disabled at both the UI and API level by setting this value to "false".

**`<authCookieName>`**
The authentication cookie which is set in the browser when a backoffice user logs in, and defaults to `UMB_UCONTEXT`. This setting is excluded from the configuration file but can be added in if a different cookie name needs to be set.

**`<authCookieDomain>`**
The authentication cookie which is set in the browser when a backoffice user logs in is automatically set to the current domain. This setting is excluded from the configuration file but can be added in if a different domain is required.

## Scripting

The [`<scripting>` section](index-vpre6.0.0.md) is about legacy scripting and is by default not present in new versions.

## viewstateMoverModule

The viewstate mover module is included by default. It enables you to move all ASP.NET viewstate information to the end of the page, thereby making it easier for search engines to index your content instead of going through viewstate JavaScript code. Please note that this does not work will all ASP.NET controls.

```xml
<!-- This moves the ASP.NET viewstate data to the end of the html document instead of having it in the beginning-->
<viewstateMoverModule enable="false" />
```

## RequestHandler

The options in the request handler let us do some quite useful things, like setting domain prefixes, deciding whether or not to use trailing slashes and setting URL replacement for special characters.
Let's have a further look at each option below.

```xml
<requestHandler>
    <!-- this will add a trailing slash (/) to urls when in directory url mode -->
    <addTrailingSlash>true</addTrailingSlash>
    <urlReplacing toAscii="false">
        <char org=" ">-</char>
        <char org="&quot;"></char>
        <char org="'"></char>
        <char org="%"></char>
        <char org="."></char>
        <char org=";"></char>
        <char org="/"></char>
        <char org="\"></char>
        <char org=":"></char>
        <char org="#"></char>
        <char org="+">plus</char>
        <char org="*">star</char>
        <char org="&amp;"></char>
        <char org="?"></char>
        <char org="æ">ae</char>
        <char org="ø">oe</char>
        <char org="å">aa</char>
        <char org="ä">ae</char>
        <char org="ö">oe</char>
        <char org="ü">ue</char>
        <char org="ß">ss</char>
        <char org="Ä">ae</char>
        <char org="Ö">oe</char>
        <char org="|">-</char>
        <char org="&lt;"></char>
        <char org="&gt;"></char>
    </urlReplacing>
</requestHandler>
```

### `<addTrailingSlash>`

As mentioned in the comment above, this will add a trailing slash to the url when **`<addTrailingSlash>`** is set to "true".
If you don't want to have a trailing slash, set the value to **false**.

### `<urlReplacing>`

The **toAscii** attributes tells Umbraco to convert all urls to ASCII using the built-in transliteration library. It is disabled by default, ie by default urls remain UTF8. Set it to **true** if you want to have ASCII urls.

Introduced in Umbraco 7.6.4 the toAscii attribute can be set to **try**. This will make the engine try to convert the name to an ASCII implementation. If it fails, it will fallback to the name. Reason is that some languages don't have ASCII implementations, therefore the urls would end up being empty.

Within the **`<urlReplacing>`** section there are a lot of **`<char>`** elements with an **org** attribute. The attribute holds the character that should
be replaced and within the **`<char>`** tags the value it should be replaced with is entered.

So, if **`<char org="ñ">n</char>`** is added above the **ñ** will be shown as a **n** in the url.

## Logging

Most of the logging configuration is moved to the Serilog config files.

```xml
<logging>
    <maxLogAge>-1</maxLogAge>
</logging>
```

### `<maxLogAge>`

The maximum log age in minutes used for the internal audit log scrubbing.

## Web.Routing

### `<trySkipIisCustomErrors>`

defines the value of Response.TrySkipIisCustomErrors when an error (404, 400, 500...) is encountered. You probably want it to be true in order to prevent IIS from displaying its own 404 or 500 pages, and instead have your own page displayed.

### `<internalRedirectPreservesTemplate>`

when true, an internal redirect does not reset the alternative template, if any.

### `<disableAlternativeTemplates>`

when true, the entire alternative templates feature of Umbraco is disabled.

**validateAlternativeTemplates**
Will not load the template from the database. If `false` the template might not exists in the database. Otherwise the template need to exist in the database.

### `<disableFindContentByIdPath>`

when true, urls such as /1234 do _not_ find content with ID 1234.

### `<disableRedirectUrlTracking>`

When you move and rename pages in Umbraco, 301 permanent redirects are automatically created, set this to true if you do not want this behavior.

### `<urlProviderMode>`

Possible values are:

- `Default`: Indicates that the URL provider should do what it has been configured to do.
- `Relative`: Indicates that the URL provider should produce relative URLs exclusively.
- `Absolute`: Indicates that the URL provider should produce absolute URLs exclusively.
- `Auto`: Indicates that the URL provider should determine automatically whether to return relative or absolute URLs.

### `umbracoApplicationUrl`

Defines the Umbraco application url, i.e. how the server should reach itself. By default, Umbraco will guess that url from the first request made to the server. Use that setting if the guess is not correct (because you are behind a load-balancer, for example). Format is: "http://www.mysite.com/umbraco" i.e. it needs to contain the scheme (http/https), complete hostname, and Umbraco path.

```xml
<web.routing
    trySkipIisCustomErrors="false"
    internalRedirectPreservesTemplate="false"
    disableAlternativeTemplates="false"
    validateAlternativeTemplates="false"
    disableFindContentByIdPath="false"
    disableRedirectUrlTracking="false"
    urlProviderMode="Auto"
    umbracoApplicationUrl=""
/>
```

## KeepAlive

```xml
<keepAlive
    disableKeepAliveTask="false"
    keepAlivePingUrl="{umbracoApplicationUrl}/api/keepalive/ping"
/>
```

### `<disableKeepAliveTask>`

Allows you to disable the keep alive http calls

### `<keepAlivePingUrl>`

If you want to change the url you need to call to keep the site alive, update this property.
