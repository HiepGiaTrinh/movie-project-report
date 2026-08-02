---
title: "Integrate the Application on EC2"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

The repository does not create EC2 resources. The GitHub Actions workflow assumes that an existing host is available, reachable over SSH, and prepared to run Docker Compose.

## 1. Create an EC2 Instance in the AWS Console

1. Open **Amazon EC2** → **Instances** → **Launch instances**.
2. Enter the name `movie-recommendation-server` and add the tag `Environment=<ENVIRONMENT>`.
3. Select the **Ubuntu Server 24.04 LTS, 64-bit (x86)** AMI or an organization-approved Linux AMI.
4. Select an appropriate instance type. The current workshop environment uses `t3.micro`; increase the capacity if Docker builds or actual traffic exceed its resources.
5. Under **Key pair (login)**, select an existing key pair or create a new one. Store the private key once in a secure location; AWS does not allow the private key to be downloaded again.
6. Under **Network settings**, select the correct VPC and subnet. Enable auto-assign public IPv4 only when the workshop requires direct Internet access.
7. Select or create a security group dedicated to the application host, and configure its inbound rules to allow only the traffic the application actually needs.
8. Under **Configure storage**, select a `gp3` volume large enough for the OS, source code, Docker images, containers, and logs. Enable EBS encryption.
9. Open **Advanced details** and attach the backend IAM instance profile; do not place access keys in user data.
10. Review **Summary** → **Launch instance** → wait until the instance state is `Running` and both status checks pass.

## 2. Prepare the EC2 Host

The platform owner must provide:

- `<EC2_INSTANCE_ID>`.
- IAM instance profile.
- Application directory on the host.
- An inbound rule for `<APPLICATION_PORT>` or a reverse proxy.
- Disk capacity.
- DNS and TLS if the application is public.
- Git access.
- Docker Engine and Docker Compose v2.

The repository does not currently specify the AMI, instance type, VPC, subnet, security group, disk, DNS, or TLS configuration.

The deployment environment screenshot confirms that the `movie-recommendation-server` instance is `Running`, uses the `t3.micro` instance type, and has both public and private IPv4 addresses. This is evidence of the current environment, not a substitute for Infrastructure as Code.

![EC2 instance details for the movie recommendation application](/images/5-Workshop/5.4-Recommendation-pipeline/5.4.3-integrate-ec2-application/ec2-instance-summary.jpg)

*The EC2 Console confirms the application host's status, instance type, network addresses, hostname, and VPC.*

![Successful SSH connection to the EC2 host](/images/5-Workshop/5.4-Recommendation-pipeline/5.4.3-integrate-ec2-application/ec2-ssh-session.jpg)

*The SSH session confirms access to Ubuntu 24.04.4 LTS on EC2.*

## 3. Configure the Application

Place `.env` directly on EC2 according to the approved secret-management process. Do not commit `.env` or create this file in GitHub Actions.

On EC2, prefer an instance profile so the AWS SDK obtains credentials through the default provider chain. Do not copy permanent access keys to the server.

![GitHub deploy key for EC2](/images/5-Workshop/5.4-Recommendation-pipeline/5.4.3-integrate-ec2-application/github-ec2-deploy-key.jpg)

*The repository has configured the `EC2 Deploy` deploy key in read-only mode so the host can retrieve the source code.*

## 4. Deployment Workflow

When a commit is pushed to the `main` branch, the `deploy.yml` workflow runs the **Build frontend** and **Build backend** jobs in parallel, followed by the **Deploy to EC2** job:

**Build frontend / Build backend** (run in parallel)
1. Builds the frontend.
2. Installs backend dependencies and runs `compileall`.

**Deploy to EC2** (runs after both build jobs succeed)
1. Connects to EC2 over SSH.
2. Changes to `EC2_APP_DIR`.
3. Pulls source from `main`.
4. Runs Docker Compose.

![Successful GitHub Actions workflow build](/images/5-Workshop/5.4-Recommendation-pipeline/5.4.3-integrate-ec2-application/github-actions-build-success.png)

## 5. Runtime Integration

When the backend starts, it:

1. Creates a boto3 session.
2. Verifies the STS identity.
3. Describes the key schemas of the DynamoDB tables.
4. Checks the S3 bucket.
5. Describes the SageMaker endpoint through a health check that does not block the entire application.
6. Initializes repositories, services, and the provider.

When the endpoint is unavailable, the guest API can continue to run; a personalized cache miss returns a controlled error.

## 6. Start the Application

From the application directory on EC2, after the code, `.env`, Docker, and IAM role are ready:

```bash
docker compose config --quiet
docker compose up --build -d
docker compose ps
docker compose logs backend --tail 100
```

## 7. Verify the Services

```bash
curl -f "http://127.0.0.1:<BACKEND_PORT>/health"

curl -f \
  "http://127.0.0.1:<BACKEND_PORT>/api/v1/movies?limit=1"
```

Expected results:

- The backend container is healthy.
- The frontend returns HTML.
- `/health` returns `{"status":"ok"}`.
- `/movies` returns a JSON array, or a controlled `503` error if a data resource is configured incorrectly.
- Startup logs do not expose credentials.

![Swagger UI for the Movie Recommendation API running on EC2](/images/5-Workshop/5.4-Recommendation-pipeline/5.4.3-integrate-ec2-application/ec2-fastapi-swagger-ui.png)

## 8. Distinguish the EC2 Application from EC2 Retraining

`ml/deploy/ec2_bootstrap.sh` configures a systemd timer for retraining, not web deployment. This template currently requires two fixes:

- Its default subdirectory does not match the `ml` submodule path.
- Its `events/` event prefix does not match the canonical `datasets/exports/` configuration.
