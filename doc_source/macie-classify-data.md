# Classifying Data with Amazon Macie<a name="macie-classify-data"></a>

Macie can help you classify your sensitive and business\-critical data stored in the cloud\. Currently, Macie analyzes and processes data stored in AWS S3 buckets\. To classify your data, Macie also uses CloudTrail's ability to capture object\-level API activity on S3 objects \(data events\)\. However, Macie only monitors CloudTrail data events if you specify at least one S3 bucket for Macie to monitor\. 

Once you specify the S3 bucket\(s\) for Macie to monitor, you enable Macie to continuously monitor and discover new data as it enters your AWS infrastructure\. For more information on how to specify S3 buckets for Macie to monitor, see [Specify Data for Macie to Monitor](macie-integration.md#macie-integration-services)\.

**Note**  
Macie's content classification engine processes up to the first 20 MB of an S3 object\. 

If you specify S3 buckets that include files of a format that is not supported in Macie, then Macie does not classify them and your Macie usage charges do not include any costs for this content\. Your Macie usage charges include only the costs for the content that Macie processes\. For example, Macie cannot extract text from \.wav files \(images or movies\), therefore it doesn’t process that content and you’re not charged for it\.

**Topics**
+ [Supported Compression and Archive File Formats](macie-compression-archive-formats.md)
+ [Content Type](macie-classify-objects-content-type.md)
+ [File Extension](macie-classify-objects-file-extension.md)
+ [Theme](macie-classify-objects-theme.md)
+ [Regex](macie-classify-objects-regex.md)
+ [Personally Identifiable Information \(PII\)](macie-classify-objects-pii.md)
+ [Support Vector Machine\-Based Classifier](macie-classify-objects-classifier.md)
+ [Object Risk Level](#compound-score)
+ [Retention Duration for S3 Metadata](#metadata-retention-duration)

## Object Risk Level<a name="compound-score"></a>

Through the automatic classification methods described above, a Macie\-monitored object is assigned various risk levels based on each content type, file extension, theme, regex, PII, and SVM artifact that is assigned to it\. The object's compound \(final\) risk level is then set to the highest value of its assigned risk levels\.

## Retention Duration for S3 Metadata<a name="metadata-retention-duration"></a>

Macie stores metadata about your S3 objects for the default duration of 1 month\. You can extend this duration up to 12 months\.