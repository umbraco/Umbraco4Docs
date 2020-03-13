---
versionFrom: 7.0.0
---

# Manage environments

When working with an Umbraco Cloud project you can add and remove extra environments. If you are on a Pro plan you can add and remove environments whenever you like, for a Starter plan it costs extra for each environment.

<iframe width="800" height="450" src="https://www.youtube.com/embed/9AwZNyaHbVk?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Adding or removing environments

__Important:__ *Before* adding an environment you should consider if you have any changes locally that are not on Live yet. If you do you should make sure to push it as adding another environment will also push it into the deployment chain.

__Important:__ *After* adding a Development environment you need to do a fresh clone of the site. The local version you have already will be set up to push directly to Live, a fresh clone will push to Development.

You can find the interface for adding and removing environments from your project page, it is located here:

![Adding and environments](images/Manage-environments.png)

On that page you will be able to add or remove an environment, if you wish to remove one you will be prompted to type in the environment alias, so either 'Development' or 'Staging'.

![Environment overview](images/Environments.png)

Once you have added or removed the environment it will take a couple of minutes for the Cloud to set it all up, and then you will be ready to use it.
