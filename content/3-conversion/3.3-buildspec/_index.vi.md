---
title : "Cấu hình SQS trigger lambda function"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3.3. </b> "
---
* Vào SQS là **conversionQueue** đã tạo, chọn mục **Lambda triggers**.
* Chọn **Configure Lambda function trigger**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.16.png) 
* Thay đổi **Queue polices**, ta chọn **Edit**.
* **NOTE: Thay đổi tên arn để phù hợp với tài khoản của bạn.**
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
      "Resource": "arn:aws:sqs:us-east-1:471112789042:conversionQueue",
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
      "Resource": "arn:aws:sqs:us-east-1:471112789042:conversionQueue",
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
      "Resource": "arn:aws:lambda:us-east-1:471112789042:function:ConversionService"
    }
  ]
}
```
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.6.png) 
