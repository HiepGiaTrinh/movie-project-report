---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Train the first version of the recommendation model using SageMaker Training Jobs.
* Compare hyperparameter configurations to improve model quality.

### Tasks:

* Completed **"Build, Train, and Deploy ML Models with Amazon SageMaker"** on AWS Skill Builder.
* Implemented an **Alternating Least Squares (ALS)** collaborative-filtering model using the `implicit` library.
* Packaged the training script and submitted it as a **SageMaker Training Job**, instead of training only on a local machine.
* Experimented with hyperparameters (`factors`, `regularization`, `iterations`) to compare model quality.
* Saved the resulting user/item factor matrices as model artifacts on S3.

### Week 4 Achievements:

* Trained ALS model running on SageMaker with reproducible, versioned training jobs.
* Identified a reasonable hyperparameter configuration based on validation results.
* Model artifacts versioned and stored on S3, ready for evaluation.
