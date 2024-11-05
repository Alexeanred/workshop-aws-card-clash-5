---
title : "Create lambda layer"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

* To create a lambda layer, we need to upload a zip file to lambda.

* In that zip file there is a python folder, the python folder will contain the libraries we need to install.

As in this function, we need to install the markdown library and the BeautifulSoup library to convert markdown to normal text format to put into amazon comprehend.

* On the local machine, we run the following commands:
```
mkdir lambda-bs4-markdown
cd lambda-bs4-markdown
mkdir python
pip install beautifulsoup4 markdown -t .

```
After installing the library, we zip it again.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.17.png)
* Then, go to lambda, select **Layers**.
* Select the **Create layer** button.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip2.png)
* Name: **markdown-bs4**.
* Upload the zip file.
* Compatible runtimes select python 3.12 .
* Select the **Create** button.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.18.png)