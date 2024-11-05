---
title : "Create S3 buckets"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

* In this article, we need 2 S3 buckets. 1 is to contain markdown files, 1 is to contain html files.
* Go to S3, select the **Create bucket** button.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.1.png)
* Name the bucket containing markdown files: ```markdown-files-xxx```. You can add a number to make the bucket name unique.
* Leave the remaining settings as default, we will change them later.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.2.png)
* Select **Create bucket**.
* ![s3](/workshop-aws-card-clash-5/images/2.prerequisite/2.3.png)

* Create another bucket named: ```html-converted-output``` with the same steps above.

* After having 2 buckets, we will create 1 SNS topic.