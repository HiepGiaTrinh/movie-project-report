---
title: "Pipeline gợi ý"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Pipeline hỗ trợ nhiều chiến lược để phục vụ từng trạng thái người dùng.

## Các chiến lược gợi ý

- **Popularity:** xếp hạng phổ biến có trọng số theo phong cách IMDb cho khách chưa đăng nhập.
- **Content-based:** sử dụng TF-IDF và cosine similarity cho người dùng mới dựa trên thể loại onboarding.
- **Collaborative filtering:** sử dụng implicit ALS để tạo candidate cho người dùng quay lại.
- **Hybrid ranking:** kết hợp weighted Reciprocal Rank Fusion, độ tương đồng gần đây và business-rule filtering.

{{< mermaid align="center" >}}
flowchart LR
    Input[S3 processed/training/exports] --> Job[Local retrain hoặc SageMaker Processing]
    Job --> ALS[Implicit ALS]
    Job --> Eval[Offline Evaluation]
    Eval --> Gate[Promotion Gate]
    Gate --> Artifacts[Versioned Artifacts]
    Artifacts --> Latest[LATEST.json]
    Artifacts --> S3[(S3 models/evaluation)]
    Latest -. cần serving package .-> Endpoint[SageMaker Endpoint]
    Endpoint --> API[FastAPI Provider]
{{< /mermaid >}}

<!-- IMAGE-5.4-01: Sơ đồ train, evaluation, promotion và serving. -->

## Input và output

Input của pipeline gồm:

- Movie features.
- Interaction split theo thời gian.
- Thể loại onboarding.
- Các interaction gần đây.
- Danh sách movie ID cần loại trừ.

Recommendation engine trả:

- Danh sách `movie_id` theo thứ tự.
- `score`.
- `reason_code`.
- `reason_context`.

Backend bổ sung metadata từ bảng `Movies` rồi cache `movie_id`, `score` và `reason_code`.

## Vai trò của SageMaker và EC2

SageMaker Processing Job được triển khai để chạy retraining. EC2 có thể chạy Docker application hoặc tùy chọn chạy retraining định kỳ bằng systemd.

Backend đã có client gọi SageMaker real-time endpoint, nhưng source hiện chưa có endpoint packaging và deployment.

{{% notice warning %}}
Processing Job, local inference engine và real-time endpoint là ba thành phần khác nhau. Không mô tả Processing Job như SageMaker Training Job hoặc endpoint.
{{% /notice %}}

## Nội dung

1. [Chuẩn bị môi trường gợi ý](5.4.1-prepare-environment/)
2. [Huấn luyện, đánh giá và chạy SageMaker Processing Job](5.4.2-train-and-deploy-model/)
3. [Tích hợp ứng dụng trên EC2](5.4.3-integrate-ec2-application/)
4. [Kiểm thử đầu cuối](5.4.4-end-to-end-testing/)

**Nguồn đối chiếu:** `ml/README.md`, `ml/configs/model_serving.yaml`, `ml/scripts/sagemaker_retrain_job.py` và backend recommendation provider.
