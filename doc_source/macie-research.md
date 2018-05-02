# Researching Through Macie\-Monitored Data<a name="macie-research"></a>

You can use the **Research** tab in the Macie console to construct and run queries in the Query Parser and conduct in\-depth investigative research of your Macie\-monitored data and activity\. You can navigate to the **Research** tab at any time and construct queries from scratch in the empty parser\. For more information, see [Constructing Queries in Macie](#macie-query)\. Or you can be redirected to the **Research** tab from various places throughout the Macie console, for example, any of the **Dashboard** views \(see [Using the Macie Dashboard](macie-dashboard.md)\) or the **Basic alerts** list \(see [Amazon Macie Alerts](macie-alerts.md)\)\. When redirected to the **Research** tab from other places in the console, your data selection is translated into an automatically generated query that is displayed in the query parser\.

**Topics**
+ [Constructing Queries in Macie](#macie-query)
+ [Research Filters](#research-controls)
+ [Save a Query as an Alert](#searchalert)
+ [Favorite Queries](#searchsavefavorite)
+ [Researching AWS CloudTrail Data](cloudtraildata.md)
+ [Researching S3 Bucket Properties Data](s3bucketsdata.md)
+ [Researching S3 Objects Data](s3objectsdata.md)

## Constructing Queries in Macie<a name="macie-query"></a>

Macie allows you to construct queries in the Query Parser in the **Research** tab\. This Query Parser is a lexer which interprets a string into a Lucene Query using JavaCC\. For more information about query syntax, see [Apache Lucene \- Query Parser Syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html)\.

The following are example queries for common searches: 
+ You can use the following query to search for any console login not that did not originate from IP addresses owned by Amazon: `eventNameIsp.compound:/ConsoleLogin:~(Amazon.*)/`
+ You can use the following query to search for PII artifacts inside a public S3 bucket: `filesystem_metadata.bucket:"my-public-bucket" AND (pii_impact:"moderate" OR pii_impact:"high")`

The following tables contains example queries for the Macie date, integer, and string field types:

### Date field type example queries:<a name="macie-query_date"></a>


| Example query | Description | Data repository | 
| --- | --- | --- | 
| `objectsRead.key:* AND @timestamp:[2017-08-01 TO 2017-12-31]` | Search for S3 objects read in the fourth quarter of 2017\. | CloudTrail data | 
| `sourceIPAddress.ip_intel.type:"TOR" AND @timestamp:[now-1M TO now]` | Search for anonymous accesses to your Macie\-monitored data from Tor exit notes over the last month\. | CloudTrail data | 
| `macieUniqueId:"085924634393\:assumed-role\:malicious_user" AND @timestamp:[2018-01-18 TO *]` | Search for AWS activities of an assumed role named "malicious\_user" in the AWS account ID 085924634393, starting from January 18, 2018\. | CloudTrail data | 

### Integer field type example queries:<a name="macie-query_integer"></a>


| Example query | Description | Data repository | 
| --- | --- | --- | 
| `dlp_risk>6 AND filesystem_metadata.server_encryption:"none"` | Search for S3 objects with a dlp\_risk score greater than 6 and without a server\-side encryption\. | S3 objects | 
| `filesystem_metadata.size: [10240 TO 1024000] AND pii_types:*` | Search for S3 objects between the sizes of 10 MB to 1 GB that contain potential PII data\. | S3 objects | 

### String field type example queries:<a name="macie-query_string"></a>


| Example query | Description | Data repository | 
| --- | --- | --- | 
| `dlp_risk>5 AND key: /.*contract.*\|.*agreement.*\|.*terms.*/ AND @timestamp:[now-1M/M TO now]` | Search for S3 object keys \(names\) that contain the keywords "contract","agreement", or "terms", with a dlp\-risk score higher than 5, and that were last modified less than a month ago\.  Some regex queries may result in long search times\. We recommend conducting searches for limited time frames\.   | S3 objects | 
| `mimetypes:"Adobe PDF \(application/pdf\)" AND key: /~(.*\.pdf\|.*\.PDF)/` | Search for S3 objects containing pdf data but in files with file extensions other than PDF/pdf\.  This query also returns archived objects \(zip,7z, etc\.\) containing PDF documents\.   | S3 objects | 
| `acl.Grants.Grantee.DisplayName: admin` | Search for S3 buckets with ACL grantee display names set to "admin"\.  | S3 bucket properties | 
| `acl.Grants.Grantee.DisplayName: admi?` | Search for S3 buckets with ACL grantee display names set to "admi\(?\)" \(wildcard\), including "admin"\.  | S3 bucket properties | 
| `bucket: *test*` | Search for S3 buckets with keywords "test"\.  | S3 bucket properties | 

## Research Filters<a name="research-controls"></a>

In the Macie **Research** tab, you can apply the following filters to your searches:

### Data index<a name="where-to-search"></a>

The first **Research** tab filter \(pull\-down list\), with the pre\-selected default value of **Cloudtrail data**, allows you to specifying the index \(or the data repository\) that you want Macie to search through\. This filter includes the following options: 
+ **CloudTrail data** \- a collection of 5\-minute aggregates of raw Cloudtrail data
+ **S3 bucket properties ** \- a collection of metadata about the S3 buckets that Macie is monitoring
+ **S3 objects** \- a collection of metadata about the S3 objects that are stored in the buckets that Macie is monitoring

### Number of Results to Display<a name="numberofresults"></a>

The next **Research** tab filter with the pre\-selected default value of **Top 10**, allows you to control the number of results to display when you do your initial search and the number of additional results to display if more results are available\. This filter includes the following options:
+ Top 10
+ Top 50
+ Top 100
+ Top 500

### Time Range<a name="timerange"></a>

The third **Research** tab filter with the pre\-selected default value of **Past 30 days**, allows you to define a time range for which you want to display your search results\. This filter includes the following options:
+ Past 7 days
+ Past 30 days
+ Past 90 days
+ Past 365 days
+ All
+ Custom time range

## Save a Query as an Alert<a name="searchalert"></a>

You can use the following procedure to save a query that is displayed in the query parser as a basic alert\. For more information about basic alerts, see [Amazon Macie Alerts](macie-alerts.md)\.

1. In the Macie console's **Research** tab, either autogenerate or construct a query in the query parser\.

1. Choose the **Save query as alert** icon\.

1. Fill out the Basic alert definition form and then choose **Save**\. For more information, see [Adding New and Editing Existing Custom Basic Alerts](macie-alerts.md#macie-alert-working-edit-add-basic)\.

## Favorite Queries<a name="searchsavefavorite"></a>

You can mark queries that you frequently run as favorite and quickly view a list of your favorite queries\.

1. In the Macie console's **Research** tab, either autogenerate or construct a query in the query parser\.

1. Choose the **Mark query as favorite** icon\.

1. Fill out the **Favorite query definition** form by specifying the name and the description for the favorite query, and then choose **Save**\.

1. To view the list of your favorite queries, in the Macie console's **Research** tab, choose the **Favorite queries** icon\.