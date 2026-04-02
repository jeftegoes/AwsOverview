# EC2 Image Builder <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. AMI Process (from an EC2 instance)](#2-ami-process-from-an-ec2-instance)
  - [2.1. Encryption and copying](#21-encryption-and-copying)
  - [2.2. Instance Migration between AZ](#22-instance-migration-between-az)
  - [2.3. Instance Migration between Regions](#23-instance-migration-between-regions)
  - [2.4. Cross-Account AMI Sharing](#24-cross-account-ami-sharing)
  - [2.5. AMI Sharing with KMS Encryption](#25-ami-sharing-with-kms-encryption)
- [3. Cross-Account AMI Copy](#3-cross-account-ami-copy)
  - [3.1. AMI Copy with KMS Encryption](#31-ami-copy-with-kms-encryption)

# 1. EC2 Image Builder

- Used to automate the creation of Virtual Machines or container images.
- Automate the creation, maintain, validate and test **EC2 AMIs**.
- Can be run on a schedule (weekly, whenever packages are updated, etc...).
- Free service (only pay for the underlying resources).
- **Can publish AMI to multiple regions and multiple accounts.**
  ![Amazon EC2 - Image Builder](/Images/Compute/AmazonEC2ImageBuilder.png)

# 2. CICD Architecture

![Amazon EC2 - Image Builder - CICD Architecture](/Images/Compute/AmazonEC2ImageBuilderCICDArchitecture.png)

# 3. Sharing using RAM

- Use AWS RAM to share **Images**, **Recipes**, and **Components** across AWS accounts or through AWS Organization.
  ![Amazon EC2 - Image Builder - Sharing Using RAM](/Images/Compute/AmazonEC2ImageBuilderSharingUsingRAM.png)

# 4. Tracking Latest AMIs

![Amazon EC2 - Image Builder - Tracking Latest AMIs](/Images/Compute/AmazonEC2ImageBuilderTrackingLatestAMIs.png)
