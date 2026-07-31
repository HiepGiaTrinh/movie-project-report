---
title: "Prepare Recommendation Environment"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

## 1. Initialize ML Submodule

At the repository root:

```bash
git submodule update --init --recursive
```

Verify that the submodule contains:

- `configs/`
- `src/`
- `scripts/`
- `train.py`
- `evaluate.py`
- `retrain.py`

## 2. Create Python Environment

Inside the `ml` directory:

```bash
python -m venv .venv
python -m pip install \
  -r requirements.txt \
  -r requirements-aws.txt
```

`requirements-aws.txt` adds boto3, botocore, and the SageMaker SDK required by job launcher scripts.

## 3. Review Configuration Files

| File | Content |
|---|---|
| `configs/data_pipeline.yaml` | Data file paths and pipeline execution steps |
| `configs/model_serving.yaml` | Model hyperparameters and serving logic rules |
| `configs/aws.yaml` | AWS region, S3 bucket/prefixes, processing instance types, and promotion criteria |

Do not hard-code ARNs in documentation. Use placeholders such as `<SAGEMAKER_ROLE_ARN>` for illustrations.

{{% notice warning %}}
Some sample resources defined in ML configuration files serve as examples. Replace them with values confirmed by platform owners; do not use example values in production environments.
{{% /notice %}}

## 4. Prepare Pipeline Inputs

The data pipeline must produce:

- `interactions_train.parquet`.
- Validation and test temporal split files.
- Content feature artifacts.
- Serving lookup tables.
- Initial model directory structure or a specific model version tag.

Run data validation prior to model training:

```bash
python scripts/validate_data.py \
  --config configs/data_pipeline.yaml
```

Expected result: exit code `0`, and `interactions_train.parquet` contains valid user, movie, and interaction value columns.

## 5. Dry-run SageMaker Processing Job Launcher

The dry-run constructs the source bundle and prints the execution plan without making calls to AWS APIs:

```bash
python scripts/sagemaker_retrain_job.py --dry-run
```

Verify that the console output displays:

- Job name pattern.
- Target AWS region.
- Target S3 bucket and input prefix.
- SageMaker processing instance type.
- Execution role placeholder/configuration.
- Command-line arguments passed to wrapper scripts.

Source bundles must not contain large datasets or model binary artifacts.

<!-- IMAGE-5.4.1-01: Dry-run console output with bucket names and role ARNs redacted. -->

## Completion Criteria

- [ ] ML submodule is checked out to the pinned commit.
- [ ] Dependencies install successfully.
- [ ] Data validation returns exit code `0`.
- [ ] `LATEST.json` or a specific model version tag is identified.
- [ ] Dry-run executes cleanly with zero network mutations.
- [ ] S3 buckets, IAM roles, and SageMaker quotas are confirmed before running cloud jobs.

**Reference Sources:** `ml/requirements*.txt`, `ml/scripts/validate_data.py`, and `ml/scripts/sagemaker_retrain_job.py`.
