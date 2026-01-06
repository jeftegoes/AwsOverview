# AWS Tips to AWS Certified DevOps Engineer - Professional <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. AZs](#1-azs)
- [2. Amazon Inspector](#2-amazon-inspector)
  - [2.1. Requires:](#21-requires)
- [3. CloudWatch](#3-cloudwatch)
  - [3.1. Subscription Filter](#31-subscription-filter)

# 1. AZs

- With their own power infrastructure, the AZs are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles of each other).

# 2. Amazon Inspector

## 2.1. Requires:

- **EC2 instance setup in Inspector requires**
  - **SSM Agent is running.**
  - Be an SSM Managed Instance (IAM Role or Default Host Management Config.).
  - Outbound 443 to Systems Manager endpoint.

![Systems Manager Integration](/Images/AmazonInspectorSystemsManager.png)

# 3. CloudWatch

## 3.1. Subscription Filter

- **Subscription Filter:** Filter which logs are events delivered to your destination.
- Get a real-time log events from CloudWatch Logs for processing and analysis.
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda.
  ![Amazon CloudWatch Logs Subscriptions](/Images/AmazonCloudWatchLogsSubscriptions.png)
  ![Amazon CloudWatch Logs Subscriptions](/Images/AmazonCloudWatchSubscriptionFilters.png)
