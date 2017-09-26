# Working with databases
When working with Umbraco Cloud, the way you work with databases might differ from what you're used to. One important aspect of Umbraco Cloud is that you always work isolated to avoid interfering with colleagues or a running website. This includes the database as well.

So when you clone a site locally, Umbraco Cloud automatically creates a local database and populates it with data from your website running on the Cloud. If you don't specify anything before starting up your site locally, it'll be a SQL CE database that lives in the /App_Data folder. If you wish to use a local SQL Server instead, you can update the connection string in the web.config, but it's important that you do so before your site start up the first time.

## Using Custom Tables with Umbraco Cloud
Umbraco Cloud will ensure that your Umbraco related data is always up to date, but it won't know anything about data in custom tables unless told. Nothing new here, it's basically just like any other host when it comes to non-Umbraco data.

The good news is that you have full access to the SQL Azure databases running on Umbraco Cloud and you can create custom tables just like you'd expect on any other hosting provider. The easiest way to do this is to connect using SQL Management Studio.

## Connecting to SQL Azure on Umbraco Cloud Using SQL Management Studio
For security, your database on Umbraco Cloud is running behind a firewall so in order to connect to the database, you'll need to open the firewall for the relevant IPs. This can be a single IP, a list of IPs or even an IP Range. It's done from the Connection Details page on your project, simply click the "Settings" menu in the upper right corner of your project and select "Connection Details". If you don't see the menu item, it's due to permissions and you'll need to contact the administrator of your project.

### Opening the firewall
The easiest way to open the firewall for your ip address is to simply click the "Add Now" link. It'll automatically add your current ip address and save the settings. It might take up to five minutes for the firewall to be open for your IP.

If you need to open for specific ip addresses, simply click the "Add New IP Address" button.

## Setting up SQL Management Studio
Once the firewall is open, it's time to fire up SQL Management Studio and connect to the database. Be aware that a database exist for each environment you have on Umbraco Cloud and any changes you make to custom tables needs to be done for each of them.

To connect, simply go choose Connect Database Engine and copy paste the values from the Connection Details page on Umbraco Cloud where you'll find handy copy-short cut buttons to the right of each value. In the "Connect to Server" dialog in SQL Management Studio, choose "SQL Server Authentication" as the authentication type and also remember to click the "Options" button *before you connect* and paste the name of your database in the "Database" input field (if you don't security settings on Umbraco Cloud will prevent you from connecting). You can see it all in this short gif:
![Example video on connecting to the database with SQL Management Studio](images/sqlmanagementstudio.gif)

## Moving on
Now that you've connected you can work with the databases on Umbraco Cloud, like you could on any other host. Just remember to let Umbraco Cloud do the work when it comes to the Umbraco related tables (Umbraco* and CMS* tables).
