---
title: "IAM, nguyên tắc đặc quyền tối thiểu và bảo mật"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Repository sử dụng default credential provider chain của boto3:

- Developer nên dùng AWS IAM Identity Center hoặc profile.
- EC2 sử dụng instance profile.
- SageMaker sử dụng execution role.

Exact role name, JSON policy, trust relationship và ARN chưa được lưu trong repository.

![Luồng credential giữa developer, EC2 và các dịch vụ AWS](/images/5-Workshop/5.5-IAM-security/security-credential-flow.png)

*Developer profile hoặc EC2 instance profile cung cấp credential cho boto3; JWT secret được quản lý riêng cho FastAPI authentication.*

## Ma trận principal và quyền

| Principal | Quyền cần thiết | Phạm vi tài nguyên |
|---|---|---|
| Backend EC2 role | `sts:GetCallerIdentity` | Identity hiện tại |
| Backend EC2 role | DynamoDB describe/get/batch-get/put/query/scan theo code path | ARN của năm bảng và index liên quan |
| Backend EC2 role | S3 list cho startup validation | Một bucket, giới hạn prefix khi có thể |
| Backend EC2 role | `sagemaker:DescribeEndpoint`, `sagemaker:InvokeEndpoint` | Một endpoint |
| S3/ML operator | S3 list/get/put | Data, model và report prefix |
| Processing submitter | Tạo/mô tả Processing Job, `iam:PassRole` | Job namespace và một execution role |
| SageMaker execution role | S3 list/get/put, CloudWatch Logs | Prefix và log group cụ thể |
| GitHub deployer | SSH credential trong GitHub Secrets | Một EC2 host |

{{% notice warning %}}
Không sử dụng `Action: "*"` hoặc `Resource: "*"` chỉ để làm cho hệ thống chạy. Policy production phải được security owner review.
{{% /notice %}}

## Nhận xét về least privilege

- Backend HTTP runtime không cần `DeleteItem` vì hiện không có delete API.
- Quyền S3 upload/download nên thuộc tooling role riêng thay vì mở rộng web runtime role.
- `Users` hiện cần scan cho case-insensitive login/uniqueness vì không có identity GSI.
- Principal triển khai SageMaker và SageMaker execution role là hai principal khác nhau.
- `iam:PassRole` chỉ được phép đối với đúng execution role.

## Bảo mật ứng dụng

- Password sử dụng PBKDF2-HMAC-SHA256 với random salt và số vòng lặp được cấu hình.
- JWT HS256 kiểm tra signature, issuer, audience và expiry.
- Protected action của guest trả `401`.
- User chưa hoàn thành onboarding nhận `403` khi gọi recommendation.
- Frontend lưu access token trong `localStorage`, vì vậy cần kiểm soát XSS.
- Logout hiện không revoke JWT ở server; endpoint chỉ trả `204`.

## Positive test

Với backend role đã được gắn đúng:

```bash
aws sts get-caller-identity \
  --region "<AWS_REGION>"

aws dynamodb describe-table \
  --table-name "<AUTHORIZED_TABLE_NAME>" \
  --region "<AWS_REGION>"

aws sagemaker describe-endpoint \
  --endpoint-name "<SAGEMAKER_ENDPOINT_NAME>" \
  --region "<AWS_REGION>"
```

Tiêu chí đạt: cả ba thao tác được phê duyệt đều thành công.

{{% notice note %}}
`sts:GetCallerIdentity` có hành vi đặc biệt và không đủ để chứng minh principal có quyền truy cập DynamoDB, S3 hoặc SageMaker.
{{% /notice %}}

<!-- IMAGE-5.5-01: IAM role với policy giới hạn tài nguyên, đã che ARN/account ID. -->

## Negative test an toàn

Security owner phải cung cấp một resource ngoài phạm vi được phê duyệt dành riêng cho kiểm thử:

```bash
aws dynamodb describe-table \
  --table-name "<APPROVED_OUT_OF_SCOPE_TEST_TABLE>" \
  --region "<AWS_REGION>"
```

Kết quả mong đợi: `AccessDeniedException`.

`ResourceNotFoundException` không chứng minh least privilege vì resource có thể không tồn tại.

<!-- IMAGE-5.5-02: AccessDenied khi truy cập resource kiểm thử ngoài phạm vi. -->

{{% notice warning %}}
Nếu chưa có resource kiểm thử được phê duyệt, giữ negative test ở trạng thái chưa hoàn tất. Không chọn ngẫu nhiên một production resource để thử quyền.
{{% /notice %}}

## Checklist

- [ ] Các thao tác hợp lệ thành công.
- [ ] Thao tác ngoài phạm vi bị từ chối.
- [ ] Policy không chứa wildcard rộng.
- [ ] Report và log không chứa secret.
- [ ] EC2 sử dụng instance profile.
- [ ] SageMaker execution role tin cậy đúng service principal.
- [ ] Tên role, trust policy và resource ARN đã được security owner xác nhận.

**Nguồn đối chiếu:** `backend/app/aws/infrastructure.py`, `docs/aws/aws-setup.md`, security module và authentication middleware.
