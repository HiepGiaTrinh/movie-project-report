---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Thiết lập SageMaker Studio làm không gian làm việc chính cho pipeline ML.
* Xây dựng bước tiền xử lý dữ liệu có thể lặp lại bằng SageMaker Processing Job.

### Công việc đã làm:

* Hoàn thành khoá **"Amazon SageMaker Studio for Data Scientists"** trên AWS Skill Builder.
* Thiết lập SageMaker Studio domain và user profile tại **ap-southeast-2**.
* Xây dựng **SageMaker Processing Job** để làm sạch dữ liệu MovieLens và dựng ma trận thưa user-item.
* Triển khai **chia train/test theo thời gian** (thay vì chia ngẫu nhiên) để phản ánh đúng tính chất tuần tự của hệ thống gợi ý.
* Lưu lại dữ liệu train/test đã xử lý vào prefix `processed/` trên S3.

### Kết quả đạt được tuần 3:

* SageMaker Processing Job đầu tiên chạy thành công end-to-end.
* Pipeline tiền xử lý có thể tái lập, xuất ra bộ dữ liệu sẵn sàng để huấn luyện trên S3.
* Hiểu rõ vì sao việc chia dữ liệu theo mốc thời gian lại quan trọng với hệ thống gợi ý.
