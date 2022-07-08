---
versionFrom: 8.0.0
meta.Title: "Umbraco configuration files"
meta.Description: "Information on the various configuration files in Umbraco"
versionRemoved: 9.0.0
---

:::note
If you are searching for Umbraco v9+ documentation, it currently lives under the folder [V9-Config](../V9-Config/index.md)
:::

# Configuration Files

The release of V8 has moved many of the previous configuration options from XML configuration files in the usual /config folder to be configurable through code.

## The following Configuration Files remain in Umbraco 8

### [web.config](webconfig/)

The heart of your web application that hosts many configuration settings required for your application.

This file can be found at the following path: /web.config

### [tinyMceConfig.config](tinyMceConfig/index.md)

The 'tinyMceConfig.config' contains configuration options for the [TinyMce](https://www.tinymce.com/) editors in the Umbraco Backoffice.

This file can be found at the following path: /config/tinyMceConfig.config

### [umbracoSettings.config](umbracoSettings/index.md)

Contains many Umbraco options. Generally the default values work well in most installs; however, in some cases some of these options may need adjusting depending on each installation.

This file can be found at the following path: /config/umbracoSettings.config

### clientdependency.config

The ClientDependency configuration options can be found on the [ClientDependency website](https://github.com/Shandem/ClientDependency/wiki/Configuration).

This file can be found at the following path: /config/ClientDependency.config

### healthchecks.config

The configuration for backoffice healthcheck notifications.

This file can be found at the following path: /config/HealthChecks.config

### Image Processor

The configuration for Image Processor, the library responsible for "on the fly processing" of images in Umbraco.

There are three configuration files
/imageprocessor/cache.config
/imageprocessor/security.config
/imageprocessor/processing.config

## Replaced by code

### [404handlers.config](404handlers/index.md)

Configuration file for legacy NotFoundHandlers

### [applications.config](applications/index.md)

Configuration options for setting up sections within the Umbraco Backoffice.

### [BaseRestExtensions.config](BaseRestExtensions/index.md)

The 'BaseRestExtension.config' contains the data necessary for the /Base system when exposing methods in your class library.

### [dashboards.config](dashboard/index.md)

Configuration options for controlling which dashboards appear in which sections of the backoffice and who has the permissions to see them.

### [EmbeddedMedia.config](EmbeddedMedia/index.md)

This configuration file lists the Embedded Media Providers configured for use in your Umbraco site.

### [ExamineIndex.config](ExamineIndex/index.md)

The 'ExamineIndex.config' file contains the configuration for the Examine IndexSets used for storing indexed content in an Umbraco installation.

### [ExamineSettings.config](ExamineSettings)

The 'ExamineSettings.config' file shows all the configuration options for Examine.

### [fileSystemProviders.config](fileSystemProviders/index.md)

The 'fileSystemProviders.config' file contains the configuration for the file system providers used by Umbraco to interact with file systems.

### sections.config

The 'sections.config' file contains the configuration for custom sections loaded in the Umbraco Backoffice.

### [trees.config](trees/index.md)

The 'trees.config' file contains the configuration for trees that are loaded within each section of the Umbraco Backoffice.

### UrlRewriting.config

This is now obsolete and there are better ways to do UrlRewriting, such as within [IIS](https://docs.microsoft.com/en-us/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module). However, the UrlRewriting documentation can be [downloaded in PDF format](https://github.com/aspnetde/UrlRewritingNet/blob/master/docs/UrlRewritingNet20_English.pdf) for legacy projects.

## New to Umbraco 8

### [SeriLog.config](Serilog/index.md)

In v8 Serilog is used for logging, use these files to configure this:

* `/config/serilog.config` is used to modify the main Umbraco logging pipeline
* `/config/serilog.user.config` a sublogger that allows you to make modifications without affecting the main Umbraco logger

### NuCache

We have moved away from the traditional large XML file (`umbraco.config` file) and instead using a key value store (`NuCache.Content`) for caching. Content is now stored in a binary cache called NuCache. You can find the file at `/App_Data/TEMP/NuCache/NuCache.Content.db`.
