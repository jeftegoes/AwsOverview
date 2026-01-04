# AWS Config <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. S3 Integration](#2-s3-integration)
- [3. Config Rules](#3-config-rules)
  - [3.1. AWS Config Resource](#31-aws-config-resource)
  - [3.2. Remediations](#32-remediations)
    - [3.2.1. Required Tags](#321-required-tags)
  - [3.3. Notifications](#33-notifications)
- [4. Configuration Recorder](#4-configuration-recorder)
- [5. Aggregators](#5-aggregators)
- [6. Conformance Pack](#6-conformance-pack)
- [7. Organizational Rules](#7-organizational-rules)
  - [7.1. Custom rules](#71-custom-rules)
- [8. Organizational Rules vs. Conformance Packs](#8-organizational-rules-vs-conformance-packs)
- [9. Managed Rules](#9-managed-rules)

# 1. Introduction

- Helps with **auditing and recording compliance of ours AWS resources**.
- Helps record configurations and changes over time.
- **Questions that can be solved by AWS Config**
  - Is there unrestricted SSH access to my security groups?
  - Do my buckets have any public access?
  - How has my ALB configuration changed over time?
- We can receive alerts (SNS notifications) for any changes.
- AWS Config is a per-region service.
- Can be aggregated across regions and accounts.
- Evaluate our AWS resource configurations for desired settings.
- Get a snapshot of the current configurations of the supported resources that are associated with our AWS account.
- Retrieve configurations of one or more resources that exist in our account.
- Retrieve historical configurations of one or more resources.
- Receive a notification whenever a resource is created, modified, or deleted.
- View relationships between resources.
  - For example, we might want to find all resources that use a particular security group.

# 2. S3 Integration

- Possibility of storing the configuration data into S3 (analyzed by Athena).
  ![S3 Integration](/Images/Management%20&%20Governance/AWSConfigS3Integration.png)

# 3. Config Rules

- Can use AWS managed config rules (over 75).
- Can make custom config rules (must be defined in AWS Lambda).
  - **Ex:** Evaluate if each EBS disk is of type `gp2`.
  - **Ex:** Evaluate if each EC2 instance is `t2.micro`.
- **Rules can be evaluated / triggered**
  - For each config change.
  - **And / or:** At regular time intervals.
- **AWS Config Rules does not prevent actions from happening (no deny).**
- **Pricing:** No free tier, $0.003 per configuration item recorded per region, $0.001 per config rule evaluation per region.

## 3.1. AWS Config Resource

- View compliance of a resource over time.
- View configuration of a resource over time.
- View CloudTrail API calls of a resource over time.

## 3.2. Remediations

- Automate remediation of non-compliant resources using SSM Automation Documents.
- Use AWS-Managed Automation Documents or create custom Automation Documents.
  - **Tip:** We can create custom Automation Documents that invokes Lambda function.
- We can set **Remediation Retries** if the resource is still non-compliant after auto-remediation.
  ![Config Rules Remediations](/Images/Management%20&%20Governance/AWSConfigRulesRemediations.png)

### 3.2.1. Required Tags

- Checks if our resources have the tags that we specify.
  [Required Tags](https://docs.aws.amazon.com/config/latest/developerguide/required-tags.html)

## 3.3. Notifications

- Use [Amazon EventBridge](/Application%20Integration/Amazon%20EventBridge.md) to trigger notifications when AWS resources are non-compliant.
- Ability to send configuration changes and compliance state notifications to SNS (all events - use SNS Filtering or filter at client-side).

# 4. Configuration Recorder

- Stores the configurations of our AWS resources as Configuration Items.
- **Configuration Item:** A point-in-time view of the various attributes of an AWS resource.
  - Created whenever AWS Config detects a change to the resource (e.g., attributes, relationships, config., events...)
- **Custom Configuration Recorder to record** only the resource types that we specify.
- **Must be created before AWS Config can track our resources (created automatically when we enable AWS Config using AWS CLI or AWS Console).**

# 5. Aggregators

- The aggregator is created in one central aggregator account.
- Aggregates rules, resources, etc... across multiple accounts and regions.
- If using AWS Organizations, no need for individual Authorization.
- Rules are created in each individual source AWS account.
- Can deploy rules to multiple target accounts using CloudFormation StackSets.
  ![AWS Config Aggregators](/Images/Management%20&%20Governance/AWSConfigAggregators.png)

- **Accounts and regios**
  - Select the source accounts and regions from where we want to collect AWS Config data.
- **AWS Config Data**
  - Collection of AWS Config Data from multiple source accounts and regions.
- **Aggregator**
  - Contains the resource configurations information and the compliance data recorded in AWS Config.
- **Agregated View**
  - View all compliant and non-compliant rules and resources for each aggregator.

# 6. Conformance Pack

- Collection of AWS Config Rules and Remediation actions.
- Packs are created in YAML-formatted files (similar to CloudFormation).
- Deploy to an AWS account and regions or across an AWS Organization.
- Pre-built sample Packs or create our own **Custom Conformance Packs**.
- Can include **custom Config Rules** which are backed by Lambda functions to evaluate whether our resources are compliant with the Config Rules.
- Can pass inputs via Parameters section to make it more flexible.
- Can designate a Delegated Administrator to deploy Conformance Packs to our AWS Organization (can be Member account).

# 7. Organizational Rules

- AWS Config Rule that we can manage across all accounts within an AWS Organization.

## 7.1. Custom rules

![Custom rules](/Images/Management%20&%20Governance/AWSConfigCustomRules.png)

# 8. Organizational Rules vs. Conformance Packs

|                  | Organizational Rules                                                                            | Conformance Packs                                                                              |
| ---------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Scope            | AWS Organization                                                                                | AWS Accounts and AWS Organization                                                              |
| Evaluation Type  | Evaluate resources against predefined rules that are defined and enforced at Organization level | Evaluate resources against predefined rules that are defined and enforced at the Account level |
| Rules Count      | One rule                                                                                        | Many rules at a time                                                                           |
| Compliance Level | Managed at the Organizational level                                                             | Managed at the Account level                                                                   |

# 9. Managed Rules

- AWS Config provides **AWS managed rules**, which are predefined, customizable rules that AWS Config uses to evaluate whether our AWS resources comply with common best practices.
  ![AWS Managed Rules](/Images/Management%20&%20Governance/AWSConfigManagedRules.png)
