---
title : "Chuẩn bị môi trường"
date : 2026-07-29
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

## 1. Prerequisite

### 1.1. Điều kiện ban đầu

Trước khi triển khai, cần chuẩn bị đầy đủ các yếu tố sau:

- Tài khoản AWS có quyền tạo và quản lý resource.
- Region phù hợp, ví dụ: `ap-southeast-1` là region nhóm đang dùng.
- AWS CLI v2 đã cài đặt và đăng nhập thành công.
- Docker Engine và Docker Compose v2 đã sẵn sàng.
- Git và submodule đã được cấu hình đúng.
- IAM có quyền truy cập vào các dịch vụ cần dùng như STS, DynamoDB, S3 và SageMaker (nếu bật inference thật).

### 1.2. IAM và quyền cần thiết

Các quyền cần có có thể bao gồm:

- `sts:GetCallerIdentity`
- `dynamodb:DescribeTable`, `GetItem`, `BatchGetItem`, `PutItem`, `DeleteItem`, `Query`, `Scan`
- `s3:ListBucket`, `GetBucketLocation`, `GetObject`, `PutObject`
- `sagemaker:DescribeEndpoint`, `InvokeEndpoint` (khi real-time inference đã được bật)

### 1.3. Các tài nguyên AWS cần chuẩn bị

- 5 bảng DynamoDB: Movies, PopularMovies, Users, UserInteractions, RecommendationCache.
- 1 bucket S3 để lưu dataset và artifact.
- Các prefix S3 cần được cấu hình đúng.
- Biến môi trường `.env` với thông tin region, tên table, bucket, JWT secret và URL API.

### 1.4. Chuẩn bị mã nguồn cục bộ

Trước khi tiến hành triển khai, cần tải mã nguồn về máy cục bộ và đồng bộ submodule nếu dự án có tách module:

```bash
git clone --recurse-submodules <repo-url>
cd movie-recommendation
git submodule update --init --recursive
```

Bước này bảo đảm toàn bộ thành phần backend, frontend và module mô hình được kiểm tra trong cùng một trạng thái mã nguồn.

### 1.5. Xác thực danh tính và cấu hình AWS

Sau khi chuẩn bị mã nguồn, cần xác thực danh tính AWS CLI và vùng làm việc đang sử dụng:

```bash
aws sts get-caller-identity
aws configure list
```

Nếu môi trường sử dụng SSO, cần đăng nhập trước khi tiếp tục:

```bash
aws sso login --profile <profile-name>
```

Kết quả xác thực này được dùng để kiểm tra quyền truy cập vào S3, DynamoDB và SageMaker trong các bước triển khai tiếp theo.

![Kiểm tra kết nối tới AWS account bằng STS](/images/5-Workshop/5.3-Step-by-step/aws-identity.jpg)