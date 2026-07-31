---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Build a solid foundation for working with data on AWS.
* Prepare the S3 storage layer and the dataset the recommendation model will be trained on.

### Tasks:

* Completed **"AWS Technical Essentials"** and **"Introduction to AWS Identity and Access Management (IAM)"** on AWS Skill Builder.
* Designed and created an S3 bucket with `raw/`, `processed/`, `models/`, and `logs/` prefixes for the project.
* Wrote a least-privilege IAM policy and a dedicated IAM role for data-processing tasks.
* Downloaded the **MovieLens 100K** dataset and uploaded it to the `raw/` prefix on S3.
* Explored the dataset locally with pandas (`ratings.csv`, `movies.csv`): rating distribution, number of users/items, sparsity of the user-item matrix.

### Week 2 Achievements:

* S3 bucket structured and populated with the raw MovieLens dataset.
* A working, least-privilege IAM role for data access.
* Clear understanding of the dataset's shape and quality, which guided the preprocessing design for the following week.
