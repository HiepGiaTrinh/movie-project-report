---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice note %}}
Đây là tuần kế hoạch. Nội dung sẽ được cập nhật thành kết quả thực tế sau khi hoàn thành các công việc bên dưới.
{{% /notice %}}

### Mục tiêu tuần 8:

* Tự động hoá quá trình retraining để mô hình có thể cải thiện mà không cần can thiệp thủ công.

### Công việc dự kiến:

* Xây dựng **SageMaker Pipeline** nối các bước Processing → Training → Evaluation để tự động retraining.
* Cấu hình rule **EventBridge** để kích hoạt pipeline retraining theo lịch.
* Thêm bước "approval" có điều kiện, chỉ đưa mô hình mới vào production nếu đạt ngưỡng đánh giá tối thiểu.

### Kết quả dự kiến:

* Pipeline retraining hoạt động, có thể lên lịch, loại bỏ nhu cầu retraining thủ công.
* Có tiêu chí rõ ràng để chỉ những mô hình tốt hơn mới được đưa vào production.
