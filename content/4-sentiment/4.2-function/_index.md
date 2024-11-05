---
title : "Create lambda functions and configure IAM role"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

### Create lambda function
* In lambda, select **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.8.png)
* Function name: ```ConversionService```.
* Runtime: Python3.12 .
* Leave other parameters as default, select **Create function**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.9.png)
### Configure IAM role
* After creating the function, go to **Configuration**, select **Permissions**.
* Click on the path of the role name function that you created.
* Select **Add permissions** and select **Attach policies**.

* Add policy: ```AmazonS3FullAccess```, ```AmazonDynamoDBFullAccess```, ```ComprehendFullAccess```
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.10.png)
* After adding policy, select **Add permissions** and select **Create inline policy**.

* In policy editor, Select JSON, add the following policy:
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
* Set policy name: ```sqs-role```.
* Select **Create policy**.

* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.4.png)

### Add lambda layer to function
* In lambda function, in Code section, pull down to Layers section, select **Add a layer**.

* Select custom layers.
* Select the created layer to add markdown library, bs4 for this lambda function.
* Select version 1.
* Select **Add**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.11.png)

### Add lambda code
* Code of lambda function:
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