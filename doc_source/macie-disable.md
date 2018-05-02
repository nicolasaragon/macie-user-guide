# Disabling Amazon Macie and Deleting Collected Metadata<a name="macie-disable"></a>

You can use the Macie general settings page in the Macie console to disable Macie\.

If you disable Macie, it will no longer have access to the resources in the master account and all member accounts\. You must add member accounts again if you decide to re\-enable Macie\.

 If you disable Macie, it stops processing the resources in the master account and all member accounts\. Once Macie is disabled, the metadata that Macie collected while monitoring the data in your master and member accounts is deleted\. Within 90 days from disabling Macie, all of this metadata is expired from the Macie system backups\. 

**Important**  
Disabling Macie does not prompt the deletion of your other data within your AWS accounts\. Only the metadata that was collected by Macie while it monitored your accounts is deleted once Macie is disabled\.

1. Navigate to the **Macie general settings** page by choosing the down arrow next to your signed in name\.

1. In the **Macie general settings** page, check the following checkboxes:
   + **I understand that if I disable Macie, the service will no longer have access to the resources in the master account and all member accounts\. You must add member accounts again if you decide to re\-enable Macie\.**
   +  **I understand that if I disable Macie, the service will stop processing the resources in the master account and all member accounts\. All metadata that Macie collected while monitoring the data in these accounts will be deleted\. **

1. Choose **Disable Amazon Macie**\.