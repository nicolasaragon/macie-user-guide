# Classify Your Data with Macie<a name="macie-classify-data"></a>

Macie can help you classify your sensitive and business\-critical data stored in the cloud\. Currently, Macie analyzes and processes data stored in AWS S3 buckets\. To classify your data, Macie also uses CloudTrail's ability to capture object\-level API activity on S3 objects \(data events\)\. However, Macie only monitors CloudTrail data events if you specify at least one S3 bucket for Macie to monitor\. 

Once you specify the S3 bucket\(s\) for Macie to monitor, you enable Macie to continuously monitor and discover new data as it enters your AWS infrastructure\. For more information on how to specify S3 buckets for Macie to monitor, see [Specify Data for Macie to Monitor](macie-integration.md#macie-integration-services)\.

**Note**  
Macie's content classification engine processes up to the first 20 MB of an S3 object\. For more information, see [Specify Data for Macie to Monitor](macie-integration.md#macie-integration-services)\.


+ [Classify data with Macie](#classify-objects)
+ [Object Risk Level](#compound-score)
+ [Retention Duration for S3 Metadata](#metadata-retention-duration)

## Classify data with Macie<a name="classify-objects"></a>

**Important**  
Currently, Macie supports the following compression and archive file formats:  
BZIP
GZIP
LZO
RAR
SNAPPY
AR
CPIO
Unix dump
TAR
zip
XZ
Pack200
BZIP2
7z
ARJ
LZMA
DEFLATE
Brotli

Once Macie begins monitoring your data, it uses the following automatic content classification methods to identify and prioritize your sensitive and critical data and to accurately assign business value to your data\.

### Content Type<a name="classify-objects-content-type"></a>

To classify your data objects by a content type, Macie uses an identifier that is embedded in the file header\. Macie offers a set of managed \(Macie\-curated\) content types, each with a designated risk level between 1 and 10\. 

Macie can assign only one content type to an object\.

You cannot modify existing or add new content types\. You can enable or disable the existing content types, thus instructing Macie to either include or exclude them in its data classification process\.<a name="enable-disable-content-types"></a>

**To view, enable, or disable content types**

1. In the Macie console, navigate to the **Settings** page\.

1. In the **Classify data** section, choose **Content types**\.

1. Choose any of the listed managed content types to view its details\.

   To enable or disable a content type, on its details page, use the **Enabled/Disabled** drop\-down menu, and then choose **Save**\.

### File Extension<a name="classify-objects-file-extension"></a>

Macie can also classify your objects by their file extensions\. Macie offers a set of managed file extensions, each with a designated risk level between 1 and 10\. 

Macie can assign only one file extension to an object\.

You cannot modify existing or add new file extensions\. You can enable or disable the existing file extensions, thus instructing Macie to either include or exclude them in its data classification process\.<a name="enable-disable-file-extensions"></a>

**To view, enable, or disable file extensions**

1. In the Macie console, navigate to the **Settings** page\.

1. In the **Classify data** section, choose **File extensions**\.

1. Choose any of the listed managed file extensions to view its details\.

   To enable or disable a file extension, on its details page, use the **Enabled/Disabled** drop\-down menu, and then choose **Save**\.

### Theme<a name="classify-objects-theme"></a>

Object classification by theme is based on keywords that Macie searches for as it examines the contents of data objects\. Macie offers a set of managed themes, each with a designated risk level between 1 and 10\. 

Macie can assign one or more themes to an object\.

You cannot modify existing or add new themes\. You can enable or disable the existing themes, thus instructing Macie to either include or exclude them in its data classification process\.<a name="enable-disable-themes"></a>

**To view, enable, or disable themes**

1. In the Macie console, navigate to the **Settings** page\.

1. In the **Classify data** section, choose **Themes**\.

1. Choose any of the listed managed themes to view its details\.

   To enable or disable a theme, on its details page, use the **Enabled/Disabled** drop\-down menu, and then choose **Save**\.

### Regex<a name="classify-objects-regex"></a>

Object classification by regex is based on specific data or data patterns that Macie searches for as it examines the contents of data objects\. Macie offers a set of managed regex, each with a designated risk level between 1 and 10\. 

Macie can assign one or more regex to an object\.

You cannot modify existing or add new regex\. You can enable or disable the existing regex, thus instructing Macie to either include or exclude them in its data classification process\.<a name="enable-disable-regex"></a>

**To view, enable, or disable regex**

1. In the Macie console, navigate to the **Settings** page\.

1. In the **Classify data** section, choose **Regex**\.

1. Choose any of the listed managed regex to view its details\.

   To enable or disable a regex, on its details page, use the **Enabled/Disabled** drop\-down menu, and then choose **Save**\.

### Personally Identifiable Information \(PII\)<a name="classify-objects-pii"></a>

Object classification by PII is based on recognizing any personally identifiable artifacts based on industry standards such as NIST\-80\-122 and FIPS 199\. Macie is able to recognize the following PII artifacts: 

+ Full names

+ Mailing addresses

+ Email addresses

+ Credit card numbers

+ IP addresses \(IPv4 and IPv6\)

+ Drivers license IDs \(USA\)

+ National identification numbers \(USA\)

+ Birth dates

As part of PII object classification, Macie also assigns each matching object a PII impact of high, moderate, and low using the following criteria:

+ High

  + >= 1 full name and credit card

  + >= 50 names or emails and any combination of other PII

+ Moderate

  + >= 5 names or emails and any combination of other PII

+ Low

  + 1\-5 names or emails and any combination of PII

  + Any quantity of PII attributes above \(without names or emails\)

### Support Vector Machine\-Based Classifier<a name="classify-objects-classifier"></a>

Another method that Macie uses to classify your S3 objects is a Support Vector Machine \(SVM\) classifier that classifies content inside your Macie\-monitored S3 objects \(text, token n\-grams, and character n\-grams\) as well as their metadata features \(document length, extension, encoding, headers\) in order to achieve accurate classification of documents based on content\. This Macie\-managed classifier was trained against a large corpus of training data of various types and has been optimized to support accurate detection of various content types, including source code, application logs, regulatory documents, and database backups\. Also, the classifier has the ability to generalize its detections\. For example, if it detected a new kind of source code that doesn't match any of the types of source code that it is trained to recognize, it can generalize the detection as being just "source code"\. 

**Note**  
This data classification method is not surfaced in the Macie's Settings\. The following list of artifacts is Macie\-managed and cannot be edited, enabled, or disabled\.

The SVM classifier in Macie is trained to detect the following content types:

+ E\-books

+ Email

+ Generic encryption keys

+ Financial

  + SEC regulatory forms

+ JSON

  + AWS CloudTrail logs

  + Jupyter notebooks

+ Application logs

  + Apache format

  + AWS S3 server logs

  + Linux syslog

+ Database

  + MongoDB backup

  + MySQLbackup

  + MySQL script

+ Source code

  + F\#

  + VimL

  + ActionScript

  + Assembly

  + Bash

  + Batchfile

  + C

  + Clojure

  + Cobol

  + CoffeeScript

  + CUDA

  + Erlang

  + Fortran

  + Go

  + Haskell

  + Java

  + JavaScript

  + LISP

  + Lua

  + Matlab

  + ObjectiveC

  + Perl

  + PHP

  + PowerShell

  + Processing

  + Python

  + R

  + Ruby

  + Scala

  + Swift

  + VHDL

+ Web languages

  + CSS

  + HTML

  + XML

## Object Risk Level<a name="compound-score"></a>

Through the automatic classification methods described above, a Macie\-monitored object is assigned various risk levels based on each content type, file extension, theme, regex, PII, and SVM artifact that is assigned to it\. The object's compound \(final\) risk level is then set to the highest value of its assigned risk levels\.

## Retention Duration for S3 Metadata<a name="metadata-retention-duration"></a>

Macie stores metadata about your S3 objects for the default duration of 1 month\. You can extend this duration up to 12 months\.