# AWS CLI, SDK and IAM Policies<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. AWS CLI Dry Runs](#1-aws-cli-dry-runs)
- [2. AWS CLI STS Decode Errors](#2-aws-cli-sts-decode-errors)
- [3. AWS EC2 Instance Metadata](#3-aws-ec2-instance-metadata)
- [4. MFA with CLI](#4-mfa-with-cli)
- [5. AWS CLI Credentials Provider Chain](#5-aws-cli-credentials-provider-chain)
- [6. AWS SDK Default Credentials Provider Chain](#6-aws-sdk-default-credentials-provider-chain)
- [7. AWS Credentials Scenario](#7-aws-credentials-scenario)
  - [7.1. AWS Credentials Best Practices](#71-aws-credentials-best-practices)
- [8. Signing AWS API requests](#8-signing-aws-api-requests)
  - [8.1. SigV4 Request](#81-sigv4-request)

# 1. AWS CLI Dry Runs

- Sometimes, we'd just like to make sure we have the permissions...
- But not actually run the commands!
- Some AWS CLI commands (such as EC2) can become expensive if they succeed, say if we wanted to try to create an EC2 Instance.
- Some AWS CLI commands (not all) contain a `--dry-run` option to simulate API calls.

# 2. AWS CLI STS Decode Errors

- When you run API calls and they fail, you can get a long error message.
- This error message can be decoded using the STS command line:
  - `sts decode-authorization-message`

# 3. AWS EC2 Instance Metadata

- AWS EC2 Instance Metadata is powerful but one of the least known features to developers.
- It allows AWS EC2 instances to "learn about themselves" **without using an IAM Role for that purpose**.
- The URL is http://169.254.169.254/latest/meta-data
- You can retrieve the IAM Role name from the metadata, but you CANNOT retrieve the IAM Policy.
- Metadata = Info about the EC2 instance.
- Userdata = launch script of the EC2 instance.

# 4. MFA with CLI

- To use MFA with the CLI, you must create a temporary session.
- To do so, you must run the **STS GetSessionToken** API call
- `aws sts get-session-token` --serial-number arn-of-the-mfa-device --token-code code-from-token --duration-seconds 3600

# 5. AWS CLI Credentials Provider Chain

- The CLI will look for credentials in this order:

1. **Command line options** - --region, --output, and --profile
2. **Environment variables** - AWS_ACCESS_KEY_ID,AWS_SECRET_ACCESS_KEY, and AWS_SESSION_TOKEN
3. **CLI credentials file** -aws configure ~/.aws/credentials on Linux / Mac & C:\Users\user\.aws\credentials on Windows
4. **CLI configuration file** - aws configure ~/.aws/config on Linux / macOS & C:\Users\USERNAME\.aws\config on Windows
5. **Container credentials** - for ECS tasks
6. **Instance profile credentials** - for EC2 Instance Profiles

# 6. AWS SDK Default Credentials Provider Chain

- The Java SDK (example) will look for credentials in this order

1. **Java system properties** - aws.accessKeyId and aws.secretKey
2. **Environment variables** - AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
3. **The default credential profiles file** - ex at: ~/.aws/credentials, shared by many SDK.
4. **Amazon ECS container credentials** - for ECS containers.
5. **Instance profile credentials** - used on EC2 instances.

# 7. AWS Credentials Scenario

- An application deployed on an EC2 instance is using environment variables with credentials from an IAM user to call the Amazon S3 API.
- The IAM user has `S3FullAccess` permissions.
- The application only uses one S3 bucket, so according to best practices:
  - An IAM Role & EC2 Instance Profile was created for the EC2 instance.
  - The Role was assigned the minimum permissions to access that one S3 bucket
- The IAM Instance Profile was assigned to the EC2 instance, but it still had access to all S3 buckets. Why?
  - The credentials chain is still giving priorities to the environment variables.

## 7.1. AWS Credentials Best Practices

- **Overall, NEVER EVER STORE AWS CREDENTIALS IN YOUR CODE.**
- Best practice is for credentials to be inherited from the credentials chain.
- **If using working within AWS, use IAM Roles:**
  - => EC2 Instances Roles for EC2 Instances.
  - => ECS Roles for ECS tasks.
  - => Lambda Roles for Lambda functions.
- If working outside of AWS, use environment variables / named profiles.

# 8. Signing AWS API requests

- When you call the AWS HTTP API, you sign the request so that AWS can identify you, using your AWS credentials (access key & secret key).
- Note: some requests to Amazon S3 don't need to be signed.
- If you use the SDK or CLI, the HTTP requests are signed for you.
- You should sign an AWS HTTP request using Signature v4 (SigV4).

## 8.1. SigV4 Request

- HTTP Header option (signature in Authorization header).
- Query String option, ex: S3 pre-signed URLs (signature in X-Amz-Signature).
