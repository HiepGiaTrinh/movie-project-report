---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu

* Xây dựng nền tảng để xử lý dữ liệu trên AWS.
* Thiết lập chỗ lưu trữ và chuẩn bị bộ dữ liệu dùng để huấn luyện mô hình.

### Công việc đã thực hiện

* Hoàn thành khoá "AWS Technical Essentials" và "Introduction to AWS Identity and Access Management (IAM)" trên AWS Skill Builder.
* Tạo S3 bucket với cấu trúc prefix `raw/`, `processed/`, `models/`, `logs/`.
* Viết IAM policy và role riêng cho tác vụ xử lý dữ liệu, tuân theo nguyên tắc least-privilege.
* Tải bộ dữ liệu MovieLens 100K và tải lên prefix `raw/`.
* Khám phá dữ liệu cục bộ bằng pandas, xem xét phân bố rating, số lượng user/item và độ thưa của ma trận user-item.

### Kết quả

* S3 bucket đã có cấu trúc rõ ràng và chứa dữ liệu MovieLens thô.
* IAM role hoạt động ổn định, tuân thủ nguyên tắc least-privilege.
* Hiểu rõ đặc điểm của bộ dữ liệu, làm cơ sở thiết kế bước tiền xử lý ở tuần tiếp theo.
