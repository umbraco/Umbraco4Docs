# Frequently asked questions

## General

### Can I try before I pay?

Yes, you can [take a free trial of Umbraco Cloud](https://umbraco.com/campaigns/try-umbraco-today/) and test it for 14 days with no obligation to buy.

### Is it a special version of Umbraco that’s used?

No. It's the same as the latest version of Umbraco that you can download.

### Can I run my high traffic site on Umbraco Cloud?

Currently we have benchmarked a "well built" site with approximately 50,000 unique visitors per day (~1.5 million per month) that performs very well. For business critical, high traffic sites, we recommend that you look into Umbraco Cloud Professional and Umbraco Cloud Enterprise possibly in combination with a dedicated server.

### Can my site auto-scale or use dedicated worker resources?

Your site can't currently auto-scale, but it is something we’re investigating as a future feature. We do offer dedicated worker resources. [Reach out to us if you want to know more](https://umbraco.com/contact-us/).

### Can I setup a load balanced Umbraco Cloud site?

Not currently.

### Can I move my site from Umbraco Cloud?

Of course. Umbraco Cloud uses the very same Umbraco version that you can download and use on your own. So if you decide that Umbraco Cloud isn’t right for you or your sites then you can clone your site, restore your data locally, and delete your Umbraco Cloud project. We’ll be sad to see you go, but we understand there is a huge variety in requirements so we support and encourage you to choose the best solution for your specific site needs.

### Can I move my existing site to Umbraco Cloud?

Umbraco Cloud is best when used as the base for a new project. There is a specific way of working with Umbraco and Umbraco Cloud in order to take full-advantage of the service. That’s not to say you can’t migrate an existing site, only that some changes may be required in order for your site to fully work with Umbraco Cloud. For more information [read our guide to moving an existing site](https://our.umbraco.org/documentation/Umbraco-Cloud/Deployment/Migrate-Existing-Site/).

---

## Technology

### On what kind of server environment does my Cloud site run?

All of our infrastructure is based on Windows Azure virtual machines.

Currently all webservers run on Windows Server 2012 R2. Each Cloud site runs on a standard IIS version 8.5 instance. 

All databases always run on the latest version of SQL Azure Server.

### Can I choose which Azure Region my projects run in?

No. All services currently run in the Azure West Europe region.

### How many resources do I have available for my website?

Each site runs in an isolated environment next to other websites on the same server, which is a shared environment. This means that we don't have exact details about the amount of resources you site can use, this is managed automatically by our infrastructure.

We do have some limitations:

- If your Cloud site is using over 90% CPU for more than 10 minutes, the priority for your CPU usage will be throttled down for each time you consecutively use more than 90% CPU per 10 minutes
- Memory usages is limited to 2048 MB per Cloud site and when that limit is reached, your website will be restarted automatically to make sure sites with memory leaks don't take up all of the available memory on the server
- There's a limitation of 20 domain names that you can point to 
one Umbraco Cloud site - make sure to contact us if you need more than that

In our experience there is only a few Cloud sites that have experienced these limitations and we're happy to work with people who have sites affected by these limitations.

### Can I use Cloudflare in front of my Umbraco Cloud site

Yes. Point the DNS for your domain(s) to Cloudflare and tell Cloudflare about the IP address of your Umbraco Cloud site to use Cloudflare's full feature set.

---

## Upgrades

### When does Umbraco get upgraded in the various projects?

We upgrade when we're very confident the release is solid.

### How do Automated Upgrades work?

We automatically upgrade to the latest patch version of Umbraco CMS (such as 7.4.x). For minor version upgrades, you’ll get a button in the interface to decide if you want to move to that version (such as 7.5.0) when it is released. When we make a new patch version, we first run it through our test suite, then test it on 10 test-sites (which are copies of "real" customer sites that we’ve been given permission to use). When all that passes, we roll out the upgrade in batches of 100 to customer accounts.

[Read more about upgrades](https://our.umbraco.org/documentation/Umbraco-Cloud/Upgrades/)

### My project didn't receive the auto-upgrade. Why?

When we roll out auto-upgrades to Umbraco Cloud projects the very first thing that happens is a check of all environments on a project. This check will verify whether the environments are responding and doesn't return an HTTP status error. If the auto-upgrader encounters HTTP status errors on any of the environments during this check, the upgrade process is aborted, and your project will not receive the upgrade.

Another reason why your project wasn't auto-upgraded could be, that it failed the test we perform after applying the auto-upgrade. This test compares the state of an environment from before the upgrade with the state of the same environment after the upgrade - if they do not match, we take the appropriate measures to rollback the environment to its previous state and abort the upgrade of any remaining environments.

Other reasons why you didn't receive the auto-upgrade:

 - If you are doing a deployment at the time we tried to run the auto-upgrader on your project
 - If your environments aren't running the same minor version - e.g. if you are in the middle of upgrading to a new minor version, and one environment is running 7.6.x while another environment on the same project is running 7.7.x.

You can find all the steps of the auto-upgrade process outlined in the [Upgrades](https://our.umbraco.org/documentation/Umbraco-Cloud/Upgrades/#the-process-of-auto-upgrading-an-umbraco-cloud-project) article.

### Does leaving pending commits (dev to live) derail the upgrade process?

Pending commits won't stop the auto-upgrade.

### Is it OK to do manual updates? For example if a project on 7.4.3 is updated locally to 7.4.4, can we commit back to dev?

Yes, that’s fine. In some cases you may want to upgrade sooner than the scheduled service upgrade or you may have a site we couldn't upgrade automatically for one reason or another.

Do note, however that you will need to step through the upgrade installer manually on each environment, including live. Our automated upgrader makes sure that visitors to your live site will not be prompted to log in to the upgrade installer.


### I have customized files that Umbraco ships with, will they be overwritten during upgrades?

You will have to assume that every time we upgrade your site, any file that comes with Umbraco by default will be overwritten. Generally we only overwrite the files that have been changed in the newest release but there is no guarantee for that. So if you (for example) have customized the login page then you can assume it will be reverted on each upgrade. 

As a workaround you could have an [ApplicationEventHandler](https://our.umbraco.org/Documentation/Reference/Events/Application-Startup#use-applicationeventhandler-to-register-events) in which you check if the file is different from your customized file and overwrite it again. Note that this is NOT possible if you customize any of the Umbraco DLL files.

---

## Testing

### Are we allowed to perform penetration tests on our Umbraco Cloud site?

Yes, we're happy for people to do penetration testing on the sites they have built on Cloud. We do ask you to please tell us about these tests beforehand so our support staff knows to look out for possible strange things happening on your site.

We are also happy to receive any test results you receive, so that we can improve security in Umbraco where necessary.

Please contact us using the chat button at the bottom right corner of the [Umbraco Cloud portal](https://www.s1.umbraco.io/). 

### Are we allowed to do (D)DOS testing on our Umbraco Cloud site?

It is strictly forbidden to attempt to do a denial of service attack on your Cloud sites.

### Are we allowed to do load testing on our Umbraco Cloud site?

We would like to talk to you beforehand about your test plan for a load test on your Cloud site.

Please contact us using the chat button at the bottom right corner of the [Umbraco Cloud portal](https://www.s1.umbraco.io/). 

---

## Security and encryption

### Does Umbraco Cloud support Let's Encrypt certificates?

Yes, any certificate that can be converted into a `pfx` file is supported on  Cloud.

However, we currently don't have an automated way to generate a Let's Encrypt certificate for you and bind it to your site. This means that you'll be responsible for generating a certificate every 90 days (the maximum validity period of a Let's Encrypt certificate) and updating your site with the new certificate.

Since it is easy to forget this, we recommend you use certificate monitoring tools on your site to remind you to do this.

### Does Umbraco Cloud support http/2?

The lowest version of IIS to support http/2 is version 10, which runs only on Windows Server 2016. Currently our infrastructure is limited to Windows Server 2012 R2 instances and as such we do not support http/2 directly.

As a workaround, you could consider setting up a product like CloudFlare, which offers free support for http/2 (they call it "Opportunistic Encryption") out of the box.

### There's a ARRAffinity cookie on my site which is not sent over HTTPS, is this a security risk?

No this is not a security risk. This cookie is set by the load balancer (LB) and only used by the LB to track which server your site is on. It is set by the software we use (Azure Pack) and only useful when your website is being scaled to multiple servers. In Umbraco Cloud we cannot scale your site to multiple servers so the cookie is effectively unused.

There is no vulnerable data in this cookie and manipulating or stealing this cookie can not lead to any security issues.

In the future, the cookie will be set to `HttpOnly` on Umbraco Cloud to conform to best practices. This does not mean that there's anything wrong with the current way it is set.

For more information see [the related github issue](https://github.com/Azure/app-service-announcements/issues/12).

### Can I use wildcard certificates on Umbraco Cloud? How about an EV, DV or OV certificate?

Yes. You can use any valid certificate on Umbraco Cloud.

### I get a warning that "your connection is not private" and the certificate is served for *.umbraco.io

It seems that you didn't set up the bindings for the specific domain where this warning is showing. Check the bindings by going to the site in [the portal](https://www.s1.umbraco.io) by going to the "Manage hostnames" section for your site.

### How can I control who accesses my backoffice using IP filtering?

On Cloud it is easy to add an IP filter of your choosing, there's a few things you need to pay attention to though: Umbraco Deploy will need to be able to talk to the different environments in your Cloud website (development, staging, live) and you should of course still be able to use the site locally.

The following rule can be added to your web.config (in `system.webServer/rewrite/rules/`):

    <rule name="Backoffice IP Filter" enabled="true">
        <match url="(^umbraco/backoffice/(.*)|^umbraco($|/$))"/>
        <conditions logicalGrouping="MatchAll">

            <!-- Umbraco Cloud to Cloud connections should be allowed -->
            <add input="{REMOTE_ADDR}" pattern="52.166.147.129" negate="true" />
            <add input="{REMOTE_ADDR}" pattern="13.95.93.29" negate="true" />
            <add input="{REMOTE_ADDR}" pattern="40.68.36.142" negate="true" />
            <add input="{REMOTE_ADDR}" pattern="13.94.247.45" negate="true" />
            
            <!-- Don't apply rules on localhost so your local environment still works -->
            <add input="{HTTP_HOST}" pattern="localhost" negate="true" />

            <!-- Add other client IPs that need access to the backoffice -->
            <add input="{REMOTE_ADDR}" pattern="123.123.123.123" negate="true" />

        </conditions>
        <action type="CustomResponse" statusCode="403"/>
    </rule> 

What we're doing here is blocking all the requests to `umbraco/backoffice/` and all of the routes that start with this. 

All of the Umbraco APIs use this route as a prefix, including Umbraco Deploy. So what we need to do first is to allow Umbraco Cloud to still be allowed to access the Deploy endpoints. That is achieved with the first 4 IP addresses, they're specific to the servers we use on Cloud.

You will notice that the regex `^umbraco/backoffice/(.*)|^umbraco` also stops people from going to `yoursite.com/umbraco`, so even the login screen will not show up. Even if you remove the `|^umbraco` part in the end, it should be no problem. You'll get a login screen but any login attempts will be blocked before they reach Umbraco (because the login posts to `umbraco/backoffice/UmbracoApi/Authentication/PostLogin`).

Then the last IP address is just an example, you can add the addresses that your organization uses as new items to this list.

*Note*: It is possible to change the `umbraco/` route so if you've done that then you need to use the correct prefix. Doing this on Cloud is untested and at the moment not supported. 

### Does Umbraco Cloud use Transparent Data Encryption (TDE) for databases?

Yes, every site created after May 2nd 2017 will have TDE enabled by default. For older sites we can enable this by request.

---

## Building and deploying

### Umbraco Cloud creates a SQL CE / LocalDb database for me, can I use a shared SQL Server for my development team instead?

No, you should not use a shared database for your team. Umbraco Cloud is made so that each team member can safely make any changes they need and then send them to your development environment on Cloud. Another developer can do the same and also send their changes to dev to test. Once you're happy with all of the changes, each developer can pull down the changes from development and continue working on the next change.

Not only does this promote working in small increments it also prevents two problems:

 1. If you share a database between multiple developers, [Umbraco's flexible load balancing](https://our.umbraco.org/Documentation/Getting-Started/Setup/Server-Setup/Load-Balancing/flexible) automatically kicks in. Without a proper load balancing setup this means that often you will not see changes another team member has made, potentially overwriting their changes with your own changes.
 2. Our deployment engine (Umbraco Deploy) is not made for this and your local site will quickly get out of sync with changes both developers are making. Once you push your changes up to your Cloud instance you can expect to see errors and mismatches because changes have not been saved correctly.

### Can I use custom .NET code?

Yes, you can make your Umbraco implementations just as you're used to, including custom .NET assemblies.

Umbraco Cloud sites run on IIS 8.5 so most things you can normally do on IIS, you can do on Umbraco Cloud. We don't, however, offer support for custom components that have to be installed on the server itself. If you can ship it in the bin folder, it should generally work on Cloud.

If you have any experience with Azure Web Apps, Cloud works in the same way. So if you can make it run on Azure Web Apps, you can make it run on Umbraco Cloud.

### Is it possible to add my own custom DLLs for extending the Umbraco Backoffice?

Yes, an Umbraco Cloud project is basically a normal Umbraco website where we give you multiple environments and easy deployment of code and content between these environments. It's really easy to get the site running locally (via git) which is the best way to add your own code (templates, cs files, packages, DLLs and so forth).

### Is it possible to add custom tables in addition to the Umbraco Cloud database?

Yes, you can create custom tables in the database. Simply navigate to the connection strings to the databases of the different environments under the menu item "Connection details" in the "Settings" menu.

Note that custom database tables and data do not replicate automatically across Cloud environments. You might want to use Umbraco Migrations and our PetaPoco datalayer to make deployment of your custom data more automated.

### I would love to use Websockets on my site, is this possible?

Yes it is! Websockets are enabled on all sites.

---

## Package support

### Do you support package "x" on Umbraco Cloud?

We have an indicator on each package in [the projects section of Our Umbraco](https://our.umbraco.org/projects/) which either says "Works on Umbraco Cloud" or "Untested or doesn't work on Umbraco Cloud".

If the indicator says "Works on Umbraco Cloud" it means that Umbraco HQ has tested this package on Cloud and it works and changes made using this package are also deployable to the next environment.

If the indicator says "Untested or doesn't work on Umbraco Cloud" then we have not tested it and cannot vouch for it on Cloud. It might work, and we're happy for you to test it on Cloud. 

We're happy to hear from and work with package developers to make their packages Cloud compatible where possible. Make sure to reach out to us using the chat button at the bottom right corner of the [Umbraco Cloud portal](https://www.s1.umbraco.io/). 

### How do I make my package support Umbraco Cloud?

The biggest problem concerning Cloud support is when a package stores references to nodes, media items, or members in Umbraco.

There are two challenges here: 

 1. Your package is referring to an integer identifier, for example a content item with id `1023`. On the next environment that same content item exists but since the content is a bit different there, the id is `1039` instead. Umbraco Deploy needs to know how to connect the correct identifier.
 2. Even if the identifier is correct on both environments your package might rely on the other item (the one you're referring to) to exist in the next environment. So if the content item you're referring to (`1023`) does not exist on the environment where you're pushing the content to you might see errors in your package.

These problems can be solved with so-called Umbraco Deploy connectors. We've set up a project called [Umbraco Deploy Contrib](https://github.com/umbraco/Umbraco.Deploy.Contrib/) to collect these connectors together. Umbraco Deploy Contrib is included in all Cloud sites and we keep it upgraded to the latest version for every site. 

The code in the contrib project has plenty of code comments to help you understand what is going on and how you can build something like that for your own package.

If you need help with this, don't hesitate to reach out to us and we'll be happy to give you some tips.

---

## Backups and data retention

### What backup and restore options are available on Umbraco Cloud?

Database backups are not available as downloads by default, but a copy can be downloaded using a simple Powershell script (Umbraco Cloud support can provide you with instructions). By default 14 days point in time restore is available. Restore is dependent on your needs, requirements and database size and will be handled on a case by case basis. Contact Umbraco Cloud support through the portal to discuss your requirements. 

