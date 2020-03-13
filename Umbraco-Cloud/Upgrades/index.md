---
versionFrom: 7.0.0
---

# Umbraco Cloud Product Upgrades

_This document describes when & what product updates are rolled out on Umbraco Cloud_

## What products are auto upgraded?

* Umbraco CMS patch updates
* Forms patch updates
* Deploy / Courier patch updates
* Internal Umbraco Cloud services (generally these updates will not affect running websites but in some cases if they do we will notify Umbraco Cloud users via the status page)

When minor upgrades are available, you will need a Development environment on your project in order to get the new version. Read the [Minor Upgrades](Minor-Upgrades) article for more details.

## When do upgrades happen?

* The status page will include all important roll out information: __[http://status.umbraco.io/](http://status.umbraco.io/)__
* We will release product updates only on __Tuesday__
* The decision to roll out an upgrade will be made no later than the __Thursday__ prior and that status page will be updated accordingly
* A product upgrade will be rolled out if:
  * A fix needs to be shipped due to a critical issue in any product
  * A patch version of Umbraco Core is ready for release
  * A new version of Deploy / Courier is ready for release
  * A new version of Forms is ready for release
* Umbraco Cloud reserves the right to rollout an emergency product fix to fix a critical issue at any time

:::note
Your project will not be auto-upgraded if your environments aren't running the same **minor version**. E.g. if you are in the middle of upgrading to a new minor version, and one environment is running 7.6.x while another environment on the same project is running 7.7.x.
:::

## The auto upgrade roll out process

Before a live upgrade is rolled out on Umbraco Cloud:

* Write release notes and include special upgrade instructions and/or blog post if necessary
* Create a new version on the issue tracker
* Take the build from AppVeyor and push to NuGet
* Update our.umbraco.com release page
* Update Umbraco Cloud’s site creation engine with the new version so that all new sites are built with the latest version
* Run the auto-upgrader on Umbraco Cloud on a subset of test sites to verify there are no issues
* Run the auto-upgrader on all Umbraco Cloud sites

## The process of auto-upgrading an Umbraco Cloud project

This describes how an Umbraco Cloud project is auto-upgraded:

* The upgrade payload will have been created for the specific product(s) being upgraded
* The payload is a set of files (such as DLLs, and other ASP.NET website files)
* The upgrader will verify that the home page of all the environments (dev/staging/live) are healthy, meaning they don’t return an http status error. If all environments are ok, it will proceed.
* The upgrader will take a snapshot of the Dev site’s home page including it’s http status code result and its html contents.
* The payload is deployed to the Dev site’s Git repository and committed with a tag for the product version being updated. This new Git repository commit will replace the Umbraco product assembly (DLL) files along with other product files such as files located in /umbraco, /umbraco_client folders
* The normal Umbraco Cloud deployment process is invoked and the repository files are deployed to the website
* The upgrader will automatically ensure the web.config version and the database version are updated so that the Installer/upgrade page is not shown
* The upgrader will verify that the new http status code returned from the Dev site’s home page is OK and will verify that the html contents of the home page match that of the snapshot originally taken.
* If either of these tests fail we will be notified and Umbraco will take appropriate measures to rollback the site to it’s previous state
* The failed upgrade is then tracked for reporting and the customer will be notified if necessary
* When the Dev site is upgraded successfully, the upgrader will continue this same process for the next environment in the chain (i.e. Dev -> Staging -> Live) depending on the number of environments that exist for the project.

## How do baseline updates work?

If a project is a project that has had child projects created off it, the upgrade process for patch versions is the same as described above. The difference is that we always upgrade the baseline as the first project, and afterwards we upgrade the child projects. This ensures that if for some reason an update is done from the baseline to the children in the meantime, the patch upgrade will also be sent to the children.

## What is a breaking change?
It is important that developers understand what is considered a breaking change in Umbraco products. In most cases an auto-upgrade will not have any breaking changes and we strive to ensure this is the case. However, in some rare cases developers may be using Umbraco’s internal code or Umbraco’s code that is not intended for public consumption and in some releases that code may change. It is important for developers to understand the risks of using Umbraco code that is not considered a breaking change when it is updated since this may directly affect a site that is auto-upgraded.

What is a breaking change is documented here: [https://our.umbraco.com/documentation/development-guidelines/breaking-changes](https://our.umbraco.com/documentation/development-guidelines/breaking-changes)

## Can I opt out of product auto upgrades?

No it´s not possible to opt out of product auto upgrades on Umbraco Cloud.

In order for us to be able to support a site on Umbraco Cloud we must ensure that all sites are running the latest versions of our products. That way, we know the sites are running in the most stable state.
