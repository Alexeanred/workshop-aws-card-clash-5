---
title : "Tạo lambda functions và cấu hình IAM role"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 4.2 </b> "
---






### Tạo lambda function
* Vào lambda, chọn **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.8.png) 
* Function name: ```ConversionService```.
* Runtime: Python3.12 .
* Các thông số khác để mặc định, ta chọn **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.9.png) 
### Cấu hình IAM role
* Sau khi tạo function xong, vào mục **Configuration**, chọn **Permissions**.
* Nhấp vào đường dẫn role name function đã tự tạo.
* Ta chọn **Add permissions** và chọn **Attach policies**.
* Thêm policy: ```AmazonS3FullAccess```, ```AmazonDynamoDBFullAccess```, ```ComprehendFullAccess```
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.10.png) 
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
            "Resource": "arn:aws:sqs:us-east-1:471112789042:sentimentQueue"
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
* Chọn layer đã tạo để thêm thư viện markdown, bs4 cho lambda function này.
* Chọn version 1.
* Chọn **Add**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.11.png) 

### Thêm lambda code vào
* Code của lambda function:
```python
import boto3
import json
import markdown
from bs4 import BeautifulSoup
from boto3.dynamodb.conditions import Key
from decimal import Decimal  # Import Decimal để chuyển đổi float

s3 = boto3.client('s3')
comprehend = boto3.client('comprehend')
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('SentimentAnalysisResults')  # DynamoDB table

def lambda_handler(event, context):
    try:
        print("Event received: ", event)  # Log toàn bộ event
        
        # Lấy message từ SQS
        for record in event['Records']:
            # Load body của message
            body = json.loads(record['body'])
            print("SQS Message Body: ", body)  # Log body của message
            
            # Lấy phần "Message" từ SNS
            sns_message = json.loads(body['Message'])
            print("SNS Message: ", sns_message)  # Log SNS message
            
            # Truy xuất thông tin từ S3 event
            s3_info = sns_message['Records'][0]['s3']
            bucket_name = s3_info['bucket']['name']
            file_key = s3_info['object']['key']
            
            # Log thông tin bucket và file
            print(f"Bucket: {bucket_name}, File: {file_key}")

            # Tải file từ S3
            s3_object = s3.get_object(Bucket=bucket_name, Key=file_key)
            markdown_content = s3_object['Body'].read().decode('utf-8')
            print("Markdown content loaded from S3: ", markdown_content[:100])  # Log 100 ký tự đầu tiên

            # Convert Markdown thành plain text
            plain_text = convert_md_to_plain_text(markdown_content)
            print("Converted plain text: ", plain_text[:100])  # Log plain text đã chuyển đổi

            # Dùng AWS Comprehend để phân tích sentiment
            sentiment_result = comprehend.detect_sentiment(Text=plain_text, LanguageCode='en')
            print("Sentiment analysis result: ", sentiment_result)  # Log kết quả sentiment từ AWS Comprehend

            # Chuyển đổi float sang Decimal trước khi ghi vào DynamoDB
            sentiment_score_decimal = {k: Decimal(str(v)) for k, v in sentiment_result['SentimentScore'].items()}
            
            # Lưu kết quả vào DynamoDB
            table.put_item(
                Item={
                    'FileKey': file_key,
                    'Sentiment': sentiment_result['Sentiment'],
                    'SentimentScore': sentiment_score_decimal
                }
            )
            print("Sentiment analysis result saved to DynamoDB")
        
        return {
            'statusCode': 200,
            'body': json.dumps('Sentiment analysis complete')
        }
    except Exception as e:
        print(f"Error occurred: {str(e)}")  # Log thông tin lỗi nếu xảy ra
        return {
            'statusCode': 500,
            'body': json.dumps(f"Error: {str(e)}")
        }

def convert_md_to_plain_text(md_content):
    html_content = markdown.markdown(md_content)
    plain_text = BeautifulSoup(html_content, "html.parser").get_text()
    return plain_text

```