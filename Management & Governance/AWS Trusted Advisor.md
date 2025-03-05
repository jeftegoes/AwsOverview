# AWS Trusted Advisor <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Checks examples](#11-checks-examples)
- [2. Support Plans](#2-support-plans)
- [3. Monitoring](#3-monitoring)
- [4. Integrations](#4-integrations)
  - [4.1. Targets](#41-targets)

# 1. Introduction

- Scans your AWS infrastructure, compares with AWS best practices.
- No need to install anything - high level AWS account assessment.
- **Analyze your AWS accounts and provides recommendation on 6 categories**
  - Cost optimization.
  - Performance.
  - Security.
  - Fault tolerance.
  - Service limits.
  - Operational Excellence.
- Core Checks and recommendations - all customers.
- Can enable weekly email notification from the console.
- Full Trusted Advisor - Available for **Business & Enterprise** support plans.
  - Ability to set CloudWatch alarms when reaching limits.
  - **Programmatic Access using AWS Support API.**

## 1.1. Checks examples

- **Cost Optimization**
  - Low utilization EC2 instances, idle load balancers, under-utilized EBS volumes...
  - Reserved instances & savings plans optimizations.
- **Performance**
  - High utilization EC2 instances, CloudFront CDN optimizations.
  - EC2 to EBS throughput optimizations, Alias records recommendations.
- **Security**
  - MFA enabled on Root Account, IAM key rotation, exposed Access Keys.
  - S3 Bucket Permissions for public access, security groups with unrestricted ports.
- **Fault Tolerance**
  - EBS snapshots age, Availability Zone Balance.
  - ASG Multi-AZ, RDS Multi-AZ, ELB configuration...
- **Service Limits.**

# 2. Support Plans

| 7 CORE CHECKS                                 | FULL CHECKS                                           |
| --------------------------------------------- | ----------------------------------------------------- |
| **Basic & Developer Support plan**            | **Business & Enterprise Support plan**                |
| S3 Bucket Permissions                         | Full Checks available on the 5 categories             |
| Security Groups - Specific Ports Unrestricted | Ability to set CloudWatch alarms when reaching limits |
| IAM Use (one IAM user minimum)                | Programmatic Access using AWS Support API             |
| MFA on Root Account                           |                                                       |
| EBS Public Snapshots                          |                                                       |
| RDS Public Snapshots                          |                                                       |
| Service Limits                                |                                                       |

# 3. Monitoring

![Trusted Advisor - Monitoring](/Images/AWSTrustedAdvisorMonitoring.png)

# 4. Integrations

- You can use [Amazon EventBridge](/Application%20Integration/Amazon%20EventBridge.md) to detect and react to changes in the status of Trusted Advisor checks.
- Then, based on the rules that you create, EventBridge invokes one or more target actions when a check status changes to the value you specify in a rule.
- Depending on the type of status change, you might want to send notifications, capture status information, take corrective action, initiate events, or take other actions.

## 4.1. Targets

- You can select the following types of targets when using [Amazon EventBridge](/Application%20Integration/Amazon%20EventBridge.md) as a part of your Trusted Advisor workflow:
  - AWS Lambda functions
  - Amazon Kinesis streams
  - Amazon Simple Queue Service queues
  - Built-in targets (CloudWatch alarm actions)
  - Amazon Simple Notification Service topics
