
# Rclone S3 Sync with OIDC Authentication

This repository contains a GitHub Actions workflow that uses `rclone` to sync files between two S3 buckets. The workflow uses OpenID Connect (OIDC) to authenticate with AWS and assumes an IAM role to obtain temporary credentials.

## Prerequisites

1. **GitHub Repository**: Ensure you have a GitHub repository set up.
2. **AWS IAM Role**: Create an IAM role in AWS that trusts the GitHub OIDC provider and has the necessary permissions to access your S3 buckets.
3. **OIDC Configuration**: Ensure that your GitHub repository is configured to use OIDC for authentication with AWS.

## AWS IAM Role Setup

1. **Create OIDC Provider**:
   - Go to the IAM console in AWS.
   - Navigate to "Identity providers" and create a new OIDC provider.
   - Use `https://token.actions.githubusercontent.com` as the provider URL.
   - Add the appropriate client IDs (`sts.amazonaws.com`).

2. **Create IAM Role**:
   - Create an IAM role with a trust relationship for the OIDC provider.
   - Attach policies to this role to grant permissions to access the S3 buckets.

## GitHub Actions Workflow

The GitHub Actions workflow performs the following steps:

1. **Checkout Repository**: Checks out the repository code.
2. **Configure AWS Credentials**: Assumes the IAM role using OIDC and outputs the temporary AWS credentials.
3. **Install Rclone**: Installs `rclone` on the GitHub runner.
4. **Configure Rclone**: Creates the `rclone` configuration file with the temporary AWS credentials.
5. **Sync Files to S3**: Uses `rclone` to sync files between the specified S3 buckets.

## Running the Workflow

Push changes to the `main` branch to trigger the workflow. The workflow will:

1. Check out the repository.
2. Configure AWS credentials using OIDC.
3. Install `rclone`.
4. Configure `rclone` with temporary credentials.
5. Sync files from the source S3 bucket to the destination S3 bucket.

## Troubleshooting

If you encounter the `SignatureDoesNotMatch` error, ensure:

- The AWS credentials are correct and valid.
- The region specified matches the S3 bucket's region.
- The IAM role has the necessary permissions.

Enable verbose logging by adding `-vv` to the `rclone` command for more detailed error messages.
