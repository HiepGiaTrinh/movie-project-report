---
title : "Tổng quan workshop"
date : 2026-07-29
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Tổng quan về phần triển khai AWS

Phần này mô tả quá trình triển khai hệ thống gợi ý phim lên AWS theo hướng end-to-end. Nội dung được tổ chức theo các giai đoạn chính: chuẩn bị môi trường, tạo tài nguyên AWS, huấn luyện mô hình, kiểm thử, tối ưu chi phí và dọn dẹp tài nguyên.

### Nội dung chính của mục 5

1. Prerequisite: xác định các điều kiện và quyền truy cập cần thiết trước khi triển khai.
2. Hướng dẫn triển khai end-to-end: mô tả các bước tạo tài nguyên và vận hành hệ thống.
3. Test & validation: trình bày các kiểm tra cần thực hiện sau khi triển khai.
4. Tối ưu: tóm tắt các lưu ý về chi phí, bảo mật và cleanup.
5. Những điểm cần bổ sung trong tương lai: liệt kê các nội dung chưa hoàn thiện.

### Điểm cần chú ý

- Hệ thống hiện được triển khai theo hướng EC2 + Docker Compose, kết hợp backend FastAPI, frontend Vite, DynamoDB, S3 và module machine learning.
- SageMaker realtime inference chưa phải là thành phần vận hành mặc định, do đó cần mô tả đúng với trạng thái triển khai thực tế.
- CloudWatch metrics và alerts chỉ nên được đề cập như nội dung định hướng nếu repository chưa có hiện thực hóa đầy đủ.
