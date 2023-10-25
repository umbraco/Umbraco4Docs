---
description: >-
  Learn how to use Azure Active Directory credentials to login to Umbraco as a
  member.
---

# Add Azure Active Directory authentication (Members)

This tutorial takes you through configuring Azure Active Directory (Azure AD) for the member login on your Umbraco CMS website.

{% hint style="warning" %}
Azure AD conflicts with Umbraco ID which is the main authentication method used on all Umbraco Cloud projects.

Due to this, we **highly recommend not using Azure AD for backoffice authentication on your Umbraco Cloud projects**.

It is still possible to use other [External Login Providers](../reference/security/external-login-providers.md) like Google Auth and OpenIdConnect, with Umbraco Cloud.
{% endhint %}

## Prerequisites

* A project with a setup for Members.
* Visual Studio, or another Integrated Development Environment (IDE).

## Step 1: Configure Azure AD

Before your applications can interact with Azure AD B2C, they must be registered with a tenant that you manage. For more information, see [Microsoft's Tutorial: Create an Azure Active Directory B2C tenant](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant).

## Step 2: Install the NuGet package

You need to install the `Microsoft.AspNetCore.Authentication.MicrosoftAccount` NuGet package. There are two approaches to installing the packages:

1. Use your favorite Integrated Development Environment (IDE) and open up the **NuGet Package Manager** to search and install the packages.
2. Use the command line to install the package.

## Step 3: Implement the Azure AD Authentication

1. Create a new class for custom configuration options: `AzureB2CMembersExternalLoginProviderOptions.cs`.

{% code title="AzureB2CMembersExternalLoginProviderOptions.cs" lineNumbers="true" %}
```csharp
using Microsoft.Extensions.Options;
using Umbraco.Cms.Core;
using Umbraco.Cms.Web.Common.Security;

namespace MyApp
{
    public class AzureB2CMembersExternalLoginProviderOptions : IConfigureNamedOptions<MemberExternalLoginProviderOptions>
    {
        public const string SchemeName = "ActiveDirectoryB2C";¨

        public void Configure(string? name, MemberExternalLoginProviderOptions options)
        {
            if (name != Constants.Security.MemberExternalAuthenticationTypePrefix + SchemeName)
            {
                return;
            }

            Configure(options);
        }

        public void Configure(MemberExternalLoginProviderOptions options)
        {
            // The following options are relevant if you
            // want to configure auto-linking on the authentication.
            options.AutoLinkOptions = new MemberExternalSignInAutoLinkOptions(

                // Set to true to enable auto-linking
                autoLinkExternalAccount: true,

                // [OPTIONAL]
                // Default: The culture specified in appsettings.json.
                // Specify the default culture to create the Member as.
                // It can be dynamically assigned in the OnAutoLinking callback.
                defaultCulture: null,

                // [OPTIONAL]
                // Specify the default "IsApproved" status.
                // Must be true for auto-linking.
                defaultIsApproved: true,

                // [OPTIONAL]
                // Default: "Member"
                // Specify the Member Type alias.
                defaultMemberTypeAlias: Constants.Security.DefaultMemberTypeAlias

            )
            {
                // [OPTIONAL] Callbacks
                OnAutoLinking = (autoLinkUser, loginInfo) =>
                {
                    // Customize the Member before it's linked.
                    // Modify the Members groups based on the Claims returned
                    // in the external login info.
                },
                OnExternalLogin = (user, loginInfo) =>
                {
                    // Customize the Member before it is saved whenever they have
                    // logged in with the external provider.
                    // Sync the Members name based on the Claims returned
                    // in the external login info

                    // Returns a boolean indicating if sign-in should continue or not.
                    return true;
                }
            };
        }
    }
}
```
{% endcode %}

2. Create a new static extension class called `MemberAuthenticationExtensions.cs`.

{% code title="MemberAuthenticationExtensions.cs" lineNumbers="true" %}
```csharp
namespace MyApp
{
    public static class MemberAuthenticationExtensions
    {
        public static IUmbracoBuilder ConfigureAuthenticationMembers(this IUmbracoBuilder builder)
        {
            builder.Services.ConfigureOptions<AzureB2CMembersExternalLoginProviderOptions>();
            builder.AddMemberExternalLogins(logins =>
            {
                logins.AddMemberLogin(
                    membersAuthenticationBuilder =>
                    {
                        membersAuthenticationBuilder.AddMicrosoftAccount(

                            // The scheme must be set with this method to work for the external login.
                            membersAuthenticationBuilder.SchemeForMembers(AzureB2CMembersExternalLoginProviderOptions.SchemeName),
                            options =>
                            {
                                // Callbackpath: Represents the URL to which the browser should be redirected to.
                                // The default value is /signin-oidc.
                                // This needs to be unique.
                                options.CallbackPath = "/umbraco-b2c-members-signin";

                                //Obtained from the AZURE AD B2C WEB APP
                                options.ClientId = "YOURCLIENTID";
                                //Obtained from the AZURE AD B2C WEB APP
                                options.ClientSecret = "YOURCLIENTSECRET"; 

                                options.SaveTokens = true;
                            });
                    });
            });
            return builder;
        }
    }
}
```
{% endcode %}

{% hint style="info" %}
Ensure to replace `YOURCLIENTID` and `YOURCLIENTSECRET` in the code with the values from the Azure AD tenant.
{% endhint %}

4. Add the Members authentication configuration to the `ConfigureServices` method in the `Startup.cs` file:

{% code title="Startup.cs" lineNumbers="true" %}
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddUmbraco(_env, _config)
        .AddBackOffice()
        .AddWebsite()
        .AddComposers()

        //Add Members ConfigureAuthentication
        .ConfigureAuthenticationMembers()

        .Build();
}
```
{% endcode %}

5. Build the project.
6. Run the website.

![AD Login Screen](<../../../10/umbraco-cms/reference/security/images/AD\_Login\_Members (1).png>)
