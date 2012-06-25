# uQuery

uQuery is an API giving read/write access the content, media and member data, as well as extending the relations API. uQuery originated from uComponents and was added into Umbraco from v4.8. To use uQuery declare the umbraco namespace in addition to those required for nodes, documents, media, members and relations.


	using umbraco; // uQuery
	using umbraco.NodeFactory; // Node
	using umbraco.cms.businesslogic.web; // Document
	using umbraco.cms.businesslogic.media;
	using umbraco.cms.businesslogic.member;
	using umbraco.cms.businesslogic.relation;


## UmbracoObjectType
uQuery also exposes an Enum with associated helper methods to represent each of the Guids used by Umbraco.

	public enum UmbracoObjectType
	{
		Unknown,
		[Guid("EA7D8624-4CFE-4578-A871-24AA946BF34D")] ROOT,
		[Guid("C66BA18E-EAF3-4CFF-8A22-41B16D66A972")] Document,
		[Guid("B796F64C-1F99-4FFB-B886-4BF4BC011A9C")] Media,
		[Guid("39EB0F98-B348-42A1-8662-E7EB18487560")] Member,
		[Guid("7A333C54-6F43-40A4-86A2-18688DC7E532")] ContentItemType,
		[Guid("10E2B09F-C28B-476D-B77A-AA686435E44A")] ContentItem,
		[Guid("A2CB7800-F571-4787-9638-BC48539A0EFB")] DocumentType,
		[Guid("4EA4382B-2F5A-4C2B-9587-AE9B3CF3602E")] MediaType,
		[Guid("9B5416FB-E72F-45A9-A07B-5A9A2709CE43")] MemberType,
		[Guid("366E63B9-880F-4E13-A61C-98069B029728")] MemberGroup,
		[Guid("6FBDE604-4178-42CE-A10B-8A2600A2F07D")] Template,
		[Guid("01BB7FF2-24DC-4C0C-95A2-C24EF72BBAC8")] RecycleBin,
		[Guid("9F68DA4F-A3A8-44C2-8226-DCBD125E4840")] Stylesheet,
		[Guid("30A2A501-1978-4DDB-A57B-F7EFED43BA3C")] DataType
	}

### GetUmbracoObjectType(string)
Returns: `uQuery.UmbracoObjectType`

	uQuery.UmbracoObjectType umbracoObjectType = uQuery.GetUmbracoObjectType("Document");
	

### GetUmbracoObjectType(Guid)
Returns: `uQuery.UmbracoObjectType`

Gets the Enum value corresponding to the supplied Guid. This method iterates the Enum looking for one with a matching Guid attribute, if not found then UmbracoObjectType.Unknown is returned.

	uQuery.UmbracoObjectType umbracoObjectType = uQuery.GetUmbracoObjectType(
													Guid.Parse("C66BA18E-EAF3-4CFF-8A22-41B16D66A972"));

### GetUmbracoObjectType(int)
Returns: `uQuery.UmbracoObjectType`

Gets the UmbracoObjectType for the supplied Umbraco id. This method queries the umbracoNode table, getting the guid in the nodeObjectType field where the id matches that supplied. This guid is then used to the find the Enum by attribute value, iif it's not found then UmbracoObjectType.Unknown is returned.

	uQuery.UmbracoObjectType umbracoObjectType = uQuery.GetUmbracoObjectType(1100);



### GetGuid()
Retuns: `Guid`	
	
	Guid guid = uQuery.GetUmbracoObjectType("Document").GetGuid();



### GetName()
Returns: `string`

	string name =  uQuery.GetUmbracoObjectType(Guid.Parse("C66BA18E-EAF3-4CFF-8A22-41B16D66A972")).GetName();
