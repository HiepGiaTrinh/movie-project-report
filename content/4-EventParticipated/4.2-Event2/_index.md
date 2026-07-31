---
title: "Event 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# FCAJ x Agentic AI Build Week Hackathon — sharing session

Agentic AI Build Week (AABW), July 8–12 2026 in Ho Chi Minh City, was billed as the biggest Agentic AI buildathon in the region. A few FCAJ interns competed there, and afterward the community put together a session where those teams came back and walked everyone through what they built.

### What got shared

Four teams presented.

**3KA — S.H.E.P.H.E.R.D.** A crowd-monitoring system that watches live camera feed to spot congestion before it gets bad. YOLO + ByteTrack for detecting and tracking people, a SageMaker endpoint for inference, and a Bedrock AgentCore + Strands agent that turns what it sees into alerts and suggested actions for staff. On the AWS side: Kinesis Video Stream, an ECS-hosted processor, DynamoDB/S3 for incident logs, CloudFront/WAF/API Gateway/Lambda on the dispatcher side, plus the usual Cognito/IAM/Secrets Manager/CloudTrail/CloudWatch. What stuck with me most was how honest their retrospective was — starting out scared they weren't good enough, then actually shipping something that worked.

**OneTeam — KFC Bot Agent.** A conversational ordering bot across Zalo/Messenger (WhatsApp planned) so people can order without leaving the chat or making an account. They pitched it off the back of McDonald's actually pulling their AI drive-thru after it flopped at 100+ locations. Architecture-wise, Bedrock AgentCore does the decision-making, Lambda handles the ingestion/orchestration side, SQS queues tasks, DynamoDB/OpenSearch handle session and vector storage, and there's a full security stack on top (CloudWatch, X-Ray, CloudTrail, GuardDuty, Secrets Manager, IAM). They won the AWS Track.

**Signal Scout.** Tracks public signals of corporate restructuring for strategy/risk/competitive-intel teams. The interesting bit is splitting the agent in two — a Crawler subagent (pulling data via TinyFish and Apify) and an Analysis subagent (running Bedrock Guardrails, logging through Langfuse). They also broke down running costs pretty precisely — something like $17–130/month on AWS alone, more once you add the third-party tools. Good reminder that agents aren't free to run.

**Plan V — Solution Architect Professional AI Native App.** A tool that does the boring parts of an SA's job for them: reads a customer's request in plain language, drafts a rough architecture, spits out a draw.io diagram using actual AWS icons, and gives a ballpark cost estimate. Runs on ECS Fargate + PostgreSQL in a VPC, calls Bedrock, talks to Draw.io and AWS Pricing over MCP, deployed with Terraform. The premise was pretty relatable — a customer wanting a design "right now" while an SA still needs time to actually think it through.

### My role

Just listening — following all four teams through their 24 hours, the architecture, and what actually went wrong along the way.

### What I got out of it

The same core piece — Bedrock AgentCore — showed up in basically every architecture, even though the problems were completely different. Splitting work across sub-agents instead of stuffing everything into one agent seems like the right instinct, and it's something I'd consider if I ever add agentic pieces to my own SageMaker pipeline. The Signal Scout cost breakdown was a good nudge to keep an eye on my own $200 budget. And the "we were scared and it was messy" tone from 3KA was honestly reassuring — a personal project doesn't need to be perfect on day one, it just needs to work.

### Event Photos

![FCAJ x Agentic AI Build Week Hackathon sharing session](/images/4-EventParticipated/4.2-Event2/event2.jpg)
