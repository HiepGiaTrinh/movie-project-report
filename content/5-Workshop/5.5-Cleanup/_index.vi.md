---
title : "Tối ưu và cleanup"
date : 2026-07-29
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

## 4. Tối ưu

### 4.1. Chi phí

Một số cách tối ưu chi phí đã phù hợp với dự án hiện tại:

- Dùng DynamoDB on-demand khi chưa có nhu cầu lớn.
- Tránh bật SageMaker real-time endpoint nếu chưa cần thiết.
- Dùng lifecycle policy cho S3 để quản lý dữ liệu cũ.
- Dừng container khi không dùng đến.

### 4.2. Bảo mật cơ bản

- Không lưu access key cố định trong repo.
- Dùng IAM role hoặc SSO thay vì key dài hạn.
- Giới hạn quyền theo least privilege.
- Bật Block Public Access cho bucket S3.
- Đảm bảo biến môi trường nhạy cảm không bị commit vào git.

### 4.3. Clean-up để tránh phát sinh chi phí

Sau khi test hoặc demo xong, nên làm các bước sau:

```bash
docker compose down
aws s3 rm s3://<bucket-name> --recursive --only-show-errors
aws s3 rb s3://<bucket-name> --force
```

Nếu có resource tạo riêng cho demo thì nên xóa luôn:

- EC2 instance hoặc security group không dùng nữa.
- Alarm/notification nếu đã tạo.
- Endpoint hoặc stack không còn cần thiết.
