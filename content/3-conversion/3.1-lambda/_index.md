---
title : "Create lambda layer"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1. </b> "
---

* To create a lambda layer, we need to upload a zip file to lambda.

* In that zip file there is a python folder, the python folder will contain the libraries we need to install.

* As in this article, we need to install the markdown library to convert markdown to html format.

* On the local machine, we run the following commands:
```
mkdir lambda_layer
cd lambd_layer
mkdir python
pip install markdown -t .

```
After installing the library, we zip it again.

* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip.png)
* Then, we go to lambda, select **Layers**.

* Select the **Create layer** button.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip2.png) 
* Name: **python-markdown**.
* Upload zip file.
* Compatible runtimes choose python 3.12.
* Select the **Create** button.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip3.png)