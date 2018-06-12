# Checkbox List

`Returns: Comma Separated String`

Displays a list of preset values as a list of checkbox controls. The text saved is a comma separated string of text values.

NOTE: Unlike other property editors, the Prevalue IDs are not directly accessible in Razor

## Data Type Definition Example

![True/Checkbox List Definition](images/wip.png)

## Content Example 

![Checkbox List Example](images/wip.png)

## MVC View Example

### Typed

	@{
	  if (Model.Content.HasValue("superHeros")){                                                     
	       <ul>                                                        
	      @foreach(var item in Model.Content.GetPropertyValue<string>("superHeros").Split(',')) { 
	        <li>@item</li>
	      }
	    </ul>                                                                                       
	  }
	}

### Dynamic (Obsolete)

See [Common pitfalls](https://our.umbraco.org/documentation/reference/Common-Pitfalls/#dynamics) for more information about why the dynamic approach is obsolete.

	@{
	    if (CurrentPage.HasValue("superHeros"))
	    {
	        <ul>
	            @foreach (var item in CurrentPage.superHeros.Split(','))
	            {
	                <li>@item</li>
	            }
	        </ul>
	    }
	}
    
