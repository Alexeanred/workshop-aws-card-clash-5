---
title : "Create SNS topic"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---
* In Amazon SNS, select **Topics**.
* Select the **Create topic** button.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.4.png)
* Select the **Standard** type.
* Name: ```S3EventNotificationPubMessaging```.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.5.png)
* In the Access policy section, select **Advanced**.
* Copy and edit the policy content.
``yaml
{
  "Version": "2012-10-17",
  "Id": "example-ID",
  "Statement": [
    {
      "Sid": "Example SNS topic policy",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:us-east-1:471112789042:S3EventNotificationPubMessaging",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "471112789042"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:*:*:markdown-files-150903"
        }
      }
    }
  ]
}
```
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.6.png)

* Select the **Create topic** button.

* After having the SNS topic, we return to the S3 bucket markdown-files.
* Select the **Properties** section.
* In the **Event notifications** section, we select **Create event notification**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.7.png)
* Enter the name: ```S3BucketObjectNotification```
* Select Event types: **All object create events**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.8.png)
* In the Destination section, we select **SNS topic**.
* Select **Choose from your SNS topics**.
* Select the SNS topic created above.
* Select **Save changes**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.9.png)