---
title: "Tạo bảng DynamoDB"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

## 2. Tạo bảng DynamoDB

### 2.1. Mục tiêu

DynamoDB được sử dụng để lưu metadata phim, thông tin người dùng, lịch sử tương tác và bộ nhớ đệm gợi ý. Năm bảng chính cần được khởi tạo gồm `Movies`, `PopularMovies`, `Users`, `UserInteractions` và `RecommendationCache`.

### 2.2. Các bước thực hiện

1. Xác định tên bảng và khóa chính phù hợp với luồng truy cập.
2. Tạo các bảng theo chế độ on-demand để tránh cấu hình capacity thủ công.
3. Kiểm tra lại danh sách bảng sau khi tạo để bảo đảm tài nguyên đã sẵn sàng.
4. Đồng bộ tên bảng giữa backend, pipeline dữ liệu và cấu hình AWS.

```bash
aws dynamodb create-table --table-name Movies \
  --billing-mode PAY_PER_REQUEST --region ap-southeast-1
aws dynamodb create-table --table-name UserInteractions \
  --billing-mode PAY_PER_REQUEST --region ap-southeast-1
aws dynamodb create-table --table-name PopularMovies \
  --billing-mode PAY_PER_REQUEST --region ap-southeast-1
```

Kết quả sau khi tạo các bảng được minh họa như sau:

![Kiểm tra DynamoDB tables](/images/5-Workshop/5.3-Step-by-step/dynamodb-check.jpg)

### 2.3. Lưu ý triển khai

- Bảng `UserInteractions` là nguồn dữ liệu đầu vào cho vòng lặp retraining.
- Chế độ on-demand phù hợp với quy mô đồ án và giảm rủi ro phát sinh chi phí cố định.
- Tên bảng phải thống nhất với backend để tránh lỗi truy cập dữ liệu.

