# AWS Config <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. AWS Config Auto Remediation](#1-aws-config-auto-remediation)
  - [1.1. S3 Bucket Logging](#11-s3-bucket-logging)
    - [1.1.1. Solution Overview](#111-solution-overview)
    - [1.1.2. How It Works](#112-how-it-works)
    - [1.1.3. Key Requirements](#113-key-requirements)
      - [1.1.3.1. AWS Config Enabled](#1131-aws-config-enabled)
      - [1.1.3.2. Automation Role (Critical)](#1132-automation-role-critical)

# 1. AWS Config Auto Remediation

## 1.1. S3 Bucket Logging

- Automatically fix non-compliant S3 buckets that **do not have logging enabled**.

### 1.1.1. Solution Overview

- Use **AWS Config Auto Remediation**.
- Target rule: `s3-bucket-logging-enabled`.
- Remediation action:
  - `AWS-ConfigureS3BucketLogging` (SSM Automation document).

### 1.1.2. How It Works

1. AWS Config evaluates S3 buckets against the rule.
2. If a bucket is **non-compliant** (logging disabled):
   - Auto Remediation is triggered.
3. Systems Manager Automation runs:
   - Enables S3 bucket logging automatically.

### 1.1.3. Key Requirements

#### 1.1.3.1. AWS Config Enabled

- Must be active in the account.
- Rule `s3-bucket-logging-enabled` must exist.

#### 1.1.3.2. Automation Role (Critical)

- `AutomationAssumeRole` must:
  - Be **assumable by Systems Manager (SSM)**.
  - Have permissions required by the remediation document.
- The user creating the remediation must have:
  - `iam:PassRole` permission for this role.
