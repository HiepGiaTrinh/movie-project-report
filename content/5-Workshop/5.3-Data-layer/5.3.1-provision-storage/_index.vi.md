---
title: "Kiểm tra và chuẩn bị tài nguyên lưu trữ"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# Kiểm tra và chuẩn bị tài nguyên lưu trữ

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

<!-- IMAGE-5.3.1-01: Danh sách năm bảng ACTIVE và key schema, đã che ARN/account ID. -->

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

```bash
aws s3api head-bucket \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-public-access-block \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-bucket-encryption \
  --bucket "<S3_BUCKET_NAME>"
```

Bucket phải tồn tại, có Block Public Access và encryption phù hợp. Versioning và lifecycle được khuyến nghị trong tài liệu nhưng không được provision bởi source code.

<!-- IMAGE-5.3.1-02: Thiết lập encryption, public access block và versioning của S3, đã che thông tin nhạy cảm. -->

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

{{% notice warning %}}
Không tự tạo bảng hoặc bucket bằng câu lệnh chưa được review. Tài liệu cũ trong ML submodule mô tả schema khác với schema backend hiện tại và không được dùng làm runbook chính thức.
{{% /notice %}}

## Tiêu chí hoàn tất

- [ ] Năm bảng đều `ACTIVE`.
- [ ] Key schema khớp hoàn toàn.
- [ ] S3 bucket có thể truy cập.
- [ ] Block Public Access và encryption đã được cấu hình.
- [ ] TTL `expire_at` được ghi là đã xác nhận hoặc chưa xác nhận.
- [ ] Không tài nguyên nào bị tạo hoặc thay đổi trong bước kiểm tra.

**Nguồn đối chiếu:** `backend/app/aws/infrastructure.py`, `docs/aws/dynamodb.md` và `docs/aws/aws-setup.md`.
