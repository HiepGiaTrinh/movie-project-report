---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice note %}}
Tuần này vẫn đang được thực hiện tại thời điểm viết báo cáo. Phần kết quả bên dưới phản ánh những gì đã hoàn thành tính đến hiện tại.
{{% /notice %}}

### Mục tiêu

* Tích hợp endpoint với ứng dụng hoàn chỉnh và kiểm thử end-to-end.
* Bắt đầu thiết kế feedback loop.

### Công việc đã thực hiện

* Kết nối endpoint SageMaker với ứng dụng chạy trên EC2 của nhóm, thực hiện kiểm thử end-to-end đầy đủ: đăng nhập, duyệt danh sách phim và nhận kết quả gợi ý.
* Xử lý một số lỗi tích hợp giữa backend và endpoint, bao gồm định dạng payload và xử lý lỗi.
* Bắt đầu thiết kế feedback loop: rating mới được ghi vào DynamoDB và export sang S3 để phục vụ retraining.
* Phác thảo luồng retraining: DynamoDB Stream → Lambda → S3 → SageMaker Pipeline.

### Kết quả (đang thực hiện)

* Đạt được bản demo end-to-end hoạt động cùng phần còn lại của hệ thống nhóm.
* Có thiết kế ban đầu cho feedback loop, sẵn sàng triển khai ở tuần tiếp theo.
