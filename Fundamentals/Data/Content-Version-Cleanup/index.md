---
meta.Title: "Content version cleanup"
versionFrom: 9.1.0
versionTo: 10.0.0
---

# Content Version Cleanup

A new version is created whenever you save and publish a content item in Umbraco. This is how you can roll back to a previous version. Every saved version stores a record in the database, not only for the version but also for each content item property for that version. In a multi-lingual site, further rows are added for every culture variation. Over time this amount of data can build and swallow up the capacity of your SQL Server and slow the performance of the Umbraco backoffice.

Umbraco 9.1.0 introduced a feature to clean up historic content versions (inspired by [Our.Umbraco.UnVersion](https://our.umbraco.com/packages/website-utilities/unversion/)).

## How it works

The default cleanup policy will:
 - Not delete any versions created over the previous 4 days. The recent version history is preserved. See the `KeepAllVersionsNewerThanDays` setting.
 - 'Prune' versions 4 days after they are created. The last version of a content item saved on a particular day will be kept but earlier versions from that day will be deleted.
 - Delete all versions older than 90 days. See the `KeepLatestVersionPerDayForDays` setting.
 - Never delete any 'published' versions.
 - Never delete any specific versions marked as 'Prevent Cleanup' in the backoffice version history.

The feature is enabled by default via configuration for new installs starting from 9.1.0. It will require to opt-in for those upgrading from 9.0.0.

The feature can be configured in the `appSettings.json`:

```json
{
  "Umbraco": {
    "CMS": {
      "Content": {
        "ContentVersionCleanupPolicy": {
          "EnableCleanup": true,
          "KeepLatestVersionPerDayForDays": 90,
          "KeepAllVersionsNewerThanDays": 4
        }
      }
    }
  }
}
```

For sites with stricter requirements, it is possible to opt-out of both options globally 
(see [ContentSettings](/documentation/Reference/Configuration/ContentSettings/index.md#contentversioncleanuppolicy)) and by Document Type.

Additionally, it is possible to keep the feature enabled but mark specific versions to keep forever.

It is worth noting that whilst we delete rows, we do not shrink database files or rebuild indexes. For upgraded sites with a lot
of history you may wish to perform these tasks. If they are not part of your regular database maintenance plan already.

## Overriding global settings

It is possible to override the global settings per Document Type in the backoffice to prevent unwanted cleanup. This can be managed in the "permissions"
Content App for each Document Type.

![Content Version Cleanup - Document Type overrides](images/per-doctype-override.png)

## Prevent cleanup of important versions

It is possible to mark important content versions as "prevent cleanup". This is to ensure they are never removed via the new and improved 
rollback modal. This view can be found on the "info" content app for each document.

1. Open rollback modal.
![Content Version Cleanup - prevent cleanup part 1](images/prevent-cleanup-part-1.png)

1. Click "prevent cleanup" button for each important version.
![Content Version Cleanup - prevent cleanup part 2](images/prevent-cleanup-part-2.png)
