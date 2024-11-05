---
title : "Introduction"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---
* ![arch](/workshop-aws-card-clash-5/images/kien.png)

## Architectural idea:
* **User will convert and analyze markdown file through the following process:**
    * Markdown file is uploaded to S3 bucket.
    * SNS will receive notification from publisher that S3 bucket has new file pushed.
    * SNS will distribute notification to subscribers which are SQS queues.
    * SQS messages will be sent to lambda functions to perform tasks.
    * There are 2 main streams: conversion and sentimment analysis.
    * Lambda function in conversion stream will convert to html file and save to new s3 bucket.
    * Lambda function in sentiment analysis stream will analyze file sentiment and save data to dynamodb table.
## Service Introduction:

**Amazon S3**: A reliable and scalable object storage service, S3 allows users to securely store and retrieve markdown files. When a file is uploaded to an S3 bucket, it triggers a notification to other services in the architecture, allowing for continuous transformation and analysis.

**AWS Lambda**: This serverless computing service enables code execution without managing servers. Lambda functions are triggered by notifications from SQS, performing tasks like converting markdown files to HTML and analyzing the sentiment of content, without worrying about infrastructure.

**Amazon DynamoDB**: A fully managed NoSQL database service, DynamoDB provides fast data storage with low latency. In this architecture, it is used to store sentiment analysis results from markdown files, allowing for quick access and processing of data.

**Amazon SNS (Simple Notification Service)**: SNS is a notification service that helps connect components in the architecture. When a markdown file is uploaded to S3, SNS will broadcast notifications to subscribers such as SQS queues, initiating further processing.

**Amazon SQS (Simple Queue Service)**: SQS provides a message queue management mechanism, allowing messages to be transferred between services without direct interaction. It ensures that messages are processed in order and correctly, which is important for forwarding messages to Lambda functions to perform analysis and transformation tasks.

**AWS Comprehend**: This service provides automated semantic analysis, allowing sentiment, topic, and semantic analysis from text. In your workflow, AWS Comprehend will be used to analyze the contents of the markdown file after conversion, and store the analysis results in DynamoDB for later use.