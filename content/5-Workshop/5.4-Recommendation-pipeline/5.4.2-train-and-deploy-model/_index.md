---
title: "Train, Evaluate, and Run SageMaker Processing Job"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

## 1. Train Collaborative Model

`train.py` reads chronological training splits, applies positive, negative, and neutral interaction rules, and trains an **implicit ALS** model.

Inside the `ml` directory:

```bash
python train.py --version "<MODEL_VERSION>"
```

Versioned model artifacts include:

- User and item factor matrices.
- User/item index mappings.
- Model manifest file.
- Model configuration settings.
- Content feature artifacts.

## 2. Evaluate Model

```bash
python evaluate.py --version "<MODEL_VERSION>"
```

`evaluate.py` calculates evaluation metrics across groups:

- HitRate.
- Precision.
- NDCG.
- Catalog coverage.

{{% notice note %}}
Do not copy metric values from legacy reports or external documentation. Reports must record actual metric values generated during the execution of this workshop run.
{{% /notice %}}

## 3. Promotion Gate

Execute retraining in dry-run mode:

```bash
python retrain.py \
  --version "<MODEL_VERSION>" \
  --dry-run
```

`retrain.py` updates the `LATEST.json` model pointer only when candidate models:

1. Evaluate against a sufficient count of test users.
2. Outperform popularity baselines on configured metrics.
3. Show zero performance regression beyond allowed tolerances compared to the active serving model.

Candidate models failing promotion gates are retained for inspection but are not promoted to serving state.

{{% notice warning %}}
`--force-promote` bypasses quality gates and must not be used in standard workshop procedures.
{{% /notice %}}

## 4. Run SageMaker Processing Job

The launcher constructs a source bundle and utilizes `FrameworkProcessor` to execute the `deploy/sagemaker_retrain.py` wrapper script. The job fetches inputs from S3 and uploads generated outputs back to S3.

After performing a dry-run, review IAM permissions and cost estimates, then execute:

```bash
python scripts/sagemaker_retrain_job.py \
  --version "<MODEL_VERSION>" \
  --events "s3://<S3_BUCKET_NAME>/<INTERACTION_EXPORT_PREFIX>" \
  --wait
```

<!-- IMAGE-5.4.2-01: Console output showing SageMaker Processing Job in Completed status with ARNs redacted. -->

Check job status via CLI:

```bash
aws sagemaker describe-processing-job \
  --processing-job-name "<PROCESSING_JOB_NAME>" \
  --region "<AWS_REGION>"
```

The job status must reach `Completed`.

## 5. Verify Artifacts and Reports

Confirm that S3 contains the expected model version directory, manifest, updated `LATEST.json`, and evaluation report.

<!-- IMAGE-5.4.2-02: Evaluation report and promotion decision screenshot with user data redacted. -->

## 6. Real-Time Endpoint Unimplemented Gap

{{% notice warning %}}
The repository does not contain `model_fn`, `input_fn`, `predict_fn`, `output_fn`, `transform_fn`, or scripts to provision SageMaker Model, EndpointConfig, and Endpoint resources. Consequently, real-time endpoint deployment cannot be completed from the current codebase.
{{% /notice %}}

Include screenshots of `InService` endpoints only after the engineering team implements:

- Serving handlers compatible with backend request/response contracts.
- Automated `model.tar.gz` packaging logic.
- Compatible inference container configurations.
- Provisioning scripts for SageMaker Model, EndpointConfig, and Endpoint resources.
- Rollback mechanisms and auto-scaling rules.

<!-- IMAGE-5.4.2-03: Include only after endpoint deployment logic is added and verified. -->

## Completion Criteria

- [ ] Model version directory is created.
- [ ] Evaluation report lists metric names and evaluated user counts.
- [ ] Promotion report records pass/fail status for each gate criteria.
- [ ] Processing Job reaches `Completed` status.
- [ ] S3 contains artifacts and reports under matching prefixes.
- [ ] Endpoint deployment remains documented as incomplete without simulated success.

**Reference Sources:** `ml/train.py`, `ml/evaluate.py`, `ml/retrain.py`, and `ml/scripts/sagemaker_retrain_job.py`.
