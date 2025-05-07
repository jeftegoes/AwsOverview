# AWS Tips to AWS Certified DevOps Engineer - Professional <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. IAM](#1-iam)
- [2. AZs](#2-azs)
- [3. Amazon Inspector](#3-amazon-inspector)
  - [3.1. Requires:](#31-requires)
- [4. CloudWatch](#4-cloudwatch)
  - [4.1. Subscription Filter](#41-subscription-filter)

# 1. IAM

- AWS proactively monitors popular code repository sites for IAM access keys that have been publicly exposed.
- Upon detection of an exposed IAM access key, AWS Health generates an `AWS_RISK_CREDENTIALS_EXPOSED` event in the AWS account related to the exposed key.

![IAM Access Key Exposed](/Images/AWSHealthDashboardIAMAccessKeyExposed.png)

# 2. AZs

- With their own power infrastructure, the AZs are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles of each other).

# 3. Amazon Inspector

## 3.1. Requires:

- **EC2 instance setup in Inspector requires**
  - **SSM Agent is running.**
  - Be an SSM Managed Instance (IAM Role or Default Host Management Config.).
  - Outbound 443 to Systems Manager endpoint.

![Systems Manager Integration](/Images/AmazonInspectorSystemsManager.png)

# 4. CloudWatch

## 4.1. Subscription Filter

- **Subscription Filter:** Filter which logs are events delivered to your destination.
- Get a real-time log events from CloudWatch Logs for processing and analysis.
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda.
  ![Amazon CloudWatch Logs Subscriptions](/Images/AmazonCloudWatchLogsSubscriptions.png)
  ![Amazon CloudWatch Logs Subscriptions](/Images/AmazonCloudWatchSubscriptionFilters.png)
