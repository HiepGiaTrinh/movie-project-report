---
title: "Deploy lên SageMaker Endpoint"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

## 4. Deploy lên SageMaker Endpoint

### 4.1. Mục tiêu

Sau khi artifact mô hình đã được tạo, mô hình được đóng gói và triển khai thành SageMaker Endpoint để phục vụ suy luận theo yêu cầu. Cách triển khai này cho phép hệ thống trả về khuyến nghị mà không cần train lại trong thời gian chạy.

### 4.2. Các bước thực hiện

1. Đóng gói artifact mô hình thành model bundle.
2. Thực hiện dry-run để kiểm tra tham số triển khai.
3. Triển khai endpoint lên SageMaker.
4. Gửi request kiểm tra để xác nhận phản hồi hợp lệ.
5. Theo dõi trạng thái endpoint và log nếu cần.

```bash
python scripts/build_model_bundle.py --upload
python scripts/deploy_endpoint.py --dry-run
python scripts/deploy_endpoint.py
python scripts/invoke_endpoint.py --demo
```

### 4.3. Lưu ý triển khai

- Endpoint chỉ xử lý suy luận, còn metadata phim được backend ghép từ bảng `Movies`.
- Request kiểm tra nên được chạy ngay sau khi deploy để xác nhận artifact đã nạp đúng.
- Kết quả trả về cần được đối chiếu với bản chạy cục bộ để bảo đảm tính nhất quán.

