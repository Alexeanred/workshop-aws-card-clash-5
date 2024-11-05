---
title : "Create SQS queue"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

* We need to prepare 2 SQS queues in advance.
* First, we will create conversionQueue first.
* Go to Amazon SQS, select **Create queue**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.10.png)
* Type: **Standard**.
* Name: ```conversionQueue```
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.11.png)
* We leave the default parameters and we will edit the Access policy section later.
* Select **Create queue**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.12.png)
* Create another queue called ```sentimentQueue``` similar to above.
* After creating 2 queues, we will register with SNS topic.
* Go to sentimentQueue, select **Subcribe to Amazon SNS topic**.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.13.png)
* Select the created SNS topic, select the **Save** button.
* ![queue](/workshop-aws-card-clash-5/images/2.prerequisite/2.14.png)
* Do the same with conversionQueue to register with SNS topic.