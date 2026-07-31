---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6

* Đưa model ra thành endpoint thật sự phục vụ request, chứ không chỉ nằm im.
* Nối nó với phần còn lại của app.

### Đã làm gì

* Đọc về các kiểu inference của SageMaker, tập trung vào Serverless Inference.
* Deploy model lên endpoint Serverless Inference ở ap-southeast-2.
* Test bằng CLI và boto3 với vài user ID mẫu.
* Trao đổi với bạn làm Backend để nối endpoint vào lớp Lambda/API Gateway.
* Đo thời gian cold start và độ trễ trung bình.

### Kết quả

* Endpoint chạy và trả kết quả gợi ý thật.
* Xác nhận luồng request/response giữa API và endpoint hoạt động đúng.
* Có số liệu độ trễ để so sánh về sau.
