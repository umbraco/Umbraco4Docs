# Content Apps

These are a new concept in v8. 'Content' and 'Info', highlighted in the image below, are the built-in content apps for all items in the Content section. 

![Content Apps in back office](images/content-apps-location.png)

We can also add our own custom content apps to appear alongside the built-in ones. These can be for all content and media items, or they can be dependent on content type.  They can also be dependent on the current user's permissions.

They are intended to enhance the editor's experience by displaying extra information, such as Google Analytics data. not for doing data-entry.

## Creating a Custom Content App

This guide explains how to set up a custom content app in the following steps:

* Adding a content app that appears for all content and media items and displays some information about the current item
* Limiting the content app to appear for only specific content types
* Limiting which user groups can see the content app

A basic understanding of how to use AngularJS with Umbraco is required.  If you have created a property value editor before, this will all feel very familiar.

### Setting up the Plugin

The first thing we do is create a new folder inside `/App_Plugins` folder. We will call it `MyContentApp`

Next we will create a simple manifest file to describe what this content app does. This manifest will tell Umbraco about our new content app and allows us to inject any needed files into the application, so we create the file `/App_Plugins/MyContentApp/package.manifest`

Inside this package manifest we add a bit of JSON to describe the content app. Have a look at the inline comments in the JSON below for details on each bit:

	{
		// define the content apps you want to create
		contentApps: [
            {
                "name": "Cake", // required - the name that appears under the icon, everyone loves cake, right?
                "alias": "appCake", // required - unique alias for your app
                "weight": 0, // optional, default is 0, use values between -99 and +99 to appear between the existing Content (-100) and Info (100) apps
                "icon": "icon-cupcake", // required - the icon to use
                "view": "~/App_Plugins/MyContentApp/mycontentapp.html", // required - the location of the view file
            }
        ]
		,
		// array of files we want to inject into the application on app_start
		javascript: [
		    '~/App_Plugins/MyContentApp/mycontentapp.controller.js'
		]
	}

### Creating the View and the Controller

Then add 2 files to the /app_plugins/MyContentApp/ folder:
- `mycontentapp.html`
- `mycontentapp.controller.js`

These will be our main files for the app, with the .html file handling the view and the .js file handling the functionality.

In the .js file we declare our AngularJS controller and inject umbraco's editorState and userService:

    angular.module("umbraco")
        .controller("My.MyContentApp", function ($scope, editorState, userService) {

            var vm = this;
            vm.CurrentNodeId = editorState.current.id;
            vm.CurrentNodeAlias = editorState.current.contentTypeAlias;

            var user = userService.getCurrentUser().then(function (user) {
                console.log(user);
                vm.UserName = user.name;
            });
        });

And in the .html file:

    <div class="umb-box" ng-controller="My.MyContentApp as vm">
        <div class="umb-box-header">
            <div class="umb-box-header-title">
                Hello
            </div>
        </div>
        <div class="umb-box-content">
            <ul>
                <li>Current node id: <b>{{vm.CurrentNodeId}}</b></li>
                <li>Current node alias: <b>{{vm.CurrentNodeAlias}}</b></li>
                <li>Current user: <b>{{vm.UserName}}</b></li>
            </ul>
        </div>
    </div>

### Checking it works

After the above edits are done, restart your application. Go to any content node and you should now see an app called Cake. Clicking on the icon should say "Hello" and confirm the details of the current item.

### Limiting according Content Type

TODO

### Limiting according to User Permissions

TODO

## Creating a Content App in C#

*Coming soon!*