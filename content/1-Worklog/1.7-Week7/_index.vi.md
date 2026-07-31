---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice note %}}
Tuần này vẫn đang trong quá trình thực hiện tại thời điểm viết báo cáo — phần kết quả bên dưới phản ánh những gì đã hoàn thành tính đến hiện tại.
{{% /notice %}}

### Mục tiêu tuần 7:

* Tích hợp endpoint SageMaker với ứng dụng của nhóm theo hướng end-to-end.
* Thiết kế feedback loop và pipeline retraining.

### Công việc đã làm:

* Tích hợp endpoint SageMaker với ứng dụng chạy trên EC2 của nhóm, chạy thử end-to-end: user đăng nhập → xem danh sách phim → nhận gợi ý.
* Xem log và xử lý các lỗi tích hợp giữa backend và endpoint ML (định dạng payload, xử lý lỗi).
* Bắt đầu thiết kế feedback loop: ghi nhận rating mới của user vào DynamoDB và export sang S3 để phục vụ retraining.
* Phác thảo kiến trúc retraining: DynamoDB Stream → Lambda → S3 → SageMaker Pipeline.

### Kết quả đạt được tuần 7 (đang thực hiện):

* Demo thành công luồng gợi ý end-to-end cùng với phần còn lại của hệ thống nhóm.
* Có thiết kế rõ ràng cho feedback loop và pipeline retraining, sẵn sàng triển khai ở tuần sau.
