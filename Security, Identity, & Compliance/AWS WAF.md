# AWS WAF - Web Application Firewall<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Managed Rules](#2-managed-rules)
- [3. Logging](#3-logging)
- [4. AWS Firewall Manager](#4-aws-firewall-manager)
- [5. WAF vs. Firewall Manager vs. Shield](#5-waf-vs-firewall-manager-vs-shield)
- [6. Security Policies](#6-security-policies)

# 1. Introduction

- Protects your web applications from common web exploits (Layer 7).
- Deploy on [ALB](AWS%20ELB%20and%20ASG.md) (localized rules).
- Deploy on [API Gateway](AWS%20API%20Gateway.md) (rules running at the regional or edge level).
- Deploy on [CloudFront](AWS%20CloudFront.md) (rules globally on edge locations).
  - Used to front other solutions: CLB, EC2 instances, custom origins, S3 websites.
- Deploy on AppSync (protect your GraphQL APIs).
- WAF is not for DDoS protection.
- Define Web ACL (Web Access Control List):
  - Rules can include **IP addresses**, HTTP headers, HTTP body, or URI strings.
  - Protects from common attack - **SQL injection** and Cross-Site Scripting (XSS).
  - Size constraints, Geo match.
  - Rate-based rules (to count occurrences of events).
- Rule Actions: Count | Allow | Block | CAPTCHA.

# 2. Managed Rules

- Library of over 190 managed rules.
- Ready-to-use rules that are managed by AWS and AWS Marketplace Sellers.
- **Baseline Rule Groups:** General protection from common threats
  - `AWSManagedRulesCommonRuleSet`
  - `AWSManagedRulesAdminProtectionRuleSet`
  - ...
- **Use-case Specific Rule Groups:** Protection for many AWS WAF use cases
  - `AWSManagedRulesSQLiRuleSet`
  - `AWSManagedRulesWindowsRuleSet`
  - `AWSManagedRulesPHPRuleSet`
  - `AWSManagedRulesWordPressRuleSet`
  - ...
- **IP Reputation Rule Groups:** Block requests based on source (e.g., malicious IPs).
  - `AWSManagedRulesAmazonIpReputationList`
  - `AWSManagedRulesAnonymousIpList`
- **Bot Control Managed Rule Group:** Block and manage requests from bots
  - `AWSManagedRulesBotControlRuleSet`

# 3. Logging

- You can send your logs to an:
  - Amazon CloudWatch Logs log group - 5 MB per second.
  - Amazon Simple Storage Service (Amazon S3) bucket - 5 minutes interval.
  - Amazon Kinesis Data Firehose - limited by Firehose quotas.

# 4. AWS Firewall Manager

- **Manage rules in all accounts of an AWS Organization.**
- Security policy: common set of security rules:
  - WAF rules (Application Load Balancer, API Gateways, CloudFront).
  - AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront).
  - Security Groups for EC2, Application Load BAlancer and ENI resources in VPC.
  - AWS Network Firewall (VPC Level).
  - Amazon Route 53 Resolver DNS Firewall.
  - Policies are created at the region level.
- **Rules are applied to new resources as they are created (good for compliance) across all and future accounts in your Organization.**

# 5. WAF vs. Firewall Manager vs. Shield

- **WAF, Shield and Firewall Manager are used together for comprehensive protection.**
- Define your Web ACL rules in WAF.
- For granular protection of your resources, WAF alone is the correct choice.
- If you want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Firewall Manager with AWS WAF.
- Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting.
- If you're prone to frequent DDoS attacks, consider purchasing Shield Advanced.

# 6. Security Policies

- **Policy Type: AWS WAF**
  - Enforce applying WebACLs to all ALBs in all accounts in AWS Organization.
  - **Identify resources that don't comply, but don't auto remediate:** Create the WebACL in each account without applying the WebACL to any resources.
  - **Auto remediate any noncompliant resources:** Automatically apply the WebACLs to existing resources.
- **Policy Type: Shield Advanced**
  - Enforce Shield Advanced protections in all accounts in AWS Organization.
  - **Option to view only compliance (to assess impact) or auto remediate.**
- **Security Group Policy Type: Common Security Groups**
  - Enforce applying SGs to all EC2 instances in all accounts in AWS Organization.
- **Security Group Policy Type: Auditing of Security Group Policy**
  - Check and manage SGs Rules in all accounts in AWS Organization.
- **Security Group Policy Type: Usage Audit Security Group Policy**
  - Monitor unused and redundant SGs and optionally perform cleanup.
- **Policy Type: Network Firewall**
  - Centrally manage Network Firewall firewalls in all accounts in AWS Organization.
  - **Distributed:** Creates and maintains firewall endpoint in each VPC.
  - **Centralized:** Creates and maintains firewall endpoint in a centralized VPC.
  - **Import Existing Firewalls:** Import existing firewalls using Resource Sets.
- **Policy Type: Route 53 Resolver DNS Firewall**
  - Manage associations between Resolver DNS Firewall Rule Groups and VPCs in all accounts in AWS Organization.
