# Monitoring Amazon Macie Alerts with Amazon CloudWatch Events<a name="macie-cloudwatch"></a>

Amazon Macie sends notifications based on Amazon CloudWatch Events when any change in the Macie alerts takes place\. This includes newly generated alerts and updates to existing alerts\. Notifications are sent for all Macie alert types, including predictive alerts and basic alerts, both managed and custom\. For more information about alert types, see [Amazon Macie Alerts](macie-alerts.md)\.

Macie sends notifications based on Amazon CloudWatch Events for the alerts generated in both master and member Macie accounts\. However, only the master Macie account has access to the generated CloudWatch events\. For more information about master and member accounts, see [Concepts and Terminology](macie-concepts.md)\.

The CloudWatch [event](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) for Macie has the following format: 

**Note**  
In the sample format below, the fictional account ID of "111122223333" represents the ID of the master Macie account\.

```
{
  "version": "0",
  "id": "CWE-event-id",
  "detail-type": "Macie Alert",
  "source": "aws.macie",
  "account": "111122223333",
  "time": "2017-04-24T22:28:49Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id/alert/alert_id",
    "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id"
  ],
  "detail": {
    "notification-type": "ALERT_CREATED",
    "name": "Scanning bucket policies",
    "tags": [
      "Custom_Alert",
      "Insider"
    ],
    "url": "https://lb00.us-east-1.macie.aws.amazon.com/111122223333/posts/alert_id",
    "alert-arn": "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id/alert/alert_id",
    "risk-score": 8,
    "trigger": {
      "rule-arn": "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id",
      "alert-type": "basic",
      "created-at": "2017-01-02 19:54:00.644000",
      "description": "Alerting on failed enumeration of large number of bucket policies",
      "risk": 8
    },
    "created-at": "2017-04-18T00:21:12.059000",
    "actor": "555566667777:assumed-role:superawesome:aroaidpldc7nsesfnheji",
    "summary": {ALERT_DETAILS_JSON}
  }
}
```

You can complete the following procedure to configure your master Macie account to receive CloudWatch events from Macie and to pipe those events into an Amazon Simple Queue Service \(SQS\) queue\. Before completing this procedure, make sure to create the SQS queue for the events from Macie\. For more information, see [Tutorial: Creating an Amazon SQS Queue](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html)\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\. 

1. In the navigation pane, choose **Events**, and then choose **Create rule**\. 

1. Choose **Edit** and type in the following event pattern for the Macie events:

   ```
   {
      "source": [	
      "aws.macie"
     ]
   }
   ```

1. In the **Targets** pane, choose **Add target**, select **SQS queue** in the target drop\-down menu, and then specify your queue for the events from Macie\.

You should now be able to see Macie alerts in your specified queue in the SQS console\.