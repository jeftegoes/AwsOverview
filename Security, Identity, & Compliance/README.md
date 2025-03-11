# Security, Identity, & Compliance <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon Cognito](#1-amazon-cognito)
- [2. AWS IAM Identity Center (Successor to AWS Single Sign-On)](#2-aws-iam-identity-center-successor-to-aws-single-sign-on)
- [3. AWS STS - Security Token Service](#3-aws-sts---security-token-service)
- [4. AWS WAF - Web Application Firewall](#4-aws-waf---web-application-firewall)
- [5. AWS KMS (Key Management Service)](#5-aws-kms-key-management-service)
- [6. AWS Certificate Manager (ACM)](#6-aws-certificate-manager-acm)
- [7. Amazon GuardDuty](#7-amazon-guardduty)
- [8. Amazon Inspector](#8-amazon-inspector)
- [9. AWS Directory Services](#9-aws-directory-services)
- [10. Summary](#10-summary)

# 1. Amazon Cognito

- **Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily.** [AWS Cognito](Amazon%20Cognito.md)

# 2. AWS IAM Identity Center (Successor to AWS Single Sign-On)

[AWS IAM Identity Center](AWS%20IAM%20Identity%20Center.md)

# 3. AWS STS - Security Token Service

[AWS STS](AWS%20STS.md)

# 4. AWS WAF - Web Application Firewall

- **AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources.** [AWS WAF](AWS%20WAF.md)

# 5. AWS KMS (Key Management Service)

- [AWS KMS](AWS%20KMS.md)

# 6. AWS Certificate Manager (ACM)

- **AWS Certificate Manager is a service that lets you easily provision, manage, and deploy public and private Secure Sockets Layer/Transport Layer Security (SSL/TLS) certificates for use with AWS services and your internal connected resources.** [AWS ACM](AWS%20Certificate%20Manager.md)

# 7. Amazon GuardDuty

- **Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads.** [Amazon GuardDuty](Amazon%20GuardDuty.md)

# 8. Amazon Inspector

- **Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS.**
- **It helps you test the network accessibility of your Amazon EC2 instances and the security state of your applications running on the instances.** [Amazon Inspector](Amazon%20Inspector.md)

# 9. AWS Directory Services

[AWS Directory Services](AWS%20Directory%20Services.md)

# 10. Summary

- **IAM**
  - Identity and Access Management inside your AWS account.
  - For users that you trust and belong to your company.
  - **IAM Roles are sets of permissions making AWS service requests, which will be used by AWS services, but they do not provide temporary security credentials.**
- Security Token Service (STS): temporary, limited-privileges credentials to access AWS resources.
- Cognito: create a database of users for your mobile & web applications.
- Directory Services: integrate Microsoft Active Directory in AWS.
- Single Sign-On (SSO): one login for multiple AWS accounts & applications.
- WAF: Firewall to filter incoming requests based on rules
- KMS: Encryption keys managed by AWS
- GuardDuty: Find malicious behavior with VPC, DNS & CloudTrail Logs
- Inspector: For EC2 only, install agent and find vulnerabilities
