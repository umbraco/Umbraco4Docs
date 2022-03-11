---
versionFrom: 8.0.0
meta.Title: "Umbraco Deploy settings"
meta.Description: "Various settings for Umbraco Deploy"
---

# Configurations for Deployments

The UmbracoDeploy.Settings.config file is empty by default, but there are some optional settings you can set in the file to ignore certain types of file, increase timeout limits, etc.

## ExcludedEntityTypes

This setting allows you to exclude a certain type of entity from being deployed. This is **not** recommended to set, but sometimes there may be issues with the way a custom media fileprovider works with your site and you will need to set it for media files. Here is an example:

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
  <excludedEntityTypes>
    <add type="media-file" />
  </excludedEntityTypes>
</settings>
```

## RelationTypes

This setting allows you to manage how relations are deployed between environments. You will need to specify an alias and a mode for each relationtype. The mode can be either:

- `Exclude` - This causes the relation to be excluded and not transferred on deployments.
- `Weak` - This causes the relation to be deployed if both content items are found on the target environment.
- `Strong` - This requires the content item that is related is set as a dependency, so if anything is added as a relation it would also add it as a dependency.

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <relationTypes>
        <relationType alias="relateParentDocumentOnDelete" mode="Weak" />
        <relationType alias="relateShopItemOnCreate" mode="Exclude" />
    </relationTypes>
</settings>
```

## ValueConnectors

This setting is used by package creators who wants their custom editors to work with Deploy. The packages should be creating this setting automatically. There is a community driven package that has value connectors for Deploy called [Deploy Contrib](https://github.com/umbraco/Umbraco.Deploy.Contrib).

Here is an example of how the setting can look:

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <valueConnectors>
        <valueConnector alias="nuPickers.DotNetCheckBoxPicker"
                        type="Umbraco.Deploy.Contrib.Connectors.ValueConnectors.NuPickersValueConnector,Umbraco.Deploy.Contrib.Connectors" />
        <valueConnector alias="nuPickers.DotNetDropDownPicker"
                        type="Umbraco.Deploy.Contrib.Connectors.ValueConnectors.NuPickersValueConnector,Umbraco.Deploy.Contrib.Connectors" />
        <valueConnector alias="nuPickers.DotNetPrefetchListPicker"
                        type="Umbraco.Deploy.Contrib.Connectors.ValueConnectors.NuPickersValueConnector,Umbraco.Deploy.Contrib.Connectors" />
        <valueConnector alias="nuPickers.DotNetTypeaheadListPicker"
                        type="Umbraco.Deploy.Contrib.Connectors.ValueConnectors.NuPickersValueConnector,Umbraco.Deploy.Contrib.Connectors" />
    </valueConnectors>
</settings>
```

## Timeout settings

Umbraco Deploy have a few built-in timeouts, which on larger sites might need to be modified. You will usually see these timeouts in the backoffice with an exception mentioning a timeout. It will be as part of a full restore or a full deploy of an entire site. In the normal workflow you should never hit these timeouts.

The defaults will cover most though. Changing the defaults by updating the `/Config/UmbracoDeploy.settings.config`. There are four settings available:

- `sessionTimeout`
- `sourceDeployTimeout`
- `httpClientTimeout`
- `databaseCommandTimeout`

These timeout settings default to 20 minutes, but if you are transferring a lot of data you may need to increase it. All of these times are in *seconds*:

:::note
It's important that these settings are added to both the source and target environments in order to work.
:::

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <deploy sessionTimeout="1800" sourceDeployTimeout="1800" httpClientTimeout="1800" databaseCommandTimeout="1800" />
</settings>
```

## Transfer Forms data as content

In order for Deploy to handle Forms data as content, you'll need to add the following setting to `UmbracoDeploy.Settings.config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <forms transferFormsAsContent="true" />
</settings>
```

## Transfer dictionary items as content

In a similar way, Deploy can be configured to allow for backoffice transfers of dictionary items instead of using files serialized to disk, by adding the following setting to `UmbracoDeploy.Settings.config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <dictionary transferDictionaryAsContent="true" />
</settings>
```

## Transfer members

As of version 4.5.0, it's also possible to transfer members and member groups via the back-office between environments.  This is disabled by default as a deliberate decision to make use of the feature needs to be taken, as for most installations it will make sense to have member data created and managed only in production. There are obvious potential privacy concerns to consider too.  However, if being able to deploy and restore this information between environments makes sense for the specific workflow of your project, it's a supported scenario.

To enable, add the following setting to your `UmbracoDeploy.Settings.config` configuration file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <members allowMembersDeploymentOperations="None|Restore|Transfer|All"
             transferMemberGroupsAsContent="true|false" />
</settings>
```

The `allowMembersDeploymentOperations` setting can take four values:

- `None` - member deployment operations are not enabled (the default value if the setting is missing)
- `Restore` - restore of members from upstream environments via the backoffice is enabled
- `Transfer` - transfer of members to upstream environments via the backoffice is enabled
- `All` - restore and transfer of members from upstream environments via the backoffice is enabled

With `transferMemberGroupsAsContent` set to `true`, member groups can also be transferred via the backoffice, and groups identified as dependencies of members being transferred will be automatically deployed.

Note that it's important to ensure a common machine key is setup for all environments, to ensure hashed passwords remain usable in the destination environment.

## Exporting member groups

This setting is to be defined and set to false only if you are using an external membership provider for your members. You will not want to export Member Groups that would no longer be managed by Umbraco but by an external membership provider.

Setting the `exportMemberGroups` to false will no longer export Member Groups to .uda files on disk. By default if this setting is not present, its value will automatically be set to true as most sites use Umbraco's built-in membership provider and thus will want the membership groups exported.

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <deploy exportMemberGroups="false" />
</settings>
```

## Ignore broken dependencies

When restoring or transferring content, Umbraco Deploy will make checks to ensure that any dependent content, media or other items are either present in the target environment, or can be deployed from the source environment.

For example, if you have a media picker on a content item, that references a media item that's been deleted or is in the recycle bin, you'll get an error and the deploy won't complete until the issue is resolved (by removing the reference to the deleted media item).

You can configure deploy to ignore these issues and proceed with the transfer operation without warning, with the following setting:

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <deploy ignoreBrokenDependencies="true" />
</settings>
```

For finer control of the above setting you can add a further attribute named `ignoreBrokenDependenciesBehavior` and set the value to either `All` (the default), `Transfer` or `Restore`.

For example, using the following settings, you will have an installation that ignores broken dependencies when restoring from an upstream environment.  It will however still prevent deployment and report any dependency issues when attempting a transfer to an upstream environment.

```xml
<?xml version="1.0" encoding="utf-8"?>
<settings xmlns="urn:umbracodeploy-settings">
    <deploy ignoreBrokenDependencies="true"
            ignoreBrokenDependenciesBehavior="Restore" />
</settings>
```


