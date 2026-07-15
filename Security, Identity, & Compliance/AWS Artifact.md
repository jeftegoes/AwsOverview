# AWS Artifact (not really a service) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Purpose](#2-purpose)
- [3. Third-Party Reports](#3-third-party-reports)

# 1. Introduction

- Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements.
- **Artifact Reports:** Allows us to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports.
- **Artifact Agreements:** Allows us to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in our organization.
- Can be used to **support internal audit or compliance**.

# 2. Purpose

- Central resource for compliance-related information.
- **Provides**
  - **On-demand access to AWS security & compliance REPORTS**
    - SOC reports.
    - PCI reports.
    - Certifications from global accreditation bodies.
  - **Access to select agreements**
    - Business Associate Addendum (BAA).
    - Nondisclosure Agreement (NDA).
- **Access**
  - Available to **all AWS accounts**.
  - **Root users** and **IAM admins** can download all audit artifacts (after accepting terms).
  - **Non-admin IAM users** require explicit IAM permissions to access AWS Artifact without access to other services.

# 3. Third-Party Reports

- On-demand access to security compliance reports of Independent Software Vendors (ISVs).
- ISV compliance reports will only be accessible to the AWS customers who have been granted access to AWS Marketplace Vendor Insights for a specific ISV.
- Ability to receive notifications when new reports are available.
  ![AWS Artifact - Third-Party Reports](/Images/Machine%20Learning/AWSArtifactThirdPartyReports.png)
