# AWS Control Tower <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Account Factory](#2-account-factory)
- [3. Policy Violations](#3-policy-violations)
  - [3.1. Guardrail](#31-guardrail)
    - [3.1.1. Guardrails Levels](#311-guardrails-levels)
- [4. Landing Zone](#4-landing-zone)
- [5. Account Factory - Customization (AFC)](#5-account-factory---customization-afc)
- [6. Customizations for AWS Control Tower (CfCT)](#6-customizations-for-aws-control-tower-cfct)
- [7. AWS Config Integration](#7-aws-config-integration)
- [8. AWS Config Conformance Packs](#8-aws-config-conformance-packs)
- [9. Account Factory for Terraform (AFT)](#9-account-factory-for-terraform-aft)

# 1. Introduction

- Easy way to **set up and govern a secure and compliant multi-account AWS environment** based on best practices.
- AWS Control Tower uses [AWS Organizations](AWS%20Organizations.md) to create accounts.
- **Benefits**
  - Automate the set up of your environment in a few clicks.
  - Automate ongoing policy management using guardrails.
  - Detect policy violations and remediate them.
  - Monitor compliance through an interactive dashboard.
- AWS Control Tower runs on top of [AWS Organizations](AWS%20Organizations.md):
  - It automatically sets up [AWS Organizations](AWS%20Organizations.md) to organize accounts and implement SCP's (Service Control Policies).

# 2. Account Factory

- Automates account provisioning and deployments.
- Enables you to create pre-approved baselines and configuration options for AWS accounts in your organization (e.g., VPC default configuration, subnets, region...).
- Uses AWS Service Catalog to provision new AWS accounts.

# 3. Policy Violations

## 3.1. Guardrail

- Provides ongoing governance for your Control Tower environment (AWS Accounts).
- **Preventive:** Using SCPs (e.g., Disallow Creation of Access Keys for the Root User).
- **Detective:** Using AWS Config (e.g., Detect Whether MFA for the Root User is Enabled).
- **Example:** Identify non-compliant resources (e.g., untagged resources).
  ![AWS Control Tower - Guardrails](/Images/Management%20&%20Governance/AWSControlTowerGuardRails.png)

### 3.1.1. Guardrails Levels

- **Mandatory**
  - Automatically enabled and enforced by AWS Control Tower.
  - **Example:** Disallow public Read access to the Log Archive account.
- **Strongly Recommended**
  - Based on AWS best practices (optional).
  - **Example:** Enable encryption for EBS volumes attached to EC2 instances.
- **Elective**
  - Commonly used by enterprises (optional).
  - **Example:** Disallow delete actions without MFA in S3 buckets.

# 4. Landing Zone

- Automatically provisioned, secure, and compliant multi-account environment based on AWS best practices.
- Landing Zone consists of:
  - **AWS Organization:** Create and manage multi-account structure.
  - **Account Factory:** Easily configure new accounts to adhere to configurations and policies.
  - **Organizational Units (OUs):** Group and categorize accounts based on purpose.
  - **Service Control Policies (SCPs):** Enforce fine-grained permissions and restrictions.
  - **IAM Identity Center:** Centrally manage user access to accounts and services.
  - **Guardrails:** Rules and policies to enforce security, compliance and best practices.
  - **AWS Config:** To monitor and assess your resources' compliance with Guardrails.

# 5. Account Factory - Customization (AFC)

- Automatically customize resources in **new and existing accounts** created through Account Factory.
- **Custom Blueprint**
  - CloudFormation template that defines the resources and configurations you want to customize in the account.
  - Defined in the form on a Service Catalog Product.
  - Stored in a **Hub Account**, which stores all the Custom Blueprints (**recommended:** don't use the Management account).
  - Also available pre-defined blueprints created by AWS Partners.
- **Only one blueprint can be deployed to the account**
- You can react to new accounts created using EventBridge

# 6. Customizations for AWS Control Tower (CfCT)

- GitOps-style customization framework created by AWS.
- Helps you add customizations to your Landing Zone using your custom CloudFormation templates and SCPs.
- Automatically deploy resources to new AWS accounts created using Account Factory.
- Note: CfCT is different from AFC (Account Factory Customization; blueprint).

# 7. AWS Config Integration

- Control Tower uses AWS Config to **implement Detective Guardrails**.
- Control Tower automatically enables AWS Config in enabled Regions.
- AWS Config Configuration History and snapshots delivered to S3 bucket in a centralized Log Archive account.
- Control Tower uses CloudFormation StackSets to create resources like Config Aggregator, CloudTrail, and centralized logging.

# 8. AWS Config Conformance Packs

- **Config Conformance Packs:** Enables you to create packages of Config Compliance Rules & Remediations for easy deployment at scale.
- Deploy to individual AWS accounts or entire and AWS Organization.

# 9. Account Factory for Terraform (AFT)

- Helps you provision and customize AWS accounts in Control Tower through Terraform using a deployment pipeline.
- You create an account request Terraform file which triggers the AFT workflow for account provisioning.
- Offers built-in feature options (disabled by default)
  - **AWS CloudTrail Data Events:** Creates a Trail & enables CloudTrail Data Events.
  - **AWS Enterprise Support Plan:** Turns on the Enterprise Support Plan.
  - **Delete The AWS Default VPC:** Deletes the default VPCs in all AWS Regions.
- Terraform module maintained by AWS.
- Works with Terraform Open-source, Terraform Enterprise, and Terraform Cloud.
