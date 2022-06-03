---
versionFrom: 7.0.0
needsV8Update: "true"
---

# Examine Configuration

_This shows all of the configuration options for Examine. None of these options are required and if not specified will use the Examine default values._

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>

  <!-- Define configuration sections -->

  <configSections>
    <section name="Examine" type="Examine.Config.ExamineSettings, Examine"/>
    <section name="ExamineLuceneIndexSets" type="Examine.LuceneEngine.Config.IndexSets, Examine"/>
  </configSections>

  <!-- Define provider(s) -->


  <!--
    * 	RebuildOnAppStart = (optional, default is true)
        If the index does not exist when the application is started
        then this will rebuild all indexes on startup. If you don't want this to occur
        then you can set this to false but you will need to manually rebuild your
        indexes if they don't exist. Warning: if you don't have indexes when your site is
        running, parts of your site will not function as expected such as searching
        and media queries.
  -->
  <Examine RebuildOnAppStart="true">

    <!-- Indexers... -->

    <ExamineIndexProviders>
    <providers>

        <!--
        Shows all of the configuration options available for the UmbracoContentIndexer.
        The defaults are the values used in the configuration below.
        (if you don't specify any of these, the defaults will be used)

        *   dataService = the type that this provider will instantiate in order to query
            Umbraco for the data that it requires. Generally this shouldn't need to change
            unless you want to use test data from a non-Umbraco source or you have very
            custom requirements.
        *   indexSet = (optional, will try to determine this based on naming conventions)
            Explicitly specifies the index set to use.
        *   supportUnpublished = if you want the indexer to index content that is not published
        *   supportProtected = if you want the indexer to index content that is protected
        *   analyzer = the Lucene.Net analyzer to use when storing data.
            See: http://www.aaron-powell.com/lucene-analyzer
        *   enableDefaultEventHandler = (optional, default is true) will automatically listen for Umbraco
            events and index when required
        *   logLevel="Info" or "Verbose". (optional Info is the default) Verbose will show more detailed logs
        -->
        <add name="CWSIndexer"
            type="UmbracoExamine.UmbracoContentIndexer, UmbracoExamine"
            dataService="UmbracoExamine.DataServices.UmbracoDataService, UmbracoExamine"
            indexSet="CWSIndexSet"
            supportUnpublished="false"
            supportProtected="false"
            analyzer="Lucene.Net.Analysis.Standard.StandardAnalyzer, Lucene.Net"
            enableDefaultEventHandler="true"/>

        <!--
        Used to index PDF content in Umbraco's media section.

        **** NOTE: Not all PDFs can have text read from them!!! ****

        This shows the PDF specific configuration and the default values applied when
        they are not specified.

        *   extensions = comma separated list of file extensions that our PDFs have
            the default is just .pdf
        *   umbracoFileProperty = the property of the media type that contains files
        -->
        <add name="PDFIndexer"
            type="UmbracoExamine.PDF.PDFIndexer, UmbracoExamine.PDF"
            extensions=".pdf"
            umbracoFileProperty="umbracoFile"/>

        <!--
        The SimpleDataIndexer provider allows you to index content from ANY data source
        and is agnostic to Umbraco.

        The properties shown below are for testing, you will need to define your own.

        *   dataService = You must define your own Examine.LuceneEngine.ISimpleDataService.
            This is used to get the data from your data source and consists of one method
        *   indexTypes = You can define as many index types as you like which will allow you
            to categorize your data to be searched against (i.e. content vs media)
        -->
        <add name="SimpleIndexer"
            type="Examine.LuceneEngine.Providers.SimpleDataIndexer, Examine"
            dataService="Examine.Test.DataServices.TestSimpleDataProvider, Examine.Test"
            indexTypes="Documents,Pictures" />

    </providers>
    </ExamineIndexProviders>

    <!--
    Searchers ...

    **** YOU MUST SPECIFY A DEFAULT SEARCH PROVIDER *****
    This is used when issuing the search command without explicitly referring to a
    named provider: ExamineManager.Instance.Search(...)
    -->
    <ExamineSearchProviders defaultProvider="CWSSearcher">
    <providers>

        <!--
        This search provider is used to search the CWSIndexSet.

        *   indexSet = explicitly specifies the index set to use. Generally this is wired up
            based on naming conventions.
        -->
        <add name="CWSSearcher"
            type="UmbracoExamine.UmbracoExamineSearcher, UmbracoExamine"
            indexSet="CWSIndexSet"/>

        <!-- This search provider is used to search the PDFIndexSet. -->
        <add name="PDFSearcher"
            type="UmbracoExamine.UmbracoExamineSearcher, UmbracoExamine" />

        <!-- This search provider is used to search the SimpleIndexSet. -->
        <add name="SimpleSearcher"
            type="Examine.LuceneEngine.Providers.LuceneSearcher, Examine" />

    </providers>
    </ExamineSearchProviders>
  </Examine>


  <!--
  Define your index sets, this is how you define what will go into your indexes
  **** NOTE: All index sets must have a different index path! ****
  -->

  <ExamineLuceneIndexSets>

    <!--
    Shows the bare minimum setup required for an index set,
    this will index every type of document type/media type and field in Umbraco
    -->
    <IndexSet SetName="AllDataIndexSet" IndexPath="~/App_Data/ConvensionNamedTest" />

    <!--
    When indexing custom data (non Umbraco), only the IndexUserFields need to be specified,
    if none are specified, nothing will be indexed.
    -->
    <IndexSet SetName="SimpleIndexSet" IndexPath="~/App_Data/SimpleIndexSet">
    <IndexUserFields>
        <add Name="Author" />
        <add Name="DateCreated" EnableSorting="true" />
        <add Name="Title" />
        <add Name="Photographer" />
    </IndexUserFields>
    </IndexSet>

    <!--
    Shows most options except for exclude

    * EnableSorting is used if you want to have Examine/Lucene sort your
    results based on this field.

    * Type is the data type of the field. By default the data type is a string
    Data types supported are:

        NUMBER, INT, FLOAT, DOUBLE, LONG, DATE, DATETIME, DATE.YEAR, DATE.MONTH,
        DATE.DAY, DATE.HOUR, DATE.MINUTE,

    Data types are used for Range and mathematical queries

    -->
    <IndexSet SetName="CWSIndexSet"
            IndexPath="~/App_Data/CWSIndexSetTest"
            IndexParentId="-1">
    <IndexAttributeFields>
        <add Name="id" EnableSorting="true" Type="Number" />
        <add Name="nodeName" EnableSorting="true" />
        <add Name="updateDate" EnableSorting="true" Type="DateTime" />
        <add Name="writerName" />
        <add Name="path" />
        <add Name="nodeTypeAlias" />
        <add Name="parentID" />
    </IndexAttributeFields>
    <IndexUserFields>
        <add Name="headerText" />
        <add Name="bodyText" />
        <add Name="metaDescription" />
        <add Name="metaKeywords" />
        <add Name="bodyTextColOne" />
        <add Name="bodyTextColTwo" />
        <add Name="xmlStorageTest" />
    </IndexUserFields>
    <IncludeNodeTypes>
        <add Name="CWS_Home" />
        <add Name="CWS_Textpage" />
        <add Name="CWS_TextpageTwoCol" />
        <add Name="CWS_NewsEventsList" />
        <add Name="CWS_NewsItem" />
        <add Name="CWS_Gallery" />
        <add Name="CWS_EventItem" />
        <add Name="Image" />
    </IncludeNodeTypes>
    <ExcludeNodeTypes />
    </IndexSet>

    <!--
    This is really all that is needed to index Umbraco PDFs, the PDFIndexer
    will only index media and the PDFs it contains.
    -->
    <IndexSet SetName="PDFIndexSet" IndexPath="~/App_Data/PDFIndexSet" />

  </ExamineLuceneIndexSets>

</configuration>
```
