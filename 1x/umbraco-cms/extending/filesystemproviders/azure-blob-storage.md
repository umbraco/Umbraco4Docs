---
description: Setup your site to use Azure Blob storage for media and ImageSharp cache
---

# Using Azure Blob Storage for Media and ImageSharp Cache

For Umbraco sites, there are some scenarios when you may want or need to consider using Azure Blob Storage for your media, particularly if your site contains large amounts of media. Having your site's media in Azure Blob Storage can also help your deployments complete more quickly, and has the potential to positively affect site performance, as the ImageSharp cache is moved to Azure Blob Storage.

The setup consists of adding a package to your site, setting the correct configuration, and adding the services and middleware. Before you begin you’ll need to create an Azure Storage Account and a container for your media and ImageSharp cache. In this example, we assume your container name is "mysitestorage" and has already been created.

See the [Microsoft documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) for a quickstart guide on how to create a blob storage container.

## Installing the package

Before you begin, you need to install the `Umbraco.StorageProviders.AzureBlob` and the `Umbraco.StorageProviders.AzureBlob.ImageSharp` NuGet packages. There are two approaches to installing the packages:

1. Use your favorite Integrated Development Environment (IDE) and open up the NuGet Package Manager to search and install the packages
2. Use the command line to install the packages

### Installing through command line

Navigate to your project folder, which is the folder that contains your `.csproj` file. Now use the following `dotnet add package` commands to install the packages:

```
dotnet add package Umbraco.StorageProviders.AzureBlob
dotnet add package Umbraco.StorageProviders.AzureBlob.ImageSharp
```

The correct packages will have been installed in your project.

## Configuring Blob storage

The next step is to configure your blob storage. There are multiple approaches for this, but in this document, we're going to do it through `appsettings.json`. For more configuration options, see the [readme](https://github.com/umbraco/Umbraco.StorageProviders#umbracostorageproviders) on the Github repository.

Open up your `appsettings.json` file and add the connection string and container name under `Umbraco:Storage:AzureBlob:Media`. Your Umbraco section of appsettings will look something like this:

```json
  "Umbraco": {
    "Storage": {
      "AzureBlob": {
        "Media": {
          "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<media account name>;AccountKey=<media account key>;EndpointSuffix=core.windows.net",
          "ContainerName": "mysitestorage"
        }
      }
    },
    "CMS": {
      "Hosting": {
        "Debug": false
      },
      {...}
    }
  }
```

Note that in this example, the container name is `mysitestorage`.

{% hint style="info" %}
You can get your connection string from your Azure Portal under "Access Keys".
{% endhint %}

## Setting the services and middleware

You're almost there. The last step is to set up the required services and middleware. This may sound daunting, but thankfully there are extension methods that do all this for you. All you need to do is invoke them in the `ConfigureServices` and `Configure` methods in the `startup.cs` file.

Invoke the `.AddAzureBlobMediaFileSystem()` and the `.AddAzureBlobImageSharpCache()` extension methods in the `ConfigureServices` method:

```
        public void ConfigureServices(IServiceCollection services)
        {
#pragma warning disable IDE0022 // Use expression body for methods
            services.AddUmbraco(_env, _config)
                .AddBackOffice()
                .AddWebsite()
                .AddComposers()
                .AddAzureBlobMediaFileSystem() // This configures the required services for Media
                .AddAzureBlobImageSharpCache() // This configures the required services for the Image Sharp cache
                .Build();
#pragma warning restore IDE0022 // Use expression body for methods

        }
```

{% hint style="info" %}
**Upgrading from Umbraco 9/10**:

As of [version 11.0.0](https://github.com/umbraco/Umbraco.StorageProviders/releases/tag/release-11.0.0) of `Umbraco.StorageProviders`, the ImageSharp dependency has been separated into its own package.

Therefore, if you're planning on upgrading your site from Umbraco 9/10 to 11+, don't forget to install and setup the new `Umbraco.StorageProviders.AzureBlob.ImageSharp` package. This will ensure that your ImageSharp cache continues to be stored in your blob storage container.

{% endhint %}

Now when you launch your site again, the blob storage will be used to store media items as well as the ImageSharp cache. Do note though that the `/media` and `/cache` folders do not get created until a piece of media is uploaded.

## Existing Media files

Any media files you already have on your site will not automatically be added to the blob storage container. You will need to copy the contents on the `/wwwroot/media` folder and upload them to a new folder called `/media` in your blob storage container. Once you've done that, you can safely delete the `wwwroot/media` folder locally, as it is no longer needed.

Any new media files you upload to the site will automatically be added to the Blob Storage.
