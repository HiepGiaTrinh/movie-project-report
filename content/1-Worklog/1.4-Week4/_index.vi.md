---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu

* Huấn luyện phiên bản đầu tiên của mô hình bằng SageMaker Training job.
* So sánh một số cấu hình hyperparameter.

### Công việc đã thực hiện

* Hoàn thành khoá "Build, Train, and Deploy ML Models with Amazon SageMaker" trên AWS Skill Builder.
* Triển khai mô hình collaborative-filtering ALS bằng thư viện `implicit`.
* Đóng gói script huấn luyện và chạy dưới dạng SageMaker Training job thay vì chỉ huấn luyện cục bộ.
* So sánh nhiều tổ hợp factors, regularization và iterations.
* Lưu các ma trận factor thu được lên S3.

### Kết quả

* Có được mô hình ALS đã huấn luyện, với các training job có thể tái lập và quản lý phiên bản.
* Xác định được cấu hình hyperparameter hợp lý dựa trên kết quả validation.
* Model artifact được lưu trên S3, sẵn sàng cho bước đánh giá.
