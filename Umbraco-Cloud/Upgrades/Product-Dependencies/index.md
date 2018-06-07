# Dependencies on Umbraco Cloud

This article gives an overview of the dependencies between the products on Umbraco Cloud.

The products are: 

* Umbraco CMS
* Umbraco Forms
* Umbraco Courier
* Umbraco Deploy

In the table below you can see which version of Umbraco Forms and Courier / Deploy you should be running on your Umbraco Cloud project depending on which version of Umbraco CMS, you are running.

|Umbraco CMS   |Umbraco Forms   |Umbraco Courier   |Umbraco Deploy   |
|--------------|----------------|------------------|-----------------|
|**< 7.5**     |4.x             |3.x               |--               |
|**7.5**       |4.x             |3.x               |--               |
|**7.6**       |6.x             |--                |1.x              |
|**7.7**       |6.x             |--                |2.x              |
|**7.8**       |6.x             |--                |2.x              |
|**7.9**       |7.x             |--                |2.x              |
|**7.10**      |7.x             |--                |2.x              |

**NOTE**: All Umbraco Cloud projects running Umbraco 7.6 or above should be using Umbraco Deploy instead of Umbraco Courier. If you are upgrading from Umbraco 7.5.x to 7.6 or above, you also need to [replace Umbraco Courier with Umbraco Deploy](../Moving-from-Courier-to-Deploy).

## [How to upgrade Umbraco CMS manually](../Manual-upgrades/Manual-CMS-upgrade.md)

## [How to upgrade Umbraco Forms manually](https://our.umbraco.org/documentation/Add-ons/UmbracoForms/Installation/ManualUpgrade)

## [How to upgrade Umbraco Courier / Deploy manually](../Manual-upgrades/Manual-Deploy-and-Courier-Upgrade)
