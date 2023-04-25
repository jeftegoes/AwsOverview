# AWS Limits (Quotas)<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. API Rate Limits](#1-api-rate-limits)
- [2. Service Quotas (Service Limits)](#2-service-quotas-service-limits)
- [3. Exponential Backoff (any AWS service)](#3-exponential-backoff-any-aws-service)

# 1. API Rate Limits

- **DescribeInstances** API for EC2 has a limit of 100 calls per seconds.
- **GetObject** on S3 has a limit of 5500 GET per second per prefix.
- For Intermittent Errors: Implement Exponential Backoff.
- For Consistent Errors: Request an API throttling limit increase.

# 2. Service Quotas (Service Limits)

- Running On-Demand Standard Instances: 1152 vCPU.
- You can request a service limit increase by **opening a ticket**.
- You can request a service quota increase by using the **Service Quotas API**.

# 3. Exponential Backoff (any AWS service)

- If you get **ThrottlingException** intermittently, use exponential backoff.
- Retry mechanism already included in AWS SDK API calls.
- Must implement yourself if using the AWS API as-is or in specific cases.
  - **Must only implement the retries on 5xx server errors and throttling.**
  - Do not implement on the 4xx client errors.
