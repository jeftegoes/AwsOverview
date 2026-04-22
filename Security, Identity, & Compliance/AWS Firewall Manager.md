# AWS Firewall Manager <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Prerequisites](#2-prerequisites)

# 1. Introduction

- **Manage rules in all accounts of an AWS Organization.**
- **Security policy:** Common set of security rules:
  - WAF rules (Application Load Balancer, API Gateways, CloudFront).
  - AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront).
  - Security Groups for EC2, Application Load BAlancer and ENI resources in VPC.
  - AWS Network Firewall (VPC Level).
  - Amazon Route 53 Resolver DNS Firewall.
  - Policies are created at the region level.
- **Rules are applied to new resources as they are created (good for compliance) across all and future accounts in your Organization.**

# 2. Prerequisites

- To use Firewall Manager, we must have:
  1. AWS Organizations
     - Required for multi-account management.
     - All features must be enabled.
  2. Firewall Manager Administrator Account
     - One account must be designated as the **admin**.
     - Responsible for deploying and managing policies.
  3. AWS Config Enabled
     - Must be enabled in all accounts.
     - **Allows detection of**
       - New resources.
       - Configuration changes.
