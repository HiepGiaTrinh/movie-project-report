---
title : "Triển khai end-to-end"
date : 2026-07-29
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## 2. Hướng dẫn triển khai end-to-end

### Step 1. Clone repository và khởi tạo submodule

```bash
git clone --recurse-submodules <repo-url>
cd movie-recommendation
git submodule update --init --recursive
```

Mục đích của bước này là tải đầy đủ source code, bao gồm frontend, backend và ML submodule.

![Placeholder: Screenshot clone repo và submodule](/images/5-Workshop/placeholder-step1-clone.png)

### Step 2. Xác thực AWS identity

Sử dụng AWS CLI để kiểm tra tài khoản đang dùng:

```bash
aws sts get-caller-identity
aws configure list
```

Nếu dùng SSO thì nên đăng nhập trước:

```bash
aws sso login --profile <profile-name>
```

![Placeholder: Screenshot kết quả AWS CLI](/images/5-Workshop/placeholder-step2-aws-identity.png)

### Step 3. Cấu hình biến môi trường

Sao chép file mẫu và điền thông tin cần thiết:

```bash
cp .env.example .env
```

Một số biến quan trọng cần điền:

- `AWS_REGION`
- `AWS_DYNAMODB_*` table names
- `AWS_S3_BUCKET`
- `AWS_S3_*_PREFIX`
- `JWT_SECRET_KEY`
- `VITE_API_URL`
- `VITE_TMDB_POSTER_BASE_URL`

> [!NOTE]
> Đây là nơi cần bổ sung thêm bảng biến môi trường đầy đủ và mô tả ý nghĩa cụ thể cho từng biến trong tương lai.

![Placeholder: Screenshot file .env](/images/5-Workshop/placeholder-step3-env.png)

### Step 4. Kiểm tra tài nguyên AWS trước khi deploy

Trước khi chạy hệ thống, nên kiểm tra:

```bash
aws dynamodb describe-table --table-name <movies-table>
aws dynamodb describe-table --table-name <users-table>
aws s3api head-bucket --bucket <bucket-name>
```

Nếu resource chưa tồn tại, cần tạo trước hoặc cập nhật lại quy trình tạo resource phù hợp với môi trường hiện tại.

![Placeholder: Screenshot kiểm tra DynamoDB và S3](/images/5-Workshop/placeholder-step4-resource-check.png)

### Step 5. Build và chạy hệ thống bằng Docker Compose

```bash
docker compose config --quiet
docker compose up --build -d
docker compose ps
docker compose logs backend --tail 100
```

Bước này sẽ khởi động cả frontend và backend, giúp hệ thống có thể truy cập qua port tương ứng.

![Placeholder: Screenshot Docker Compose running](/images/5-Workshop/placeholder-step5-docker.png)

### Step 6. Kiểm tra backend và frontend sau khi deploy

- Backend health check:

```bash
curl http://127.0.0.1:8000/health
```

- Frontend check:

```bash
curl http://127.0.0.1:5173
```

- OpenAPI docs:

```bash
http://127.0.0.1:8000/docs
```

![Placeholder: Screenshot browser truy cập frontend và API](/images/5-Workshop/placeholder-step6-verify.png)

### Step 7. Kiểm tra luồng nghiệp vụ end-to-end

Sau khi deploy thành công, nên thử các tình huống sau:

1. Guest browsing: xem danh sách phim và chi tiết phim.
2. Đăng ký / đăng nhập / profile / onboarding.
3. Gửi interaction như click, rating, reaction, share.
4. Kiểm tra recommendation flow với cache và fallback.

> [!NOTE]
> Hiện tại phần realtime SageMaker inference chưa được bật hoàn toàn, nên cache miss có thể trả về trạng thái 503 thay vì trả recommendation giả. Đây là điểm cần ghi rõ trong báo cáo để tránh hiểu nhầm.

![Placeholder: Screenshot test giao diện người dùng](/images/5-Workshop/placeholder-step7-e2e.png)