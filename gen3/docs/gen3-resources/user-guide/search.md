
# Searching and Exploring Structured Data
The data in a Gen3 data commons can be browsed and downloaded using several different methods. The following general documentation will cover some standard methods of data access in a Gen3 data commons. Ultimately, however, the methods of data access offered in a Gen3 data commons is determined by agreements made between the data commons’ sponsors and data contributors.

Various levels of data access can be configured in a Gen3 data commons using the Fence and Arborist services. If open access data is hosted, a data commons can be configured to allow anonymous access to data, which means users can explore data without logging in. This is the case for the [Gen3 Data Hub][Gen3 Data Hub].

In cases where data is controlled access, typically external users will receive instructions on how to access data and may be required to sign a DUA (Data Use Agreement) legal document.

The following sections provide details on how to explore and access data from within the data commons website and from the command-line by sending requests to the Gen3 open APIs.

## Searching for Data from the Data Portal
The Gen3 software stack offers a data portal service that creates a website with graphical tools for performing the basic functionality of a data commons, like browsing data in projects, building patient cohorts across projects, downloading metadata or data files for cohorts, and building database queries. The data portal relies on the same [API queries][API instructions] that you can explore directly if you prefer.

### Exploration Page
The primary tool for exploring data within a Gen3 data commons is the Exploration Page, which offers faceted search of data across projects, for example, https://gen3.datacommons.io/explorer. This page can be accessed from the /explorer endpoint or the top navigation bar, by clicking on the “Exploration” icon.

![Screenshot of the Gen3 toolbar, boxing the exploration icon][img Gen3 Toolbar Exploration]

The exploration page has one or several tabs at the top, which each represent a flattened ElasticSearch document of structured metadata records across all the projects in the data commons, which is displayed as a table towards the bottom center of the page. For example, there may be a “Subjects” tab for building patient cohorts, which displays a table of all the records and associated metadata in the subject node, like medical history and demographics. Most commons also have one or more “File” tabs for filtering all the files in a data commons based on things like the file format, data type, or other linked contextual variables, like linked patient demographics or medical history.

Each of these main tabs will have filters on the left-hand side, which can be used to filter the data displayed in the table. There may be an optional button on each tab to download the flattened metadata table as a JSON file. This button should download the table in its filtered state. To remove a filter, click “clear” on individual facets, and you can remove all filters by reloading the page.

> Note: The main tabs in the Exploration Page, the available filters, and the properties listed in the data table are entirely customizable and will be different for various Gen3 data commons.

If the table is a list of files, there should be a button for downloading a JSON file that serves as a manifest to use with the [gen3-client][Gen3 client] for [downloading multiple files][Gen3 bulk download]. Otherwise, to download single a file listed in the table, simply click on the GUID (globally unique identifier, or object_id), which should open a page with a download button.

![GIF showing how to use the Gen3 Explorer][img Explorer GIF]

>Note: Some data commons have security measures in place that limit what environments users can access data files. For example, users may be required to download and analyze data files in a protected environment, such as a virtual machines (VM) in a virtual private cloud (VPC) or in the built-in Gen3 Workspace, which is accessed by clicking on “Workspace” in the top navigation bar of the data commons website. For more information on the Workspace, see the [documentation on how to access and use the Gen3 Workspace][Gen3 Workspace].



### Access Data using the API
All the functionality of the data portal is available by sending requests to the open APIs of the Gen3 system. Detailed API specifications of the Gen3 services can be browsed in [the developer documentation][Developer API specs].

#### Querying Structured Data Programmatically
Users can submit queries to the Gen3 APIs to access structured data across the projects in the data commons. Queries can be sent using, for example, the Python “requests” package or similar functions in other programming languages. Further details on how to send queries to the API are documented [here][API documentation].

#### Exporting Structured Data Programmatically
Users with “read” access to a project can export entire structured metadata records by sending requests to the API. Single records can be exported or all records in a specific node of a project can be retrieved. For more information, see the [documentation on using the API][API documentation].

#### The Gen3 SDK
To make sending requests to the Gen3 APIs easier, the bioinformatics team at the Center for Translational Data Science (CTDS) at University of Chicago has put together a basic Python SDK (software development kit) to help users interact with the Gen3 APIs.

