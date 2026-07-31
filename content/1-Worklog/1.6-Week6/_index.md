---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Objectives

* Deploy the model as a live inference endpoint.
* Connect it to the rest of the application.

### What I did

* Reviewed SageMaker inference options and focused on Serverless Inference.
* Deployed the model to a SageMaker Serverless Inference endpoint in ap-southeast-2.
* Tested the endpoint using the AWS CLI and boto3 with sample user IDs.
* Coordinated with the Backend team member to connect the endpoint to the shared Lambda/API Gateway layer.
* Measured cold-start time and average inference latency.

### Results

* The endpoint is live and returning recommendations.
* Verified that the request/response flow between the API layer and the endpoint works correctly.
* Recorded baseline latency figures for future comparison.
