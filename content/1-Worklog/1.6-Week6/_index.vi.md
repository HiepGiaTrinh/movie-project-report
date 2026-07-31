---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu

* Triển khai mô hình thành một endpoint inference hoạt động thực tế.
* Kết nối endpoint với phần còn lại của ứng dụng.

### Công việc đã thực hiện

* Tìm hiểu các phương án inference của SageMaker, tập trung vào Serverless Inference.
* Triển khai mô hình lên endpoint SageMaker Serverless Inference tại khu vực ap-southeast-2.
* Kiểm thử endpoint bằng AWS CLI và boto3 với các user ID mẫu.
* Phối hợp với thành viên phụ trách Backend để kết nối endpoint với lớp Lambda/API Gateway chung.
* Đo thời gian cold-start và độ trễ inference trung bình.

### Kết quả

* Endpoint đã hoạt động và trả về kết quả gợi ý.
* Xác nhận luồng request/response giữa lớp API và endpoint hoạt động chính xác.
* Ghi nhận số liệu độ trễ cơ sở để so sánh trong các bước tiếp theo.
