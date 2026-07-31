---
title: "Prerequisites"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## 1. Required Tools

The hands-on environment requires:

- Git with submodule support.
- Docker Engine and Docker Compose v2.
- AWS CLI v2.
- Python 3.11.
- Node.js 20 and npm.
- Internet connectivity for installing dependencies.

The backend container uses Python 3.11; the frontend Dockerfile uses Node.js 20.

## 2. Repository and ML Submodule Initialization

Machine learning code is managed as a Git submodule. At the repository root, run:

```bash
git submodule update --init --recursive
```

After completion, verify:

```bash
test -f ml/train.py
```

The command must return exit code `0`.

## 3. Verify AWS Identity

Prefer **IAM Identity Center/SSO** for developers and IAM roles for EC2/SageMaker. Do not store long-term access keys in Git.

```bash
aws sts get-caller-identity --region "<AWS_REGION>"
```

Expected result: the command succeeds and the identity belongs to the intended AWS account. Do not include full Account IDs or ARNs in the report.

<!-- IMAGE-5.2-01: Terminal screenshot verifying AWS identity with Account ID and ARN redacted. -->

## 4. Required Existing Infrastructure

The repository does not auto-provision infrastructure. The following must exist prior to the workshop:

- An S3 bucket with logical prefixes as declared in `.env.example`.
- Five DynamoDB tables with matching key schemas.
- A valid `PopularMovies list_id`.
- A compatible SageMaker endpoint in `InService` status if testing personalized cache misses.
- A provisioned EC2 host if using the GitHub Actions deployment workflow.

{{% notice warning %}}
Do not create or enable an endpoint solely to pass prerequisites. The guest workflow remains fully functional even when SageMaker is offline.
{{% /notice %}}

## 5. Dataset Preparation

CSV files declared in `ml/configs/data_pipeline.yaml` include metadata, ratings, links, credits, keywords, and small profiling samples. Raw data should not be committed to Git; place them in the designated input directory configured in the ML project.

## 6. Environment Variable Configuration

At the repository root:

```bash
cp .env.example .env
```

Populate values appropriate for your environment:

- `<AWS_REGION>`
- `<JWT_SECRET_VALUE>`
- Names of five DynamoDB tables
- `<POPULAR_LIST_ID>`
- `<S3_BUCKET_NAME>` and logical prefixes
- `<SAGEMAKER_ENDPOINT_NAME>`
- Frontend API URL and TMDB poster URL

Then validate the Docker Compose configuration:

```bash
docker compose config --quiet
```

Expected result: exit code `0`, and `.env` does not appear in `git status`.

{{% notice warning %}}
The file `frontend/src/config/environment.ts` is currently ignored by `.gitignore` even though it is imported by source code. A clean clone may fail to build until this configuration file is generated via an official process or checked into Git.
{{% /notice %}}

## 7. Install Dependencies

```bash
cd backend
python -m pip install -r requirements.txt

cd ../frontend
npm ci

cd ../ml
python -m pip install -r requirements.txt -r requirements-aws.txt
```

## 8. Least Privilege Permission Matrix

| Principal | Required Permissions |
|---|---|
| Backend runtime | STS identity; DynamoDB describe/read/write/query/scan based on access patterns; S3 list for startup validation; SageMaker describe/invoke |
| S3/ML tooling | List/Get/Put on approved prefixes |
| SageMaker launcher | Create and describe Processing Jobs; `iam:PassRole` for the execution role |
| SageMaker execution role | Read/write approved S3 prefixes and publish CloudWatch Logs |
| EC2 application | Use instance profiles instead of static credentials |

Exact IAM policies are not included in the repository and must be provided by security administrators before production deployment.

## Checklist

- [ ] `ml/train.py` exists.
- [ ] `aws sts get-caller-identity` succeeds.
- [ ] `docker compose config --quiet` succeeds.
- [ ] `.env` is ignored by Git.
- [ ] Frontend configuration module has a generation workflow or repository file.
- [ ] Resource names and region are confirmed via secure channels.

**Reference Sources:** `.env.example`, Dockerfiles, `requirements*.txt`, `docs/aws/project-deployment.md`, and `.github/workflows/deploy.yml`.
