---
title: "CI/CD deploy EC2 qua GitHub Workflows"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

## 5. CI/CD deploy EC2 qua GitHub Workflows

### 5.1. Mục tiêu

Nhánh chính của repository web được tự động hóa bằng GitHub Actions để build frontend, kiểm tra backend và triển khai lên EC2 qua SSH. Cách triển khai này giúp cập nhật phiên bản mới mà không cần thao tác thủ công toàn bộ quy trình.

### 5.2. Các bước thực hiện

1. Kích hoạt workflow khi có push lên nhánh `main` hoặc khi chạy thủ công.
2. Build frontend trên môi trường runner của GitHub Actions.
3. Cài dependencies và kiểm tra cú pháp backend.
4. Kết nối SSH tới EC2 bằng secret được cấu hình sẵn.
5. Thực hiện `git pull`, `docker compose pull` và `docker compose up -d` trên máy đích.

```yaml
on:
  push:
    branches:
      - main
  workflow_dispatch:
```

### 5.3. Mở rộng cho EC2 phục vụ retraining

Đối với EC2 dùng cho tác vụ retraining định kỳ, script `ml/deploy/ec2_bootstrap.sh` thực hiện việc cài đặt môi trường, clone repository, tạo virtualenv và kích hoạt systemd timer.

1. Cài đặt gói hệ thống cần thiết.
2. Lấy mã nguồn từ repository.
3. Tạo môi trường ảo Python và cài dependencies.
4. Cài systemd service và timer.
5. Kích hoạt lịch chạy tự động.

Kết quả triển khai EC2 được minh họa qua ba hình ảnh sau:

![Kết quả deploy EC2 - kiểm tra bằng AWS CLI](/images/5-Workshop/5.3-Step-by-step/ec2-cli.jpg)

![Kết quả deploy EC2 - kiểm tra trên AWS Console](/images/5-Workshop/5.3-Step-by-step/ec2-console.jpg)

![Kết quả deploy EC2 - khóa SSH và cấu hình truy cập](/images/5-Workshop/5.3-Step-by-step/ec2-deploy-key.jpg)
