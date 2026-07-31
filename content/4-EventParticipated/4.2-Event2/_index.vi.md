---
title: "Event 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# FCAJ x Agentic AI Build Week Hackathon — Buổi chia sẻ

Agentic AI Build Week (AABW), 8–12/07/2026 tại TP.HCM, nghe nói là buildathon Agentic AI lớn nhất khu vực. Vài bạn intern FCAJ có tham gia thi, sau đó cộng đồng tổ chức một buổi để mấy đội đó về kể lại đã build cái gì.

### Nội dung chia sẻ

Có 4 đội trình bày.

**3KA — S.H.E.P.H.E.R.D.** Hệ thống giám sát đám đông qua camera trực tiếp, phát hiện nguy cơ tắc nghẽn trước khi nó tệ đi. Dùng YOLO + ByteTrack để phát hiện/theo dõi người, một endpoint SageMaker để inference, và một agent Bedrock AgentCore + Strands biến những gì nó thấy thành cảnh báo và đề xuất hành động cho nhân viên. Phía AWS thì có Kinesis Video Stream, một processor chạy trên ECS, DynamoDB/S3 lưu bằng chứng sự cố, CloudFront/WAF/API Gateway/Lambda phía dispatcher, cộng thêm bộ quen thuộc Cognito/IAM/Secrets Manager/CloudTrail/CloudWatch. Điều mình nhớ nhất là phần chia sẻ của họ khá thật lòng — bắt đầu thì sợ mình không đủ giỏi, rồi cuối cùng vẫn build ra được cái gì đó chạy được.

**OneTeam — KFC Bot Agent.** Bot đặt hàng qua hội thoại trên Zalo/Messenger (định mở rộng WhatsApp), để khách đặt món mà không cần rời khỏi khung chat hay tạo tài khoản. Ý tưởng lấy cảm hứng từ việc McDonald's từng phải bỏ thử nghiệm AI order ở drive-thru vì gặp sự cố ở hơn 100 điểm bán. Về kiến trúc thì Bedrock AgentCore lo phần ra quyết định, Lambda xử lý ingestion/orchestration, SQS xếp hàng tác vụ, DynamoDB/OpenSearch lưu session và vector store, cộng thêm một lớp bảo mật đầy đủ (CloudWatch, X-Ray, CloudTrail, GuardDuty, Secrets Manager, IAM). Đội này đoạt giải AWS Track.

**Signal Scout.** Theo dõi tín hiệu tái cấu trúc doanh nghiệp công khai, phục vụ mấy đội chiến lược/quản trị rủi ro/competitive-intel. Cái hay là họ tách agent làm hai — một Crawler subagent (lấy data qua TinyFish và Apify) và một Analysis subagent (chạy Bedrock Guardrails, log qua Langfuse). Họ cũng tính chi phí vận hành khá kỹ, đâu đó $17–130/tháng riêng AWS, chưa tính thêm mấy dịch vụ bên thứ ba. Nhắc mình nhớ là chạy agent không phải miễn phí.

**Plan V — Solution Architect Professional AI Native App.** Công cụ làm giúp phần việc nhàm chán của Solution Architect: đọc yêu cầu khách hàng bằng ngôn ngữ thường, phác kiến trúc sơ bộ, tự ra sơ đồ draw.io bằng đúng icon AWS, với ước tính chi phí tương đối. Chạy trên ECS Fargate + PostgreSQL trong VPC riêng, gọi Bedrock, nối với Draw.io và AWS Pricing qua MCP, deploy bằng Terraform. Lý do ra đời khá dễ đồng cảm — khách hàng muốn có thiết kế "ngay bây giờ" trong khi SA vẫn cần thời gian để suy nghĩ cho đàng hoàng.

### Vai trò của mình

Chỉ ngồi nghe — theo dõi cả 4 đội kể lại 24 giờ hackathon, kiến trúc, và những gì thực sự trục trặc dọc đường.

### Rút ra được gì

Cùng một thành phần — Bedrock AgentCore — xuất hiện ở gần như mọi kiến trúc, dù bài toán khác nhau hoàn toàn. Tách việc ra nhiều sub-agent thay vì nhồi hết vào một agent duy nhất có vẻ là hướng đi đúng, và mình sẽ để ý cái này nếu sau này thêm phần agentic vào pipeline SageMaker của mình. Bảng chi phí của Signal Scout là một cú nhắc để mình để ý ngân sách $200 của chính mình. Và cái tinh thần "tụi mình cũng sợ, cũng loạn" của team 3KA thật ra khá là an ủi — một project cá nhân không cần hoàn hảo ngay từ đầu, chỉ cần chạy được là được.

### Hình ảnh sự kiện

![Buổi chia sẻ FCAJ x Agentic AI Build Week Hackathon](/images/4-EventParticipated/4.2-Event2/event2.jpg)
