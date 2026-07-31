---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice note %}}
Chưa tới tuần này lúc viết — sẽ cập nhật lại thành thực tế sau khi làm xong.
{{% /notice %}}

### Mục tiêu tuần 8

* Thôi retrain bằng tay, để nó tự chạy.

### Kế hoạch

* Xây SageMaker Pipeline nối Processing → Training → Evaluation.
* Set EventBridge để tự trigger theo lịch.
* Thêm bước kiểm tra, model mới chỉ được lên production nếu thật sự tốt hơn bản cũ.

### Kỳ vọng

* Pipeline retrain tự chạy, khỏi phải ngồi làm tay.
* Có chốt chặn để model tệ hơn không lỡ tay đưa lên production.
