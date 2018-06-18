# Using the Macie Dashboard<a name="macie-dashboard"></a>

The Macie **Dashboard** draws a comprehensive picture of all of your Macie\-monitored data and activity\. This topic describes the metrics and views that you can use in the **Dashboard** to view your monitored data grouped by various interest points\. Each metric and view provides you with one or more ways of navigating to the Macie console's **Research** tab, where you can construct and run queries in the query parser and conduct in\-depth investigative research of your Macie\-monitored data and activity\.

## Dashboard Metrics<a name="dashboard-metrics"></a>

The following **Dashboard** metrics allow you to view your monitored data grouped by several key interest points:
+ **High\-risk S3 objects** \- While [classifying data](macie-classify-data.md), Macie assigns a risk value to each monitored data object\. This is Macie's way of helping you identify and prioritize your sensitive data over other, less business\-critical data\. This metric allows you to see all of your Macie\-monitored data objects with a risk levels of 8 through 10\.
+ **Total event occurrences** \- As part of [securing data](macie-protect-data.md), Macie analyzes and processes CloudTrail\-logged events \(API calls\) that occur within your infrastructure\. This metric provides the total count of all Macie\-monitored event occurrences that took place within your infrastructure since you enabled Macie\.
+ **Total user sessions** \- A user session is a 5\-minute aggregate of CloudTrail data\. This metric provides the total count of all user sessions of CloudTrail data that Macie analyzed and processed since it was enabled\. 

## Dashboard Views<a name="dashboard-views"></a>

Follow this procedure to use the predefined Macie **Dashboard** views and generate distinct subsets of your Macie\-monitored data and activity:<a name="procedure-dashboard-views"></a>

**To use Macie Dashboard views**

