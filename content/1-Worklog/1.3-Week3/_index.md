---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Set up SageMaker Studio as the main workspace for the ML pipeline.
* Build a repeatable data-preprocessing step using SageMaker Processing Jobs.

### Tasks:

* Completed **"Amazon SageMaker Studio for Data Scientists"** on AWS Skill Builder.
* Set up a SageMaker Studio domain and user profile in **ap-southeast-2**.
* Built a **SageMaker Processing Job** to clean the MovieLens data and construct the user-item sparse matrix.
* Implemented a **time-based train/test split** (instead of a random split) to reflect the sequential nature of a recommender system.
* Stored the processed train/test artifacts back to the `processed/` prefix on S3.

### Week 3 Achievements:

* First SageMaker Processing Job running successfully end-to-end.
* A reproducible preprocessing pipeline that outputs a ready-to-train dataset on S3.
* Solid understanding of why chronological splitting matters for recommendation systems.
