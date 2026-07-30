---
title: "Tải dataset và train mô hình"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

## 3. Tải dataset và train mô hình

### 3.1. Mục tiêu

Sau khi hạ tầng lưu trữ sẵn sàng, pipeline dữ liệu được chạy theo hướng batch-first. Dữ liệu thô được làm sạch, sinh đặc trưng, sau đó huấn luyện mô hình và đánh giá trước khi đồng bộ artifact lên S3.

### 3.2. Các bước thực hiện

1. Chạy pipeline xử lý dữ liệu cục bộ.
2. Sinh tập phim tương tự để phục vụ khuyến nghị theo nội dung.
3. Huấn luyện mô hình ALS trên tập dữ liệu đã xử lý.
4. Đánh giá mô hình để ghi lại chỉ số tham chiếu.
5. Đồng bộ artifact và báo cáo lên S3.

```bash
python scripts/run_data_pipeline.py
python scripts/build_similar_movies.py
python train.py --version v1.0.0
python evaluate.py --sample-users 5000
python scripts/aws_sync.py push --only artifacts reports
```

### 3.3. Kết quả mong đợi

- Artifact mô hình được tạo ra và sẵn sàng cho bước đóng gói.
- Báo cáo đánh giá được ghi lại để làm mốc so sánh cho các lần retrain sau.
- Dữ liệu phục vụ đã có thể được backend và quy trình triển khai sử dụng.

