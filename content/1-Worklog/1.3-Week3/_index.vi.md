---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3

* Setup SageMaker Studio làm chỗ làm việc chính.
* Có được bước tiền xử lý chạy lại được, khỏi phải làm tay mỗi lần.

### Đã làm gì

* Học "Amazon SageMaker Studio for Data Scientists" trên Skill Builder.
* Setup SageMaker Studio domain và profile ở ap-southeast-2.
* Viết SageMaker Processing job để làm sạch data MovieLens và dựng ma trận user-item.
* Chia train/test theo thời gian thay vì random — hợp lý hơn cho bài toán recommender.
* Lưu data đã xử lý ngược lại S3, folder `processed/`.

### Kết quả

* Processing job đầu tiên chạy xong hết mà không cần ngồi canh.
* Có pipeline chạy lại được bất cứ lúc nào, ra ngay data sẵn sàng để train.
* Hiểu rõ hơn vì sao chia dữ liệu theo mốc thời gian lại quan trọng ở bài toán này.
