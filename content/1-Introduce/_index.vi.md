---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
* ![arch](/workshop-aws-card-clash-5/images/kien.png) 

## Ý tưởng kiến trúc: 
* **User sẽ chuyển đổi và phân tích file markdown thông qua quy trình sau:**
    * File markdown được upload lên S3 bucket.
    * SNS sẽ nhận thông báo từ publisher là S3 bucket có file mới được push lên.
    * SNS sẽ phân phối thông báo cho các subscribers là các SQS queues.
    * Các SQS messages sẽ được gửi cho lambda functions để thực hiện các tác vụ.
    * Có 2 luồng chính là conversion và sentimment analysis.
        * Lambda function ở luồng conversion sẽ chuyển đổi thành file html và lưu vào s3 bucket mới.
        * Lambda function ở luồng sentiment analysis sẽ phân tích cảm xúc file và lưu data vào dynamodb table.
## Giới thiệu dịch vụ:

**Amazon S3**: Dịch vụ lưu trữ đối tượng đáng tin cậy và có khả năng mở rộng, S3 cho phép người dùng lưu trữ và truy xuất file markdown một cách an toàn. Khi một file được upload lên S3 bucket, nó kích hoạt một thông báo đến các dịch vụ khác trong kiến trúc, cho phép quy trình chuyển đổi và phân tích diễn ra liên tục.

**AWS Lambda**: Dịch vụ điện toán serverless này cho phép thực thi mã mà không cần quản lý máy chủ. Lambda function sẽ được kích hoạt khi có thông báo từ SQS, thực hiện các tác vụ như chuyển đổi file markdown thành HTML và phân tích cảm xúc của nội dung, mà không phải lo lắng về cơ sở hạ tầng.

**Amazon DynamoDB**: Là một dịch vụ cơ sở dữ liệu NoSQL hoàn toàn quản lý, DynamoDB cung cấp khả năng lưu trữ dữ liệu nhanh chóng với độ trễ thấp. Trong kiến trúc này, nó được sử dụng để lưu trữ kết quả phân tích cảm xúc từ các file markdown, cho phép truy cập và xử lý dữ liệu nhanh chóng.

**Amazon SNS (Simple Notification Service)**: SNS là dịch vụ thông báo giúp kết nối giữa các thành phần trong kiến trúc. Khi một file markdown được upload lên S3, SNS sẽ phát thông báo đến các subscriber như SQS queues, khởi động quy trình xử lý tiếp theo.

**Amazon SQS (Simple Queue Service)**: SQS cung cấp cơ chế quản lý hàng đợi tin nhắn, cho phép truyền tải thông điệp giữa các dịch vụ mà không cần tương tác trực tiếp. Nó đảm bảo rằng các tin nhắn được xử lý theo thứ tự và chính xác, rất quan trọng cho việc chuyển tiếp các thông điệp đến Lambda functions để thực hiện các tác vụ phân tích và chuyển đổi.

**AWS Comprehend**: Dịch vụ này cung cấp khả năng phân tích ngữ nghĩa tự động, cho phép phân tích cảm xúc, chủ đề và ngữ nghĩa từ văn bản. Trong quy trình của bạn, AWS Comprehend sẽ được sử dụng để phân tích nội dung của file markdown sau khi chuyển đổi, và lưu trữ kết quả phân tích vào DynamoDB để sử dụng sau này.



