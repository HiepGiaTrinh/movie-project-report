---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Objectives

* Build a working foundation for handling data on AWS.
* Set up storage and bring in the dataset the model will be trained on.

### What I did

* Completed "AWS Technical Essentials" and "Introduction to AWS Identity and Access Management (IAM)" on AWS Skill Builder.
* Created an S3 bucket structured with `raw/`, `processed/`, `models/`, and `logs/` prefixes.
* Wrote a least-privilege IAM policy and role dedicated to data-processing tasks.
* Downloaded the MovieLens 100K dataset and uploaded it to the `raw/` prefix.
* Explored the dataset locally using pandas, reviewing rating distribution, the number of users and items, and the sparsity of the user-item matrix.

### Results

* S3 bucket is structured and populated with the raw MovieLens dataset.
* IAM role is in place and follows least-privilege principles.
* Gained a clear understanding of the dataset, which informed the preprocessing design for the following week.
