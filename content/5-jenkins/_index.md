---
title : "Bonus: IaC with Terraform"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
### Introduction
* **After you have completed creating the system via AWS management console, you can try creating resources via terraform through the following steps.**

* I have uploaded the terraform code to github via the following repository: https://github.com/Alexeanred/terraform-aws-card-clash-cp-5.git

* You will clone this git repository and edit it to suit your architecture.

* Basic terraform commands to run this architecture:
* 1. Initialize Terraform
* This command will install the necessary plugins and set up the working environment for Terraform.
```
terraform init
```
* 2. Plan
* This command will create an execution plan and save it to the tf.plan file:
```
terraform plan -out tf.plan
```
* 3. View the plan
* You can view the execution plan details with the following command, the result will be saved to the tfplan.txt file
```
terraform show -no-color tf.plan > tfplan.txt
```
* 4. Apply the plan
* Finally, apply the created plan to create resources in AWS:
```
terraform apply "tf.plan"
```
* 5. Delete the architecture
* If you want to delete all resources.
```
terraform destroy
```

### Explain the structure of the terraform file
* To manage resources effectively, I have created modules folders for each resource as in the structure there are 5 modules: S3, SNS, SQS, DYNAMODB, LAMBDA.
* In the main folder, we have the main.tf file. In that file, we will import the modules, create more resources to connect the resource modules together such as creating S3 bucket notification between S3 and SNS, subcription between SNS and SQS, SQS trigger lambda function. Then output the values ​​to see the resources created by terraform.
* If we need to get the name, properties in another module used in the module, in that module we declare variables.tf and define variables when importing the module in main.tf. As in this article, we need the dynamodb table name and html bucket name for the lambda function.
* At the same time, you need to pay attention to whether the created tf file format is UTF-8 or not to avoid errors: This character is not used within the language.