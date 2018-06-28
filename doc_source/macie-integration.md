# Integrate Member Accounts and Amazon S3 with Amazon Macie<a name="macie-integration"></a>

You can use the Macie console's **Integrations** tab to integrate member accounts with Macie and to integrate Amazon S3 with Macie for both your master account and member accounts\. For more information about the master and member accounts, see [Concepts and Terminology](macie-concepts.md)\.

**Topics**
+ [Integrate Member Accounts with Macie](#macie-integration-member)
+ [Specify Data for Macie to Monitor](#macie-integration-services)
+ [Encrypted Objects](#macie-encrypted-objects)

## Integrate Member Accounts with Macie<a name="macie-integration-member"></a>

When you integrate member accounts with Macie you are enabling Macie to monitor resources and activity in these member accounts\. 

**To integrate member accounts with Macie**

1. Log in to AWS with the credentials of the AWS account that you want to integrate with Macie as a member account\.

1. 
**Important**  
This is a required step if you're adding an AWS account as a Macie member account for the first time\.  
You can skip this step if you're re\-adding an AWS account as a Macie member account after disassociating it from Macie\.

   Create the IAM role called `AmazonMacieHandshakeRole` that grants this account the required permissions to be successfully integrated with Macie\. You can create this role by launching the AWS CloudFormation stack templates found at the URLs listed below\. This role only needs to be created once for use in all regions\. For more information about AWS CloudFormation and CloudFormation stacks, see [What is AWS CloudFormation?](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) and [Working with Stacks](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)\. 
**Important**  
Make sure to specify the master AWS account ID when running the stack templates below\.
   + US East \(Virginia\): [Macie CloudFormation template for a member account](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=MacieServiceRolesMembers&templateURL=https://s3.amazonaws.com/us-east-1.macie-redirection/cfntemplates/MacieServiceRolesMember.template)
   + US West \(Oregon\): [Macie CloudFormation template for a member account](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=MacieServiceRolesMember&templateURL=https://s3-us-west-2.amazonaws.com/us-west-2.macie-redirection/cfntemplates/MacieServiceRolesMember.template)

1. Log in to AWS with the Macie master account, navigate to the Macie console, and then choose the **Integrations** tab\.

1. To integrate a member account, choose the **\+** icon next to **Member accounts**\.

1. In the **Add member AWS account\(s\)** pop up window, enter one or more AWS account IDs\. Separate multiple account numbers with commas\. Choose **Add accounts**\.

**Important**  
Once Macie is enabled in this member account, Macie is assigned a service\-linked role called `AWSServiceRoleForAmazonMacie`\. This service\-linked role includes the permissions and the trust policy that Macie requires to discover, classify, and protect sensitive data in AWS in this account and to generate alerts about potential security issues\. The following are the details of `AWSServiceRoleForAmazonMacie`:  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Resource": "*",
            "Action": [
                "cloudtrail:DescribeTrails",
                "cloudtrail:GetEventSelectors",
                "cloudtrail:GetTrailStatus",
                "cloudtrail:ListTags",
                "cloudtrail:LookupEvents",
                "iam:ListAccountAliases",
                "s3:Get*",
                "s3:List*"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": "arn:aws:cloudtrail:*:*:trail/AWSMacieTrail-DO-NOT-EDIT",
            "Action": [
                "cloudtrail:CreateTrail",
                "cloudtrail:StartLogging",
                "cloudtrail:StopLogging",
                "cloudtrail:UpdateTrail",
                "cloudtrail:DeleteTrail",
                "cloudtrail:PutEventSelectors"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::awsmacie-*",
                "arn:aws:s3:::awsmacietrail-*",
                "arn:aws:s3:::*-awsmacietrail-*"
            ],
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:DeleteBucketPolicy",
                "s3:DeleteBucketWebsite",
                "s3:DeleteObject",
                "s3:DeleteObjectTagging",
                "s3:DeleteObjectVersion",
                "s3:DeleteObjectVersionTagging",
                "s3:DeleteReplicationConfiguration",
                "s3:PutBucketPolicy"
            ]
        }
    ]
}
```
 For more information, see [Using Service\-Linked Roles for Amazon Macie](using-service-linked-roles.md)\. For more information about service\-linked roles, see [Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)\. 

If you disable Macie, the service no longer has access to the resources in your member account\. However, the `AmazonMacieHandshakeRole` IAM role that you created for this member account in Step 2 above and the `AWSServiceRoleForAmazonMacie` service\-linked role that was automatically created for this member account when it was integrated with Macie remain intact after you disable Macie\. These existing roles are used again if you decide to re\-enable Macie for this member account\. To re\-enable Macie for a member account, you must use the steps above to integrate this member account with Macie\.

## Specify Data for Macie to Monitor<a name="macie-integration-services"></a>

In order for Macie to start monitoring and classifying your data, you must specify what data you want Macie to monitor and classify\. You can use the **Integrations**/**Services** tab to specify the S3 buckets that contain the data that you want Macie to monitor\. 

**Important**  
Currently, Macie can only monitor objects stored in Amazon S3 buckets\.

**Important**  
Macie has a default limit on the amount of data that it can classify in an AWS account\. Once this data limit is reached, Macie stops classifying the data in this AWS account\. The default data classification limit is 3TB\. You can contact customer support and request an increase to the default limit\. 

You can integrate S3 with Macie \(in other words, specifying one or more S3 buckets for Macie to monitor\) during the initial Macie setup\. For more information and instructions, see [Setting Up Amazon Macie](macie-setting-up.md)\. You can also use the the following procedure to integrate S3 with Macie at any time after you've enabled Macie\.

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

## Encrypted Objects<a name="macie-encrypted-objects"></a>

If objects stored in your Amazon S3 buckets are encrypted, Macie might not be able to read and classify those objects: 
+ If your Amazon S3 objects are encrypted using [Amazon S3\-managed encryption keys \(SSE\-S3\)](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html), Macie can read and classify the objects using the roles created during the setup process\.
+ If your Amazon S3 objects are encrypted using [AWS KMS\-managed keys \(SSE\-KMS\)](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html), Macie can read and classify the objects only if you add the `AWSMacieServiceCustomerServiceRole` IAM role or the `AWSServiceRoleForAmazonMacie` service\-linked role as a [key user](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-users) for the KMS customer master key \(CMK\)\. If you don't add either of these roles as a key user for the KMS CMK, Macie cannot read and classify the objects\. However, Macie still stores metadata on the object, including which KMS CMK was used to protect the object\.
+ If your Amazon S3 objects are encrypted using client\-side encryption, Macie cannot read and classify the objects, but still stores metadata on the object\.