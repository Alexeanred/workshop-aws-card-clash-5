---
title : "Tạo SQS queue"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

* Ta cần chuẩn bị trước 2 SQS queue.
* Đầu tiên, ta sẽ tạo conversionQueue trước.
* Vào Amazon SQS, chọn **Create queue**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.10.png) 
* Type: **Standard**.
* Name: ```conversionQueue```
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.11.png) 
* Ta để các thông số mặc định và ta sẽ chỉnh sửa phần Access policy sau.
* Chọn **Create queue**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.12.png) 
* Tạo thêm 1 queue nữa là ```sentimentQueue``` tương tự như trên.
* Sau khi tạo xong 2 queues, ta sẽ đăng ký với SNS topic.
* Vào sentimentQueue, chọn **Subcribe to Amazon SNS topic**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.13.png) 
* Chọn SNS topic đã tạo, chọn nút **Save**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.14.png) 
* Làm tương tự với conversionQueue để đăng ký với SNS topic.