# Amazon Inspector <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. What does AWS Inspector evaluate?](#2-what-does-aws-inspector-evaluate)
- [3. Systems Manager Integration](#3-systems-manager-integration)

# 1. Introduction

- **Automated Security Assessments.**
- **For EC2 instances**
  - Leveraging the [AWS System Manager (SSM)](/Management%20&%20Governance/AWS%20Systems%20Manager.md) agent.
    > **IMPORTANT!** The **new Amazon Inspector** does **not require a dedicated Inspector agent**.
  - Analyze against **unintended network accessibility**.
  - Analyze the **running OS** against **known vulnerabilities**.
- **For Containers push to Amazon ECR**
  - Assessment of containers as they are pushed.
- **For Lambda Functions**
  - Identifies software vulnerabilities in function code and package dependencies.
  - Assessment of functions as they are deployed.
- Reporting & integration with AWS Security Hub.
- Send findings to Amazon Event Bridge.
  ![Amazon Inspector Diagram](/Images/Security,%20Identity,%20&%20Compliance/AmazonInspectorDiagram.png)

# 2. What does AWS Inspector evaluate?

- **IMPORTANT! Only for EC2 instances, Container Images and Lambda functions.**
- Continuous scanning of the infrastructure, only when needed.
- Package vulnerabilities (EC2, ECR and Lambda).
  - Database of CVE (Common Vulnerabilities and Exposures).
- Network reachability (EC2).
- A risk score is associated with all vulnerabilities for prioritization.

# 3. Systems Manager Integration

- **EC2 instance setup in Inspector requires**
  - **SSM Agent is running.**
  - Be an SSM Managed Instance (IAM Role or Default Host Management Config.).
  - Outbound 443 to Systems Manager endpoint.
    ![Systems Manager Integration](/Images/Security,%20Identity,%20&%20Compliance/AmazonInspectorSystemsManager.png)
