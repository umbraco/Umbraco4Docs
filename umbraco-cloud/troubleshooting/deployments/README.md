# Troubleshooting deployments

Issues with deployments on Umbraco Cloud often come down to a misunderstanding on how to work with Umbraco Cloud. It is important to always work left to right as mentioned [here](../../deployment/).

There are two ways to deploy on Umbraco Cloud, a deployment that transfers content and media:

1. A Content [Transfer](../../deployment/content-transfer.md) / [Restore](../../deployment/restoring-content/)
2. A [Deployment](../../deployment/cloud-to-cloud.md) that transfers structure files (doc types, data types, templates, dll's, etc.)

There are some common errors associated with both of these. Most of the time it is caused by conflicting [UDA files](../../set-up/power-tools/generating-uda-files.md#what-are-uda-files) between the two environments you are deploying between.

The most common [Deployment](../../deployment/cloud-to-cloud.md) issues are listed below with guides on how to fix them:

* [Collision Errors](structure-error.md)
* [Dependency Exception](dependency-exceptions.md)
* [Colliding Data Types](colliding-datatypes.md)
* [Language Mismatch](language-mismatch.md)
* [Deployment Failed (with no error message)](deployment-failed.md)
* [Changes not being applied](changes-not-being-applied.md)

The most common Content [Transfer](../../deployment/content-transfer.md) / [Restore](../../deployment/restoring-content/) issues are listed below:

* [Schema mismatch](schema-mismatches.md)
* [SQL Timeouts](../../../umbraco-deploy/deploy-settings.md#timeout-issues)
* [Dependency Exception](dependency-exceptions.md)
* [Media path too long](path-too-long-exception.md)
