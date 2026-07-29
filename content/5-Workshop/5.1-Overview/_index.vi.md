---
title : "Tổng quan workshop"
date : 2026-07-29
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Tổng quan về phần triển khai AWS

Phần này tập trung vào việc trình bày cách triển khai hệ thống movie recommendation của nhóm lên AWS theo hướng end-to-end. Nội dung được viết theo cấu trúc từ chuẩn bị môi trường, cấu hình tài nguyên AWS, chạy hệ thống, kiểm thử, đến tối ưu chi phí và cleanup.

### Nội dung chính của mục 5

1. Prerequisite: nêu các điều kiện cần có trước khi triển khai.
2. Hướng dẫn triển khai end-to-end: chia thành các bước rõ ràng để người khác có thể làm lại.
3. Test & validation: kiểm tra log, request, chức năng và kết quả mong đợi.
4. Tối ưu: đề cập đến chi phí, bảo mật cơ bản và cleanup.
5. Những điểm cần bổ sung trong tương lai: ghi chú các phần còn thiếu để hoàn thiện sau.

### Điểm cần chú ý

- Dự án hiện đang triển khai theo hướng EC2 + Docker Compose, kết hợp backend FastAPI, frontend Vite, DynamoDB, S3 và ML module.
- SageMaker realtime inference chưa được bật đầy đủ, nên phần này cần được trình bày trung thực và có ghi chú rõ ràng.
- Không nên mô tả CloudWatch metrics/alerts như một tính năng đã có sẵn nếu repo chưa triển khai trực tiếp.

> [!NOTE]
> Đây là bản khung báo cáo ban đầu. Những phần chưa có thông tin đầy đủ sẽ được đánh dấu để bổ sung sau.

![Placeholder: Tổng quan triển khai AWS](/images/5-Workshop/placeholder-workshop-overview.png)
