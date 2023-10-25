# Setup Static Website using Terraform 

This Terraform code is used to configure Static Website using Terrraform 

## Prerequisites

Before applying this configuration, make sure you have:

1. AWS CLI installed and configured with appropriate credentials

## Configuration

Edit the `backend.tf` file to match your specific settings. Here's what each parameter means:

- `bucket`: The name of the S3 bucket where your Terraform state files will be stored. Replace `"ctl-tf-state-lock-demo"` with your desired bucket name.

- `key`: The key is a unique identifier for your state file within the S3 bucket. Replace `"s3"` with your desired key.

- `region`: The AWS region where the S3 bucket and DynamoDB table will be created.

- `dynamodb_table`: (Optional) If you want to enable state locking, provide the name of a DynamoDB table to be used for locking. Replace `"tf-lock-state"` with your desired table name.

- `variables.tf`: Update variables.tf file Replace `"ctl-static-website-devops"`  and `"logs-ctl-static-website-devops"` with actual bucket name which for static website and log file bucket

- `website-bucket.tf`: Update website-bucket.tf with your bucket name and another bucket used for logging purpose. Also update S3  ARN

 ```shell
   bucket = "ctl-static-website-devops"
   Resource": "arn:aws:s3:::ctl-static-website-devops/*


## Usage

1. Initialize the Terraform configuration:

   ```shell
   terraform init


   terraform apply
terraform destroy

Certainly! Here's a basic `README.md` file for the provided Terraform code:

```markdown
# Terraform S3 Backend Configuration

This Terraform code is used to configure the backend for storing your Terraform state files in an AWS S3 bucket. Using a remote backend like S3 is important for collaboration and state management.

## Prerequisites

Before applying this configuration, make sure you have:

1. AWS CLI installed and configured with appropriate credentials.

## Configuration

Edit the `backend.tf` file to match your specific settings. Here's what each parameter means:

- `bucket`: The name of the S3 bucket where your Terraform state files will be stored. Replace `"ctl-tf-state-lock-demo"` with your desired bucket name.

- `key`: The key is a unique identifier for your state file within the S3 bucket. Replace `"s3"` with your desired key.

- `region`: The AWS region where the S3 bucket and DynamoDB table will be created.

- `dynamodb_table`: (Optional) If you want to enable state locking, provide the name of a DynamoDB table to be used for locking. Replace `"tf-lock-state"` with your desired table name.

## Usage

1. Initialize the Terraform configuration:

   ```shell
   terraform init
   ```

2. Apply the configuration:

   ```shell
   terraform apply
   ```

3. Terraform will create the S3 bucket and, if specified, the DynamoDB table for state locking.

4. Your Terraform state files will be stored remotely in the S3 bucket specified.

## State Locking (DynamoDB)

State locking is a crucial feature to prevent conflicts when multiple users or processes are managing infrastructure. If you have specified a DynamoDB table, Terraform will use it for state locking. Ensure the table exists and has the necessary permissions.

**Problem Statement: Setup CI/CD Pipeline for Static Website **

Our organization is looking to streamline the deployment process for our static website hosted in an AWS S3 bucket. Currently, the deployment process is manual and error-prone, leading to inconsistencies in the production environment. To address this issue, we need to implement a Continuous Integration and Continuous Deployment (CI/CD) pipeline that automates the building and deployment of our static website.

**Project Description:**

The goal of this project is to create a CI/CD pipeline that automatically builds and deploys our static website hosted in an AWS S3 bucket whenever changes are pushed to the GitHub repository. To achieve this, we will use GitHub Actions as our CI/CD platform.

**Key Objectives:**

1. Automate the build process: Whenever changes are pushed to the GitHub repository, the CI/CD pipeline should automatically build the static website from source code.

2. Automate deployment: After a successful build, the pipeline should deploy the website to an AWS S3 bucket, making it instantly accessible to users.

3. Ensure that the pipeline runs only when changes are pushed to the `main` branch to avoid unnecessary deployments for feature branches.

4. Implement proper error handling and notifications in case of build or deployment failures.

**CI/CD Pipeline Steps:**

1. **Linting and Testing**: Ensure the code passes linting and testing checks to maintain code quality.

2. **Build**: Generate the production-ready static website from source code.

3. **Deployment**: Deploy the website to an AWS S3 bucket.

4. **Notification**: Send a notification on successful deployment or in case of any failures.

**GitHub Action Workflow:**

Create a GitHub Actions workflow file (e.g., `.github/workflows/ci-cd.yml`) with the following code to set up the CI/CD pipeline. This example assumes you have already configured AWS credentials in your GitHub repository secrets.

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy to S3
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: us-east-1  # Replace with your desired region
        run: |
          aws s3 sync build/ s3://your-s3-bucket-name

      - name: Notify on success
        if: success()
        run: echo "Deployment successful!"

      - name: Notify on failure
        if: failure()
        run: echo "Deployment failed!"
```

Replace the placeholders (`your-s3-bucket-name`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`) with your specific S3 bucket name and AWS credentials. This workflow will be triggered when changes are pushed to the `main` branch, and it will perform linting, building, and deployment as specified in the steps.

Ensure that you've installed the AWS CLI in your GitHub Actions runner environment and configured it with the necessary AWS credentials. Also, adapt the build and deployment steps to match your project's build and deployment process.

This setup will help you create an automated CI/CD pipeline for your static website hosted in AWS S3 using GitHub Actions.


## Cleanup

To destroy the resources created by this configuration, you can run:

```shell
terraform destroy
```

