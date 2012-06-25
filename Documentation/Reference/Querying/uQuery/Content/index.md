# Content

Querying content can be done via **Nodes** where the source data comes from the xml cache, which is fast (the current published version data) or via **Documents** where the data is retrieved from the database (slower, but the data represents the latest version whether it's published or not).

	using umbraco; // uQuery
	using umbraco.NodeFactory; // Nodes
	using umbraco.cms.Web.Document; // Documents

uQuery has a number of static methods to get collections of Nodes and Documents, as well as extension methods on the `umbraco.NodeFactory.Node` `umbraco.cms.businesslogic.Web.Document` objects.
