---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice note %}}
Tuần này chưa bắt đầu tại thời điểm viết báo cáo. Nội dung sẽ được cập nhật thành kết quả thực tế sau khi hoàn thành.
{{% /notice %}}

### Mục tiêu

* Chuyển từ retraining thủ công sang một pipeline tự động.

### Kế hoạch

* Xây dựng SageMaker Pipeline nối các bước Processing, Training và Evaluation.
* Cấu hình rule EventBridge để kích hoạt retraining theo lịch.
* Bổ sung bước kiểm soát (promotion gate) để chỉ triển khai mô hình mới khi vượt trội hơn phiên bản hiện tại.

### Kết quả kỳ vọng

* Pipeline retraining tự động, có thể lên lịch.
* Cơ chế đảm bảo mô hình kém hơn không được đưa vào production.
