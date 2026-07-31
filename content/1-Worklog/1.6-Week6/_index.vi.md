---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Triển khai mô hình gợi ý thành một endpoint inference thực tế.
* Kết nối endpoint ML với phần backend chung của nhóm.

### Công việc đã làm:

* Tìm hiểu các phương án inference của SageMaker trên AWS Skill Builder, tập trung vào **Serverless Inference**.
* Triển khai mô hình đã phê duyệt lên endpoint **SageMaker Serverless Inference** tại ap-southeast-2.
* Kiểm thử endpoint trực tiếp bằng AWS CLI và boto3 (`invoke_endpoint`) với các user ID mẫu.
* Phối hợp với thành viên phụ trách Backend để kết nối endpoint với lớp Lambda / API Gateway chung.
* Đo và ghi lại thời gian cold-start và độ trễ inference trung bình của endpoint.

### Kết quả đạt được tuần 6:

* Mô hình gợi ý đã được đưa ra thành endpoint Serverless Inference hoạt động thực tế.
* Xác minh được luồng request/response giữa lớp API và endpoint SageMaker.
* Có số liệu độ trễ nền (baseline) để tối ưu ở các bước sau.
