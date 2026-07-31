---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu

* Thiết lập SageMaker Studio làm không gian làm việc chính.
* Xây dựng bước tiền xử lý dữ liệu có thể tái sử dụng thay vì thao tác thủ công.

### Công việc đã thực hiện

* Hoàn thành khoá "Amazon SageMaker Studio for Data Scientists" trên AWS Skill Builder.
* Thiết lập SageMaker Studio domain và user profile tại khu vực ap-southeast-2.
* Xây dựng SageMaker Processing job để làm sạch dữ liệu MovieLens và dựng ma trận user-item.
* Triển khai phương pháp chia train/test theo thời gian thay vì ngẫu nhiên, phản ánh đúng tính chất tuần tự của hệ thống gợi ý.
* Lưu dữ liệu đã xử lý trở lại S3 tại prefix `processed/`.

### Kết quả

* SageMaker Processing job đầu tiên chạy thành công end-to-end.
* Xây dựng được pipeline có thể tái sử dụng, cho ra bộ dữ liệu sẵn sàng để huấn luyện.
* Hiểu rõ hơn tầm quan trọng của việc chia dữ liệu theo mốc thời gian đối với hệ thống gợi ý.
