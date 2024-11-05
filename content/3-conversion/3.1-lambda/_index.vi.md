---
title : "Tạo lambda layer"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---

* Để tạo một lambda layer, ta cần upload 1 file zip lên lambda. 
* Trong file zip đấy có chứa 1 folder python, folder python sẽ chứa các library ta cần install.
* Như trong bài này, ta cần install markdown library để chuyển markdown sang dạng html.
* Ở máy local, ta chạy các câu lệnh sau:
```
mkdir lambda_layer
cd lambd_layer
mkdir python
pip install markdown -t . 
```
Sau khi install xong thư viện, ta zip lại.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip.png) 
* Sau đó, ta vào lambda, chọn **Layers**.
* Chọn nút **Create layer**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip2.png) 
* Name: **python-markdown**.
* Upload file zip lên.
* Compatible runtimes chọn python 3.12 . 
* Chọn nút **Create**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip3.png) 