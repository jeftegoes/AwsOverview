# Management & Governance<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. AWS OpsWorks](#1-aws-opsworks)
- [2. CloudFormation](#2-cloudformation)
- [3. AWS Systems Manager (SSM)](#3-aws-systems-manager-ssm)
- [4. AWS Service Catalog](#4-aws-service-catalog)
  - [4.1. AWS Trusted Advisor](#41-aws-trusted-advisor)
- [5. CloudWatch vs CloudTrail vs Config](#5-cloudwatch-vs-cloudtrail-vs-config)
- [6. Summary](#6-summary)

# 1. AWS OpsWorks

[AWS OpsWorks](Management%20&%20Governance/AWS%20OpsWorks.md)

# 2. CloudFormation

[AWS CloudFormation](AWS%20CloudFormation.md)

# 3. AWS Systems Manager (SSM)

- **AWS Systems Manager gives you visibility and control of your infrastructure on AWS. It is used for patching systems at scale.** [AWS Systems Manager](AWS%20Systems%20Manager.md)

# 4. AWS Service Catalog

[AWS Service Catalog](AWS%20Service%20Catalog.md)

## 4.1. AWS Trusted Advisor

- **AWS Trusted Advisor is an online tool that provides you real time guidance to help you provision your resources following AWS best practices, including performance, security, and fault tolerance, but also cost optimization and service limits.**
  [AWS Trusted Advisor](AWS%20Trusted%20Advisor.md)

# 5. CloudWatch vs CloudTrail vs Config

- **CloudWatch**
  - Performance monitoring (metrics, CPU, network, etc...) & dashboards.
  - Events & Alerting.
  - Log Aggregation & Analysis.
- **CloudTrail**
  - Record API calls made within your Account by everyone.
  - Can define trails for specific resources.
  - Global Service.
- **Config**
  - Record configuration changes.
  - Evaluate resources against compliance rules.
  - Get timeline of changes and compliance.

# 6. Summary

- **CloudFormation: (AWS only)**
  - Infrastructure as Code, works with almost all of AWS resources.
  - Repeat across Regions & Accounts.
- **Systems Manager (hybrid):** Patch, configure and run commands at scale.
- **OpsWorks (hybrid):** Managed Chef and Puppet in AWS.
- **CloudWatch**
  - **Metrics:** Monitor the performance of AWS services and billing metrics.
  - **Alarms:** Automate notification, perform EC2 action, notify to SNS based on metric.
  - **Logs:** Collect log files from EC2 instances, servers, Lambda functions...
  - **Events (or EventBridge):** React to events in AWS, or trigger a rule on a schedule.
- **CloudTrail:** Audit API calls made within your AWS account.
- **CloudTrail Insights:** Automated analysis of your CloudTrail Events.
- **Service Health Dashboard:** Status of all AWS services across all regions.
- **Personal Health Dashboard:** AWS events that impact your infrastructure.
