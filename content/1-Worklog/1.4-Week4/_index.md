---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Objectives

* Train an initial version of the model using SageMaker Training jobs.
* Compare a few hyperparameter configurations.

### What I did

* Completed "Build, Train, and Deploy ML Models with Amazon SageMaker" on AWS Skill Builder.
* Implemented an ALS collaborative-filtering model using the `implicit` library.
* Packaged the training script and ran it as a SageMaker Training job rather than training locally only.
* Compared several combinations of factors, regularization, and iterations.
* Saved the resulting factor matrices to S3.

### Results

* Have a trained ALS model, with training jobs that are now reproducible and versioned.
* Identified a reasonable hyperparameter configuration based on validation results.
* Model artifacts are stored on S3 and ready for evaluation.
