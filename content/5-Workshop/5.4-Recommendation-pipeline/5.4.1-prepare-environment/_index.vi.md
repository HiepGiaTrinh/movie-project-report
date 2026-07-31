---
title: "Chuẩn bị môi trường gợi ý"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

## 1. Khởi tạo ML submodule

Tại thư mục gốc repository:

```bash
git submodule update --init --recursive
```

Xác nhận submodule có đủ:

- `configs/`
- `src/`
- `scripts/`
- `train.py`
- `evaluate.py`
- `retrain.py`

## 2. Tạo môi trường Python

Tại thư mục `ml`:

```bash
python -m venv .venv
python -m pip install \
  -r requirements.txt \
  -r requirements-aws.txt
```

`requirements-aws.txt` bổ sung boto3, botocore và SageMaker SDK cho launcher.

## 3. Kiểm tra cấu hình

| File | Nội dung |
|---|---|
| `configs/data_pipeline.yaml` | Đường dẫn dữ liệu và các bước pipeline |
| `configs/model_serving.yaml` | Hyperparameter và quy tắc serving |
| `configs/aws.yaml` | Region, bucket, prefix, processing instance và promotion gate |

Không hard-code ARN trong Hugo. Sử dụng placeholder `<SAGEMAKER_ROLE_ARN>` khi minh họa.

{{% notice warning %}}
Một số resource trong cấu hình ML chỉ là ví dụ. Phải thay bằng giá trị đã được owner xác nhận, không dùng trực tiếp cho môi trường production.
{{% /notice %}}

## 4. Chuẩn bị input

Pipeline cần tạo:

- `interactions_train.parquet`.
- Validation và test split.
- Content feature artifact.
- Serving lookup.
- Model directory ban đầu hoặc một model version cụ thể.

Chạy validation trước khi train:

```bash
python scripts/validate_data.py \
  --config configs/data_pipeline.yaml
```

Kết quả mong đợi: exit code `0` và `interactions_train.parquet` có các cột user, movie và interaction value.

## 5. Dry-run SageMaker Processing Job

Dry-run dựng source bundle và in job plan nhưng không gọi AWS:

```bash
python scripts/sagemaker_retrain_job.py --dry-run
```

Kiểm tra output có:

- Job name.
- AWS region.
- S3 bucket và input prefix.
- Processing instance type.
- Execution role placeholder/config.
- Các argument truyền vào wrapper.

Source bundle không được chứa dataset hoặc artifact lớn.

<!-- IMAGE-5.4.1-01: Dry-run output, đã che bucket và role ARN. -->

## Tiêu chí hoàn tất

- [ ] ML submodule checkout đúng commit đã ghim.
- [ ] Dependency cài thành công.
- [ ] Data validation trả exit code `0`.
- [ ] `LATEST.json` hoặc model version cụ thể đã được xác định.
- [ ] Dry-run không thực hiện network mutation.
- [ ] S3 bucket, role và quota SageMaker đã được xác nhận trước khi chạy cloud job.

**Nguồn đối chiếu:** `ml/requirements*.txt`, `ml/scripts/validate_data.py` và `ml/scripts/sagemaker_retrain_job.py`.
