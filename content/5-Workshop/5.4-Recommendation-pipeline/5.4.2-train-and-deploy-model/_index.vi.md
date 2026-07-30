---
title: "Huấn luyện, đánh giá và chạy SageMaker Processing Job"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# Huấn luyện, đánh giá và chạy SageMaker Processing Job

## 1. Huấn luyện collaborative model

`train.py` đọc chronological training split, áp dụng quy tắc positive, negative và neutral, sau đó huấn luyện mô hình **implicit ALS**.

Tại thư mục `ml`:

```bash
python train.py --version "<MODEL_VERSION>"
```

Artifact được lưu theo version và có thể gồm:

- User factors và item factors.
- User/item index.
- Manifest.
- Model configuration.
- Content artifact.

## 2. Đánh giá mô hình

```bash
python evaluate.py --version "<MODEL_VERSION>"
```

`evaluate.py` tính các nhóm metric:

- HitRate.
- Precision.
- NDCG.
- Catalog coverage.

{{% notice note %}}
Không sử dụng số metric từ tài liệu hoặc báo cáo cũ làm kết quả hiện tại. Report phải lấy metric từ artifact được tạo bởi chính lần chạy workshop.
{{% /notice %}}

## 3. Promotion gate

Chạy retraining ở chế độ dry-run:

```bash
python retrain.py \
  --version "<MODEL_VERSION>" \
  --dry-run
```

`retrain.py` chỉ cập nhật `LATEST.json` khi candidate:

1. Có đủ số user được đánh giá.
2. Tốt hơn popularity baseline theo metric cấu hình.
3. Không regression quá giới hạn cho phép so với model đang phục vụ.

Candidate không vượt gate vẫn được giữ lại để điều tra nhưng không được promote.

{{% notice warning %}}
`--force-promote` bỏ qua quality gate và không nên được sử dụng trong quy trình workshop thông thường.
{{% /notice %}}

## 4. Chạy SageMaker Processing Job

Launcher dựng source bundle rồi dùng `FrameworkProcessor` để chạy wrapper `deploy/sagemaker_retrain.py`. Job lấy input từ S3 và đẩy output trở lại S3.

Sau khi dry-run, review IAM và xác nhận chi phí:

```bash
python scripts/sagemaker_retrain_job.py \
  --version "<MODEL_VERSION>" \
  --events "s3://<S3_BUCKET_NAME>/<INTERACTION_EXPORT_PREFIX>" \
  --wait
```

<!-- IMAGE-5.4.2-01: SageMaker Processing Job ở trạng thái Completed, đã che ARN/account ID. -->

Kiểm tra trạng thái:

```bash
aws sagemaker describe-processing-job \
  --processing-job-name "<PROCESSING_JOB_NAME>" \
  --region "<AWS_REGION>"
```

Job phải đạt trạng thái `Completed`.

## 5. Kiểm tra artifact và báo cáo

Xác nhận S3 có đúng model version, manifest, `LATEST.json` và evaluation report.

<!-- IMAGE-5.4.2-02: Evaluation report và promotion decision, không chứa dữ liệu người dùng. -->

## 6. Khoảng trống real-time endpoint

{{% notice warning %}}
Repository không chứa `model_fn`, `input_fn`, `predict_fn`, `output_fn`, `transform_fn` hoặc script tạo SageMaker Model, EndpointConfig và Endpoint. Vì vậy không có câu lệnh hợp lệ để hoàn tất endpoint deployment từ source hiện tại.
{{% /notice %}}

Chỉ thêm ảnh endpoint `InService` sau khi team bổ sung:

- Serving handler tương thích request/response contract của backend.
- Quy trình tạo `model.tar.gz`.
- Image/runtime tương thích.
- Model, EndpointConfig và Endpoint deployment.
- Cơ chế rollback và autoscaling.

<!-- IMAGE-5.4.2-03: Chỉ thêm sau khi endpoint deployment được bổ sung và xác minh. -->

## Tiêu chí hoàn tất

- [ ] Model version directory được tạo.
- [ ] Evaluation report có metric name và số user được đánh giá.
- [ ] Promotion report ghi rõ pass/fail cho từng gate.
- [ ] Processing Job đạt `Completed`.
- [ ] S3 có artifact và report đúng prefix.
- [ ] Endpoint deployment vẫn được ghi là chưa hoàn chỉnh, không giả lập thành công.

**Nguồn đối chiếu:** `ml/train.py`, `ml/evaluate.py`, `ml/retrain.py` và `ml/scripts/sagemaker_retrain_job.py`.
