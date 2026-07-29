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
- Region phù hợp, ví dụ: `ap-southeast-1` hoặc region mà nhóm đang dùng.
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

> [!NOTE]
> Đây là phần cần xem xét lại theo môi trường thực tế của nhóm. Nếu IAM hiện tại đã được thiết kế sẵn thì nên cập nhật lại chính xác danh sách quyền vừa đủ, thay vì ghi chung chung.

### 1.3. Các tài nguyên AWS cần chuẩn bị

- 5 bảng DynamoDB: Movies, PopularMovies, Users, UserInteractions, RecommendationCache.
- 1 bucket S3 để lưu dataset và artifact.
- Các prefix S3 cần được cấu hình đúng.
- Biến môi trường `.env` với thông tin region, tên table, bucket, JWT secret và URL API.

![Placeholder: Screenshot chuẩn bị AWS account và IAM](/images/5-Workshop/placeholder-prerequisite-iam.png)

![Placeholder: Screenshot kiểm tra AWS CLI và STS](/images/5-Workshop/placeholder-prerequisite-cli.png)