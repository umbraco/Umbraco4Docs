---
versionFrom: 7.0.0
versionTo: 10.0.0
---

# Usage on your Umbraco Cloud project

In the Umbraco Cloud Settings menu you can find a page called _Usage_.

On the Usage page, you will find an usage overview that displays your usage and evalues it against the plan limitations of your project.
On the page you will also find top 10 for the bandwidth usage of your project that can give you important insight into where you can optimally optimize resource management.

## Usage overview

The usage overview shows the bandwidth usage of the project this month, the size of the media library and the number of custom domains added to the project.
In this overview you will also find the usage limitations for your Umbraco Cloud project as well as the plan that the project is on.

![Usage on Cloud](images/Usage.png)

The usage shown is for the Live environment of your project as it is the usage in this environment that is measured against the plan usage limits.
For _media storage_ it is the size of all files in the blob storage including the cache that is considered.

## Bandwidth Top 10's

You will find a couple of top 10 for the bandwidth in the project's live environment.

### Top 10 - Bandwidth Usage Paths

The first is displaying the 10 resources that are contributing the most to the total bandwidth of your project. Each resource is represented by its path together with the number of requests and its total contribution of bandwidth.
![top 10 bandwidth](images/Top10BandwidthPaths2.png)

### Top 10 - Bandwidth Usage Referers

The second you will find, is a top 10 of the HTTP referers causing the most bandwidth. A referer is the name of an optional HTTP header field that identifies the address of the web page, from which the resource has been requested. It is the bandwidth generated from these resource requests that counts in the monthly usage limit of the project.

![top 10 bandwidth](images/Top10BandwidthReferer2.png)

## Top 50 - Media Files

It is also possible to see the top 50 media files on your live environment.

The list shows the name of the file, its path, size, and type (whether it is a jpeg or a png file).

![top 50 media files](images/Top-50-media.png)

## Usage limits

In the below image, you can see the Usage limitatons for the specific plans on Umbraco Cloud:

![Usage limits on a starter plan](images/Plan_limits.png)

On Umbraco Cloud, you can always upgrade your project to a higher plan if you have reached the limit of what you are allowed on your project. You can **Upgrade Plan** from the **Settings** drop-down list on your project.

You can see the prices for the different plans on Umbraco Cloud on our [website](https://umbraco.com/umbraco-cloud-pricing/) or when you are upgrading your plan.

:::warning
When one of the limits reaches 90%, you’ll see a warning banner in the portal and an email is sent to the project owner and the technical contact(s) of the project, notifying you that you’re getting close to your limit(s).

![USage Warning](images/warnings_usage.png)
:::
