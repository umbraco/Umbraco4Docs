---
versionFrom: 10.0.0
---
 
# Migrating an Existing Site to Umbraco Cloud

Sometimes you may already have a Umbraco site built that did not start with a clone of a Umbraco Cloud site. Or perhaps you have decided to move a site that's already live on Umbraco Cloud. In any case, migrating an existing site is not difficult, but it does require some specific steps, and an understanding of how Umbraco Cloud deployments work can be very helpful.

These are the steps you need to go through to complete the migration successfully:

1. [Requirements](#1-requirements)
2. [Tools](#2-tools)
3. [Prepare your site](#3-prepare-your-site)
4. [Prepare your Cloud project](#4-prepare-your-cloud-project)
5. [Clone down the Cloud project](#5-clone-down-the-cloud-project)
6. [Move and Merge files](#6-move-and-merge-files)
7. [Generate meta data](#7-generate-meta-data)
8. [Deploy to Umbraco Cloud](#8-deploy-to-umbraco-cloud)

If you prefer following a written guide, continue to read below.

## 1. Requirements

Before you start migrating your Umbraco site to Umbraco Cloud there are a few things you need to consider. To migrate your site smoothly, we have made a list of requirements your project(s) needs to meet.

Your Umbraco site has to fulfill these requirements:

* Contains no member data
  * If you do have member data, these will need to be imported manually after the migration
* No obsolete/old packages
  * Not all packages will work on Umbraco Cloud
  * Read more about this in the section below
* Isn’t a site that has been upgraded from versions below Umbraco 7
  * Legacy code from older versions can potentially cause issues

If you have a site that does not meet the above requirements, feel free to contact us and we will help you find the best solution for your site.

## 2. Tools

There are a few tools we recommend using to make the migration process as smooth as possible. We've made a checklist for you here:

* Git needs to be installed on your computer
  * Optional: Git client, like [Fork](https://git-fork.com/), [SourceTree](https://www.sourcetreeapp.com/), or [GitKraken](https://www.gitkraken.com/)
* Visual Studio OR Visual Studio Code + IIS Express
* Merging tool - like [WinMerge](http://winmerge.org/) or [DiffMerge](https://sourcegear.com/diffmerge/)

Aside from these tools, you'll also need:

* A local copy of your existing site
* A new and clean Umbraco Cloud project
  * We strongly recommend having a project with **at least 2 environments**

## 3. Prepare your site

After making sure that your existing site meets all the requirements for being migrated to Umbraco Cloud, you are now ready to get started.

### Upgrade to the latest Umbraco version

The first order of business is to **upgrade your own Umbraco site to the latest minor version of Umbraco**. Why? Because Umbraco Cloud always runs the latest version and you need to make sure your project runs the same Umbraco version as Umbraco Cloud.

You can download the latest version of Umbraco from [Our](https://our.umbraco.com/download/).

If you need help upgrading your project, we have some excellent [Upgrade instructions](https://our.umbraco.com/documentation/Getting-Started/Setup/Upgrading/general) you can follow. Be thorough when upgrading, as the latest upgrade might contain breaking changes and/or updated configuration.

If you have been using Umbraco Forms on your project, you will also need to upgrade this to the latest version. You can find and download the latest version of Umbraco Forms under [Projects on Our](https://our.umbraco.com/projects/developer-tools/umbraco-forms/). As with Umbraco CMS, we also have documentation on how to [Upgrade Umbraco Forms](https://our.umbraco.com/documentation/Add-ons/UmbracoForms/Installation/ManualUpgrade).

After upgrading your project make sure it runs without any errors. *Hint: Check the umbracoTraceLog.txt log file.*

### Cleaning your project

Before moving on to the next step, you need to clean up the local clone of your existing site a bit.

While the site is running you need to:

* Go to the backoffice of your project
* Empty the recycle bins from both the Content and Media sections

Now, shut down the project, and delete the following files and folders from `/Umbraco/Data`

* `/TEMP`

That was it! Now you are ready to start the actual migration process, or in other words: **now the real fun begins!**

## 4. Prepare your Cloud project

In this next part, you are going to set up your Umbraco Cloud project and clone it down to your local machine.

### Setup your Umbraco Cloud project

Before the migration process can start, you will need to have a Umbraco Cloud project you can migrate your project into.

![How to start a Umbraco Cloud trial](images/start-trial.gif)

1. The best way to get started with Umbraco Cloud is to [create a trial project](https://umbraco.com/)
2. When your project is starting choose to start with a *clean slate* - you need to have an empty Cloud project for the migration to be successful
3. We recommend that you set up your project with at least two environments.

![Manage environments](images/setup-dev-env.png)

If you on your existing site have been working with members and made changes to the default Member Type, you must follow these steps on the Umbraco Cloud environments:

1. Head to the backoffice of the Development environment
2. Navigate to the *Settings* section
3. Open the *Member types* folder
4. Delete **Member**
5. Repeat these steps on all the Cloud environments

![Default media types](images/MemberType-delete.png)

## 5. Clone down the Cloud project

<iframe width="800" height="450" src="https://www.youtube.com/embed/e3spd6Nqrf8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

With your Umbraco Cloud project ready for migration, it is time to clone down the project to your local machine.

Run the site locally and verify your project and the cloned Umbraco Cloud project are using the same Umbraco version. After you've verified this, shut down the site, and it's now time to start merging the two projects.

## 6. Move and merge files

Merging your existing site into the Umbraco Cloud project is a matter of moving and merging files between the two projects. When following the steps outlined below you mustn't overwrite any settings and configurations related to Umbraco Deploy.

1. Copy and replace the following folders from your project with the Umbraco Cloud project

    * `/wwwroot`
    * `/Views`
    * `/Umbraco
    * If your existing site uses Umbraco Forms, make sure you **do not overwrite** the `\umbraco\Licenses\umbracoForms.lic` file
2. Merge Appsettings.JSON files from your existing site with the cloud site.

    * Make sure that you merge your Appsetting.JSON files with the ones from your existing site so that the settings you used are moved over to your Cloud project

3. Merge your Program.cs and Startup.cs with the one from your existing site

4. If you are using SQLite
    * Make sure the `Umbraco.sqlite.db`, `Umbraco.sqlite.db-shm` and `Umbraco.sqlite.db-wal` files from your project replaces the ones provided with your Umbraco Cloud project
    * You can find them in `/umbraco/Data/Umbraco.sqlite.db`
5. If you are using a local SQL server make sure to update the connection string in the `Appsettings.JSON` for the Umbraco Cloud project.

That's it! Now that you've merged your existing site with the local clone of the Cloud project, you need to make sure the project runs and verify that

* You can log in using your Umbraco ID user
* All the content is there
* All Document Types, Templates, Stylesheets, etc, are in the backoffice

:::note
Umbraco Identity (Umbraco ID) is the single sign-on (SSO) feature across all Umbraco Cloud services and is required to access any project pages as well as backoffices.

Any users that you might have had on your existing Umbraco site will be migrated over to the local clone of the Cloud project along with the database. These users, however, will not be able to access the Cloud environments of the project or any of the backoffices associated with those environments.

For the users to continue having access to the project, they will need to be re-invited either as a [Team Member](../../Set-Up/Team-Members) on a project level or as a [User](../../Set-Up/Users-on-Cloud) to the backoffice of one or more Cloud environments.
:::

With that confirmed, it's time to prepare to migrate the project to Umbraco Cloud.

## 7. Generate meta data

You have now moved and merged the files from your existing site into the Umbraco Cloud project files. So far, so good!

In this next part, it is time to generate the so-called UDA-files for all your project's meta data.

For more details about UDA files, read the [UDA Files](../../Set-Up/Power-Tools/Generating-UDA-files/#what-are-uda-files) article.

* Make sure the folder `/Umbraco/Deploy/Revision` on your Umbraco Cloud project is empty
  * If you have any files in the folder, you can safely remove those at this point
* Start your local cloud project
* Go to the settings section in your project
* Select `Extract Schema To Data Files` Command from the **Deploy Operations** drop-down and click **Trigger Operation**.
  * Generating the UDA files may take a while, depending on the extent of your project
  * You will see a `Last deployment operation completed` status once the export is done
  * Run `Schema Deployment From Data Files` to check that the UDA files have been generated correctly
  * When you see a `Last deployment operation completed` status, it means everything is working as expected
* You should now see that your `/Umbraco/Deploy/revision` folder has been populated with UDA files corresponding to your project's metadata

![Run echo > deploy-export](images/deployDashBoard.png)

Now the migrated project is ready to be deployed to the Umbraco Cloud environment.

## 8. Deploy to Umbraco Cloud

All project files have been merged and we've generated UDA files for all the meta data. In this section, we are going to deploy the migrated project to the Umbraco Cloud Development environment.

### Deploying the metadata

1. In your Git client you should see a lot of changes ready to be committed
2. Stage and commit the changes
3. Do a *pull* to ensure everything is in sync
4. **Push** your migrated project to the Umbraco Cloud environment - check that the *'Deploy Complete'* message is displayed
    * If you have a very large commit to push, you may need to configure your Git client for this
    * Use: git config http.postBuffer 524288000

When the push is complete go check out the Umbraco Cloud Portal to verify the indicator on the Development environment is still *green*.

Go to the backoffice of your Development environment and make sure all your metadata is there. You won't see any content or media on the environment yet - this you will move in the next few steps.

![Changes committed to Development environment](images/changes-on-dev.png)

### Transfer your content and media

1. With all your metadata in place, it's time to transfer your content and media as well
2. Go to the backoffice of your local clone of the Umbraco Cloud project
3. Right-click the top of the Content tree and choose *'Queue for transfer'*
    * **NOTE**: If you have a large amount of content and media you may have the best result in deploying content and media independently
    * **Media**: If you have more than "a few" media items see our recommendations for working with [media in Umbraco Cloud](../../Set-up/Media/).

    :::note
    Records from the Redirect URL Management are not transferred by deploy.
    You will need to manually migrate them using SQL.
    :::

![Queue for transfer](images/transfer-v9.gif)

**Voila!** You've now migrated your site to Umbraco Cloud.

The very final step is to deploy the migration to the next environment - Staging or Live. You do this from the Umbraco Cloud Portal, using the green button on your Development environment *'Deploy changes to Staging/Live'*. Transfer content and media from the backoffice following the steps outlined above.
