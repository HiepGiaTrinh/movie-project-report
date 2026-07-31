---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Xây dựng nền tảng vững chắc để làm việc với dữ liệu trên AWS.
* Chuẩn bị lớp lưu trữ S3 và bộ dữ liệu dùng để huấn luyện mô hình gợi ý.

### Công việc đã làm:

* Hoàn thành khoá **"AWS Technical Essentials"** và **"Introduction to AWS Identity and Access Management (IAM)"** trên AWS Skill Builder.
* Thiết kế và tạo S3 bucket với các prefix `raw/`, `processed/`, `models/`, `logs/` cho dự án.
* Viết IAM policy theo nguyên tắc least-privilege và tạo IAM role riêng cho tác vụ xử lý dữ liệu.
* Tải bộ dữ liệu **MovieLens 100K** và upload lên prefix `raw/` trên S3.
* Khám phá dữ liệu cục bộ bằng pandas (`ratings.csv`, `movies.csv`): phân bố rating, số lượng user/item, độ thưa (sparsity) của ma trận user-item.

### Kết quả đạt được tuần 2:

* S3 bucket đã có cấu trúc rõ ràng và chứa dữ liệu MovieLens thô.
* IAM role hoạt động tốt, tuân thủ nguyên tắc least-privilege cho việc truy cập dữ liệu.
* Hiểu rõ đặc điểm và chất lượng dữ liệu, làm cơ sở để thiết kế bước tiền xử lý ở tuần sau.
