---
title: "Xử lý, tải và xác minh dữ liệu"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# Xử lý, tải và xác minh dữ liệu

Phần này chạy data pipeline cục bộ, kiểm tra tính quyết định của output, đồng bộ artifact lên S3 và xác định rõ khoảng trống nạp dữ liệu vào DynamoDB.

## 1. Chuẩn bị dữ liệu thô

Đặt các file CSV đúng tên trong thư mục raw được khai báo bởi `ml/configs/data_pipeline.yaml`. `ratings.csv` và `links.csv` là nguồn interaction lịch sử chính; các tập `small` chỉ phục vụ profiling.

Không commit dataset vào Git.

## 2. Chạy data pipeline

Tại thư mục `ml`:

```bash
python scripts/run_data_pipeline.py \
  --config configs/data_pipeline.yaml
```

Pipeline thực hiện tuần tự:

1. Profile dữ liệu.
2. Làm sạch dữ liệu.
3. Xây dựng feature.
4. Tạo split và serving export.
5. Chạy validation.

Critical validation failure phải trả exit code khác `0`.

## 3. Quy tắc xử lý dữ liệu

- MovieLens movie ID được ánh xạ sang TMDB ID bằng `links.csv`.
- Metadata trùng lặp giữ record đầy đủ hơn theo deterministic rule.
- Interaction alias trùng giữ timestamp mới nhất.
- Dữ liệu được chia theo từng user: interaction mới nhất vào test, interaction liền trước vào validation.
- JSONL output chỉ chứa các serving field nằm trong allowlist.

Ví dụ minh họa schema, không phải dữ liệu production:

```json
{
  "movie_id": "<MOVIE_ID>",
  "title": "<TITLE>",
  "genres": ["<GENRE>"],
  "poster_path": "<RELATIVE_TMDB_PATH>"
}
```

## 4. Kiểm tra tính quyết định và test

```bash
python scripts/check_determinism.py \
  --config configs/data_pipeline.yaml

python -m pytest -q
```

Kết quả mong đợi:

- Validation không có critical failure.
- Determinism check thành công.
- Các test ML pass trong môi trường dependency tương thích.

<!-- IMAGE-5.3.2-01: Kết quả data validation, đã che username và local path. -->

## 5. Đồng bộ lên S3

Luôn chạy dry-run trước:

```bash
python scripts/aws_sync.py push --dry-run
```

Sau khi review danh sách object và được phê duyệt:

```bash
python scripts/aws_sync.py push
```

<!-- IMAGE-5.3.2-02: Danh sách artifact trong các prefix S3, đã che bucket/account ID. -->

{{% notice warning %}}
`aws_sync.py` hiện không có raw data trong `sync.pairs`; hướng dẫn `--only raw` trong tài liệu S3 cấp root không khớp với code hiện tại.
{{% /notice %}}

## 6. Nạp dữ liệu vào DynamoDB

Pipeline tạo các serving JSONL nhưng repository chưa có loader chính thức cho `Movies` và `PopularMovies`.

{{% notice warning %}}
Không tự viết câu lệnh nạp DynamoDB trong report rồi mô tả như một bước đã được hỗ trợ. Chỉ coi data layer sẵn sàng khi loader, idempotency rule và bước xác minh item đã được bổ sung.
{{% /notice %}}

## 7. Export interaction cho retraining

Exporter quét bảng `UserInteractions`, chuẩn hóa trạng thái reaction/rating và có thể ghi JSONL cục bộ hoặc tải lên interaction export prefix:

```bash
python scripts/export_interactions.py --upload
```

Full table scan có chi phí tỷ lệ với kích thước bảng. Repository hiện chưa có scheduler chuẩn nối exporter với SageMaker Processing Job.

## Tiêu chí hoàn tất

- [ ] Required Parquet/JSONL artifact tồn tại.
- [ ] Validation report phân loại rõ `PASS`, `WARNING` và `FAIL`.
- [ ] S3 object nằm đúng prefix.
- [ ] Serving export không có duplicate primary key.
- [ ] DynamoDB loading vẫn được ghi là chưa hoàn chỉnh cho đến khi có loader.

**Nguồn đối chiếu:** `ml/DATA_PIPELINE.md`, `ml/configs/data_pipeline.yaml`, `ml/scripts/aws_sync.py` và `ml/scripts/export_interactions.py`.
