# Application startup events & event registration#

In order to bind to certain events in the Umbraco application you need to make these registrations during application startup. 

## Use ApplicationEventHandler to register events ##

**Applies to: Umbraco 6.1.0+**

_if you are using a version previous to 6.1.0, see [here](../Events/application-startup.md) for application event details_

The ApplicationEventHandler is an easy way to hook in to the Umbraco application startup process. It is a base class so all you need to do is override the methods that you wish to handle. It is important to know the difference between each of the methods (information is below). Almost always, you just want to use the __ApplicationStarted__ method.

This example will populate some default data for newly created content items:

    using Umbraco.Core;
    using Umbraco.Core.Events;
    using Umbraco.Core.Logging;
    using Umbraco.Core.Models;
    using Umbraco.Core.Services;

    namespace MyProject.EventHandlers
    {
        public class RegisterEvents : ApplicationEventHandler
        {
            protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
            {
                //Listen for when content is being saved
                ContentService.Saving += ContentService_Saving;     
            }
            
            /// <summary>
            /// Listen for when content is being saved, check if it is a new item and fill in some
            /// default data.
            /// </summary>
            private void ContentService_Saving(IContentService sender, SaveEventArgs<IContent> e)
            {                
                foreach (var content in e.SavedEntities
                    //Check if the content item type has a specific alias
                    .Where(c => c.Alias.InvariantEquals("MyContentType"))
                    //Check if it is a new item
                    .Where(c => c.IsNewEntity()))
                {
                    //check if the item has a property called 'richText'
                    if (content.HasProperty("richText"))
                    {
                        //get the rich text value
                        var val = c.GetValue<string>("richText");
                        
                        //if there is a rich text value, set a default value in a 
                        // field called 'excerpt' that is the first
                        // 200 characters of the rich text value
                        c.SetValue("excerpt", val == null
                            ? string.Empty 
                            : string.Join("", val.StripHtml().StripNewLines().Take(200)));
                    }
                }
            }
        }
    }

## Startup methods

These methods only execute if the application is installed and the database is ready. This will prevent many errors from occurring especially if Umbraco is not installed yet.

It's important to understand the difference between the methods that you can use! The methods that can be used/overridden are:

* ApplicationInitialized (EXPERT)
	* Executes after the `ApplicationContext` and plugin resolvers are created
	* This could be used to create your own plugin resolvers, otherwise it should not be used
	* Never execute any logic or access any Umbraco services in this method
	* _Executes 1st_
* ApplicationStarting (EXPERT)
	* Executes before resolution is frozen so that you are able to modify any plugin resolvers
	* This could be used to modify any plugin resolvers (to add/remove/filter them) - this is the generally the only reason to use this method
	* Never execute any logic or access any Umbraco services in this method
	* _Executes 2nd_
* ApplicationStarted
	* __This will be the most common method to put logic in__
	* Executes when the Umbraco boot sequence is complete, after resolution is frozen so you can retrieve objects from the plugin resolvers. 
	* You could access the Umbraco services in this method if you needed to
	* This executes __before__ ASP.NET has booted, before the global.asax Init method has executed and before any HttpModules have been initialized
	* _Executes 3rd_

If you want more control over execution you can override these properties:

* ExecuteWhenApplicationNotConfigured
	* By default this is false but if you want these methods to fire even when the application is not configured you can override this property and return true
* ExecuteWhenDatabaseNotConfigured
	* By default this is false but if you want these methods to fire even if the database is not installed/ready then you can overrride this property and return true

### IBootManager (EXPERT)

In some cases you may be using a custom `IBootManager` which has the following methods: `Initialize`, `Startup`, `Complete`, this sequence of events and the logic that should be performed in these methods is exactly the same as the methods mentioned above in this order: 
* `Initialize` --> `ApplicationInitialized`
* `Startup` --> `ApplicationStarting`
* `Complete` --> `ApplicationStarted`

## Binding to HttpApplication events

Umbraco allows you to bind directly to HttpApplication events which is very handy since normally you would require an HttpModule to bind to these types of events. The HttpApplication events are listed here: [http://msdn.microsoft.com/en-us/library/system.web.httpapplication_events(v=vs.110).aspx](http://msdn.microsoft.com/en-us/library/system.web.httpapplication_events(v=vs.110).aspx)

In order to bind to these events you need to first listen to the `UmbracoApplicationBase.ApplicationInit` event. Here is an example:

    using Umbraco.Core;
    using Umbraco.Core.Events;
    using Umbraco.Core.Logging;
    using Umbraco.Core.Models;
    using Umbraco.Core.Services;

    namespace MyProject.EventHandlers
    {
        public class RegisterEvents : ApplicationEventHandler
        {
            protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
            {
                //Listen for the ApplicationInit event which then allows us to bind to the
                //HttpApplication events.
                UmbracoApplicationBase.ApplicationInit += UmbracoApplicationBase_ApplicationInit;     
            }
            
            /// <summary>
            /// Bind to the events of the HttpApplication
            /// </summary>
            void UmbracoApplicationBase_ApplicationInit(object sender, EventArgs e)
            {
                var app = (HttpApplication) sender;
                app.PostRequestHandlerExecute += UmbracoApplication_PostRequestHandlerExecute;
            }

            /// <summary>
            /// At the end of a handled request do something... 
            /// </summary>            
            void UmbracoApplication_PostRequestHandlerExecute(object sender, EventArgs e)
            {
                //Do something...
            }
        }
    }
