---
title: "Tạo bucket S3"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

## 1. Tạo bucket S3

### 1.1. Mục tiêu

Bucket S3 được sử dụng để lưu dữ liệu đầu vào, dữ liệu đã xử lý, dữ liệu phục vụ và artifact mô hình. Việc khởi tạo bucket cần được thực hiện trước để các bước đồng bộ dữ liệu và huấn luyện mô hình có thể ghi kết quả lên cùng một không gian lưu trữ.

### 1.2. Các bước thực hiện

1. Tạo bucket với tên duy nhất trên toàn cầu.
2. Bật chặn truy cập công khai để hạn chế rủi ro cấu hình sai.
3. Bật versioning để bảo toàn các phiên bản artifact khi ghi đè.
4. Thiết lập lifecycle rule để kiểm soát dung lượng lưu trữ khi versioning được bật.

```bash
aws s3 mb s3://movie-recommendation-fcaj --region ap-southeast-1
aws s3api put-public-access-block --bucket movie-recommendation-fcaj \
  --public-access-block-configuration "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"
aws s3api put-bucket-versioning --bucket movie-recommendation-fcaj \
  --versioning-configuration Status=Enabled
```

Kết quả sau khi tạo bucket và cấu hình versioning được minh họa như sau:

![Kiểm tra S3 bucket](/images/5-Workshop/5.3-Step-by-step/s3-check.jpg)

### 1.3. Cấu trúc lưu trữ đề xuất

- `raw/`: dữ liệu thô từ nguồn gốc.
- `processed/`: dữ liệu đã làm sạch.
- `features/`: đặc trưng phục vụ huấn luyện.
- `splits/`: tập train, validation và test.
- `serving/`: dữ liệu phục vụ suy luận.
- `artifacts/`: artifact của các phiên bản mô hình.
- `events/`: file JSONL xuất từ tương tác người dùng.
- `reports/`: báo cáo đánh giá và retrain.

