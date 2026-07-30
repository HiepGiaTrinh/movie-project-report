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
