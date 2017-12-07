# Users in Macie<a name="macie-users"></a>

The **Users** tab can help you draw a comprehensive picture of all of the Macie\-monitored data and activity for a particular selected user\. This topic describes how to search for the users whose activity you want to investigate further in the **Users** tab and the views that you can use in this tab to see the selected users' monitored data grouped by various interest points\. Each view provides you with one or more ways of navigating to the Macie console's **Research** tab, where you can construct and run queries in the Query Parser and conduct in\-depth investigative research of the Macie\-monitored data and activity for the selected users\. 


+ [MacieUniqueID](#macie-users-concepts)
+ [User Categories in Macie](#macie-users-categories)
+ [Investigating Users](#investigate-user)

## MacieUniqueID<a name="macie-users-concepts"></a>

In the context of Macie a user is the AWS Identity and Access Management \(IAM\) identity that makes a particular request\. Macie uses the CloudTrail `userIdentity` element to distinguish the following user types\. For more information, see [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\. 

+ Root – The request was made with your AWS account credentials\.

+ IAM user – The request was made with the credentials of an IAM user\. 

+ Assumed role – The request was made with temporary security credentials that were obtained with a role via a call to the AWS Security Token Service \(AWS STS\) AssumeRole API\. 

+ Federated user – The request was made with temporary security credentials that were obtained via a call to the AWS STS GetFederationToken API\.

+ AWS account – The request was made by another AWS account\. 

+ AWS service – The request was made by an AWS account that belongs to an AWS service\. 

When specifying a user in the Macie console, for example searching for a user in the **Users** tab or constructing a query in the **Research** tab, or whitelisting a user in a basic alert with the index of **Cloudtrail data**, you must use a special Macie format called **macieUniqueId**\. The macieUniqueId is a combination of the IAM UserIdentity element and the recipientAccountId\. For more information, see [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html) and the definition of recipientAccountId in [CloudTrail Record Contents](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)\. 

The examples below list various structures of macieUniqueId, depending on the user identity type:


| userIdentity | MacieUniqueId | 
| --- | --- | 
|  

```
"userIdentity": {
    "type": "AssumedRole"
    "arn": "arn:aws:sts::123456789012:assumed-role/Accounting-Role/Mary"
}
```  |  123456789012:assumed\-role/accounting\-role  | 
|  

```
"userIdentity": {
    "type": "IAMUser",
    "arn": "arn:aws:iam::123456789012:user/Bob",
    "userName": "Bob"
}
```  |  123456789012:user:bob  | 
|  

```
"userIdentity": {
    "type": "FederatedUser"
    "arn": "arn:aws:sts::123456789012:federated-user/Alice",
    "principalId": "123456789012:Alice",
}
```  |  123456789012:federated\-user:alice  | 
|  

```
"recipientAccountId": "123456789012",
"userIdentity": {
    "type": "AWSAccount"
    "accountId": "ANONYMOUS_PRINCIPAL",
}
```  |  123456789012:ANONYMOUS\_PRINCIPAL   | 
|  

```
"macieUniqueId": "123456789012:root:root",
"userIdentity": {
    "type": "Root"
    "sourceARN": "arn:aws:iam::123456789012:root",
}
```  |  123456789012:root:root  | 
|  

```
"userIdentity": {
    "invokedBy": "codepipeline.amazonaws.com",
    "type": "AWSService"
}
"recipientAccountId": "123456789012",
```  |  123456789012:codepipeline\.amazonaws\.com   | 
|  

```
"recipientAccoundId": "123456789012",
"userIdentity": {
    "type": "AWSAccount"
    "accountId": "987654321098",
    "principalId": "AIDABCDEFGHI123456XYZ",
}
```  |  123456789012:AIDABCDEFGHI123456XYZ  | 

## User Categories in Macie<a name="macie-users-categories"></a>

Based on their activity \(API calls\), users in Macie are grouped into the following categories:

+ **Platinum**: these IAM users or roles have a history of making high risk API calls indicative of an administrator or root user, such as creating users, authorizing security group ingress, or updating policies\. These accounts should be monitored closely for signs of account compromise\. 

+ **Gold**: these IAM users or roles have a history of making infrastructure\-related API calls indicative of a power user, such as running instances or writing data to Amazon S3\. These accounts should be monitored closely for signs of account compromise\. 

+ **Silver**: these IAM users or roles have a history of issuing high quantities of medium\-risk API calls, such as Describe\* and List\* operations, or read\-only access requests to Amazon S3\.

+ **Bronze**: these IAM users or roles typically execute lower quantities of Describe\* and List\* API calls in the AWS environment\.

## Investigating Users<a name="investigate-user"></a>

Follow this procedure to generate a comprehensive picture of all of the Macie\-monitored data and activity for the specified user:

1. In the Macie console's Users tab, specify a user name in the Search field and press Enter\.
**Note**  
When specifying a user, you must use a special Macie format called **macieUniqueId**\. For example, 123456789012:root or 123456789012:user/Bob, or 123456789012:assumed\-role/Accounting\-Role/Mary, depending on the identity type of the user you want to whitelist\. For more information, see the definition of the 'user' concept in [Concepts and Terminology](macie-concepts.md)\.

1. When the user data is generated, choose the corresponding icon to select any of the following views to display various subsets of this user's Macie\-monitored data and activity:

   + High\-risk CloudTrail events

   + High\-risk CloudTrail errors

   + Activity location

   + CloudTrail events

   + Activity ISPs

   + CloudTrail user identity types

1. If present in the selected view, locate and move the **Minimum risk** slider to the desired value\. The **Minimum risk** slider enables you to only view items with the assigned risk equal to and greater than the selected value\. 

### High\-risk CloudTrail Events<a name="user-high-risk-events"></a>

This view provides a visual representation of the top 20 \(by assigned risk and based on the value selected on the **Minimum risk** slider\) CloudTrail data and management events that occurred during the last 60 days for the selected user\. You can use the available **Daily** or **Weekly** radio buttons to modify the graph to view daily or weekly results\. 

To navigate from this view to the **Research** tab, you can select \(double\-click\) any square that represents a particular event that you would like to investigate further\. The number in parenthesis next to the event name represents the number of user sessions \(5\-minute aggregates of CloudTrail data\) in which this event is present\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the Query Parser\. For more information, see [Using the Macie Research Tab](macie-research.md)

### High\-risk CloudTrail Errors<a name="user-high-risk-errors"></a>

This view provides a visual representation of the top 20 \(by assigned risk and based on the value selected on the **Minimum risk** slider\) CloudTrail errors that occurred during the last 60 days for the selected user\. You can use the available **Daily** or **Weekly** radio buttons to modify the graph to view daily or weekly results\. 

To navigate from this view to the **Research** tab, you can select \(double\-click\) any square that represents a particular error that you would like to investigate further\. The number in parenthesis next to the error name represents the number of user sessions \(5\-minute aggregates of CloudTrail data\) in which this error is present\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\. For more information, see [Using the Macie Research Tab](macie-research.md)\.

### Activity Location<a name="user-activity-location"></a>

This view includes a map that shows the locations of activity that Macie is monitoring for a selected time period for the specified user\. To view details, you can use the available time period pull\-down menu \(past 15 days, past 30 days, past 90 days, or past year\), and then choose any location pin\. 

To navigate from this view to the **Research** tab, choose the number of events that appears in a tooltip window for a location pin\. In the **Research** tab, your selection is automatically translated into a query that is displayed in the query parser\. For more information, see [Using the Macie Research Tab](macie-research.md)\. 

### CloudTrail Events<a name="user-cloudtrail-events"></a>

This view provides the complete list of Macie\-monitored CloudTrail data and management events for the specified user\. For each event, the total count of the user sessions \(5\-minute integrations of CloudTrail data\) in which this event is present and the percentage that this total represents of the total number of user sessions is displayed\.

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed events\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. For more information, see [Using the Macie Research Tab](macie-research.md)\. 

### Activity ISPs<a name="user-activity-isp"></a>

This view provides the complete list of Macie\-monitored CloudTrail data and management events, grouped by the associated internet service providers \(ISPs\) for the specified user\. For each ISP, the total count of the user sessions \(5\-minute integrations of CloudTrail data\) in which this ISP is present and the percentage that this total represents of the total number of user sessions is displayed\. 

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed themes\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. For more information, see [Using the Macie Research Tab](macie-research.md)\. 

### CloudTrail User Identity Types<a name="user-cloudtrail-user-identity-type"></a>

This view provides the complete list of Macie\-monitored CloudTrail data and management events, grouped by the user identity type \(for more information, see the definition for 'user' in [Concepts and Terminology](macie-concepts.md)\) for the specified user\. For each user identity type, the total count of the user sessions \(5\-minute integrations of CloudTrail data\) in which this user identity type is present and the percentage that this total represents of the total number of user sessions is displayed\. 

To navigate from this view to the **Research** tab, you can choose the looking glass icon next to any of the displayed themes\. Your selection is automatically translated into a query that is displayed in the query parser in the **Research** tab\. For more information, see [Using the Macie Research Tab](macie-research.md)\. 