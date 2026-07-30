---
title : "Triển khai end-to-end"
date : 2026-07-29
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## 2. Quy trình triển khai end-to-end

Phần này mô tả trình tự triển khai các thành phần hạ tầng và mô hình của hệ thống lên AWS. Mỗi mục nhỏ tương ứng với một bước triển khai riêng biệt.

1. [Tạo bucket S3](5.3.1-S3-bucket/)
2. [Tạo bảng DynamoDB](5.3.2-DynamoDB/)
3. [Tải dataset và train mô hình](5.3.3-Training/)
4. [Deploy lên SageMaker Endpoint](5.3.4-SageMaker-endpoint/)
5. [CI/CD deploy EC2 qua GitHub Workflows](5.3.5-CICD-EC2/)