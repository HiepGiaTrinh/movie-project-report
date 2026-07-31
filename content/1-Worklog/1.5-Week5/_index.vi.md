---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu

* Xây dựng phương pháp đánh giá mô hình một cách định lượng.
* Bắt đầu quản lý phiên bản mô hình một cách chính thức.

### Công việc đã thực hiện

* Triển khai các chỉ số Precision@10, Recall@10 và NDCG@10 trên tập test.
* Bổ sung cơ chế fallback "Top Trending" để xử lý user mới chưa có lịch sử rating.
* Đăng ký mô hình vào SageMaker Model Registry.
* Ghi lại model card ngắn gọn, bao gồm phiên bản dataset, hyperparameter và kết quả đánh giá.

### Kết quả

* Xác nhận mô hình hoạt động tốt hơn baseline chỉ dựa trên độ phổ biến (trending).
* Triển khai được chiến lược xử lý cold-start ở mức cơ bản.
* Phiên bản mô hình đầu tiên được đăng ký và theo dõi trong Model Registry.
