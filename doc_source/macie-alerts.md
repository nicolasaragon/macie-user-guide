# Amazon Macie Alerts<a name="macie-alerts"></a>

An alert is a notification about a potential security issue discovered by Amazon Macie\. This section describes the following information: 

**Topics**
+ [Basic and Predictive Macie Alerts](#macie-alert-types)
+ [Alert Categories in Macie](#macie-alert-categories)
+ [Severity Levels for Alerts in Macie](#macie-alert-severity)
+ [Locating and Analyzing Macie Alerts](#macie-alert-working-locate)
+ [Adding New and Editing Existing Custom Basic Alerts](#macie-alert-working-edit-add-basic)
+ [Working with Existing Alerts](#macie-alert-working-archive-useful-notuseful)
+ [Group Archiving Alerts](#macie-alert-working-group-archive)
+ [Whitelisting Users or Buckets for Basic Alerts](#macie-alert-working-group-whitelist)

## Basic and Predictive Macie Alerts<a name="macie-alert-types"></a>

Macie generates two types of alerts:
+ *Basic alerts* \- alerts generated by the security checks that Macie performs\. There are two types of basic alerts in Macie:
  + Managed \(Macie\-curated\) basic alerts which you cannot modify\. You can enable or disable the existing managed basic alerts\.
**Note**  
You can identify managed basic alerts by the value of `MacieDefault` in the **Created by** field in the **Basic alerts** list in the **Settings** tab\.
  + Custom basic alerts which you can create and modify to your exact specifications\. For more information, see [Adding New and Editing Existing Custom Basic Alerts](#macie-alert-working-edit-add-basic)\.
+ *Predictive alerts* \- automatic alerts based on activity within your AWS infrastructure that deviates from the established 'normal' activity baseline\. More specifically, Macie continuously monitors activity within your AWS infrastructure and builds a model of the 'normal' behavior\. It then looks for deviations from that normal baseline and when such activity is detected, it generates automatic predictive alerts\. For example a user uploading or downloading a large number of S3 objects in a single day might trigger an alert if that user typically downloads one or two S3 objects over the course of a week\.

## Alert Categories in Macie<a name="macie-alert-categories"></a>

Macie's basic alerts \(managed and custom\) can be of the following categories:
+ **Configuration compliance** \- related to compliance\-controlled content, policy, configuration settings, control and data plane logging, and patch level\.
+ **Data compliance** \- related to the discovery of compliance or security\-controlled content, such as the existence of Personally Identifiable Information \(PII\), or access credentials\.
+ **File hosting** \- related to you hosting possible malware, unsafe software, or attackers' command and control infrastructure through compromised hosts or storage services\.
+ **Service disruption** \- configuration changes that can lead to you not being able to access resources in your own environment\.
+ **Ransomware** \- potentially malicious software or activity designed to block your access to your own computer system until a sum of money is paid\.
+ **Suspicious access** \- access to your resources from a risky anomalous IP address, user, or system, such as an attacker masquerading their connection through a compromised host\.
+ **Identity enumeration** \- a series of API calls or accesses enumerating access levels to your systems that can possibly indicate the early stages of an attack or compromised credentials\.
+ **Privilege escalation** \- successful or unsuccessful attempts to gain elevated access to resources that are normally protected from an application or user, or attempts to gain access to your system or network for an extended period of time\.
+ **Anonymous access** \- attempted access to your resources from an IP address, user, or service with the intent to hide a user's true identity\. Examples include the use of proxy servers, virtual private networks and other anonymity services such as Tor\.
+ **Open permissions** \- identification of sensitive resources protected by potentially overly permissive \(and thus risky\) access control mechanisms\. 
+ **Location anomaly** \- an anomalous and risky location of the access attempt to your sensitive data\.
+ **Information loss** \- an anomalous and risky access to your sensitive data\.
+ **Credentials loss** \- possible compromise of your credentials\.

To quickly view a list of your existing alerts of a particular category, choose that category from the **Categories** list on the Macie console's **Alerts** tab\.

## Severity Levels for Alerts in Macie<a name="macie-alert-severity"></a>

Each Macie alert has an assigned severity level\. This reduces the need to prioritize one alert over another in your analyses\. It can also help you determine your response when an alert highlights a potential problem\. **Critical**, **High**, **Medium**, and **Low** levels all indicate a security issue that can result in compromised information confidentiality, integrity, and availability within your infrastructure\. The **Informational** level simply highlights a security configuration detail of your Macie\-monitored infrastructure\. Following are recommended ways to respond to each:
+ **Critical** – Describes a security issue that can result in a compromise of the information confidentiality, integrity, and availability within your infrastructure\. We recommend that you treat this security issue as an emergency and implement an immediate remediation\. The main difference between a Critical and High severity is that a Critical severity alert might be informing you of a security compromise of a large number of your resources or systems\. A High severity alert is informing you of a security compromise of one or several of your resources or systems\.
+ **High** – Describes a security issue that can result in a compromise of the information confidentiality, integrity, and availability within your infrastructure\. We recommend that you treat this security issue as an emergency and implement an immediate remediation\.
+ **Medium** – Describes a security issue that can result in a compromise of the information confidentiality, integrity, and availability within your infrastructure\. We recommend that you fix this issue at the next possible opportunity, for example, during your next service update\.
+ **Low** \- Describes a security issue that can result in a compromise of the information confidentiality, integrity, and availability within your infrastructure\. We recommend that you fix this issue as part of one of your future service updates\.
+ **Informational** – Describes a particular security configuration detail of your infrastructure\. Based on your business and organization goals, you can either simply make note of this information or use it to improve the security of your systems and resources\.

## Locating and Analyzing Macie Alerts<a name="macie-alert-working-locate"></a>

You can use the following procedure to locate and analyze existing alerts:

1. To view your generated alerts \(including **Active** and **Archived** basic or predictive alerts\), in the Macie console, navigate to the **Alerts** page\. 

   Each alert has a summary section that contains the following information:
   + Alert severity, which can be **Critical**, **High**, **Medium**, **Low**, or **Informational**\. For more information, see [Severity Levels for Alerts in Macie](#macie-alert-severity)\.
   + A timestamp that indicates when the alert was generated or last updated\.
   + The alert category \- for more information, see [Alert Categories in Macie](#macie-alert-categories)\.
   + One of the following:
     + If the alert's index is **CloudTrail data**, a user that engaged in the activity that prompted Macie to generate the alert\. For more information, see the definition of a 'user' concept in the context of Macie in [Concepts and Terminology](macie-concepts.md)
     + If the alert's index is **S3 bucket properties** or **S3 objects**, a bucket name that was involved in or that contains the objects that were involved in the activity that prompted Macie to generate the alert\.
**Important**  
In Macie, each alert is based on one of the following:   
For the alerts with the index of **CloudTrail data**, only one user \- the IAM identity whose activity prompted Macie to generate the alert\. 
For the alerts with the index of **S3 bucket properties** or **S3 objects**, only one S3 bucket that was involved in or that contains objects that were involved in the activity that prompted Macie to generate the alert\. 
   + A number of comments that were left on the alert\.
   + A total number of results, which can consist of a list of user sessions, or a list of 3 buckets, or a list of S3 objects that match the query that is included in the definition of the alert\. For more information, see [Adding New and Editing Existing Custom Basic Alerts](#macie-alert-working-edit-add-basic)\.
   + A number of views on the alert\.
   + The AWS region in which the activity captured in this alert took place\.

1. To analyze any alert further, choose the alert to expand its details pane\. The following information is included in the alert details:
   + The alert summary that includes the description and the total number of results \- a number of user sessions, or a number of S3 buckets, or a number of S3 objects that match the query that is included in the definition of the alert\.
   + A list of the alert results\. This is a list of user sessions, or a list of S3 buckets, or a list of S3 objects, depending the index that is specified in the definition for this alert\. For more information, see [Adding New and Editing Existing Custom Basic Alerts](#macie-alert-working-edit-add-basic)\.
     + If you specified **CloudTrail data** as the index, the alert details contain a list of user sessions that match the query specified in the alert definition for a particular user\.
     + If you specified **S3 buckets** as the index, the alert details contain a list of S3 buckets that match the query specified in the alert definition for a particular user\.
     + If you specified **S3 objects** as the index, the alert details contain a list of S3 objects that match the query specified in the alert definition for a particular user\.

     You can choose each result to examine it further and view all its fields\. For more information, see Researching AWS Data, Researching S3 Bucket Properties Data, or Researching S3 Objects Data sections in [Researching Through Macie\-Monitored Data](macie-research.md)

     You can also use the **Research** looking glass icon to navigate to the **Research** tab and view the results of a particular alert there\. The **Query Parser** in the **Research** tab is then pre\-populated with the query that can be used to generate these results\.

## Adding New and Editing Existing Custom Basic Alerts<a name="macie-alert-working-edit-add-basic"></a>

You can use the following procedure to add new and edit existing custom basic alerts:

1. In the Macie console, navigate to the **Settings** page and choose the icon for **Basic alerts**\.

1. On the **Basic alerts** page, either choose the edit icon for the alert that you want to modify\. Or, to add a new basic alert, choose **Add new**\.

1. Do one of the following:
   + If you're editing the existing alert, make your changes, including enabling or disabling the alert, and then choose **Save**\.
   + If you're adding a new alert, on the **Basic alert definition** page, specify the following:
     + Alert title \- for example, "An S3 bucket has an IAM policy that grants 'read' rights to everyone\." 
     + Description for the alert \- for example, "An IAM policy on an S3 bucket contains a clause that effectively grants 'read' access to any user\. It is recommended that you audit this S3 bucket and its data and confirm that this is intentional\.
     + Alert category \- for more information, see [Alert Categories in Macie](#macie-alert-categories)\.
     + Alert query \- a query that describes the activity that you want Macie to generate an alert about\. For example, `s3_world_readability:"true"`\. This query looks for an IAM policy on an S3 bucket that grants 'read' access to any user\. For more information about constructing queries, see [Constructing Queries in Macie](macie-research.md#macie-query)\.
**Note**  
You can use the looking glass icon next to an existing alert to navigate to the **Research** tab\. This alert's query is then automatically displayed in the **Query Parser** and the results of this query are displayed in the **Research** tab\. 
     + Query index \- this is the repository of data against which Macie will run the query specified in this alert\. You can select either CloudTrail data, S3 buckets, or S3 objects\. Depending on your selection, the alert will contain either a list of cloud trail user sessions \(5\-minute aggregates of raw CloudTrail data\), a list of S3 buckets, or a list of S3 objects that match the activity that your alert defines\.
     + A minimum number of activity matches that must occur before an alert is generated\.
     + Alert severity \- for more information, see [Severity Levels for Alerts in Macie](#macie-alert-severity)
     + Whitelisted users or whitelisted buckets, depending on the selected alert index\. If you whitelist a user or a bucket, Macie will not generate an alert for this user or bucket when they are involved in the activity that the alert defines\.
**Important**  
In Macie, each alert is based on one of the following:   
For the alerts with the index of **CloudTrail data**, only one user \- the IAM identity whose activity prompted Macie to generate the alert\. 
For the alerts with the index of **S3 bucket properties** or **S3 objects**, only one S3 bucket that was involved in or that contains objects that were involved in the activity that prompted Macie to generate the alert\. 

       When whitelisting a user in a basic alert with the index of CloudTrail data, you must use a special Macie format called **macieUniqueId**\. For example, 123456789012:root or 123456789012:user/Bob, or 123456789012:assumed\-role/Accounting\-Role/Mary, depending on the identity type of the user you want to whitelist\. For more information, see the definition of the 'user' concept in [Analyzing Macie\-Monitored Data by User Activity](macie-users.md)\.
     + Specify whether this alert is enabled or disabled\.

## Working with Existing Alerts<a name="macie-alert-working-archive-useful-notuseful"></a>

You can use the following procedure to archive or unarchive alerts or to choose edit the existing basic alerts\.

1. In the Macie console, navigate to the **Alerts** page and locate the alert that you want to either archive, unarchive \(if it's an archived alert\), or edit\.

1. Choose the down arrow in the alert summary pane and then choose either of the following:
   + **Archive**
**Note**  
Or **Unarchive**, if this is an archived alert\.
   + **Edit basic alert** 
**Important**  
This option is not available for predictive alerts\. You cannot edit predictive alerts, which are automatically generated by Macie based on activity within your AWS infrastructure that deviates from the established 'normal' activity baseline\. For more information, see [Basic and Predictive Macie Alerts](#macie-alert-types)\.

## Group Archiving Alerts<a name="macie-alert-working-group-archive"></a>

You can use the following procedure to group archive alerts:

1. In the Macie console's **Alerts** page, choose **Group Archive**\.

1. In the **Group archive** window, use the available settings to archive or unarchive multiple alerts at the same time\.

## Whitelisting Users or Buckets for Basic Alerts<a name="macie-alert-working-group-whitelist"></a>

Macie allows you to whitelist users \(if the alert's index is **CloudTrail data**\) and buckets \(if the alert's index is **S3 bucket properties** or **S3 objects**\) for both Macie\-managed and custom basic alerts\.

**Note**  
Macie does not allow you to whitelist users or buckets for predictive alerts\.

You can use the following procedure to whitelist a specific user or a specific bucket that engaged in or was involved in the activity that prompted Macie to generate a specific alert\.

**Important**  
In Macie, each alert is based on one the following:   
For the alerts with the index of **CloudTrail data**, only one user \- the IAM identity whose activity prompted Macie to generate the alert\. 
For the alerts with the index of **S3 bucket properties** or **S3 objects**, only one S3 bucket that was involved in or that contains objects that were involved in the activity that prompted Macie to generate the alert\. 

**Whitelist users or S3 buckets for custom basic alerts using the Alerts tab**

1. In the Macie console's **Alerts** tab, locate the custom basic alert for which you want to whitelist a user or \(S3 bucket\) listed in the alert's summary\.

1. Choose the down arrow in the alert summary pane and then choose **Whitelist user** \(if this alert's index is **CloudTrail data**\) or **Whitelist bucket** \(if the alert's index is **S3 bucket properties** or **S3 objects**\)\.

1. In the **Whitelist user** \(or **Whitelist bucket**\) window, verify the user or bucket that you want to whitelist \(automatically pre\-selected and matching the user or bucket listed in the alert's summary\), and then choose **Submit**\.

You can use the following procedure to whitelist multiple users or buckets at the same time for custom basic alerts\.

**Whitelist users or S3 buckets for custom basic alerts using the Settings tab**

1. In the Macie console's **Settings** tab, choose **Basic alerts**, and then locate the custom basic alert for which you want to whitelist users or S3 buckets\.

1. Choose the edit icon next to the alert\. 

1. Specify users or S3 buckets that you want to whitelist in either **Whitelisted users** \(if this alert's index is **CloudTrail data**\) or **Whitelisted buckets** \(if the alert's index is **S3 bucket properties** or **S3 objects**\) fields and then choose **Save**\.
**Note**  
When whitelisting a user in a basic alert with the index of CloudTrail data, you must use a special Macie format called **macieUniqueId**\. For example, 123456789012:root or 123456789012:user/Bob, or 123456789012:assumed\-role/Accounting\-Role/Mary, depending on the identity type of the user you want to whitelist\. For more information, see the definition of the 'user' concept in [Analyzing Macie\-Monitored Data by User Activity](macie-users.md)\.

**Whitelist users or S3 buckets for Macie\-managed basic alerts**

1. In the Macie console's **Alerts** tab, locate the Macie\-managed basic alert for which you want to whitelist users or S3 buckets\.

1. Choose the down arrow in the alert summary pane and then choose **Whitelist user** \(if this alert's index is **CloudTrail data**\) or **Whitelist bucket** \(if the alert's index is **S3 bucket properties** or **S3 objects**\)\.

1. In the **Whitelist user** or **Whitelist bucket** window, check the **Clone and disable the default managed alert** checkbox and then choose **Submit**\.

1. Navigate to the Macie console's **Settings** tab\. 

   Note that the original managed alert that you worked with in the previous step is now disabled\. Note also that this alert has been cloned into a new custom basic alert\. For example, if your original managed basic alert was called "An S3 bucket has an IAM policy that grants 'read' rights to everyone", this alert is now disabled and a new \(cloned\) custom basic alert called "An S3 bucket has an IAM policy that grants 'read' rights to everyone \(modified\)" is created\.

1. Choose the edit icon next to the cloned custom basic alert\.

1. Specify users or S3 buckets that you want to whitelist in either **Whitelisted users** \(if this alert's index is **CloudTrail data**\) or **Whitelisted buckets** \(if the alert's index is **S3 bucket properties** or **S3 objects**\) fields and then choose **Save**\.
**Note**  
When whitelisting a user in a basic alert with the index of CloudTrail data, you must use a special Macie format called **macieUniqueId**\. For example, 123456789012:root or 123456789012:user/Bob, or 123456789012:assumed\-role/Accounting\-Role/Mary, depending on the identity type of the user you want to whitelist\. For more information, see the definition of the 'user' concept in [Analyzing Macie\-Monitored Data by User Activity](macie-users.md)\.