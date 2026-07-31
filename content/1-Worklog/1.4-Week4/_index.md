---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### What I wanted to get done this week

* Train an actual first version of the model using SageMaker Training jobs.
* Try a few hyperparameter combos and see what works.

### What I did

* Took "Build, Train, and Deploy ML Models with Amazon SageMaker" on Skill Builder.
* Built an ALS collaborative-filtering model using the `implicit` library.
* Packaged it up and ran it as a SageMaker Training job instead of just running it on my laptop.
* Tried a few combos of factors/regularization/iterations to compare.
* Saved the resulting factor matrices to S3.

### How it went

* Have a trained ALS model, and the training runs are reproducible and versioned now, not one-off scripts.
* Found a hyperparameter setup that looked reasonable on validation.
* Model artifacts are on S3 and ready for the next step — evaluation.
