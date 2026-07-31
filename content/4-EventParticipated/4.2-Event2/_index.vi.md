---
title: "Event 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch: FCAJ x Agentic AI Build Week Hackathon — Buổi chia sẻ

### Bối cảnh sự kiện

Agentic AI Build Week (AABW) được giới thiệu là buildathon Agentic AI lớn nhất Đông Nam Á, diễn ra từ 8–12/07/2026 tại TP.HCM. Một số bạn intern trong cộng đồng FCAJ đã tham gia thi đấu tại đây, và sau đó cộng đồng FCAJ tổ chức buổi chia sẻ lại để các đội trình bày project cũng như kinh nghiệm từ 24 giờ build sản phẩm.

### Nội dung chia sẻ — 4 dự án

**1. S.H.E.P.H.E.R.D (Team 3KA)** — hệ thống giám sát mật độ đám đông và phát hiện nguy cơ tắc nghẽn theo thời gian thực từ camera trực tiếp. Hệ thống dùng YOLO + ByteTrack để phát hiện và theo dõi người, một endpoint SageMaker để inference, và "Operation Agent" xây trên Amazon Bedrock AgentCore + Strands để biến kết quả phát hiện thành cảnh báo và đề xuất hành động cho nhân viên vận hành. Trên AWS, luồng dữ liệu đi qua Kinesis Video Stream, một stream processor chạy trên ECS, DynamoDB và S3 lưu bằng chứng sự cố, cùng nhánh hướng tới dispatcher qua CloudFront, WAF, API Gateway, Lambda, được hỗ trợ bởi Cognito, IAM, Secrets Manager, CloudTrail và CloudWatch. Điều ấn tượng ở phần chia sẻ của đội này là sự thật thà về cảm xúc trong một cuộc hackathon — từ nghi ngờ bản thân, sợ không đủ giỏi lúc bắt đầu, đến niềm tự hào khi thực sự build ra được một hệ thống end-to-end hoạt động.

**2. KFC Bot Agent (Team OneTeam)** — một AI agent đặt hàng qua hội thoại đa kênh (Zalo, Messenger, dự kiến mở rộng WhatsApp), thiết kế để khách hàng không cần rời khỏi khung chat, không cần tải app hay tạo tài khoản mới để đặt món. Ý tưởng xuất phát trực tiếp từ một bài học thất bại có thật: McDonald's đã ngừng thử nghiệm đặt hàng bằng AI tại drive-thru sau khi gặp sự cố tại hơn 100 địa điểm ở Mỹ. Kiến trúc lấy Amazon Bedrock AgentCore (Runtime + Gateway) làm lõi ra quyết định, Lambda xử lý ingestion/orchestration/worker, SQS làm hàng đợi tác vụ, DynamoDB và OpenSearch lưu session và vector store, cùng một tầng bảo mật/giám sát đầy đủ (CloudWatch, X-Ray, CloudTrail, GuardDuty, Secrets Manager, IAM). Đội này đoạt giải **AWS Track** tại AABW.

**3. Signal Scout** — nền tảng theo dõi các "tín hiệu" tái cấu trúc doanh nghiệp công khai, hỗ trợ ra quyết định cho các đội chiến lược doanh nghiệp, quản trị rủi ro và competitive intelligence. Điểm thiết kế thú vị nhất là tách agent thành 2 sub-agent Bedrock AgentCore riêng biệt: **Crawler Subagent** (thu thập dữ liệu qua TinyFish và Apify) và **Analysis Subagent** (phân tích, áp Bedrock Guardrails, ghi log qua Langfuse). Đội cũng trình bày bảng chi phí vận hành khá chi tiết (khoảng $17–130/tháng chỉ riêng AWS, cộng thêm chi phí bên thứ ba cho Apify/TinyFish/Langfuse) — một lời nhắc hữu ích về việc phải ước tính chi phí vận hành thực tế của một AI agent trước khi bắt tay vào build.

**4. Solution Architect Professional AI Native App (Team Plan V)** — công cụ tự động hoá phần việc lặp đi lặp lại của Solution Architect: đọc yêu cầu khách hàng bằng ngôn ngữ tự nhiên, soạn kiến trúc high-level, tự sinh sơ đồ draw.io bằng đúng bộ AWS Architecture Icons chính thức, và đưa ra ước tính chi phí AWS mang tính định hướng. Hệ thống chạy trên ECS Fargate (backend + agent) trong một VPC riêng, dùng PostgreSQL, gọi Amazon Bedrock, tích hợp với Draw.io và AWS Pricing qua MCP, và được triển khai bằng Terraform. Lý do ra đời ý tưởng rất thực tế: khách hàng yêu cầu thiết kế hệ thống AI "ngay lập tức", trong khi Solution Architect vẫn cần thời gian để trích xuất yêu cầu, phác thảo kiến trúc và ước tính chi phí.

### Vai trò của mình

Mình tham gia với vai trò người nghe, theo dõi cả 4 đội trình bày lại hành trình 24 giờ hackathon, kiến trúc kỹ thuật, và những khó khăn thực tế khi build một AI agent có hình hài production trong thời gian rất ngắn.

### Bài học rút ra / đóng góp cá nhân

- Nhìn ra được một pattern chung dù các bài toán rất khác nhau (giám sát, đặt hàng, tình báo doanh nghiệp, sinh kiến trúc): **Amazon Bedrock AgentCore (Runtime + Gateway + Memory)** gần như là xương sống của hầu hết kiến trúc agent được trình bày.
- Việc tách trách nhiệm bằng nhiều sub-agent (Signal Scout tách Crawler/Analysis, S.H.E.P.H.E.R.D tách detection/operation) là một pattern gọn gàng hơn hẳn so với dồn toàn bộ logic vào một agent duy nhất — điều mình sẽ nhớ nếu sau này mở rộng pipeline SageMaker của mình bằng các thành phần agentic.
- Bảng chi phí của Signal Scout là một lời nhắc thực tế về việc phải theo dõi chi phí sát sao — liên quan trực tiếp đến phần workshop SageMaker của mình và ngân sách $200 credit đang có.
- Tinh thần "học từ thất bại" trong phần chia sẻ của team 3KA là một lời nhắc hữu ích rằng một dự án cá nhân không cần hoàn hảo ngay từ ngày đầu — quan trọng là build được một bản MVP chạy được rồi cải thiện dần.

### Hình ảnh sự kiện

![Buổi chia sẻ FCAJ x Agentic AI Build Week Hackathon](/images/4-EventParticipated/4.2-Event2/event2.jpg)
