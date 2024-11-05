---
title : "Tạo lambda functions và cấu hình IAM role"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2. </b> "
---
### Tạo lambda function
* Vào lambda, chọn **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.1.png) 
* Function name: ```ConversionService```.
* Runtime: Python3.12 .
* Các thông số khác để mặc định, ta chọn **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.2.png) 
### Cấu hình IAM role
* Sau khi tạo function xong, vào mục **Configuration**, chọn **Permissions**.
* Nhấp vào đường dẫn role name function đã tự tạo.
* Ta chọn **Add permissions** và chọn **Attach policies**.
* Thêm policy: ```AmazonS3FullAccess```.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.3.png)
* Sau khi thêm xong policy, ta chọn **Add permissions** và chọn **Create inline policy**.
* Ở policy editor, Chọn JSON, thêm policy sau:
```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sqs:DeleteMessage",
                "sqs:GetQueueAttributes",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:471112789042:conversionQueue"
        }
    ]
}
```
* Đặt policy name: ```sqs-role```.
* Chọn **Create policy**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.4.png) 

### Thêm lambda layer vào function
* Ở lambda function, ở phần Code kéo xuống có phần Layers, ta chọn **Add a layer**.
* Chọn custom layers.
* Chọn layer đã tạo để thêm thư viện markdown cho lambda function này.
* Chọn version 1.
* Chọn **Add**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.7.png) 

### Thêm lambda code vào
* Code của lambda function:
```python
import json
import boto3
import markdown

s3_client = boto3.client('s3')

def lambda_handler(event, context):
    # Log toàn bộ event để kiểm tra
    print(json.dumps(event))
    
    # Lấy body của SQS message (chứa SNS notification)
    for record in event['Records']:
        sns_message = json.loads(record['body'])
        s3_event = json.loads(sns_message['Message'])
        
        # Trích xuất thông tin bucket và object từ S3 event
        bucket_name = s3_event['Records'][0]['s3']['bucket']['name']
        object_key = s3_event['Records'][0]['s3']['object']['key']
        
        # Tải file markdown từ S3
        response = s3_client.get_object(Bucket=bucket_name, Key=object_key)
        markdown_content = response['Body'].read().decode('utf-8')
        
        # Convert markdown to HTML
        html_content = markdown.markdown(markdown_content)
        
        # Lưu HTML vào S3 (có thể sử dụng một bucket khác để làm static web hosting)
        html_key = object_key.replace(".md", ".html")
        output_bucket = 'html-converted-output'  # Thay bằng bucket chứa file HTML
        s3_client.put_object(Bucket=output_bucket, Key=html_key, Body=html_content, ContentType='text/html')
        
        return {
            'statusCode': 200,
            'body': json.dumps(f"File {object_key} đã được convert và lưu vào {output_bucket}/{html_key}")
        }

```