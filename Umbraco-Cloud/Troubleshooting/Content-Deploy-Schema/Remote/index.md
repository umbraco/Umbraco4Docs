# Troubleshooting Content deployments on Umbraco Cloud

## When working on Umbraco Cloud

If the schema (this includes DocumentTypes, MediaTypes, DataTypes, Templates, Macros and Dictionary items) is different between the two environments you are deploying between, you will need to deploy the updates for these before you can complete the Content transfer (this can contain Media from the Media section as well). Environments needs to be in-sync before a Content and/or Media transfer can succeed.

While Content and Media transfer are done using the Umbraco backoffice you use the Umbraco Cloud Portal in order to deploy the schema changes, which exists on disk and are deployed through the underlying git repository. The deployment between your Umbraco Cloud environments is simple to do:
 * You will see a number of pending changes on your environments
 * Click the green "Deploy changes to Staging/Live" button and wait for it to finish.
   ![Pending deployments](images/pending-deployments.png) 
 * When it’s done, all of the schema changes from the source environment (typically Development) will have been deployed to and extracted in the target environment (typically Live).

Now that the schema changes are in sync between your project's environments you should be able to transfer your Content changes through the Umbraco backoffice. Go to the Deployment dashboard in the Content section and reload the queue if it’s not shown. In some cases the queue may have been reset during the previous deployment, so if it remains empty please go back and select the Content and/or Media that you want to transfer.

If you continue to see conflicts between the schema parts that were deployed then please refer to the Debugging section below.

## Debugging

When you run into schema mismatch errors, they will usually look something like this:

![Schema Mismatch error message](images/schema-mismatch-on-transfer.png)

In this error message you are able to see exactly which schema mismatches is preventing the content transfer. If you do not have any pending deployments on your source environment (Development or Staging) in the Umbraco Cloud Portal, there are two ways to go about resolving the schema mismatch:

1. On the source environment (Development or Staging) make a minor change to the schema with mismatches (in the example above it would be the **Homepage** document type). Deploy the change to the next environment
    * This will update your schema on the target environment (Staging or Live) and ensure it's in sync with the source environment
2. If the mismatches are differences to `aliases` or `names`, changing these manually on the target environment will enable you to transfer your content
    * Do **not** make any major schema changes or create new schema on the Staging or Live environments. There is no way to sync these changes down to your *lower* environments

## Debugging (Courier)

*If your project is using Courier, please see the documentation below for debugging schema mismatch errors.*

If you continue to see conflicts between the schema parts (being DocumentTypes, DataTypes, Templates, etc.) that was just deployed you need to dive into the log files to debug exactly what the problem is.

In order to find the log entries that deals with conflicts in the schema you should log for an entry like the following:


    2015-04-27 14:59:20,546 [10] INFO  Umbraco.Courier.Core.Packaging.RevisionPackaging - [Thread 45] Document types: Home hash-mismatch (local/remote) e5c6dc5f2eee6521b2d024f7777bbd9e / 2628e7c3e4bc7215fd398a2bbb13f423

This error is not very descriptive and if you’re not sure what the difference are then you can investigate it a little bit deeper. If you add a key to your appSettings section in web.config you get to actually see what data we’ve tried to compare, unhashed.
In web.config (on both ends, both source and target), you can add the following key (note: on both the source and target environment):

    <add key="DeployHashDebug" value="true" />

Now when you get the above error, you’ll get the same message with a little more information:

![clone dialog](images/image07.png)

Using a text compare tool like WinMerge we can pretty easily figure out that the property “Test” was added to the local document type but isn’t found on the remote instance.

![clone dialog](images/image00.png)