The SDK is essentially a collection of Python wrapper functions for sending requests to the API. It is open source and can be found on [Github][Gen3 SDK GitHub pg]. Thorough documentation for the SDK can be found in the GitHub repository [documentation page][SDK doc pg].

#### Sending Queries using the SDK
The [Gen3Submission class][Gen3Submission Python SDK class] of the Python SDK has functions for [sending queries to the API][send query to API] and also for [retrieving the graphQL schema][Retrieve graphQL schema] of the data commons. Queries can be used to pinpoint specific data of interest by providing query arguments that act as filters on records in the database and providing lists of properties to retrieve for those records. If all the structured data in a record or node is desired, as opposed to only specific properties, then see the export functions below.

#### Exporting Structured Data using the SDK
Entire structured data records can be exported as a JSON or TSV file using the [Gen3Submission Python SDK class][Gen3Submission Python SDK class]. The [`export_record`][export_record] function will export a single structured metadata record in a specific node of a specific project, whereas the [`export_node`][export_node] function will export all the structured metadata records in a specified node of a specific project.

More SDK examples and how to get started with the SDK can be also found in the [analyze-data section][Using Gen3 SDK].



### Submission Page

#### Browsing the List of Projects
A graphical model of the structured data in individual data projects of a data commons can be browsed by navigating to the /submission endpoint of a data commons website or by clicking on the “Browse Data” or “Submit Data” buttons in the top navigation bar, for example, [https://gen3.datacommons.io/submission][Gen3 Data Submission pg]. This page lists all the data projects within a commons a user has authorization to view. Clicking the “Browse” or “Submit Data” button by a project ID will open a view of that individual project’s structured metadata graph, which can be further inspected by clicking on a node in the graph model and then viewing individual records by clicking “View” by the submitter_id or downloading all the records in that node by clicking the “Download All” button.

> Note: Users who are authorized to submit data may see a “Submit Data” button instead of “Browse Data”, and will also be able to upload or create structured data in the project on this page.

##### Example: Browse Data in Individual Projects
![Image showing options for browsing nodes in individual projects using either the dropdown list or project graph model][img Browse Nodes in Projects]

In the graphical model of a data project, the number you see underneath the node name, for example ‘subject’, reflects the number of records in that node of the project. The “Toggle View” button is used to show or hide nodes in the data model that the project has no records for.

![GIF showing how to view the graphical model of a project, toggling to show or hide nodes that have no records][img Graphing a project]



### Query Page
The easiest way to query structured data in a Gen3 data commons is done by using the [graphQL query language][GraphQL] with the GraphiQL interface, which can be accessed by clicking “Query” in the top navigation bar or by navigating to the URL: [https://gen3.datacommons.io/query][Query page]. The URL https://gen3.datacommons.io can be replaced with the URL of other Gen3 data commons.

This query portal has been optimized to autocomplete fields based on content, increase speed and responsiveness, pass variables, and generally make it easier for users to find information. The “Docs” button will display documentation of the queryable nodes and properties. From the GraphiQL interface of the data portal, you can switch between *Graph Model* or *Flat Model* – each using endpoints that query different databases (Postgres and ElasticSearch, respectively). Notably, the same queries can be sent to both the flat and graph model API endpoints from the command-line.

#### Graph Model

In the Graph Model, our microservice *Peregrine* converts GraphiQL queries and hits the PostgreSQL database.

![Animation showing how you can use the Peregrine UI to build a GraphQL query that hits the PostgreSQL database and returns a response][gif Peregrine]

##### List all nodes of a particular node category

```
{
  _node_type (category: "medical_history") {
    category
    id
  }
}
```

##### Find specific files by querying a datanode

Metadata for specific files can be obtained by including arguments in “datanode” queries. The following are some commonly used arguments (not an exhaustive list):

* `submitter_id: "a_submitter_id"`: get information for a specific submitter_id
* `quick_search: "a_substring"`: get information for all files with partial matches in submitter_id
* `file_name: "a_filename.txt"`: get information for files matching a specified filename.

For example, the following arguments can be used to obtain similar results:

* `quick_search: "sub-70080"` will return files with the substring “sub-70080” in the submitter_id.
* `file_name: "sub-70080_T1w.nii.gz"` will return only files with that exact filename.
* `submitter_id: "OpenNeuro-ds000030_sub-70080_T1w.nii_6ff0"` will return only the file with that exact submitter_id, which must be unique within a node.

The following example query can be pasted into the GraphiQL interface at [https://gen3.datacommons.io/query][Query page] (be sure to click “Switch to Graph Model”). Note that in this example, there are three individual “datanode” queries that are sent simultaneously and assigned labels (“match_file_name”, “match_quick_search”, and “match_submitter_id”):
```
{
  match_file_name: datanode (file_name: "sub-70080_T1w.nii.gz") {
        project_id object_id id md5sum file_size file_name
        data_type data_format data_category
    }
  match_quick_search: datanode (quick_search: "sub-70080") {
    project_id object_id id md5sum file_size file_name
    data_type data_format data_category
  }
  match_submitter_id: datanode (submitter_id: "OpenNeuro-ds000030_sub-70080_T1w.nii_6ff0") {
      project_id object_id id md5sum file_size file_name
      data_type data_format data_category
  }
}
```
Result: applying the `file_name` and `submitter_id` arguments returns only the files that match the provided string exactly, while the `quick_search` argument returns all files with a submitter_id that matches the sub-string. There are two in this case:
```
{
  "data": {
    "match_file_name": [
      {
        "data_category": "T1-weighted Image",
        "data_format": "NII/NIfTI",
        "data_type": "fMRI Image",
        "file_name": "sub-70080_T1w.nii.gz",
        "file_size": 11427935,
        "id": "e95e513e-b76e-4de9-aef5-6b9d74e2e60f",
        "md5sum": "c800fe80a333e8d3439c854dea3fdad2",
        "object_id": "31525da9-7b09-48ea-966d-dd9e93786ff0",
        "project_id": "OpenNeuro-ds000030"
      }
    ],
    "match_quick_search": [
      {
        "data_category": "T1-weighted Image",
        "data_format": "NII/NIfTI",
        "data_type": "fMRI Image",
        "file_name": "sub-70080_T1w.nii.gz",
        "file_size": 11427935,
        "id": "e95e513e-b76e-4de9-aef5-6b9d74e2e60f",
        "md5sum": "c800fe80a333e8d3439c854dea3fdad2",
        "object_id": "31525da9-7b09-48ea-966d-dd9e93786ff0",
        "project_id": "OpenNeuro-ds000030"
      },
      {
        "data_category": "Diffusion-weighted Image",
        "data_format": "NII/NIfTI",
        "data_type": "fMRI Image",
        "file_name": "sub-70080_dwi.nii.gz",
        "file_size": 39484002,
        "id": "f50e6f27-8d02-49f1-b30b-9a3d32c6075d",
        "md5sum": "c37bbeb51c85471a7da4f3675c836f71",
        "object_id": "94b1d6ef-5e4b-4945-bf31-bb7f06881c97",
        "project_id": "OpenNeuro-ds000030"
      }
    ],
    "match_submitter_id": [
      {
        "data_category": "T1-weighted Image",
        "data_format": "NII/NIfTI",
        "data_type": "fMRI Image",
        "file_name": "sub-70080_T1w.nii.gz",
        "file_size": 11427935,
        "id": "e95e513e-b76e-4de9-aef5-6b9d74e2e60f",
        "md5sum": "c800fe80a333e8d3439c854dea3fdad2",
        "object_id": "31525da9-7b09-48ea-966d-dd9e93786ff0",
        "project_id": "OpenNeuro-ds000030"
      }
    ]
  }
}
```
In the case that too many results are returned, a timeout error might occur. In that case, use [pagination][GraphQL pagination] to break up the query.

For example, if there are 2,550 records returned, and the GraphiQL query is timing out with `(first:3000)`, then break the query into multiple queries with offsets:
```
(first:1000, offset:0) 		# this will return records 0-1000
(first:1000, offset:1000) 	# this will return records 1000-2000
(first:1000, offset:2000) 	# this will return records 2000-2,550
```

#### Flat Model

In the Flat Model, our microservice *Guppy* converts GraphiQL queries and hits the Elasticsearch database. Here, queries support Aggregations for string (bin counts; number of records that each key has) and numeric (summary statistics such as minimum, maximum, sum, etc) fields. For more details see the full description in the [Guppy repository][Guppy].

##### Querying
Guppy allows you to query the raw data with offset, the maximum number of rows, sorting, and filters. Queries by default return the first 10 entries. To return more entries, the query call can specify a larger number such as `(first: 100)`.

Example:
```
{
  subject(offset: 5, first: 100, sort: [
    {
      gender: "asc"
    },
    {
      _aligned_reads_files_count: "desc"
    }
  ]) {
    subject_id
    gender
    ethnicity
    _aligned_reads_files_count
  }
}
```
The maximum number of results returned is 10,000, which can be requested with the `(first: 10000)` argument. If you need to access more than that number, we suggest using the [Guppy download endpoint][Guppy download endpoint].

##### Aggregations

Aggregation query is wrapped within the `_aggregation` keyword. In total, five aggregations are feasible at the moment:

1. Total Count Aggregation,
2. Text Aggregation,
3. Numeric Aggregation,
4. Nested Aggregation,
5. Sub-aggregation.

For more examples see the full description in the [Github repository][Guppy].

Example for 1) Total Count Aggregation that includes a filter:
```
query ($filter: JSON) {
 _aggregation  {
   subject(filter: $filter) {
     _totalCount
   }
 }
}
```

##### Filtering

Currently, Guppy uses `JSON`-based syntax for filters. Filters can be text/string/number-based, combined, or nested. For more examples, see the full description in the [Github repository][Guppy].

Example for a basic filter unit:
```
{
  "filter": {
    "=": {
      "gender": "Female"
    }
  }
}
```
Example query including a filter:
```
query ($filter: JSON) {
  subject (filter: $filter, first: 20) {
    gender
    race
    ethnicity
    _matched {
      field
      highlights
    }
  }
}
```



<!-- Access Metadata section. -->
<!-- NOTE: these hyperlinks link to headings in the document - it's a table of contents -->
<!-- Exploration section. -->
[img Gen3 Toolbar Exploration]: /gen3-resources/user-guide/img/Gen3_Toolbar_exploration.png
[img Explorer GIF]: /gen3-resources/user-guide/img/explorer_gif_2020.gif
[Gen3 Data Hub]: https://gen3.datacommons.io/
[API instructions]: /gen3-resources/user-guide/search/#access-data-using-the-api
[Gen3 client]: /gen3-resources/user-guide/access-data/#download-files-using-the-gen3-client
[Gen3 bulk download]: /gen3-resources/user-guide/access-data/#multiple-file-download-with-manifest

<!-- API section -->
[Developer API specs]: https://gen3.org/resources/developer/microservice
[API documentation]: /gen3-resources/user-guide/using-api/
[SDK doc pg]: https://uc-cdis.github.io/gen3sdk-python/_build/html/index.html
[Gen3Submission Python SDK class]: https://uc-cdis.github.io/gen3sdk-python/_build/html/submission.html
[send query to API]: https://github.com/uc-cdis/gen3sdk-python/blob/master/gen3/submission.py#L289
[Retrieve graphQL schema]: https://github.com/uc-cdis/gen3sdk-python/blob/master/gen3/submission.py#L327
[export_record]: https://github.com/uc-cdis/gen3sdk-python/blob/master/gen3/submission.py#L223
[export_node]: https://github.com/uc-cdis/gen3sdk-python/blob/master/gen3/submission.py#L255
[Using Gen3 SDK]: /gen3-resources/user-guide/analyze-data/#using-the-gen3-python-sdk
[Gen3 SDK GitHub pg]: https://github.com/uc-cdis/gen3sdk-python


<!-- Submission page -->
[Gen3 Data Submission pg]: https://gen3.datacommons.io/submission
[img Browse Nodes in Projects]: /gen3-resources/user-guide/img/projects-nodes-view.png
[img Graphing a project]: /gen3-resources/user-guide/img/graph-a-project.gif
[GraphQL]: https://graphql.org/
[Query page]:  https://gen3.datacommons.io/query
[gif Peregrine]: /gen3-resources/user-guide/img/simple_query_2020.gif
[GraphQL pagination]: http://graphql.org/learn/pagination/
[Guppy]: https://github.com/uc-cdis/guppy/blob/master/doc/queries.md
[Guppy download endpoint]: https://github.com/uc-cdis/guppy/blob/master/doc/download.md