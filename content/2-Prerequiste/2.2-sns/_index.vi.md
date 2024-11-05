---
title : "Tạo SNS topic"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---
* Ở Amazon SNS, chọn mục **Topics**.
* Chọn nút **Create topic**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.4.png) 
* Chọn loại **Standard**.
* Name: ```S3EventNotificationPubMessaging```.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.5.png) 
* Ở mục Access policy, chọn **Advanced**.
* Copy và chỉnh sửa lại nội dung policy.
```yaml
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

* Chọn nút **Create topic**.

* Sau khi có SNS topic xong, ta quay lại S3 bucket markdown-files.
* Chọn mục **Properties**.
* Ở mục **Event notifications**, ta chọn **Create event notification**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.7.png) 
* Điền tên: ```S3BucketObjectNotification```
* Chọn Event types: **All object create events**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.8.png) 
* Phần Destination, ta chọn **SNS topic**.
* Chọn **Choose from your SNS topics**.
* Chọn SNS topic đã tạo ở trên.
* Chọn **Save changes**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.9.png) 

