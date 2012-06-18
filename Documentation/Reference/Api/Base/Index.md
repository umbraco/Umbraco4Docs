#Umbraco /Base

/Base is a extendable system for creating raw feeds directly from Umbraco using very basic Url's. This enables developers to access umbraco data through javascript, f.ex or flash.  It allows even to modify umbraco data directly via url's.
Although /Base could be called a very simple REST system, it is not completely true.  A rest based API would implement all the HTTP methods.  

## Why using umbracoBase?
The main advantage is that you don't need any masterpages or scripts.  This is a perfect and fast way to expose (or change) nodes.

To use /Base you need to know some .Net programming and some Umbraco basics.  

## How it works
/Base doesn't generate any data by itself, it just acts as a proxy and outputs data based on the urls passed to it. It is actually a very basic system for getting data out of umbraco with very few lines of code.  

/Base enables exposes .Net methods via URLs. The URL structure is 
    `/Base/Class/MethodName`
The Class can be aliased and doesn't need to be the same as the class name.  If a method is not explicitily exposed then /Base won't make it available via a URL.

If you add parameters, the url will become `/Base/ClassAlias/MethodName/Parameter1/Parameter2`.  If you have not enabled directory urls <!-- todo: what is the link? -->, you need to add an .aspx extention at the end.  

## Setting up
There are 2 ways to make your own .net classes /Base enabled.

  1. By using the [RestExtentions.config](The-RestExtentions.config-file.md) file
  2. Since umbraco 4.6 <!-- can someone confirm the version number? --> you can decorate classes and methods with .Net Attributes to make them /Base enabled.  See the [hello world](helloworld.md) to find out how.

## Advanced examples

*   using [parameters](examples/parameters.md)
*   special [return types](Return-types.md)
*   using [XPathNodeIterator as return type](examples/XPathNodeIterator-returntype.md)


