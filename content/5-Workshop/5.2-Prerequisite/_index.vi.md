---
title : "Chuẩn bị môi trường"
date : 2026-07-29
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

## 1. Prerequisite

### 1.1. Điều kiện ban đầu

Trước khi triển khai, cần bảo đảm các điều kiện nền tảng sau:

- Tài khoản AWS có quyền tạo và quản lý tài nguyên.
- Region phù hợp; trong dự án này sử dụng `ap-southeast-1`.
- AWS CLI v2 đã được cài đặt và xác thực thành công.
- Docker Engine và Docker Compose v2 sẵn sàng trên máy cục bộ.
- Git và submodule được cấu hình đúng.
- IAM có quyền truy cập vào STS, DynamoDB, S3 và SageMaker khi cần.

### 1.2. IAM và quyền cần thiết

Nhóm quyền tối thiểu thường bao gồm:

- `sts:GetCallerIdentity`
- `dynamodb:DescribeTable`, `GetItem`, `BatchGetItem`, `PutItem`, `DeleteItem`, `Query`, `Scan`
- `s3:ListBucket`, `GetBucketLocation`, `GetObject`, `PutObject`
- `sagemaker:DescribeEndpoint`, `InvokeEndpoint` (khi sử dụng real-time inference)

### 1.3. Các tài nguyên AWS cần chuẩn bị

- 5 bảng DynamoDB: Movies, PopularMovies, Users, UserInteractions, RecommendationCache.
- 1 bucket S3 để lưu dataset và artifact mô hình.
- Các prefix S3 cần được cấu hình nhất quán với cấu trúc lưu trữ.
- Biến môi trường `.env` với region, tên bảng, bucket, JWT secret và URL API.

### 1.4. Chuẩn bị mã nguồn cục bộ

Trước khi tiến hành triển khai, cần tải mã nguồn về máy cục bộ và đồng bộ submodule nếu dự án có tách module:

```bash
git clone --recurse-submodules <repo-url>
cd movie-recommendation
git submodule update --init --recursive
```

Bước này bảo đảm toàn bộ thành phần backend, frontend và module mô hình được kiểm tra trên cùng một phiên bản mã nguồn.

### 1.5. Xác thực danh tính và cấu hình AWS

Sau khi chuẩn bị mã nguồn, cần xác thực danh tính AWS CLI và kiểm tra cấu hình vùng làm việc:

```bash
aws sts get-caller-identity
aws configure list
```

Nếu môi trường sử dụng SSO, cần đăng nhập trước khi tiếp tục:

```bash
aws sso login --profile <profile-name>
```

Kết quả xác thực này được dùng làm cơ sở kiểm tra quyền truy cập vào S3, DynamoDB và SageMaker ở các bước triển khai sau.

![Kiểm tra kết nối tới AWS account bằng STS](/images/5-Workshop/5.3-Step-by-step/aws-identity.jpg)