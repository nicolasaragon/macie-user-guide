# Setting Up Amazon Macie<a name="macie-setting-up"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services in AWS, including Amazon Macie\. If you don't have an account, use the following procedure to create one\. 

**To sign up for AWS**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign in to a different account**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

**Topics**
+ [Step 1 \- Enable Macie](#macie-setting-up-enable)
+ [Step 2 \- Integrate Amazon S3 with Macie](#macie-integrates3)
+ [Using Service\-Linked Roles for Amazon Macie](using-service-linked-roles.md)

## Step 1 \- Enable Macie<a name="macie-setting-up-enable"></a>

When you launch the Macie console for the first time, choose **Get Started** and complete the following procedure to enable Macie\. 

**Important**  
The AWS account that you use to enable Macie is automatically designated as your master account\. For more information, see [Concepts and Terminology](macie-concepts.md)\.

**To enable Amazon Macie**

1. The IAM identity \(user, role, group\) that you use to enable Macie must have the required permissions\. To grant the permissions required to enable Macie, attach the AmazonMacieFullAccess managed policy to this IAM user, group, or role\. For more information, see [AWS Managed \(Predefined\) Policies for Macie](macie-access-control.md#managed-policies)\. 

1. Use the credentials of the IAM identity from Step 1 to sign in to the Macie console\. When you open the Macie console for the first time, choose **Get Started**\. 

1. On the **Enable Amazon Macie** page, verify region preferences by reviewing the value in the drop\-down menu under the **Region** section\.
**Note**  
The region to which you are currently signed in is automatically selected\.

1. Choose **Enable Macie**\.

   Note the following about enabling Macie:
   + Macie is assigned a service\-linked role called AWSServiceRoleForAmazonMacie\. This service\-linked role includes the permissions and trust policy that Macie requires to discover, classify, and protect sensitive data in AWS on your behalf and to generate alerts about potential security issues\. To view the details of AWSServiceRoleForAmazonMacie, on the **Enable Amazon Macie** page, choose **View service role permissions**\. For more information, see [Using Service\-Linked Roles for Amazon Macie](using-service-linked-roles.md)\. For more information about service\-linked roles, see [Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)\. 
   + After you enable Macie, it immediately begins pulling and analyzing independent streams of data from AWS CloudTrail in order to generate alerts\. Because Macie only consumes this data for purposes of determining if there are potential security issues, Macie doesn't manage AWS CloudTrail for you or make its events and logs available to you\. If you have enabled AWS CloudTrail independent of Macie, you will continue to have the option to configure its settings through the AWS CloudTrail console or APIs\. For more information, see [What is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
   + You can disable Macie at any time to stop it from processing and analyzing AWS CloudTrail events\. For more information, see [Disabling Amazon Macie and Deleting Collected Metadata](macie-disable.md)\.

## Step 2 \- Integrate Amazon S3 with Macie<a name="macie-integrates3"></a>

To classify and protect your data, Macie analyzes and processes information from AWS CloudTrail and Amazon S3\. Enabling CloudTrail in your account is required in order to enable Macie\. Integrating S3 with Macie \(in other words, initially specifying one or more S3 buckets for Macie to monitor\) is not required in order to enable Macie\. However we strongly recommend that you integrate with S3 as part of setting up Macie and specify at least one S3 bucket that contains objects that you want Macie to classify and monitor\. For more information and details about how Macie classifies your data, see [Classifying Data with Amazon Macie](macie-classify-data.md)\.

When you integrate with S3, Macie creates a trail and a bucket to store the logs about the S3 object\-level API activity \(data events\) that it will now analyze along with other CloudTrail logs that it processes\. 

**Important**  
Macie has a default limit on the amount of data that it can classify in an AWS account\. Once this data limit is reached, Macie stops classifying the data in this AWS account\. The default data classification limit is 3TB\. You can contact customer support and request an increase to the default limit\. 

You can use the following procedure to integrate with S3 as part of setting up Macie:

1. Log in to AWS with the credentials of the AWS account that is serving as your Macie master account\.

1. In the Macie console's **Integrations** tab, choose the **Services** tab\. 

1. In the **Services** tab, select the account id \(master or member\) in the **Select an account** drop\-down\. The **Amazon S3** tile is then displayed\.

1. Choose the **Details** button in the **Amazon S3** tile\.

1. On the **Selected S3 buckets and prefixes** page, choose the edit icon, and then select either the full buckets \(recommended\) or bucket/prefix combinations for Macie to monitor\. You can select up to 250 S3 buckets and prefixes\. 
**Note**  
You can only select S3 buckets in your current AWS region\.
**Important**  
When you specify an S3 bucket, by default, Macie only classifies objects that are added to the bucket after your bucket selection is complete\. However you can instruct Macie to classify all existing objects in the specified S3 bucket by checking the **Classify all** checkbox\.   
If you decide to **Classify all** S3 objects in a bucket, make sure to note the following values:  
**Total size** \- total size of the data within this bucket, which is the sum of the sizes of all the objects in the bucket\. This value is displayed in the **Total size** column on the **Select S3 buckets for Macie to monitor** page\. 
**Total processed** \- total size of the data from the bucket that Macie will actually classify\. This value is displayed in the **Total processed** column on the **Select S3 buckets for Macie to monitor** page\. 
**Total cost estimate** \- the total content classification cost estimate for each S3 bucket\. This value is displayed in the **Total cost estimate** column on the **Select S3 buckets for Macie to monitor** page\. 
These values are greyed out if **Classify all** checkbox is not checked\. These values are calculated based on the snapshot of the contents of your S3 buckets taken within the last 48 hours\.  
The **Total cost estimate** for each bucket is based on the **Total processed** value for that bucket and the general Macie pricing of $5 per GB processed by the content classification engine\. The total cost estimates are provided only for S3 buckets, not for prefixes\. For more information, see [Amazon Macie Pricing](https://aws.amazon.com/macie/pricing/)\.   
The **Total processed** value for each bucket is calculated as follows:  
If an object's size is less than 1KB, 1KB is added to the **Total processed** value\. Otherwise, the object's actual size is added to the **Total processed** value\.
If the object's size is greater than 20MB, 20MB is added to the **Total processed** value\. Otherwise, the object's actual size is added to the **Total processed** value\.
For object in Amazon Glacier vaults, 0 is added to the **Total processed** value\.
Note that it is possible for the **Total processed** value of an S3 bucket to be higher than the **Total size** value\.

1. When you've finished your selections, choose **Review and Save**\. And when you've finished reviewing your selections, choose **Save**\.