1. Choose the corresponding icon to select any of the following views to display various subsets of your Macie\-monitored data and activity:
   + [S3 objects for a selected time range](#s3objectsovertimerange)
   + [S3 objects](#s3objects)
   + [S3 objects by PII](#s3objectspii)
   + [S3 public objects by buckets](#s3objectsbuckets)
   + [S3 objects by ACL](#s3objectsacl)
   + [CloudTrail events and associated users](#cloudtraileventsusers)
   + [CloudTrail errors and associated users](#cloudtrailerrorsusers)
   + [Activity location](#activitylocation)
   + [AWS CloudTrail events](#cloudtrailevents)
   + [Activity ISPs](#activityisp)
   + [AWS CloudTrail user identity types](#cloudtrailuseridentitytype)

1. If present in the selected view, locate and move the **Minimum risk** slider to the desired value\. The **Minimum risk** slider enables you to only view items with the assigned risk equal to and greater than the selected value\. 

### S3 objects for selected time range<a name="s3objectsovertimerange"></a>

This view provides a visual representation of your monitored S3 objects that match the following search criteria: 
+ At least one of the object's assigned themes is of the top 20 most frequently assigned themes
+ The object's assigned risk is either equal to or greater than the value selected on the **Minimum risk** slider
+ S3 object was last modified during one of the following time ranges:
  + The past six months
  + Between the date when Macie was enabled and a date six months prior to today

To navigate from this view to the **Research** tab, you can select \(double\-click\) any of the squares that represent the displayed time ranges or themes\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\.

You can follow this sample procedure:<a name="d1"></a>

1. In the Macie **Dashboard**, select the **S3 objects over selected time range** view\.

1. Set the **Minimum risk** slider to 5\.

1. In the generated graph, double\-click the square next to **Range: 0 \- 6 months ago**\.

   As a result, you are redirected to the **Research** tab with the following query automatically displayed in the query parser:

   `themes:* AND dlp_risk:[5 TO *] AND @timestamp:[now-6M/M TO now]`

   This query matches your selection to view the Macie\-monitored S3 objects that are assigned one or more of the top 20 most frequently assigned themes, that have an assigned risk of 5 or higher, and that were last modified at some point in the past 6 months\. The results of this query are also displayed\. You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\.

### S3 objects<a name="s3objects"></a>

This view provides the complete list of your Macie\-monitored S3 objects, grouped by the assigned themes\. For each theme, a percentage that this theme represents of the total number of your Macie\-monitored S3 objects is displayed, as well as the total count of the S3 objects that were assigned this theme\. 

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed themes\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\.

You can follow this sample procedure:<a name="d2"></a>

1. In the Macie **Dashboard**, select the **S3 objects** view\.

1. From the generated list of S3 objects, choose the looking glass icon next to, for example, **json/aws\_cloudtrail\_logs**\.

   As a result, you are redirected to the **Research** tab with the following query automatically displayed in the query parser:

   `themes:"json/aws_cloudtrail_logs" `

   This query matches your selection to view the Macie\-monitored S3 objects with the assigned theme of **json/aws\_cloudtrail\_logs**\. The results of this query are also displayed\. You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\.

### S3 objects by PII<a name="s3objectspii"></a>

This view provides the following lists:
+ **S3 objects by PII priority**

  This is a complete list of your S3 objects that contain PII artifacts, grouped by the Macie\-assigned PII priority\. For each PII priority level, a percentage that the number of objects with this level represents of the total number of the S3 objects with PII artifacts is displayed, as well as the total count of the S3 objects with this PII priority level\.
+ **S3 objects by PII types**

  This is a complete list of your S3 objects that contain PII artifacts, grouped by the PII artifact types\. For each PII artifact type, a percentage that the number of objects with PII artifacts of this type represents of the total number of the S3 objects with PII artifacts is displayed, as well as the total count of the S3 objects with PII artifacts of this type\.

For more information about PII\-based object classification, see [Classifying Data with Amazon Macie](macie-classify-data.md)\.

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed PII impacts or PII types\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. 

You can follow this sample procedure:<a name="d3"></a>

1. In the Macie **Dashboard**, select the **S3 objects by PII** view\.

1. For example, let's generate a list of S3 objects with low PII priority\. In the **S3 objects by PII priority** list, choose the looking glass icon next to the low PII priority\.

   As a result, you are redirected to the **Research** tab with the following query automatically displayed in the query parser:

   `pii_impact:"low" `

   The results of this query are also displayed\. You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\.

### S3 public objects by buckets<a name="s3objectsbuckets"></a>

This is a complete list of your public S3 objects grouped by the buckets in which they are stored\. For each bucket, a percentage that this bucket's objects represent of the total number of your Macie\-monitored S3 objects is displayed, as well as the total count of the S3 objects that are stored in this bucket\.

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed buckets\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. 

### S3 objects by ACL<a name="s3objectsacl"></a>

This view provides the following lists:
+ **S3 objects by ACL URIs**

  This is a complete list of URIs that appear in access control lists \(ACL\)s that are attached to your S3 objects\. For each URI, a percentage that the number of objects with ACLs attached that contain this URI represents of the total number of the S3 objects with ACLs attached is displayed, as well as the total count of the S3 objects with ACLs attached that contain this URI\. 
+ **S3 objects by ACL display names**

  This is a complete list of user display names that appear in ACLs that are attached to your S3 objects\. For each display name, a percentage that the number of objects with ACLs attached that contain this display name represents of the total number of the S3 objects with ACLs attached is displayed, as well as the total count of the S3 objects with ACLs attached that contain this display name\.
+ **S3 objects by ACL permissions**

  This is a complete list of access permissions that appear in ACLs that are attached to your S3 objects\. For each permissions level, a percentage that the number of objects with ACLs attached that contain this permission level represents of the total number of the S3 objects with ACLs attached is displayed, as well as the total count of the S3 objects with ACLs attached that contain this permission level\.

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed URIs, ACL display names, and ACL permissions\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. 

You can follow this sample procedure:<a name="d4"></a>

1. In the Macie **Dashboard**, select the **S3 objects by ACL** view\.

1. For example, let's generate a list of S3 objects with attached ACLs that contain full control permissions\. In the **S3 objects by ACL permissions** list, choose the looking glass icon next to the **FULL\_CONTROL** permission\.

   As a result, you are redirected to the **Research** tab with the following query automatically displayed in the query parser:

   `object_acl.Grants.Permission:"FULL_CONTROL" `

   The results of this query are also displayed\. You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\.

### CloudTrail events and associated users<a name="cloudtraileventsusers"></a>

This view provides the following lists:
+ **AWS CloudTrail events**

  This is a visual representation of the top 20 \(by assigned risk and based on the value selected on the **Minimum risk** slider\) CloudTrail data and management events that occurred during the last 60 days\. You can use the available **Daily** or **Weekly** radio buttons to modify the graph to view daily or weekly results\. 

  To navigate from this view to the **Research** tab, you can select \(double\-click\) any square that represents a particular event that you would like to investigate further\. The number in parenthesis next to the event name represents the number of user sessions \(5\-minute aggregates of CloudTrail data\) in which this event is present\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\.
+ AWS Cloud trail associated users

  This is a visual representation of the users associated with the top 20 \(by assigned risk and based on the value selected on the **Minimum risk** slider\) CloudTrail data and management events that occurred during the last 60 days\. You can use the available **Daily** or **Weekly** radio buttons to modify the graph to view daily or weekly results\. 

  To navigate from this view to the **Research** tab, you can select \(double\-click\) any square that represents a particular error that you would like to investigate further\.The number in parenthesis next to the user name represents the number of user sessions \(5\-minute aggregates of CloudTrail data\) with which this user is associated\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\.

You can follow this sample procedure:<a name="d5"></a>

1. In the Macie **Dashboard**, select the **CloudTrail events and associated users** view\.

1. Set the **Minimum risk** slider to 1\.

1. For example, let's generate a list of user sessions in which **PutRestApi** event is present\. Double\-click on the square next to **PutRestApi**\. 

   As a result, you are redirected to the **Research** tab with the following query automatically displayed in the query parser:

   `eventNameIsp.key.keyword:"PutRestApi" AND @timestamp:[now-60d TO now] `

   The results of this query are also displayed\. You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\.

### CloudTrail errors and associated users<a name="cloudtrailerrorsusers"></a>

This view provides the following lists:
+ **AWS CloudTrail errors**

  This is a visual representation of the top 20 \(by assigned risk and based on the value selected on the **Minimum risk** slider\) CloudTrail errors that occurred during the last 60 days\. You can use the available **Daily** or **Weekly** radio buttons to modify the graph to view daily or weekly results\. 

  To navigate from this view to the **Research** tab, you can select \(double\-click\) any square that represents a particular error that you would like to investigate further\. The number in parenthesis next to the error name represents the number of user sessions \(5\-minute aggregates of CloudTrail data\) in which this error is present\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\.
+ AWS Cloud trail associated users

  This is a visual representation of the users associated with the top 20 \(by assigned risk and based on the value selected on the **Minimum risk** slider\) CloudTrail errors that occurred during the last 60 days\. You can use the available **Daily** or **Weekly** radio buttons to modify the graph to view daily or weekly results\.

  To navigate from this view to the **Research** tab, you can select \(double\-click\) any square that represents a particular error that you would like to investigate further\. The number in parenthesis next to the user name represents the number of user sessions \(5\-minute aggregates of CloudTrail data\) with which this user is associated\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\.

You can follow this sample procedure:<a name="d6"></a>

1. In the Macie **Dashboard**, select the **CloudTrail errors and associated users** view\.

1. Set the **Minimum risk** slider to 1\.

1. For example, let's generate a list of user sessions in which **Client\.InvalidPermission\.NotFound** error is present\. Double\-click on the square next to **Client\.InvalidPermission\.NotFound**\. 

   As a result, you are redirected to the **Research** tab with the following query automatically displayed in the query parser:

   `eventNameErrorCode.secondary:"Client.InvalidPermission.NotFound" AND @timestamp:[now-60d TO now]`

   The results of this query are also displayed\. You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\.

### Activity location<a name="activitylocation"></a>

This view includes a map that shows the locations of activity that Macie is monitoring for a selected time period\. To view details, you can use the available time period pull\-down menu \(past 15 days, past 30 days, past 90 days, or past year\), and then choose any location pin\. 

To navigate from this view to the **Research** tab, choose the number of events that appears in a tooltip window for a location pin\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\. For example, you can auto\-generate the following query to display a list of user sessions that occurred in the past 15 days in Seattle:

`geoLocation.key:"Seattle:UnitedStates:47.6145:-122.348" AND @timestamp:[now-15d TO now]`

You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\. 

### AWS CloudTrail events<a name="cloudtrailevents"></a>

**AWS CloudTrail events**

This view provides the complete list of your Macie\-monitored CloudTrail data and management events\. For each event, the total count of the user sessions \(5\-minute integrations of CloudTrail data\) in which this event is present and the percentage that this total represents of the total number of user sessions is displayed\.

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed events\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. For example, you can auto\-generate the following query to view all user sessions in which the AssumeRole event is present:

`eventNameIsp.key.keyword:"AssumeRole"`

You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\. 

### Activity ISPs<a name="activityisp"></a>

**Activity ISPs**

This view provides the complete list of your Macie\-monitored CloudTrail data and management events, grouped by the associated internet service providers \(ISPs\)\. For each ISP, the total count of the user sessions \(5\-minute integrations of CloudTrail data\) in which this ISP is present and the percentage that this total represents of the total number of user sessions is displayed\. 

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed themes\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. For example, you can auto\-generate the following query to view all user sessions that are associated with Amazon:

`eventNameIsp.secondary.keyword:"Amazon" `

You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\. 

### AWS CloudTrail user identity types<a name="cloudtrailuseridentitytype"></a>

This view provides the complete list of your Macie\-monitored CloudTrail data and management events, grouped by the user identity type \(for more information, see the definition for 'user' in [Concepts and Terminology](macie-concepts.md)\. For each user identity type, the total count of the user sessions \(5\-minute integrations of CloudTrail data\) in which this user identity type is present and the percentage that this total represents of the total number of user sessions is displayed\. 

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed themes\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. For example, you can auto\-generate the following query to view all user sessions that contain requests that were originated by the **AssumedRole** user identity type:

`userIdentityType.key:"AssumedRole" `

You can then modify the query result controls available on the **Research** tab and run the query again, and conduct in\-depth investigative research of the generated results\. For more information, see [Researching Through Macie\-Monitored Data](macie-research.md)\. 