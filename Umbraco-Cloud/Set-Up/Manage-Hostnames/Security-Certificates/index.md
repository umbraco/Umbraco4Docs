---
versionFrom: 7.0.0
---

# Upload custom certificates manually

:::note
This feature is only available for Umbraco Cloud projects on a Professional or Enterprise plan.

All projects on a Starter, Standard or Pro plans will automatically be assigned a TLS (HTTPS) certificate.

See the full list of features including in the [Umbraco Cloud pricing plans on Umbraco.com](https://umbraco.com/umbraco-cloud-pricing/).
:::

Under **Certificates** you'll find an option to manually upload your own certificate and assign it to one of the hostnames you've added.

Your certificates need to be:

1. **`.pfx`** format.

2. Must be set to use a password.

3. Can't be expired.

4. Signature algorithm must be SHA256WithRSA, SHA1WithRSA or ECDSAWithSHA256

The **`.pfx`** file can only contain one certificate. Each certificate can then be bound to a hostname you have already added to your site. Make sure you use the hostname you will bind the certificate to as the common name (CN) when generating the certificate.

![Custom Certificates](images/Manual-certificate.png)

## 1. Upload certificate

* Upload your certificate from your local machine
* Type in the password for your certificate
* Click **Add**

## 2. Bind certificate to hostname

* Click **Add new binding**
* Choose your hostname from the *Hostname* dropdown
* Choose your newly uploaded certificate from the *Certificate* dropdown
* Click **Add**

You've now successfully added your certificate to the Cloud project!

## From custom certificate to automatic TLS (HTTPS)

In some cases, you might want to switch from using your own custom certificate, to use the ones provided by the Umbraco Cloud service.

By removing your own certificate from your Cloud project, the Umbraco Cloud service will automatically assign a new TLS (HTTPS) certificate to the hostname that was using the removed certificate.

:::note
Did your manually uploaded security certificate expire?

You will need to remove the expired certificate in order for Umbraco Cloud to assign a new certificate to your hostname(s).
:::

## Read more

* [Redirect from HTTP to HTTPS](../Rewrites-on-Cloud#running-your-site-on-https-only)
