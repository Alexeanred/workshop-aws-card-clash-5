---
title : "Bonus: IaC với Terraform"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
### Giới thiệu
* **Sau khi bạn đã hoàn thành việc tạo hệ thống qua AWS management console, bạn có thể thử tạo các resources qua terraform qua những bước sau đây.**

* Mình đã đưa code terraform lên github qua repository sau đây: https://github.com/Alexeanred/terraform-aws-card-clash-cp-5.git

* Bạn sẽ clone git repository này về và chỉnh sửa lại phù hợp với kiến trúc của bạn.

* Các lệnh terraform cơ bản để chạy được kiến trúc này:
* 1. Khởi tạo Terraform
    * Lệnh này sẽ cài đặt các plugin cần thiết và thiết lập môi trường làm việc cho Terraform.
```
terraform init
```
* 2. Lập kế hoạch (Plan)
    * Lệnh này sẽ tạo ra kế hoạch thực hiện và lưu vào file tf.plan:
```
terraform plan -out tf.plan
```
* 3. Xem kế hoạch
    * Bạn có thể xem chi tiết kế hoạch thực hiện bằng lệnh sau, kết quả sẽ được lưu vào file tfplan.txt
```
terraform show -no-color tf.plan > tfplan.txt
```

* 4. Áp dụng kế hoạch
    * Cuối cùng, áp dụng kế hoạch đã lập để tạo các resources trong AWS:
```
terraform apply "tf.plan"
```

* 5. Xóa kiến trúc
    * Nếu bạn muốn xóa tất cả resources.
```
terraform destroy
```

### Giải thích cấu trúc file terraform
* Để quản lý các resources một cách hiệu quả, mình đã tạo ra các folder modules cho từng resources như trong cấu trúc là có 5 modules: S3, SNS, SQS, DYNAMODB, LAMBDA.
* Ở folder chính, ta có file main.tf. Trong file đấy ta sẽ import các modules vào, tạo thêm các resources để kết nối các modules resources lại với nhau như tạo S3 bucket notification giữa S3 và SNS, subcription giữa SNS và SQS, SQS trigger lambda function. Sau đó output ra các giá trị để xem được những resources đã tạo bởi terraform.
* Nếu ta cần lấy tên, thuộc tính ở module khác dùng trong module thì trong module đó ta khai báo variables.tf và định nghĩa variables khi import module ở main.tf. Như trong bài này là ta cần tên dynamodb table và html bucket name cho lambda function.
* Đồng thời cần để ý định dạng file tf đã tạo có phải UTF-8 không để không bị lỗi: This character is not used within the language.

