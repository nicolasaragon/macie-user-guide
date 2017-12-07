# Protect Your Data with Macie<a name="macie-protect-data"></a>


+ [CloudTrail events](#cloud-trail-events)
+ [CloudTrail errors](#cloud-trail-errors)

Macie can help you monitor how your sensitive and business\-critical data stored in the cloud is being used\. Macie applies artificial intelligence to understand historical data access patterns and automatically assesses activity of users, applications and service accounts\. This can help you detect unauthorized access and avoid data leaks\.

Once you enable Macie it uses the following automated methods to protect your data:

## CloudTrail events<a name="cloud-trail-events"></a>

Macie analyzes and processes a subset of CloudTrail\-logged data and management events \(API calls\) that can occur within your infrastructure\. Macie designates a risk level between 1 and 10 for each of the supported CloudTrail events\. 

You cannot modify existing or add new CloudTrail events to the Macie\-managed list\. You can enable or disable the supported CloudTrail events, thus instructing Macie to either include or exclude them in its data security process\.

**To view, enable, or disable supported CloudTrail events**

1. In the Macie console, navigate to the **Settings** page\.

1. In the **Protect data** section, choose **AWS CloudTrail events**\.

1. Choose any of the listed events to view its details\.

   To enable or disable an event, on its details page, use the **Enabled/Disabled** drop\-down menu, and then choose **Save**\.

## CloudTrail errors<a name="cloud-trail-errors"></a>

Macie analyzes and processes errors that can occur when Macie\-supported subset of CloudTrail\-logged data and management events \(API calls\) take place within your infrastructure\. Macie designates a risk level between 1 and 10 for each of the supported CloudTrail errors\. 

You cannot modify existing or add new CloudTrail errors to the Macie\-managed list\. You can enable or disable the supported CloudTrail errors, thus instructing Macie to either include or exclude them in its data security process\.

**To view, enable, or disable supported CloudTrail errors**

1. In the Macie console, navigate to the **Settings** page\.

1. In the **Protect data** section, choose **AWS CloudTrail errors**\.

1. Choose any of the listed errors to view its details\.

   To enable or disable an error, on its details page, use the **Enabled/Disabled** drop\-down menu, and then choose **Save**\.