#Using MVC Partial Views in Umbraco

**Applies to: Umbraco 4.10.0+**

_This section will show you how to use MVC Partial Views in Umbraco. Please note, this is documentation relating to the use of native MVC partial views, not the soon to come 'Partial View Macros'_ 

##Overview

Using Partial Views in Umbraco is exactly the same as using Partial Views in a normal MVC project. There is detailed documentation on the Internet about creating and using MVC partial views, for example:

* [http://www.asp.net/mvc/videos/mvc-2/how-do-i/how-do-i-work-with-data-in-aspnet-mvc-partial-views](http://www.asp.net/mvc/videos/mvc-2/how-do-i/how-do-i-work-with-data-in-aspnet-mvc-partial-views)

##View Locations

The locations to store Partial Views when rendering in the Umbraco pipeline is:

	~/Views/Partials

The standard MVC partial view locations will also work:

	~/Views/Shared
	~/Views/RenderMvc

The ~/Views/RenderMvc location is valid because the controller that performs the rendering in the Umbraco codebase is the: `Umbraco.Web.Mvc.RenderMvcController`

If however you are 'Hijacking an Umbraco route' and specifying your own controller to do the execution, then your partial view location can also be:

	~/Views/{YourControllerName}

##Example

A quick example of a content item that has a template that renders out a partial view template for each of it's child documents:

The MVC template markup for the document:

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
	    Layout = null;
	}
	
	<html>
	<body>
		
		@foreach(var page in Model.Content.Children.Where(x => x.IsVisible())){
			<div>
				@Html.Partial("ChildItem", page)
			</div>
		}
		
	</body>	
	</html>

The partial view (located at: `~/Views/Partials/ChildItem.cshtml`)

	@model IPublishedContent
	
	<strong>@Model.Name</strong>

#Strongly typed Partial Views

Normally you would create a partial view by simply using the `@model MyModel` syntax. However, inside of Umbraco you will probably want to have access to the handy properties available on your normal Umbraco views like the Umbraco helper: `@Umbraco` and the Umbraco context: `@UmbracoContext`. The good news is that this is completely possible, instead of using the `@model MyModel` syntax, you just need to inherit from the correct view class so do this instead:

	@inherits Umbraco.Web.Mvc.UmbracoViewPage<MyModel>

By inheriting from this view, you'll have instant access to those handy properties.

##Caching

Normally you don't really need to cache the output of Partial views just like normally you don't need to cache the output of User Controls, however there are times when this is necessary. Just like macro caching we provide caching output of partial views. This is done simply by using an HtmlHelper extension method:

	@Html.CachedPartial("MyPartialName", new MyModel(), 3600)

the above will cache the output of your partial view for 1 hr (3600 seconds). Additionally there are a few optional parameters you can specify to this method. Here is the full method signature:

	IHtmlString CachedPartial(
		string partialViewName, 
		object model, 
		int cachedSeconds,
		bool cacheByPage = false,
		bool cacheByMember = false,
		ViewDataDictionary viewData = null)

So you can specify to cache by member and/or by page and also specify additional view data to your partial view. **HOWEVER**, if your view data is dynamic (meaning it could change per page request) the cached output will still be returned, this same principle goes for if the model you are passing in is dynamic. Please be aware of this as if you have a different model or viewData for any page request, the result will be the cached result of the first execution.