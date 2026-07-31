---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice note %}}
Tuần này vẫn đang làm dở lúc viết báo cáo — phần dưới là những gì đã xong tính tới hiện tại.
{{% /notice %}}

### Mục tiêu tuần 7

* Nối endpoint vào app hoàn chỉnh và test thử end-to-end.
* Bắt đầu nghĩ cách làm feedback loop.

### Đã làm gì

* Nối endpoint SageMaker vào app chạy trên EC2 của nhóm, test thử full luồng: đăng nhập, lướt phim, nhận gợi ý.
* Vá vài lỗi tích hợp giữa backend và endpoint (định dạng payload, xử lý lỗi — mấy cái quen thuộc).
* Bắt đầu phác thảo feedback loop: rating mới đưa vào DynamoDB, export ra S3 để retrain.
* Vẽ nháp luồng retraining: DynamoDB Stream → Lambda → S3 → SageMaker Pipeline.

### Tới đâu rồi

* Demo end-to-end chạy được cùng phần còn lại của hệ thống.
* Có bản phác thảo cho feedback loop, tuần sau bắt tay vào làm thật.
