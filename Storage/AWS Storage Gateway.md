# AWS Storage Gateway <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Hybrid Cloud for Storage](#1-hybrid-cloud-for-storage)
- [2. AWS Storage Gateway](#2-aws-storage-gateway)
- [3. Types of Storage Gateway](#3-types-of-storage-gateway)
  - [3.1. Amazon S3 File Gateway](#31-amazon-s3-file-gateway)
    - [3.1.1. RefreshCache API](#311-refreshcache-api)
    - [3.1.2. Automating Cache Refresh](#312-automating-cache-refresh)
    - [3.1.3. Amazon FSx File Gateway](#313-amazon-fsx-file-gateway)
  - [3.2. Volume Gateway](#32-volume-gateway)
  - [3.3. Tape Gateway](#33-tape-gateway)
- [4. Hardware appliance](#4-hardware-appliance)
- [5. AWS Storage Gateway Overview](#5-aws-storage-gateway-overview)

# 1. Hybrid Cloud for Storage

- **AWS is pushing for "hybrid cloud"**
  - Part of your infrastructure is on-premises.
  - Part of your infrastructure is on the cloud.
- **This can be due to**
  - Long cloud migrations.
  - Security requirements.
  - Compliance requirements.
  - IT strategy.
- S3 is a proprietary storage technology (unlike EFS / NFS), so how do we expose the S3 data on-premise?
  - AWS Storage Gateway!

# 2. AWS Storage Gateway

- Bridge between on-premise data and cloud data in S3.
  ![AWS Storage Gateway Diagram](/Images/Storage/AWSStorageGatewayDiagram.png)
  ![AWS Storage Gateway](/Images//Storage/AWSStorageGateway.png)
- **Hybrid storage service to allow on- premises to seamlessly use the AWS Cloud.**
- **Use cases**
  - Disaster recovery.
  - Backup & restore.
  - Tiered storage.
  - On-premises cache & low-latency files access.
- **Types of Storage Gateway**
  - S3 File Gateway.
  - FSx File Gateway.
  - Volume Gateway.
  - Tape Gateway.

# 3. Types of Storage Gateway

## 3.1. Amazon S3 File Gateway

- Configured S3 buckets are accessible using the **NFS** and **SMB** protocol.
- **Most recently used data is cached in the file gateway.**
- Supports S3 Standard, S3 Standard IA, S3 One Zone A, S3 Intelligent Tiering.
- **Transition to S3 Glacier using a Lifecycle Policy.**
- Bucket access using IAM roles for each File Gateway.
- SMB Protocol has integration with Active Directory (AD) for user authentication.
- **IMPORTANT!** Does not support file shares in Amazon FSx for Windows File Server.
  ![AWS Storage Gateway - Amazon S3 File Gateway](/Images/Storage/AWSStorageGatewayAmazonS3FileGateway.png)

### 3.1.1. RefreshCache API

- Storage Gateway updates the File Share Cache automatically when we write files to the File Gateway.
- When we upload files directly to the S3 bucket, users connected to the File Gateway may not see the files on the File Share (accessing stale data).
- We need to invoke the `RefreshCache` API.
  ![AWS Storage Gateway RefreshCache API](/Images/Storage/AWSStorageGatewayRefreshCacheAPI.png)

### 3.1.2. Automating Cache Refresh

- **Automated Cache Refresh:** A File Gateway feature that enables we to automatically refresh File Gateway cache to stay up to date with the changes in their S3 buckets.
- Ensure that users are not accessing stale data on their file shares.
- No need to manually and periodically invoke the `RefreshCache` API.

### 3.1.3. Amazon FSx File Gateway

- Native access to Amazon FSx for Windows File Server.
- **Local cache for frequently accessed data.**
- Windows native compatibility (SMB, NTFS, Active Directory...).
- Useful for group file shares and home directories.
  ![AWS Storage Gateway - Amazon FSx File Gateway](/Images/Storage/AWSStorageGatewayAmazonFSxFileGateway.png)

## 3.2. Volume Gateway

- Block storage using iSCSI protocol backed by S3.
- Backed by EBS snapshots which can help restore on-premises volumes!
- **Cached volumes:** Low latency access to most recent data.
- **Stored volumes:** Entire dataset is on premise, scheduled backups to S3.
  ![AWS Storage Gateway - Volume Gateway](/Images/Storage/AWSStorageGatewayVolumeGateway.png)

## 3.3. Tape Gateway

- Some companies have backup processes using physical tapes (!).
- With Tape Gateway, companies use the same processes but, in the cloud.
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier.
- Back up data using existing tape-based processes (and iSCSI interface).
- Works with leading backup software vendors.
  ![AWS Storage Gateway - Tape Gateway](/Images/Storage/AWSStorageGatewayTapeGateway.png)

# 4. Hardware appliance

- Using Storage Gateway means we need on-premises virtualization.
- Otherwise, we can use a **Storage Gateway Hardware Appliance**.
- We can buy it on amazon.com.
- Works with File Gateway, Volume Gateway, Tape Gateway.
- Has the required CPU, memory, network, SSD cache resources.
- Helpful for daily NFS backups in small data centers.

# 5. AWS Storage Gateway Overview

![AWS Storage Gateway Overview](/Images/Storage/AWSStorageGatewayOverview.png)
