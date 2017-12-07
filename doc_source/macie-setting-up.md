# Setting Up Amazon Macie<a name="macie-setting-up"></a>


+ [Step 1 \- Enable Macie](#macie-setting-up-enable)
+ [Step 2 \- Integrate Amazon S3 with Macie](#macie-integrates3)

## Step 1 \- Enable Macie<a name="macie-setting-up-enable"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services in AWS, including Amazon Macie\. If you don't have an account, use the following procedure to create one\. 

**To sign up for AWS**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign In to the Console**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

When you launch the Macie console for the first time, choose **Get Started** and complete the following procedure to enable Macie\. 

**Important**  
The AWS account that you use to enable Macie is automatically designated as your master account\. For more information, see [Concepts and Terminology](macie-concepts.md)\.

**To enable Amazon Macie**

On the **Enable Amazon Macie** page, complete the following steps:

1. Verify region preferences by reviewing the value in the drop\-down menu under the **Region** section\.
**Note**  
The region that you are currently signed in to is automatically selected\.

1. **Required:** create the Identity Access Management \(IAM\) roles that provide Macie with access to your AWS account\. You can create these roles and the required policies by launching the AWS CloudFormation stack templates found at the URLs listed below\. These roles only need to be created once for use in all regions\. For more information about AWS CloudFormation and CloudFormation stacks, see [What is AWS CloudFormation?](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) and [Working with Stacks](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)\. 

   + For US East \(Virginia\): [Macie CloudFormation template for a master account](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=MacieServiceRolesMaster&templateURL=https://s3.amazonaws.com/us-east-1.macie-redirection/cfntemplates/MacieServiceRolesMaster.template) 

   + US West \(Oregon\): [Macie CloudFormation template for a master account](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=MacieServiceRolesMaster&templateURL=https://s3-us-west-2.amazonaws.com/us-west-2.macie-redirection/cfntemplates/MacieServiceRolesMaster.template)

1. **Required**: make sure that AWS CloudTrail is enabled in your account\. If CloudTrail is not enabled, you must navigate to the AWS CloudTrail console and enable AWS CloudTrail\. For more information, see [Overview for Creating a Trail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)\.

1. Grant Macie the required permissions to access your CloudTrail data by checking the checkbox in the **Permissions** section\.

1. Choose **Enable Macie**\.

## Step 2 \- Integrate Amazon S3 with Macie<a name="macie-integrates3"></a>

To classify and protect your data, Macie analyzes and processes information from AWS CloudTrail and Amazon S3\. Enabling CloudTrail in your account is required in order to enable Macie\. Integrating S3 with Macie \(in other words, initially specifying one or more S3 buckets for Macie to monitor\) is not required in order to enable Macie\. However we strongly recommend that you integrate with S3 as part of setting up Macie and specify at least one S3 bucket that contains objects that you want Macie to classify and monitor\. For more information and details about how Macie classifies your data, see [Classify Your Data with Macie](macie-classify-data.md)\.

When you integrate with S3, Macie creates a trail and a bucket to store the logs about the S3 object\-level API activity \(data events\) that it will now analyze along with other CloudTrail logs that it processes\. 

**Important**  
Macie has a default limit on the amount of data that it can classify in an AWS account\. Once this data limit is reached, Macie stops classifying the data in this AWS account\. The default data classification limit is 3TB\. You can contact customer support and request an increase to the default limit\. 

You can use the following procedure to integrate with S3 as part of setting up Macie:

1. Log in to AWS with the credentials of the AWS account that is serving as your Macie master account\.

1. Navigate to the Macie console's **Integrations** tab and choose the **Services** tab\. 

1. In the **Services** tab, select the account id \(master or member\) in the **Select an account** drop\-down\. The **Amazon S3** tile is then displayed\.

1. Choose the **Add** button in the **Amazon S3** tile\.

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