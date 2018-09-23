# Using Service\-Linked Roles for Amazon Macie<a name="using-service-linked-roles"></a>

Amazon Macie uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Macie\. Service\-linked roles are predefined by Macie and include all the permissions that Macie requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Macie easier because you don't have to manually add the necessary permissions\. Macie defines the permissions of its service\-linked role, and unless the permissions are defined otherwise, only Macie can assume the role\. The defined permissions include the trust policy and the permissions policy, and that permissions policy can't be attached to any other IAM entity\.

You can delete the Macie service\-linked role only after first disabling Macie\. This protects your Macie resources because you can't inadvertently remove permission to access them\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide* and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-Linked Role Permissions for Macie<a name="slr-permissions"></a>

Macie uses the service\-linked role named `AWSServiceRoleForAmazonMacie`\. It allows Amazon Macie to discover, classify, and protect sensitive data in AWS on your behalf\.

The `AWSServiceRoleForAmazonMacie` service\-linked role trusts the following services to assume the role:
+ `macie.amazonaws.com`

The role permissions policy allows Macie to complete the following actions on the specified resources:
+ Action: `iam:CreateServiceLinkedRole` 
+ Resources: `arn:aws:iam::*:role/aws-service-role/macie.amazonaws.com/AWSServiceRoleForAmazonMacie`

The IAM entity \(such as a user, group, or role\) you use to create the `AWSServiceRoleForAmazonMacie` service\-linked role will need privileges. To grant the required privileges for creating a service\-linked role, see [Service\-Linked Role Permissions](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a Service\-Linked Role for Macie<a name="create-slr"></a>
+ **For the master Macie account**, the `AWSServiceRoleForAmazonMacie` service\-linked role is automatically created when you enable Macie for the first time or enable Macie in a supported region where you previously didn't have it enabled\. You can also create the `AWSServiceRoleForAmazonMacie` service\-linked role manually for the master account using the IAM console, the IAM CLI, or the IAM API\. 
+ **For member Macie accounts**, the `AWSServiceRoleForAmazonMacie` service\-linked role is automatically created when the master account associates a member account with Macie\. You can also create `AWSServiceRoleForAmazonMacie` for member accounts manually using the IAM console, the IAM CLI, or the IAM API\.
**Important**  
The service\-linked role that is created for the master Macie account doesn't apply to the member Macie accounts\.

For more information about creating the role manually, see [Creating a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.

**Important**  
If you were using the Macie service before June 21, 2018, when it began supporting service\-linked roles, the IAM roles that grant Macie access to call other AWS services on your behalf already exist in your AWS account \(Macie master or member\)\. These roles are `AmazonMacieServiceRole` and `AmazonMacieSetupRole`\. They were created when you launched either the Macie AWS CloudFormation template for a master account or the Macie AWS CloudFormation template for a member account as part of setting up Macie\.   
The newly created service\-linked role replaces these previously created IAM roles \(in master and member accounts\)\.   
These previously created IAM roles aren't deleted\. They remain intact, but they're no longer used to grant Macie access to call other AWS services on your behalf\. You can use the IAM console to manage or delete these IAM roles\. 

## Editing a Service\-Linked Role for Macie<a name="edit-slr"></a>

Macie doesn't allow you to edit the `AWSServiceRoleForAmazonMacie` service\-linked role\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a Service\-Linked Role for Macie<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don't have an unused entity that isn't actively monitored or maintained\. 

**Important**  
For a master account, you must first disable Macie in order to delete the `AWSServiceRoleForAmazonMacie`\.  
 For Macie member accounts, in order to delete the service\-linked role used in a member account, the master Macie account must first disassociate this member account from Macie\.  
If the Macie service isn't disabled when you try to delete the service\-linked role, the deletion fails\. For more information, see [Disabling Amazon Macie and Deleting Collected Metadata](macie-disable.md)\.

When you disable Macie, the `AWSServiceRoleForAmazonMacie` is NOT automatically deleted\. If you then enable Macie again, it'll start using the existing `AWSServiceRoleForAmazonMacie`\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the IAM CLI, or the IAM API to delete the `AWSServiceRoleForAmazonMacie` service\-linked role\. For more information, see [Deleting a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.
