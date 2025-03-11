# AWS Secrets Manager <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Multi-Region Secrets](#2-multi-region-secrets)
- [3. Secrets Manager CloudFormation Integration RDS \& Aurora](#3-secrets-manager-cloudformation-integration-rds--aurora)
- [4. SSM Parameter Store vs Secrets Manager](#4-ssm-parameter-store-vs-secrets-manager)

# 1. Introduction

- Newer service, meant for storing secrets.
- Capability to force **rotation of secrets** every X days.
- Automate generation of secrets on rotation (uses Lambda).
- Integration with [Amazon RDS](/Database/Amazon%20RDS.md) (MySQL, PostgreSQL, Aurora).
- Secrets are encrypted using KMS.
- Mostly meant for RDS integration.

![Configure Rotation](/Images/AWSSecretManagerConfigureRotation.png)

# 2. Multi-Region Secrets

- Replicate Secrets across multiple AWS Regions.
- Secrets Manager keeps read replicas in sync with the primary Secret.
- Ability to promote a read replica Secret to a standalone Secret.
- **Use cases:** multi-region apps, disaster recovery strategies, multi-region DB...

# 3. Secrets Manager CloudFormation Integration RDS & Aurora

- ManageMasterUserPassword - creates admin secret implicitly.
- RDS, Aurora will manage the secret in Secrets Manager and its rotation.

# 4. SSM Parameter Store vs Secrets Manager

- **Secrets Manager ($$$)**
  - Automatic rotation of secrets with AWS Lambda.
  - Lambda function is provided for RDS, Redshift, DocumentDB.
  - KMS encryption is mandatory.
  - Can integration with CloudFormation.
- **SSM Parameter Store ($)**
  - Simple API.
  - No secret rotation (can enable rotation using Lambda triggered by CW Events).
  - KMS encryption is optional.
  - Can integration with CloudFormation.
  - Can pull a Secrets Manager secret using the SSM Parameter Store API.
