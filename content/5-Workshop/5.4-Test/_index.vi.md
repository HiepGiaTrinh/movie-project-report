---
title : "Test & validation"
date : 2026-07-29
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

## 3. Test & validation

### 3.1. Kiểm tra log

Có thể theo dõi log qua:

```bash
docker compose logs backend --tail 100
docker compose logs frontend --tail 100
```

Log nên được dùng để kiểm tra:

- Backend khởi động thành công.
- Request đến API có lỗi hay không.
- Thông tin AWS validation và resource check.

### 3.2. Kiểm tra metric

Hiện tại, repo chưa cung cấp một stack CloudWatch metrics/alerts được code hóa sẵn. Vì vậy, phần này nên được trình bày theo hướng:

- Metric cơ bản có thể theo dõi bằng log và health check.
- Nếu cần mở rộng trong tương lai, nên bổ sung CloudWatch metric và alarm.

> [!NOTE]
> Đây là phần cần bổ sung sau. Hiện tại chưa có bằng chứng về việc hệ thống đã tích hợp metric/alert tự động trong code.

### 3.3. Kiểm tra alert

Về alert, nhóm nên ghi nhận rằng:

- Chưa có pipeline alert thực thi sẵn trong repo.
- Có thể đề xuất dùng CloudWatch Alarm + SNS trong tương lai.

![Placeholder: Screenshot dashboard/alert planning](/images/5-Workshop/placeholder-monitoring.png)
