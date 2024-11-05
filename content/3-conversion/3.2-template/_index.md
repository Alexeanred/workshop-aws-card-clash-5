---
title : "Create lambda functions and configure IAM role"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2. </b> "
---
### Create lambda function
* Go to lambda, select **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.1.png)
* Function name: ```ConversionService```.
* Runtime: Python3.12 .
* Leave other parameters as default, select **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.2.png)
### Configure IAM role
* After creating the function, go to **Configuration**, select **Permissions**.
* Click on the path of the role name function that you created.
* Select **Add permissions** and select **Attach policies**.

* Add policy: ```AmazonS3FullAccess```.

* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.3.png)
* After adding the policy, select **Add permissions** and select **Create inline policy**.

* In the policy editor, select JSON, add the following policy:
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
* Set policy name: ```sqs-role```.
* Select **Create policy**.

* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.4.png)

### Add lambda layer to function
* In lambda function, in Code section, pull down to Layers section, select **Add a layer**.

* Select custom layers.
* Select the created layer to add markdown library for this lambda function.
* Select version 1.
* Select **Add**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.7.png)

### Add lambda code
* Lambda function code:
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