---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice note %}}
Tuần này chưa bắt đầu tại thời điểm viết báo cáo. Nội dung sẽ được cập nhật thành kết quả thực tế sau khi hoàn thành.
{{% /notice %}}

### Mục tiêu

* Xây dựng khả năng giám sát tình trạng mô hình.

### Kế hoạch

* Thiết lập SageMaker Model Monitor để theo dõi data drift trên endpoint đang hoạt động.
* Xây dựng dashboard và alarm trên CloudWatch cho số lượt gọi, độ trễ và lỗi.
* Cấu hình thông báo SNS cho các cảnh báo quan trọng.

### Kết quả kỳ vọng

* Có khả năng quan sát rõ ràng hiệu năng mô hình và data drift theo thời gian.
* Cảnh báo giúp phát hiện sự cố trước khi ảnh hưởng đến người dùng.
