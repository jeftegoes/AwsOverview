# Amazon Inspector <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. What does Amazon Inspector evaluate?](#2-what-does-amazon-inspector-evaluate)
- [3. Systems Manager Integration](#3-systems-manager-integration)
- [4. Amazon Inspector \& AMIs](#4-amazon-inspector--amis)
  - [4.1. Diagram](#41-diagram)

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

# 2. What does Amazon Inspector evaluate?

- > **IMPORTANT! Only for EC2 instances, Container Images and Lambda functions.**
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

# 4. Amazon Inspector & AMIs

- Regularly scan **golden AMIs** for vulnerabilities using an automated workflow.
  1. **Scheduled Trigger**
     - EventBridge runs **daily**.
  2. **Start Step Functions**
     - Orchestrates the entire process.
  3. **Launch EC2 Instance**
     - Created from the **golden AMI**.
     - **Tagged**
       ```text
        CheckVulnerabilities = True
       ```
  4. **Run Security Assessment**
     - Amazon Inspector runs a scan.
     - Targets instances using the tag.
     - **Uses rules like**
       - CVEs (Common Vulnerabilities and Exposures).
  5. **Terminate Instance**
     - Instance is deleted after the scan

## 4.1. Diagram

TODO update diagram
![Amazon Inspector & AMIs](/Images/Security,%20Identity,%20&%20Compliance/KW_1217_diagram.png)
