---
title: "Tầng dữ liệu với S3 và DynamoDB"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Tầng dữ liệu với S3 và DynamoDB

Amazon S3 và Amazon DynamoDB đảm nhận hai vai trò khác nhau:

- **S3** chứa dataset và artifact có kích thước lớn, được sử dụng trong các quy trình batch.
- **DynamoDB** phục vụ lookup và lưu trạng thái trong request path của ứng dụng.

{{< mermaid align="center" >}}
flowchart LR
    Raw[Kaggle CSV] --> Pipeline[Data Pipeline]
    Pipeline --> Local[Parquet, JSONL, Artifacts]
    Local --> S3[(Amazon S3)]
    Local -. Chưa có loader .-> Catalog[(Movies / PopularMovies)]
    UI[Frontend] --> API[FastAPI]
    API --> Interactions[(UserInteractions)]
    Interactions --> Exporter[Interaction Exporter]
    Exporter --> S3
    S3 --> Training[Retraining]
{{< /mermaid >}}

<!-- IMAGE-5.3-01: Sơ đồ luồng dữ liệu dựa trên các thành phần đã xác nhận. -->

## Cấu trúc dữ liệu trên S3

Các vùng logic gồm:

- `raw`: dữ liệu CSV ban đầu.
- `processed`: dữ liệu đã làm sạch và feature.
- `training`: các tập train, validation và test.
- `inference`: lookup artifact cho ML engine.
- `models`: model artifact được quản lý theo version.
- `evaluation`: báo cáo đánh giá và promotion.
- `interaction exports`: feedback từ DynamoDB phục vụ retraining.

Backend HTTP handler không tải dataset từ S3 trong request path.

## Năm bảng DynamoDB

| Bảng | Khóa chính | Mục đích |
|---|---|---|
| `Movies` | `movie_id` | Metadata phim |
| `PopularMovies` | `list_id` | Danh sách phim phổ biến cho guest |
| `Users` | `user_id` | Tài khoản, profile và onboarding |
| `UserInteractions` | `user_id`, `interaction_key` | Click, watch, rating, reaction và share |
| `RecommendationCache` | `user_id`, `scenario` | Cache kết quả gợi ý theo ngữ cảnh |

`Movies` là nguồn metadata chính. `PopularMovies` và `RecommendationCache` chỉ giữ tham chiếu tới phim thay vì nhân bản toàn bộ metadata.

## Quan hệ với hệ thống gợi ý

1. Guest ranking đọc `PopularMovies` rồi bổ sung metadata từ `Movies`.
2. Hành vi của người dùng được ghi vào `UserInteractions`.
3. Interaction có thể được export sang S3 cho lần retrain tiếp theo.
4. Cache cá nhân hóa lưu `movie_id`, `score` và `reason_code`.
5. Kết quả từ cache hoặc provider được enrich bằng `Movies` trước khi trả về API.

{{% notice note %}}
`datasets/serving/` là vùng export logic dành cho dữ liệu phục vụ DynamoDB, còn `inference/` chứa lookup data cho ML engine. Không trộn hai vùng này.
{{% /notice %}}

## Các bước tiếp theo

1. [Kiểm tra và chuẩn bị tài nguyên lưu trữ](5.3.1-provision-storage/)
2. [Xử lý, tải và xác minh dữ liệu](5.3.2-load-and-validate-data/)

**Nguồn đối chiếu:** `docs/aws/s3.md`, `docs/aws/dynamodb.md`, `backend/app/container.py` và ML data pipeline.
