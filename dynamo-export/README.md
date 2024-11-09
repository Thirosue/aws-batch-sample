# dynamo-exports

ETLTemplate is a comprehensive ETL (Extract, Transform, Load) solution, designed to facilitate seamless data processing workflows.

## Overview

This project leverages a sophisticated stack including:

- **Infrastructure Management** - [AWS CloudFormation](https://aws.amazon.com/cloudformation/): For reliable and systematic infrastructure provisioning.
- **Continuous Integration & Delivery** - [AWS CodePipeline](https://aws.amazon.com/codepipeline/): Ensures continuous integration and delivery workflows.
- **Serverless Application Model** - [AWS SAM (Serverless Application Model)](https://aws.amazon.com/serverless/sam/): Streamlines building, testing, and deploying serverless applications.
- **Dependency Management** - [Poetry](https://python-poetry.org/): A modern tool for Python dependency management and packaging.
- **Data Processing** - [Pandas](https://pandas.pydata.org/): Powerful data analysis and manipulation tool.
- **Data Modeling** - [Pydantic](https://pydantic-docs.helpmanual.io/): For data validation and settings management using Python type hinting.
- **AWS SDK** - [Boto3](https://aws.amazon.com/jp/sdk-for-python/): AWS SDK for Python, facilitating access to various AWS services.
- **Testing** - [pytest](https://docs.pytest.org/en/latest/): A mature full-featured Python testing tool.
- **Mock AWS Services** - [moto](https://github.com/spulec/moto): A library for mocking AWS services for testing purposes.
- **Code Formatting & Linting** - [pre-commit](https://pre-commit.com/), [Black](https://black.readthedocs.io/en/stable/), [isort](https://pycqa.github.io/isort/): Tools for maintaining code quality and style consistency.

ETLTemplate aims to streamline data workflows, from extraction and transformation to loading, ensuring efficiency and scalability in data processing tasks.

## Requirements

* Python 3.x
* Poetry

### Poetry Installation

https://python-poetry.org/docs/#installation

## Project setup

```bash
poetry install
```

### Check the project

```bash
poetry run pytest
```

## Git Hooks setup with code format(black) and import sort(isort)

```bash
poetry run pre-commit install
```

## VSCode setup

### Install the Python extension

### Virtual environment setup

* Click on the Python interpreter part displayed in the status bar at the bottom left.

* Select the virtual environment created by poetry.

# Sample Application Deployment Guide

## Prerequisites
- AWS CLI installed and configured.
- Docker installed.
- AWS CloudFormation template file (`template.yaml`).

---

## 1. **ECR Settings**
### Step 1: Create an ECR Repository
Create an Amazon ECR repository to store your Docker image.

```bash
aws ecr create-repository --repository-name <your-repo-name>
```

### Step 2: Authenticate Docker to ECR
Authenticate Docker to your ECR registry.

```bash
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<your-region>.amazonaws.com
```

---

## 2. **ECR Push**
### Step 1: Build the Docker Image
Build the Docker image for your application.

```bash
docker build -t <your-repo-name> .
```

### Step 2: Tag the Docker Image
Tag the Docker image with your ECR repository URI.

```bash
docker tag <your-repo-name>:latest <account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repo-name>:latest
```

### Step 3: Push the Docker Image
Push the Docker image to ECR.

```bash
docker push <account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repo-name>:latest
```

---

## 3. **Cfn Provisioning**
### Step 1: Deploy CloudFormation Stack
Deploy the CloudFormation stack to provision AWS resources (AWS Batch, Security Group, etc.).

```bash
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name <your-stack-name> \
  --parameter-overrides Subnets=<subnet-1>,<subnet-2> VpcId=<your-vpc-id> \
  --capabilities CAPABILITY_IAM
```

### Step 2: Submit an AWS Batch Job
Once the CloudFormation stack is deployed, submit an AWS Batch job to process the application.

```bash
aws batch submit-job \
  --job-name <your-job-name> \
  --job-queue <job-queue-name> \
  --job-definition <job-definition-name>
```

---

## Notes
- Ensure all AWS resources (ECR, Batch, etc.) are in the same region.
- Adjust `VPC`, `Subnet`, and other resource settings as per your environment.