---
title: "Kiểm tra và chuẩn bị tài nguyên lưu trữ"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

Repository không chứa Terraform, CloudFormation, CDK hoặc công cụ Infrastructure as Code tương đương. Vì vậy phần này chỉ kiểm tra những tài nguyên đã được platform team tạo, không tự động provision tài nguyên mới.

## 1. Kiểm tra năm bảng DynamoDB

Với từng tên bảng được cung cấp qua kênh bảo mật, chạy:

```bash
aws dynamodb describe-table \
  --table-name "<MOVIES_TABLE_NAME>" \
  --region "<AWS_REGION>"
```

Lặp lại cho bốn bảng còn lại và xác nhận:

| Bảng | HASH key | RANGE key |
|---|---|---|
| `Movies` | `movie_id` | Không có |
| `PopularMovies` | `list_id` | Không có |
| `Users` | `user_id` | Không có |
| `UserInteractions` | `user_id` | `interaction_key` |
| `RecommendationCache` | `user_id` | `scenario` |

Tất cả bảng phải ở trạng thái `ACTIVE`.

![Năm bảng DynamoDB ở trạng thái Active](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/dynamodb-tables.png)

*Năm bảng DynamoDB cùng partition key, sort key và trạng thái Active.*

## 2. Kiểm tra thuộc tính bổ sung

Thiết kế hiện tại không sử dụng GSI. `RecommendationCache` có field `expire_at`, nhưng source code không chứng minh TTL đã được bật trên tài nguyên thật.

Các thuộc tính cần xác nhận ngoài source:

- TTL dùng attribute `expire_at`.
- Billing mode/capacity.
- Point-in-time recovery.
- Encryption.
- Tags và ownership.

Nếu chưa có bằng chứng từ AWS Console hoặc CLI, hãy ghi trạng thái là **chưa xác nhận**, không suy đoán.

## 3. Kiểm tra S3 bucket

![S3 bucket của hệ thống gợi ý phim](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket.png)

*S3 bucket được sử dụng để lưu dataset, model artifact và báo cáo.*

![Thông tin tổng quan của S3 bucket](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket-overview.png)

*Bucket được triển khai tại Region `ap-southeast-1`; ảnh tổng quan xác nhận trực tiếp ARN và Region của tài nguyên đang được kiểm tra.*

```bash
aws s3api head-bucket \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-public-access-block \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-bucket-encryption \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-bucket-versioning \
  --bucket "<S3_BUCKET_NAME>"
```

Bucket phải tồn tại, có Block Public Access và encryption phù hợp. Đối với bucket được khảo sát, Block Public Access đang bật, mã hóa mặc định là SSE-S3 và versioning đang ở trạng thái `Enabled`. Source code ứng dụng không tự provision các thiết lập này; lifecycle vẫn cần được kiểm tra riêng nếu được áp dụng.

![Thiết lập Block Public Access của S3 bucket](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-block-public-access.png)

*`Block all public access` đang ở trạng thái `On`, ngăn truy cập công khai qua ACL và bucket policy.*

![Thiết lập mã hóa mặc định của S3 bucket](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket-encryption.png)

*Mã hóa mặc định phía máy chủ sử dụng khóa do Amazon S3 quản lý (SSE-S3).*

![Thiết lập versioning của S3 bucket](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket-versioning.png)

*Bucket Versioning đang ở trạng thái `Enabled`; MFA delete đang `Disabled`.*

## 4. Kiểm tra các prefix

Chỉ lấy tối đa một object để tránh đọc dữ liệu không cần thiết:

```bash
aws s3api list-objects-v2 \
  --bucket "<S3_BUCKET_NAME>" \
  --prefix "<RAW_PREFIX>" \
  --max-items 1
```

Lặp lại với các prefix `processed`, `training`, `inference`, `models`, `evaluation` và `interaction exports`.

Prefix rỗng không nhất thiết là lỗi. `AccessDenied`, sai region hoặc bucket không tồn tại mới là dấu hiệu cần xử lý.

## 5. Khi tài nguyên chưa tồn tại

Nếu thiếu bảng hoặc bucket:

1. Dừng bước triển khai.
2. Ghi lại region, key schema, billing mode, TTL, encryption, lifecycle và IAM owner cần thiết.
3. Yêu cầu platform/security team cung cấp tài nguyên hoặc IaC đã được review.

<!-- ## Tiêu chí hoàn tất

- [ ] Năm bảng đều `ACTIVE`.
- [ ] Key schema khớp hoàn toàn.
- [ ] S3 bucket có thể truy cập.
- [ ] Block Public Access và encryption đã được cấu hình.
- [ ] TTL `expire_at` được ghi là đã xác nhận hoặc chưa xác nhận.
- [ ] Không tài nguyên nào bị tạo hoặc thay đổi trong bước kiểm tra.

**Nguồn đối chiếu:** `backend/app/aws/infrastructure.py`, `docs/aws/dynamodb.md` và `docs/aws/aws-setup.md`. -->
