---
title : "Tạo lambda layer"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---



* Để tạo một lambda layer, ta cần upload 1 file zip lên lambda. 
* Trong file zip đấy có chứa 1 folder python, folder python sẽ chứa các library ta cần install.
* Như trong function này, ta cần install markdown library và BeautifulSoup library để chuyển markdown sang dạng text bình thường để đưa vào amazon comprehend.
* Ở máy local, ta chạy các câu lệnh sau:
```
mkdir lambda-bs4-markdown
cd lambda-bs4-markdown
mkdir python
pip install beautifulsoup4 markdown -t . 
```
Sau khi install xong thư viện, ta zip lại.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.17.png) 
* Sau đó, ta vào lambda, chọn **Layers**.
* Chọn nút **Create layer**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/zip2.png) 
* Name: **markdown-bs4**.
* Upload file zip lên.
* Compatible runtimes chọn python 3.12 . 
* Chọn nút **Create**.
* ![queue](/workshop-aws-card-clash-5/images/3.connect/3.18.png) 