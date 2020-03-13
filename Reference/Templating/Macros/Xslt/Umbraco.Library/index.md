---
versionFrom: 4.0.1
versionRemoved: 8.0.0
---

# Umbraco.library v4.0.1

umbraco.library is a set of helpers you can use from XSLT, to interact with an Umbraco installation.

[See UmbracoHelper for methods to use with MVC Razor Views](../../../../../Reference/Querying/UmbracoHelper/index.md).

## Available Methods

* `AddJquery()`
* `AllowedGroups(Int32 documentId, String path)`
* `ChangeContentType(String MimeType)`
* `ContextKey(String key)`
* `CultureExists(String cultureName)`
* `GetCurrentDomains(Int32 NodeId)`
* `GetCurrentMember()`
* `GetDictionaryItem(String Key)`
* `GetDictionaryItems(String Key)`
* `GetHttpItem(String key)`
* `GetItem(Int32 nodeID, String alias)`
* `GetItem(String alias)`
* `GetMedia(Int32 MediaId, Boolean Deep)`
* `GetMember(Int32 MemberId)`
* `GetMemberName(Int32 MemberId)`
* `GetNodeFromLevel(String path, Int32 level)`
* `GetPreValueAsString(Int32 Id)`
* `GetPreValues(Int32 DataTypeId)`
* `GetPropertyTypeName(String ContentTypeAlias, String PropertyTypeAlias)`
* `GetRandom()`
* `GetRandom(Int32 seed)`
* `GetRelatedNodes(Int32 NodeId)`
* `GetRelatedNodesAsXml(Int32 NodeId)`
* `GetXmlAll()`
* `GetXmlDocument(String Path, Boolean Relative)`
* `GetXmlDocumentByUrl(String Url)`
* `GetXmlDocumentByUrl(String Url, Int32 CacheInSeconds)`
* `GetXmlNodeById(String id)`
* `GetXmlNodeByXPath(String xpathQuery)`
* `GetXmlNodeCurrent()`
* `HasAccess(Int32 NodeId, String Path)`
* `HtmlEncode(String Text)`
* `IsLoggedOn()`
* `IsProtected(Int32 DocumentId, String Path)`
* `LastIndexOf(String Text, String Value)`
* `md5(String text)`
* `NiceUrl(Int32 nodeID)`
* `NiceUrlFullPath(Int32 nodeID)`
* `PublishSingleNode(Int32 DocumentId)`
* `PythonExecute(String expression)`
* `PythonExecuteFile(String file)`
* `QueryForNode(String id)`
* `RefreshContent()`
* `RegisterClientScriptBlock(String key, String script, Boolean addScriptTags)`
* `RegisterJavaScriptFile(String key, String url)`
* `RegisterStyleSheetFile(String key, String url)`
* `RemoveFirstParagraphTag(String text)`
* `RenderMacroContent(String Text, Int32 PageId)`
* `RenderTemplate(Int32 PageId)`
* `RenderTemplate(Int32 PageId, Int32 TemplateId)`
* `Replace(String text, String oldValue, String newValue)`
* `ReplaceLineBreaks(String text)`
* `RePublishNodes(Int32 nodeID)`
* `RePublishNodesDotNet(Int32 nodeID)`
* `RePublishNodesDotNet(Int32 nodeID, Boolean SaveToDisk)`
* `Request(String key)`
* `RequestCookies(String key)`
* `RequestForm(String key)`
* `RequestQueryString(String key)`
* `RequestServerVariables(String key)`
* `SendMail(String FromMail, String ToMail, String Subject, String Body, Boolean IsHtml)`
* `Session(String key)`
* `SessionId()`
* `setCookie(String key, String value)`
* `setSession(String key, String value)`
* `Split(String StringToSplit, String Separator)`
* `StripHtml(String text)`
* `Tidy(String StringToTidy, Boolean LiveEditing)`
* `TruncateString(String Text, Int32 MaxLength, String AddString)`
* `UnPublishSingleNode(Int32 DocumentId)`
* `UpdateDocumentCache(Int32 DocumentId)`
* `UrlEncode(String Text)`

## Date/Time Methods

* `CurrentDate()`
* `DateAdd(String Date, String AddType, Int32 add)`
* `DateAddWithDateTimeObject(DateTime Date, String AddType, Int32 add)`
* `DateDiff(String firstDate, String secondDate, String diffType)`
* `DateGreaterThan(String firstDate, String secondDate)`
* `DateGreaterThanOrEqual(String firstDate, String secondDate)`
* `DateGreaterThanOrEqualToday(String firstDate)`
* `DateGreaterThanToday(String firstDate)`
* `FormatDateTime(String Date, String Format)`
* `GetWeekDay(String Date)`
* `LongDate(String Date)`
* `LongDate(String Date, Boolean WithTime, String TimeSplitter)`
* `LongDateWithDayName(String Date, String DaySplitter, Boolean WithTime, String TimeSplitter, String GlobalAlias)`
* `ShortDate(String Date)`
* `ShortDate(String Date, Boolean WithTime, String TimeSplitter)`
* `ShortDateWithGlobal(String Date, String GlobalAlias)`
* `ShortDateWithTimeAndGlobal(String Date, String GlobalAlias)`
* `ShortTime(String Date)`

[you may be able to find some further information on these methods in the old Wiki](https://en.wikibooks.org/wiki/Umbraco/Reference/umbraco.library)
