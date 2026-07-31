---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5

* Đo được model tốt cỡ nào bằng số liệu, chứ không chỉ nhìn qua loa.
* Bắt đầu quản lý version model đàng hoàng.

### Đã làm gì

* Tính Precision@10, Recall@10, NDCG@10 trên tập test.
* Thêm fallback "Top Trending" cho user mới chưa có rating nào.
* Đăng ký model vào SageMaker Model Registry.
* Ghi lại model card ngắn gọn — version dataset, hyperparameter, kết quả.

### Kết quả

* Số liệu cho thấy model thật sự tốt hơn baseline chỉ dựa trending, cũng đỡ lo.
* Xử lý được case cold-start, ít nhất là ở mức cơ bản.
* Model version đầu tiên được đăng ký và theo dõi đàng hoàng, không còn nằm trơ trên S3 không ai biết là bản nào.
