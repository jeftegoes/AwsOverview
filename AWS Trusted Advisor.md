# AWS Trusted Advisor<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Checks Examples](#11-checks-examples)

# 1. Introduction

- No need to install anything - high level AWS account assessment
- **Analyze your AWS accounts and provides recommendations:**
  - Cost optimization
  - Performance
  - Security
  - Fault tolerance
  - Service limits
- Core Checks and recommendations - all customers
- Can enable weekly email notification from the console
- Full Trusted Advisor - Available for **Business & Enterprise** support plans
  - Ability to set CloudWatch alarms when reaching limits
  - **Programmatic Access using AWS Support API.**

## 1.1. Checks Examples

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
