---
title : "Tạo S3 buckets"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

* Trong bài này, ta cần 2 S3 buckets. 1 là chứa markdown files, 1 là chứa html files.
* Vào S3, chọn nút **Create bucket**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.1.png) 
* Đặt tên bucket chứa markdown files: ```markdown-files-xxx```. Bạn có thể thêm 1 số để tên bucket unique.
* Để các settings còn lại mặc định, ta sẽ thay đổi sau.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.2.png) 
* Chọn **Create bucket**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.3.png) 

* Tạo thêm 1 bucket tên: ```html-converted-output``` với các bước tương tự trên.

* Sau khi đã có 2 buckets, ta sẽ tạo 1 SNS topic.