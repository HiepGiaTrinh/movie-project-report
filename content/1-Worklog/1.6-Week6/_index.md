---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### What I wanted to get done this week

* Get the model actually serving requests, not just sitting there.
* Connect it to the rest of the app.

### What I did

* Read up on SageMaker inference options, focused on Serverless Inference.
* Deployed the model to a Serverless Inference endpoint in ap-southeast-2.
* Tested it with the CLI and boto3 using some sample user IDs.
* Talked with the Backend person about wiring the endpoint into the Lambda/API Gateway layer.
* Timed the cold start and average latency.

### How it went

* Endpoint is up and actually returning recommendations.
* Confirmed the request/response flow works between the API and the endpoint.
* Have baseline latency numbers to compare against later.
