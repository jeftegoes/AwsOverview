# AWS WAF - Web Application Firewall <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Rules](#2-rules)
  - [2.1. Rate-Based](#21-rate-based)
- [3. Fixed IP while using WAF with a Load Balancer](#3-fixed-ip-while-using-waf-with-a-load-balancer)
- [4. Managed Rules](#4-managed-rules)
- [5. Logging](#5-logging)
- [6. WAF vs. Firewall Manager vs. Shield](#6-waf-vs-firewall-manager-vs-shield)
- [7. Security Policies](#7-security-policies)
- [8. AWS Shield](#8-aws-shield)
  - [8.1. Protect from DDoS attack](#81-protect-from-ddos-attack)

# 1. Introduction

- Protects your web applications from common web exploits (Layer 7).
- **Deploy on**
  - [Application Load Balancer](/Compute/AWS%20ELB.md).
  - [API Gateway](/Networking%20&%20Content%20Delivery/AWS%20API%20Gateway.md).
  - [CloudFront](/Networking%20&%20Content%20Delivery/AWS%20CloudFront.md).
  - AppSync GraphQL API.
  - Cognito User Pool.
- **WAF is not for DDoS protection.**
- Define Web ACL (Web Access Control List) Rules:
  - **IP Set: up to 10,000 IP addresses** - use multiple Rules for more IPs.
  - HTTP headers, HTTP body, or URI strings Protects from common attack - **SQL injection** and **Cross-Site Scripting (XSS)**.
  - Size constraints, **geo-match (block countries)**.
  - **Rate-based Rules** (to count occurrences of events) - **for DDoS protection**.
- Web ACL are Regional except for CloudFront.
- A rule group is **a reusable set of rules that we can add to a web ACL**.

# 2. Rules

## 2.1. Rate-Based

- **Purpose**
  - Track request rates per originating IP.
  - Trigger an action if requests exceed a set limit.
  - Limit is defined as **requests per 5-minute period**.
  - Useful for temporarily blocking excessive requests.
- **Use case**
  - Limit illegitimate requests without affecting genuine traffic.

# 3. Fixed IP while using WAF with a Load Balancer

- WAF does not support the Network Load Balancer (Layer 4).
- We can use Global Accelerator for fixed IP and WAF on the ALB.
  ![ Fixed IP while using WAF with a Load Balancer](/Images/Security,%20Identity,%20&%20Compliance/AWSWAFFixedIPLoadBalancer.png)

# 4. Managed Rules

- Library of over 190 managed rules.
- Ready-to-use rules that are managed by AWS and AWS Marketplace Sellers.
- **Baseline Rule Groups:** General protection from common threats:
  - `AWSManagedRulesCommonRuleSet`
  - `AWSManagedRulesAdminProtectionRuleSet`
  - ...
- **Use-case Specific Rule Groups:** Protection for many AWS WAF use cases:
  - `AWSManagedRulesSQLiRuleSet`
  - `AWSManagedRulesWindowsRuleSet`
  - `AWSManagedRulesPHPRuleSet`
  - `AWSManagedRulesWordPressRuleSet`
  - ...
- **IP Reputation Rule Groups:** Block requests based on source (e.g., malicious IPs):
  - `AWSManagedRulesAmazonIpReputationList`
  - `AWSManagedRulesAnonymousIpList`
- **Bot Control Managed Rule Group:** Block and manage requests from bots:
  - `AWSManagedRulesBotControlRuleSet`

# 5. Logging

- We can send your logs to an:
  - Amazon CloudWatch Logs log group - 5 MB per second.
  - Amazon Simple Storage Service (Amazon S3) bucket - 5 minutes interval.
    - Your bucket names for AWS WAF logging must start with `aws-waf-logs-` and can end with any suffix you want.
    - For example, `aws-waf-logs-DOC-EXAMPLE-BUCKET-SUFFIX`.
  - Amazon Kinesis Data Firehose - limited by Firehose quotas.
  - **AWS WAF supports encryption with Amazon S3 buckets for key type Amazon S3 key (SSE-S3) and AWS Key Management Service (SSE-KMS) AWS KMS keys.**
  - **AWS WAF doesn't support encryption for AWS Key Management Service keys that are managed by AWS.**
    ![AWS WAF Integrations](/Images/Security,%20Identity,%20&%20Compliance/AWSWAFIntegrations.png)

# 6. WAF vs. Firewall Manager vs. Shield

- **WAF, Shield and Firewall Manager are used together for comprehensive protection.**
- Define your Web ACL rules in WAF.
- For granular protection of your resources, WAF alone is the correct choice.
- If we want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Firewall Manager with AWS WAF.
- Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting.
- If we're prone to frequent DDoS attacks, consider purchasing Shield Advanced.

# 7. Security Policies

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

# 8. AWS Shield

## 8.1. Protect from DDoS attack

- **DDoS:** Distributed Denial of Service - many requests at the same time.
- **AWS Shield Standard**
  - Free service that is activated for every AWS customer.
  - Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks.
- **AWS Shield Advanced**
  - Optional DDoS mitigation service ($3,000 per month per organization).
  - Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53.
  - 24/7 access to AWS DDoS response team (DRP).
  - Protect against higher fees during usage spikes due to DDoS.
  - Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks.
