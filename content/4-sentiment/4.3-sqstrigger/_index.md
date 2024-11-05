---
title : "Configuring SQS trigger lambda functions"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

* Go to SQS is the **sentimentQueue** created, select **Lambda triggers**.

* Select **Configure Lambda function trigger**.

* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.13.png)

* Enter lambda function SentimentAnalysis and select **Save**.

* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.14.png)
* Change **Queue polices**, we select **Edit**.

* **NOTE: Change the arn name to match your account.**
```yaml
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "sqs:SendMessage",
      "Resource": "arn:aws:sqs:us-east-1:471112789042:sentimentQueue",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-east-1:471112789042:S3EventNotificationPubMessaging"
        }
      }
    },
    {
      "Sid": "topic-subscription-arn:aws:sns:us-east-1:471112789042:S3EventNotificationPubMessaging",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "SQS:SendMessage",
      "Resource": "arn:aws:sqs:us-east-1:471112789042:sentimentQueue",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "arn:aws:sns:us-east-1:471112789042:S3EventNotificationPubMessaging"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "lambda:CreateEventSourceMapping",
        "lambda:ListEventSourceMappings",
        "lambda:ListFunctions"
      ],
      "Resource": "arn:aws:lambda:us-east-1:471112789042:function:SentimenntAnalysis"
    }
  ]
}
```
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.15.png) 
