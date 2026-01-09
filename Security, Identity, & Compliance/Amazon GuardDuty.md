# Amazon GuardDuty <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Disable \& Suspend](#2-disable--suspend)
- [3. Multi-Account Strategy](#3-multi-account-strategy)
- [4. Findings Automated Response](#4-findings-automated-response)
  - [4.1. Findings](#41-findings)
  - [4.2. Findings Types](#42-findings-types)
- [5. Architectures](#5-architectures)
- [6. Trusted and Threat IP Lists](#6-trusted-and-threat-ip-lists)
- [7. CloudFormation](#7-cloudformation)

# 1. Introduction

- Intelligent Threat discovery to protect our AWS Account.
- Uses Machine Learning algorithms, anomaly detection, 3rd party data.
- One click to enable (30 days trial), no need to install software.
- **Input data includes**
  - **CloudTrail Events Logs:** Unusual API calls, unauthorized deployments.
    - **CloudTrail Management Events:** Create VPC subnet, create trail, ...
    - **CloudTrail S3 Data Events:** Get object, list objects, delete object, ...
  - **VPC Flow Logs:** Unusual internal traffic, unusual IP address.
  - **DNS Logs:** Compromised EC2 instances sending encoded data within DNS queries.
  - **Optional Feature:** EKS Audit Logs, RDS & Aurora, EBS, Lambda, S3 Data Events...
- Can setup **EventBridge rules** to be notified in case of findings.
- EventBridge rules can target AWS Lambda or SNS.
- **Can protect against CryptoCurrency attacks (has a dedicated "finding" for it).**
  ![Amazon GuardDuty Diagram](/Images/Security,%20Identity,%20&%20Compliance/AmazonGuardDutyDiagram.png)

# 2. Disable & Suspend

- **Disabling the service will delete all remaining data**, including our findings and configurations before relinquishing the service permissions and resetting the service.
- We can **stop** Amazon GuardDuty from analyzing our data sources at any time by choosing to **suspend** the service in the general settings. This will immediately stop the service from analyzing data, but does not delete our existing findings or configurations.

# 3. Multi-Account Strategy

- We can manage multiple accounts in GuardDuty.
- Associate the Member accounts with the Administrator account.
  - Through an AWS Organization.
  - Sending invitation through GuardDuty.
- **Administrator account can**
  - Add and remove member accounts.
  - Manage GuardDuty within the associated member accounts.
  - Manage findings, suppression rules, trusted IP lists, threat lists.
- In an AWS Organization, we can specify a member account as the Organization's **delegated administrator for GuardDuty**.
  TODO: DIAGRAM

# 4. Findings Automated Response

- Findings are potential security issue for malicious events happening in our AWS account.
- Automate response to security issues revealed by GuardDuty Findings using EventBridge.
- Send alerts to SNS (email, Lambda, Slack, Chime...).
- Events are published to both the administrator account and the member account that it is originated from.
  TODO: DIAGRAM

## 4.1. Findings

- GuardDuty pulls independent streams of data directly from CloudTrail logs (management events, data events), VPC Flow Logs or EKS logs.
- Each finding has a severity value between 0.1 to 8+ (High, Medium, Low).
- Finding naming convention: `ThreatPurpose:ResourceTypeAffected/ThreatFamilyName.DetectionMechanism!Artifact`.
  - **ThreatPurpose:** Primary purpose of the threat (e.g., Backdoor, CryptoCurrency).
  - **ResourceTypeAffected:** Which AWS resource is the target (e.g., EC2, S3).
  - **ThreatFamilyName:** Describes the potential malicious activity (e.g., NetworkPortUnusual).
  - **DetectionMechanism:** Method GuardDuty detecting the finding (e.g., Tcp, Udp).
  - **Artifact:** Describes a resource that is used in the malicious activity (e.g., DNS).
- We can generate sample findings in GuardDuty to test our automations.

## 4.2. Findings Types

- **EC2 Finding Types**
  - UnauthorizedAccess:EC2/SSHBruteForce
  - CryptoCurrency:EC2/BitcoinToo.B!DNS
- **IAM Finding Types**
  - Stealth:IAMUser/CloudTrailLoggingDisabled
  - Policy:IAMUser/RootCredentialUsage
- **Kubernetes Audit Logs Finding Types**
  - CredentialAccess:Kubernetes/MaliciousIPCaller
- **Malware Protection Finding Types**
  - Execution:EC2/SuspiciousFile
  - Execution:ECS/ SuspiciousFile
- **RDS Protection Finding Types**
  - CredentialAccess:RDS/AnomalousBehavior.SuccessfulLogin
- **S3 Finding Types**
  - Policy:S3/AccountBlockPublicAccessDisabled
  - PenTest:S3/KaliLinux

# 5. Architectures

TODO: DIAGRAM

# 6. Trusted and Threat IP Lists

- Works only for public IP addresses.
- **Trusted IP List**
  - List of IP addresses and CIDR ranges that we trust.
  - GuardDuty doesn't generate findings for these trusted lists.
- **Threat IP List**
  - List of known malicious IP addresses and CIDR ranges.
  - GuardDuty generates findings based on these threat lists.
  - Can be supplied by 3rd party threat intelligence or created custom for us.
- In a multi-account GuardDuty setup, only the GuardDuty administrator account can manage those lists.

TODO: DIAGRAM

# 7. CloudFormation

- We can enable GuardDuty using a CloudFormation template.
- If GuardDuty is already enabled, the CloudFormation Stack deployment fails.
- Use CloudFormation Custom Resouce (Lambda) to conditionally enable GuardDuty if it is not already enabled.
- Deploy across all the Organization using a Stack Set.
