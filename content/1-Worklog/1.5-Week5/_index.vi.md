---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Đánh giá mô hình gợi ý một cách định lượng.
* Theo dõi phiên bản mô hình bằng SageMaker Model Registry.

### Công việc đã làm:

* Triển khai các chỉ số đánh giá cho mô hình gợi ý: **Precision@10**, **Recall@10** và **NDCG@10** trên tập test.
* Thêm cơ chế fallback **"Top Trending"** đơn giản để xử lý bài toán cold-start cho user mới chưa có lịch sử rating.
* Đăng ký phiên bản mô hình đã huấn luyện vào **SageMaker Model Registry** để theo dõi lineage và trạng thái phê duyệt.
* Ghi lại model card: phiên bản dataset, hyperparameter và kết quả đánh giá.

### Kết quả đạt được tuần 5:

* Kết quả đánh giá định lượng cho thấy mô hình vượt trội hơn baseline chỉ dựa trên trending.
* Chiến lược fallback cold-start hoạt động tốt, đã kiểm thử với user mới giả lập.
* Phiên bản mô hình đầu tiên được phê duyệt và theo dõi trong SageMaker Model Registry.
