# IContentFinder

If you want to create your own content finder implement the IContentFinder interface:

	public interface IContentFinder
	{
	  bool TryFindContent(PublishedContentRequest contentRequest);
	}
	
    // Some are registered by default
	// But feel free to use your owns
	public class ContentFinderResolver
	{ … }

Umbraco runs all content finders, stops at the first one that returns true.
Finder can set content, template, redirect…

### Example 

    public class MyContentFinder : IContentFinder
    {
      public bool TryFindContent(PublishedContentRequest request)
      {
        var path = request.Uri.GetAbsolutePathDecoded();
        if (!path.StartsWith("/woot"))
        return false; // not found

        // have we got a node with ID 1234?
        var contentCache = UmbracoContext.Current.ContentCache;
        var content = contentCache.GetById(1234);
        if (content == null) return false; // not found

        // render that node
        request.PublishedContent = content;
        return true;
      }
    }

### Example Default content finder

    public class ContentFinderByNiceUrl : IContentFinder
    {
      public virtual bool TryFindContent(PublishedContentRequest request)
      {
        string path = request.HasDomain
          // eg. 5678/path/to/node
          ? request.Domain.RootNodeId.ToString() + …
          // eg. /path/to/node
          : request.Uri.GetAbsolutePathDecoded();
      
        var node = FindContent(request, path);
        return node != null;
      }
    }

Default finder will look for content under the domain root.
This is an un-breaking change.

### Broken example

**TO CHECK: I think this example was included in the presentation to show that Umbraco v4 was broken. Using this example you can mimic this behaviour.**

    public class MyContentFinder : ContentFinderByNiceUrl
    {
      public override bool TryFindContent(PublishedContentRequest request)
      {
        if (base.TryFindContent(request)) return true;
        if (!request.HasDomain) return false;
        var path = request.Uri.GetAbsolutePathDecoded();
        var node = FindContent(request, path);
        return node != null;
      }
    }

### Example wire up

this example shows how to add custom content finder to (and how to remove ContentFinderByNiceUrl from) the ContentFinderResolver.

    public class MyApplication : ApplicationEventHandler
    {
      protected override void ApplicationStarting(…) 
      {
        // Insert my finder before ContentFinderByNiceUrl
        ContentFinderResolver.Current
          .InsertTypeBefore<ContentFinderByNiceUrl, MyContentFinder>();

        // Remove ContentFinderByNiceUrl
        ContentFinderResolver.Current.RemoveType<ContentFinderByNiceUrl>();
      }
    }

# NotFoundHandlers

To set your own 404 finder create an IContentFinder and set it as the ContentLastChanceFinder.  A ContentLastChanceFinder will always return a 404 status code. Example:

    ContentLastChanceFinderResolver.Current.SetFinder(new My404ContentFinder());