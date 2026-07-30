---
title: "Tổng kết và dọn dẹp tài nguyên"
date: 2026-07-30
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Tổng kết và dọn dẹp tài nguyên

Workshop đã trình bày:

- Data pipeline và S3 artifact layout.
- Năm bảng DynamoDB cùng access pattern.
- Huấn luyện ALS, evaluation và promotion gate.
- SageMaker Processing Job.
- FastAPI provider, cache và failure path.
- Giả định triển khai ứng dụng trên EC2.
- Ranh giới IAM theo nguyên tắc đặc quyền tối thiểu.

## Tài nguyên có thể phát sinh chi phí

- SageMaker real-time endpoint và endpoint instance.
- SageMaker Processing Job khi đang chạy.
- EC2 instance và attached storage.
- DynamoDB request, backup và table.
- S3 current object, non-current version và multipart upload.
- CloudWatch Logs.
- Data transfer.
- Các tài nguyên mạng tùy chọn chưa được mô tả trong repository.

## Thứ tự dọn tài nguyên

{{< mermaid align="center" >}}
flowchart TD
    A[Dừng traffic, CI deploy và scheduler] --> B[Lưu logs, reports và artifacts cần giữ]
    B --> C[Dừng endpoint, job và EC2]
    C --> D[Xóa endpoint trước EndpointConfig và Model]
    D --> E[Export dữ liệu trước khi xóa DynamoDB]
    E --> F[Xóa object và version trước S3 bucket]
    F --> G[Gỡ policy trước khi xóa IAM role]
    G --> H[Kiểm tra Billing và resource inventory]
{{< /mermaid >}}

Thực hiện theo thứ tự:

1. Dừng traffic, CI deployment và retraining scheduler.
2. Export hoặc lưu logs, báo cáo, model manifest và dữ liệu cần giữ.
3. Dừng SageMaker job đang hoạt động.
4. Xóa Endpoint trước EndpointConfig và Model nếu owner xác nhận các resource này thuộc workshop.
5. Stop hoặc terminate EC2 theo retention policy.
6. Export dữ liệu DynamoDB cần giữ rồi mới xóa test item hoặc table.
7. Xóa S3 current object, version và multipart upload trước khi xóa bucket.
8. Detach managed/inline policy trước khi xóa IAM role.
9. Kiểm tra Billing, Cost Explorer và resource inventory.

## Vì sao không có lệnh xóa cụ thể?

{{% notice warning %}}
Repository không có cleanup script hoặc IaC phù hợp với kiến trúc hiện tại. Tự tạo delete command có thể xóa nhầm bucket hoặc table dùng chung. Vì vậy phần này chỉ cung cấp lệnh kiểm kê read-only; resource owner phải cung cấp runbook và exact target đã được phê duyệt.
{{% /notice %}}

## Dữ liệu có thể mất vĩnh viễn

- User account và password hash.
- `UserInteractions` dùng cho retraining.
- `RecommendationCache`.
- Movie catalog và popular ranking.
- Raw/processed dataset.
- Model artifact, manifest và evaluation history.
- S3 object version.
- Log và audit evidence.

## Kiểm kê read-only

Các lệnh sau chỉ liệt kê tài nguyên, không thực hiện cleanup:

```bash
aws sagemaker list-endpoints \
  --region "<AWS_REGION>"

aws sagemaker list-processing-jobs \
  --region "<AWS_REGION>"

aws ec2 describe-instances \
  --region "<AWS_REGION>" \
  --filters Name=instance-state-name,Values=running

aws dynamodb list-tables \
  --region "<AWS_REGION>"

aws s3api list-objects-v2 \
  --bucket "<S3_BUCKET_NAME>" \
  --max-items 10
```

<!-- IMAGE-5.6-01: Kiểm kê tài nguyên sau cleanup, đã che account ID và ARN. -->

{{% notice warning %}}
Không dùng bucket hoặc table name từ `.env.example` làm cleanup target. Phải xác nhận account, region, ARN, owner, retention và shared-resource classification trước mọi thao tác xóa.
{{% /notice %}}

## Checklist sau cleanup

- [ ] Không còn endpoint, EC2 hoặc Processing Job ngoài danh sách cần giữ.
- [ ] Test data và artifact chỉ bị xóa sau retention approval.
- [ ] Đã kiểm tra S3 version, không chỉ current object.
- [ ] IAM role chỉ được gỡ sau khi không còn resource tham chiếu.
- [ ] Billing và resource inventory đã được kiểm tra lại.
- [ ] Không cleanup command nào được chạy tự động từ report.

**Khoảng trống cần bổ sung:** cleanup script đã được review, retention policy, backup plan, resource ownership, shared-resource classification và billing verification procedure.
