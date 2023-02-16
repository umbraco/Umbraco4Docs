---
versionFrom: 9.0.0
---

# The Licensing Model

Umbraco Forms is a commercial product.

For Umbraco Forms, you will have a 14-day free trial to try out the product. After your trial expires, you'll need to have a **valid license** to keep using the product on your site.

## How does it work?

Licenses for our products are sold per domain and will also work on all subdomains. With every license, you will also be able to configure two development/testing domains.

{% hint style="info" %}
The licenses are not bound to a specific product version. They will work for all versions of the related product.
{% endhint %}

### Example

Let's say that you have a license configured for your domain, `mysite.com`, and you've configured two development domains, `devdomain.com` and `devdomain2.com`.

The license will cover the following domains:

* `localhost`
* `*.mysite.com`
* `www.mysite.com`
* `mysite.com.local`
* `devdomain.com`
* `www.devdomain.com`
* `devdomain2.com`
* `www.devdomain2.com`

{% hint style="info" %}
That you can have only 1 license per Umbraco installation.
{% endhint %}

## What does a license cover?

There are a few differences as to what the license covers:

* A single license covers the installation of Umbraco Forms in 1 production domain, as well as in 2 development domains
* The production domain includes **all subdomains** (e.g. `*.mysite.com`), as well as the `.local` extension (e.g. `mysite.com.local`)
* The development domains work with or without the `www` subdomain
* The license allows for an unlimited number of forms
* The license also includes `localhost` as a valid domain

In the case you run multiple sites within one Umbraco installation, e.g. you have multiple domains pointing at the same installation, you have the option to purchase and add **additional domains** to your license.

Additional domains can be purchased from your account on [Umbraco.com](https://umbraco.com). Each additional domain includes 1 live domain and 2 development/testing domains.

{% hint style="info" %}
This is an add-on domain for existing licenses. Refunds will not be given for this product.
{% endhint %}

## Configuring and installing your license

You can purchase the license at [Umbraco Forms license](https://umbraco.com/products/umbraco-forms/).

When you've bought a license you need to configure it with your domains.

You can either configure your license right away, or you can do it later by visiting your account on Umbraco.com.

![Configuring Umbraco Forms license](../images/configure-forms-license.gif)

## Installing your license

Once you've configured your license with the correct domains, you are ready to install the license on your Umbraco installation.

1. Download your license from your Umbraco.com account - this will give you a `.lic` file
2. Place the file in the `/umbraco/Licenses` directory in your Umbraco installation

The `.lic` file must be placed in the `/umbraco/Licenses` directory in order to be registered by Umbraco Deploy or Umbraco Forms. If the file isn't placed correctly, the application will automatically switch to trial mode.

![Installing Umbraco Forms license](../images/install-forms-license-v9.gif)

## Alternative license location

If you can't include the license file in the `/umbraco/Licenses` directory for any reason it is possible to configure an alternative location for the file.

It can be configured in the Umbraco installation's `appSettings.json` file by adding the following AppSetting, where the value contains the path of your custom license directory, relative to the root of your Umbraco installation.

{% hint style="warning" %}
This will also change the location for other Umbraco-related licenses in this project.
{% endhint %}

```json
{
  "Umbraco": {
    "Licensing": {
        "Directory": "~/custom-licenses-folder/"
    }
  }
}
```
