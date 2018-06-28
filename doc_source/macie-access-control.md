# Access Control in Amazon Macie<a name="macie-access-control"></a>

The master account users have access to the Macie console where they can configure Macie and use it to monitor and protect the resources in both master and member accounts\. \(For more information about master and member accounts, see [Concepts and Terminology](macie-concepts.md) and [Integrate Member Accounts and Amazon S3 with Amazon Macie](macie-integration.md)\)\. 

## Grant Macie Administrator Access<a name="admin-access"></a>

In order for the master account users to be able to use the Macie console, they must be granted the required permissions\. To ensure this, you can use the following policy document to create and attach an IAM policy to any user identity type that belongs to your master Macie account\. This policy grants master account users permissions to use the Macie console in its full capacity:

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Resource":"*",
         "Action":[
            "macie:*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":"iam:CreateServiceLinkedRole",
         "Resource":"*",
         "Condition":{
            "StringLike":{
               "iam:AWSServiceName":"macie.amazonaws.com"
            }
         }
      }
   ]
}
```

## Grant Macie Read\-Only Access<a name="read-access"></a>

In order for a user to view any data in the Macie console, they must be granted the required permissions\. To grant read\-only access, you can create a custom policy using the following policy document and attach it to a IAM user, group, or role\. This policy grants users permissions to only view information in the Macie console:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "macie:Get*",
                "macie:List*",
                "macie:Describe*"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow"
        }
    ]
}
```

**Note**  
Currently, there is no AWS managed policy that can be used to grant read\-only access to Macie\. 

## AWS Managed \(Predefined\) Policies for Macie<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\.

The following AWS managed policies, which you can attach to users in your account, are specific to Macie:
+ **AmazonMacieFullAccess** \- Provides full access to Macie\. 
+ **AmazonMacieSetupRole** \- Provides Macie with access to your AWS account\. 
+ **AmazonMacieServiceRole** \- Grants Macie read\-only access to resource dependencies in your account in order to enable data analysis\. 