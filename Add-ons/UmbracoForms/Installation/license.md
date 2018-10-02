# Umbraco Forms licenses

Umbraco Forms is a commercial product. When you install from the backoffice you will have a 14-day free trial to try-out the product.

After that, you'll need to have a **valid license** in order for Umbraco Forms to fully work.

## Licensing model

Umbraco Forms Licenses are sold per domain and will also work on all subdomains. You'll also get 2 development domains.
A fully configured license could like this:

- localhost
- mysite.com
- *.mysite.com
- mysite.com.local
- devdomain1.com
- www.devdomain1.com
- devdomain2.com
- www.devdomain2.com

Only 1 license can exist per Umbraco instance; If you wish to extend a license with additional domains that option is also possible.

### What does a license cover?

* A single license covers a single installation of Umbraco Forms in production environments, as well as two additional development environments
* The production domain includes all subdomains (e.g. `*.domain.tld`), as well as `.local` extension (e.g. `domain.tld.local`)
* The development domains works with or without the `www` subdomain
* The license allows for an unlimited number of forms
* The license also will include `localhost` as a valid domain

Data collected on non-licensed domains will not be stored, so please validate that all site-accessible domains are licensed, or that they properly redirect to the licensed domain.
Usage of IP addresses is not available, thus hostnames must be used.

### Can I add additional domains to the license?

Yes, additional domains can be added for EUR 94. You can purchase additional domains to your license from your account on [Umbraco.com](https://umbraco.com).

**Note** that this is an add-on domain for existing licenses. Refunds will not be given for this product.

It is however only meant for websites which has several domains. If you wish to use Umbraco Forms on multiple websites you need a license per site.

## Configuring and installing your license

Licenses for Umbraco Forms can be purchased on [umbraco.com](https://umbraco.com/products/umbraco-forms/). 

Clicking the *Buy license* button from the Umbraco backoffice will redirect you to our website where you can buy an Umbraco Forms license. 

### Configuring

When you've bought an Umbraco Forms license you need to configure it. Your license can be configured with a total of 1 live domain and 2 development domains.

You can either configure your license right away when you buy it, or you can do it later by visiting your account on Umbraco.com.

![Configuring Umbraco Forms license](configure-forms-license.gif)

### Installing

Once you've configured your license with the correct domains, you are ready to install the license on your Umbraco site:

1. Download your `.lic` license from your Umbraco.com account
2. Place the file in your `/bin` folder

The `.lic` file must be placed in the `/bin` directory to be registered by Umbraco Forms, if not, the application will automatically switch to trial mode.

![Installing Umbraco Forms license](install-forms-license.gif)
