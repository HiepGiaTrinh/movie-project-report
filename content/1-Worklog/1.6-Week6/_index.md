---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Deploy the recommendation model as a live inference endpoint.
* Connect the ML endpoint to the team's shared backend.

### Tasks:

* Studied SageMaker inference options on AWS Skill Builder, focusing on **Serverless Inference**.
* Deployed the approved model to a **SageMaker Serverless Inference** endpoint in ap-southeast-2.
* Tested the endpoint directly via the AWS CLI and boto3 (`invoke_endpoint`) with sample user IDs.
* Coordinated with the Backend teammate to connect the endpoint to the shared Lambda / API Gateway layer.
* Measured and recorded endpoint cold-start time and average inference latency.

### Week 6 Achievements:

* Recommendation model exposed as a working Serverless Inference endpoint.
* Verified request/response flow between the API layer and the SageMaker endpoint.
* Latency baseline recorded to guide later optimization.
