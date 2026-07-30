---
title : "Tối ưu và cleanup"
date : 2026-07-29
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

## 4. Tối ưu

### 4.1. Chi phí

Một số biện pháp tối ưu chi phí phù hợp với dự án hiện tại gồm:

- Dùng DynamoDB on-demand khi chưa có nhu cầu lớn.
- Tránh bật SageMaker real-time endpoint nếu chưa cần thiết.
- Dùng lifecycle policy cho S3 để quản lý dữ liệu cũ.
- Dừng container khi không sử dụng.

### 4.2. Bảo mật cơ bản

- Không lưu access key cố định trong repository.
- Dùng IAM role hoặc SSO thay vì key dài hạn.
- Giới hạn quyền theo nguyên tắc least privilege.
- Bật Block Public Access cho bucket S3.
- Đảm bảo biến môi trường nhạy cảm không bị commit vào git.

### 4.3. Clean-up để tránh phát sinh chi phí

Sau khi kiểm thử hoặc demo xong, nên thực hiện các bước sau:

```bash
docker compose down
aws s3 rm s3://<bucket-name> --recursive --only-show-errors
aws s3 rb s3://<bucket-name> --force
```

Nếu có tài nguyên tạo riêng cho demo thì nên xóa luôn:

- EC2 instance hoặc security group không còn sử dụng.
- Alarm hoặc notification nếu đã tạo.
- Endpoint hoặc stack không còn cần thiết.
