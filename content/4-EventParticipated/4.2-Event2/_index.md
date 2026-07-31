---
title: "Event 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Event Report: FCAJ x Agentic AI Build Week Hackathon — Sharing Session

### Event Background

Agentic AI Build Week (AABW) was billed as the largest Agentic AI buildathon in ASEAN, held July 8–12, 2026 in Ho Chi Minh City. Several FCAJ interns competed there, and the FCAJ community organized a follow-up sharing session where those teams presented their projects and what they learned from the 24-hour build.

### What Was Shared — Four Projects

**1. S.H.E.P.H.E.R.D (Team 3KA)** — a real-time crowd-monitoring and hazard-detection system built from live camera footage. It uses YOLO + ByteTrack to detect and track people, a SageMaker endpoint for inference, and an Amazon Bedrock AgentCore + Strands "Operation Agent" to turn detections into alerts and recommended actions for venue staff. On AWS, the pipeline runs through Kinesis Video Stream, an ECS-hosted stream processor, DynamoDB and S3 for incident evidence, and a dispatcher-facing path through CloudFront, WAF, API Gateway, and Lambda, backed by Cognito, IAM, Secrets Manager, CloudTrail, and CloudWatch. The team's retrospective was unusually honest about the emotional side of a hackathon — self-doubt and fear of failing at the start, followed by the pride of actually shipping a working end-to-end system.

**2. KFC Bot Agent (Team OneTeam)** — a multi-channel conversational ordering agent (Zalo, Messenger, with WhatsApp planned) designed so customers never have to leave the chat, download an app, or create an account to order. The idea was directly inspired by a real-world failure: McDonald's ending its AI drive-thru ordering trial after issues at over 100 US locations. The architecture centers on Amazon Bedrock AgentCore (Runtime + Gateway) as the decision-making core, with Lambda handling ingestion/orchestration/worker tasks, SQS for task queuing, DynamoDB and OpenSearch for session and vector storage, and a full security/observability layer (CloudWatch, X-Ray, CloudTrail, GuardDuty, Secrets Manager, IAM). This team won the **AWS Track** at AABW.

**3. Signal Scout** — a platform that tracks public corporate-restructuring signals to support decisions by corporate-strategy, risk-management, and competitive-intelligence teams. Its most interesting design choice is splitting the agent into two separate Bedrock AgentCore sub-agents: a **Crawler Subagent** (pulling data via TinyFish and Apify) and an **Analysis Subagent** (applying Bedrock Guardrails and logging through Langfuse). The team also presented a detailed operating-cost breakdown (roughly $17–130/month on AWS alone, plus third-party costs for Apify/TinyFish/Langfuse) — a useful reminder to estimate the real running cost of an AI agent before building one.

**4. Solution Architect Professional AI Native App (Team Plan V)** — a tool that automates repetitive Solution Architect work: parsing a customer's request in natural language, drafting a high-level architecture, auto-generating a draw.io diagram using the official AWS Architecture Icons, and producing a directional AWS cost estimate. It runs on ECS Fargate (backend + agent services) inside a dedicated VPC with PostgreSQL, calls Amazon Bedrock, integrates with Draw.io and AWS Pricing via MCP, and is deployed with Terraform. The trigger for the idea was very relatable: a customer asking for an AI system design "immediately," while a Solution Architect still needs time to extract requirements, draft an architecture, and estimate cost.

### My Role

I attended as a listener, following all four teams as they walked through their 24-hour hackathon journey, their technical architecture, and the real difficulties of building a production-shaped AI agent under a tight deadline.

### Takeaways / Personal Reflection

- A clear common pattern emerged across very different problems (surveillance, ordering, corporate intelligence, architecture generation): **Amazon Bedrock AgentCore (Runtime + Gateway + Memory)** acts as the backbone of almost every agentic architecture shown.
- Splitting responsibilities across multiple sub-agents (Signal Scout's Crawler/Analysis split, S.H.E.P.H.E.R.D's detection/operation split) is a cleaner pattern than cramming all logic into a single agent — something I want to keep in mind if I extend my own SageMaker pipeline with agentic components later.
- The cost breakdown from Signal Scout was a direct, practical reminder to track spend closely — directly relevant to my own SageMaker workshop track and its $200 credit budget.
- The "learning from failure" tone in the 3KA retrospective was a useful reminder that a personal project doesn't need to be perfect on day one — shipping a working MVP and iterating matters more.

### Event Photos

![FCAJ x Agentic AI Build Week Hackathon sharing session](/images/4-EventParticipated/4.2-Event2/event2.jpg)
