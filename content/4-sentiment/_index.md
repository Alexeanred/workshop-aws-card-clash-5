---
title : "Text Sentiment Analysis"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---
* On AWS management console:
* Search for **CodePipeline**.
* ![cicd](/workshop-aws-card-clash-5/images/4.s3/4.1.png)
* Select **Create pipeline**:
* ![cicd](/workshop-aws-card-clash-5/images/4.s3/4.2.png)
* Select **Build custom pipeline**.
* Select **Next**.
* ![cicd](/workshop-aws-card-clash-5/images/4.s3/4.3.png)
* Fill in pipeline name: ```ci-cd-pipeline```
* Select **New service role**.
* Fill in role name: can be filled as shown.
* ![cicd](/workshop-aws-card-clash-5/images/4.s3/4.4.png)
* Expand the Advanced settings section, you will see the Artifact store and Encryption key sections.
* In the AWS card clash architecture, you will see an S3 bucket to store pipeline artifacts.
* Artifacts will be created as outputs of the pipeline stages.
* EncryptionKey is the encryption key used to encrypt the artifact, helping to secure the data.
* CodePipeline only supports symmetric encryption.
* ![cicd](/workshop-aws-card-clash-5/images/4.s3/4.5.png)