---
title: "Process, Load, and Validate Data"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

This section covers running the data pipeline locally, verifying output determinism, synchronizing artifacts to S3, and clarifying current gaps in DynamoDB data loading.

## 1. Prepare Raw Data

Place raw CSV files with exact filenames in the input raw directory configured in `ml/configs/data_pipeline.yaml`. `ratings.csv` and `links.csv` serve as the primary historical interaction datasets; `small` datasets are reserved for profiling.

Do not commit datasets to Git repositories.

## 2. Run Data Pipeline

From the `ml` directory, execute:

```bash
python scripts/run_data_pipeline.py \
  --config configs/data_pipeline.yaml
```

The pipeline executes sequentially:

1. Data profiling.
2. Data cleaning.
3. Feature engineering.
4. Dataset splits and serving export generation.
5. Data validation checks.

Critical validation failures must return a non-zero exit code.

## 3. Data Processing Rules

- MovieLens movie IDs are mapped to TMDB IDs via `links.csv`.
- Duplicate metadata entries are resolved deterministically by preserving more complete records.
- Duplicate interaction aliases preserve the latest timestamp.
- Datasets are split per user: the latest interaction is assigned to test, and the preceding interaction to validation.
- JSONL outputs retain only allowed serving fields.

Sample schema representation (illustrative only):

```json
{
  "movie_id": "<MOVIE_ID>",
  "title": "<TITLE>",
  "genres": ["<GENRE>"],
  "poster_path": "<RELATIVE_TMDB_PATH>"
}
```

## 4. Verify Determinism and Run Tests

```bash
python scripts/check_determinism.py \
  --config configs/data_pipeline.yaml

python -m pytest -q
```

Expected results:

- Validation completes with zero critical failures.
- Determinism checks succeed.
- ML tests pass in compatible dependency environments.

<!-- IMAGE-5.3.2-01: Data validation output with local usernames and paths redacted. -->

## 5. Synchronize Artifacts to S3

Always run a dry-run first:

```bash
python scripts/aws_sync.py push --dry-run
```

After reviewing the planned object transfers and receiving approval:

```bash
python scripts/aws_sync.py push
```

![Data prefixes in the S3 bucket](/images/5-Workshop/5.3-Data-layer/5.3.2-load-and-validate-data/s3-bucket-prefixes.png)

*The `datasets`, `evaluation`, `inference`, `logs`, `models`, and `training` storage zones in the S3 bucket.*

{{% notice warning %}}
`aws_sync.py` currently does not include raw data in `sync.pairs`; `--only raw` flags described in root-level S3 documentation do not align with current code implementation.
{{% /notice %}}

## 6. Load Data into DynamoDB

While the pipeline generates serving JSONL exports, the repository currently lacks an official data loader script for the `Movies` and `PopularMovies` tables.

{{% notice warning %}}
Do not write custom DynamoDB ingestion commands in the report and present them as supported features. Consider the data layer ready only after an official loader, idempotency rules, and item verification steps are introduced.
{{% /notice %}}

## 7. Export Interactions for Retraining

The interaction exporter scans the `UserInteractions` table, normalizes reaction/rating events, and can write local JSONL files or upload directly to the interaction export prefix:

```bash
python scripts/export_interactions.py --upload
```

Full table scans incur costs proportional to table size. The repository currently lacks a standard scheduler linking the exporter to SageMaker Processing Jobs.

![Production feedback flow from frontend interactions to retraining](/images/5-Workshop/5.3-Data-layer/5.3.2-load-and-validate-data/production-feedback-flow.jpg)

*Interactions are written to DynamoDB, exported to S3, and used as input for the next retraining cycle.*

## Completion Criteria

- [ ] Required Parquet/JSONL artifacts exist.
- [ ] Validation reports clearly categorize `PASS`, `WARNING`, and `FAIL` status.
- [ ] S3 objects reside under correct prefixes.
- [ ] Serving exports contain zero duplicate primary keys.
- [ ] DynamoDB loading remains documented as incomplete until loader scripts are available.

**Reference Sources:** `ml/DATA_PIPELINE.md`, `ml/configs/data_pipeline.yaml`, `ml/scripts/aws_sync.py`, and `ml/scripts/export_interactions.py`.
