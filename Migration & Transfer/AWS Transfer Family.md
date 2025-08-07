# AWS Transfer Family <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. AWS Transfer Family for Legacy SFTP Integration](#2-aws-transfer-family-for-legacy-sftp-integration)

# 1. Introduction

- A fully-managed service for file transfers into and out **ONLY** of [Amazon S3](/Storage/Amazon%20S3.md) or [Amazon EFS](/Storage/Amazon%20EFS.md) using the FTP protocol.
- **Supported Protocols**
  - **AWS Transfer for FTP** (File Transfer Protocol (FTP)).
  - **AWS Transfer for FTPS** (File Transfer Protocol over SSL (FTPS)).
  - **AWS Transfer for SFTP** (Secure File Transfer Protocol (SFTP)).
- Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ).
- Pay per provisioned endpoint per hour + data transfers in GB.
- Store and manage users' credentials within the service.
- Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom).
- **Usage:** Sharing files, public datasets, CRM, ERP, ...
  ![AWS Transfer Family](/Images/Migration%20&%20Transfer/AWSTransferFamily.png)

# 2. AWS Transfer Family for Legacy SFTP Integration

- Deploy an **AWS Transfer Family endpoint** with **SFTP** enabled to support vendors using legacy SFTP clients.
- Files uploaded via SFTP are stored directly in **Amazon S3**.
- Use **IAM roles** to:
  - Map each SFTP user to a specific role.
  - Restrict access to specific S3 buckets or prefixes (e.g., `s3://company-ingest/vendor1/`).
  - Enforce **least-privilege access** and prevent data leakage.
- Supports **auditing and logging** via **AWS CloudTrail** for visibility and compliance.
- Enables **secure, scalable, and fully managed file transfers** without requiring changes on the vendor side.
