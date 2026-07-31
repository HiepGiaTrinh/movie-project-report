---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4

* Train ra một bản model đầu tiên bằng SageMaker Training job.
* Thử vài combo hyperparameter xem cái nào ổn.

### Đã làm gì

* Học "Build, Train, and Deploy ML Models with Amazon SageMaker" trên Skill Builder.
* Viết model ALS collaborative-filtering bằng thư viện `implicit`.
* Đóng gói lại và chạy dưới dạng SageMaker Training job, thay vì chỉ chạy trên máy cá nhân.
* Thử vài combo factors/regularization/iterations để so sánh.
* Lưu ma trận factor kết quả lên S3.

### Kết quả

* Có model ALS đã train, và giờ training job chạy lại được, có version — không còn là script chạy một lần rồi thôi.
* Tìm được cấu hình hyperparameter khá ổn dựa trên validation.
* Model artifact đã có trên S3, sẵn sàng cho bước tiếp theo — đánh giá.
