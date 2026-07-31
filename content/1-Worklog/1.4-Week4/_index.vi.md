---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Huấn luyện phiên bản đầu tiên của mô hình gợi ý bằng SageMaker Training Job.
* So sánh các cấu hình hyperparameter để cải thiện chất lượng mô hình.

### Công việc đã làm:

* Hoàn thành khoá **"Build, Train, and Deploy ML Models with Amazon SageMaker"** trên AWS Skill Builder.
* Triển khai mô hình collaborative-filtering **Alternating Least Squares (ALS)** bằng thư viện `implicit`.
* Đóng gói script huấn luyện và submit dưới dạng **SageMaker Training Job**, thay vì chỉ train trên máy cá nhân.
* Thử nghiệm các hyperparameter (`factors`, `regularization`, `iterations`) để so sánh chất lượng mô hình.
* Lưu các ma trận user/item factor thu được dưới dạng model artifact trên S3.

### Kết quả đạt được tuần 4:

* Mô hình ALS được huấn luyện trên SageMaker với các training job có thể tái lập, có versioning.
* Xác định được cấu hình hyperparameter hợp lý dựa trên kết quả validation.
* Model artifact được đánh version và lưu trên S3, sẵn sàng cho bước đánh giá.
