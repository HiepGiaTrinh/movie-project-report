---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice note %}}
Đây là tuần kế hoạch. Nội dung sẽ được cập nhật thành kết quả thực tế sau khi hoàn thành các công việc bên dưới.
{{% /notice %}}

### Mục tiêu tuần 9:

* Có khả năng theo dõi tình trạng mô hình và chất lượng dữ liệu theo thời gian.

### Công việc dự kiến:

* Thiết lập **SageMaker Model Monitor** để theo dõi data drift trên endpoint đang chạy.
* Xây dựng dashboard và alarm trên **CloudWatch** cho số lượt gọi endpoint, độ trễ và lỗi.
* Cấu hình thông báo **SNS** cho các alarm quan trọng (ví dụ: tỉ lệ lỗi cao, phát hiện drift).

### Kết quả dự kiến:

* Có khả năng quan sát rõ ràng tình trạng mô hình và data drift theo thời gian.
* Có cảnh báo tự động để phát hiện sự cố trước khi ảnh hưởng đến người dùng.
