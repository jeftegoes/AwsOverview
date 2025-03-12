# AWS Firewall Manager <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)

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
