# Amazon EC2 - Elastic Compute Cloud <!-- omit in toc -->

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

# 1. Introduction

- AMI = Amazon Machine Image.
  - **Golden AMI** is an AMI that we standardize through configuration, consistent security patching, and hardening.
    - It also contains agents we approve for logging, security, performance monitoring, etc.
- AMI are a **customization** of an EC2 instance.
  - We add our own software, configuration, operating system, monitoring...
  - Faster boot / configuration time because all our software is pre-packaged.
- AMI are built for a **specific region**
  - **We can copy an Amazon Machine Image (AMI) across AWS Regions.**
  - **We can share an Amazon Machine Image (AMI) with another AWS account.**
- **We can launch EC2 instances from**
  - **A Public AMI:** AWS provided.
  - **Our own AMI:** We make and maintain them yourself.
  - **An AWS Marketplace AMI:** An AMI someone else made (and potentially sells).

# 2. AMI Process (from an EC2 instance)

1. Start an EC2 instance and customize it.
2. Stop the instance (for data integrity).
3. Build an AMI - this will also create EBS snapshots.
4. Launch instances from other AMIs.

## 2.1. Encryption and copying

- The following table shows encryption support for various AMI-copying scenarios.

| Scenario | Description                | Supported |
| -------- | -------------------------- | --------- |
| 1        | Unencrypted to unencrypted | Yes       |
| 2        | Encrypted to encrypted     | Yes       |
| 3        | Unencrypted to encrypted   | Yes       |
| 4        | Encrypted to unencrypted   | No        |

## 2.2. Instance Migration between AZ

- Remember **between AZ**.
  ![EC2 Instance Migration between AZ](/Images/Compute/AmazonEC2MigrationBetweenAZ.png)

## 2.3. Instance Migration between Regions

- **IMPORTANT!** When the new AMI is copied from Region A into Region B, it automatically creates a snapshot in Region B because AMIs are based on the underlying snapshots.
  ![EC2 Instance Migration Between Regions](/Images/Compute/AmazonEC2InstanceMigrationBetweenRegions.png)

## 2.4. Cross-Account AMI Sharing

- You can share an AMI with another AWS account.
- Sharing an AMI does **not affect the ownership** of the AMI.
- **You can only share AMIs that have**
  1. Unencrypted volumes.
  2. Volumes that are encrypted with a customer managed key.
- If you share an AMI with encrypted volumes,** you must also share any customer managed keys used to encrypt them**.
  ![Cross-Account AMI Sharing](/Images/Compute/AmazonEC2CrossAccountAMISharing.png)

## 2.5. AMI Sharing with KMS Encryption

![AMI Sharing with KMS Encryption](/Images/Compute/AmazonEC2AMISharingWithKMSEncryption.png)

# 3. Cross-Account AMI Copy

- If you copy an AMI that has been shared with your account, you are the owner of the target AMI in your account.
- The owner of the source AMI must grant you read permissions for the storage that backs the **AMI (EBS Snapshot)**.
- If the shared AMI has encrypted snapshots, the owner must share the key or keys with you as well.
- Can encrypt the AMI with your own CMK while copying.

![Cross-Account AMI Copy](/Images/Compute/AmazonEC2CrossAccountAMICopy.png)

## 3.1. AMI Copy with KMS Encryption

- **Cross-Region / Cross-Account Encrypted AMI Copy**
  ![AMI Copy with KMS Encryption](/Images/Compute/AmazonEC2CrossAccountAMICopyWithKMSEncryption.png)
