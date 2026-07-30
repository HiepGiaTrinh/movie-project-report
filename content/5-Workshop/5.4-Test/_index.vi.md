---
title : "Test & validation"
date : 2026-07-29
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

## 3. Test & validation

### 3.1. Kiểm tra log

Log hệ thống có thể được theo dõi thông qua:

```bash
docker compose logs backend --tail 100
docker compose logs frontend --tail 100
```

Log được sử dụng để kiểm tra các nội dung sau:

- Backend khởi động thành công.
- Request gửi đến API có phát sinh lỗi hay không.
- Thông tin AWS validation và tài nguyên liên quan.

### 3.2. Kiểm tra metric

Hiện tại, repository chưa cung cấp một stack CloudWatch metrics/alerts được triển khai sẵn. Vì vậy, phần này có thể được bổ sung theo hướng:

- Metric cơ bản có thể theo dõi bằng log và health check.
- Nếu cần mở rộng trong tương lai, nên bổ sung CloudWatch metric và alarm.

### 3.3. Kiểm tra alert

Về alert, nhóm ghi nhận rằng:

- Chưa có pipeline alert thực thi sẵn trong repository.
- Có thể đề xuất dùng CloudWatch Alarm kết hợp SNS trong tương lai.

### 3.4. Kiểm tra triển khai SageMaker Processing Job

Trong kiến trúc hiện tại, SageMaker được sử dụng cho Processing Job thay vì real-time endpoint. Vì vậy, bước kiểm thử cần tập trung vào việc xác nhận job được tạo đúng cấu hình, chạy thành công và sinh ra artifact hợp lệ.

Các bước kiểm tra chính gồm:

1. Chạy lệnh `--dry-run` để xác nhận region, bucket, source bundle và framework trước khi submit job thật.
2. Theo dõi trạng thái job bằng `describe-processing-job` cho đến khi trạng thái chuyển sang `Completed`.
3. Quan sát log trong CloudWatch để kiểm tra phiên bản Python của container, dependency được cài đặt đúng và không có lỗi trong quá trình xử lý.
4. Xác nhận các đầu ra được đẩy về S3, bao gồm artifact mô hình, `LATEST.json` và báo cáo đánh giá.

Kết quả kiểm thử cần cho thấy job không chỉ khởi tạo thành công mà còn tạo ra đầy đủ dữ liệu đầu ra để backend có thể sử dụng ở bước triển khai tiếp theo.
