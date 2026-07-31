---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2

* Làm quen với việc xử lý data trên AWS.
* Set up chỗ lưu trữ và lấy dataset thật vào để chuẩn bị train.

### Đã làm gì

* Học "AWS Technical Essentials" và "Introduction to IAM" trên Skill Builder.
* Tạo S3 bucket có mấy folder `raw/`, `processed/`, `models/`, `logs/`.
* Viết IAM policy và role riêng cho việc xử lý data, theo kiểu least-privilege.
* Tải MovieLens 100K, up lên `raw/`.
* Ngồi khám phá data bằng pandas — xem phân bố rating, bao nhiêu user/item, ma trận thưa cỡ nào.

### Kết quả

* Bucket đã có cấu trúc rõ ràng và có sẵn data MovieLens thô.
* IAM role chạy tốt, không bị dư quyền.
* Hiểu được data đang ở dạng nào, giúp hình dung bước tiền xử lý ở tuần sau.
