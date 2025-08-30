# AWS DataSync <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Transfer between AWS storage services](#2-transfer-between-aws-storage-services)
- [3. Data Transfer from On-Premises NFS to Amazon EFS using AWS DataSync](#3-data-transfer-from-on-premises-nfs-to-amazon-efs-using-aws-datasync)
- [4. AWS DataSync vs AWS Storage Gateway](#4-aws-datasync-vs-aws-storage-gateway)
  - [4.1. AWS DataSync](#41-aws-datasync)
  - [4.2. AWS Storage Gateway](#42-aws-storage-gateway)
  - [4.3. 🔑 Main Differences](#43--main-differences)

# 1. Introduction

- **Move large amount of data to and from**
  - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API...) - **needs agent**.
  - AWS to AWS (different storage services) - **no agent needed**.
  - **IMPORTANT! Except EBS**.
- **Can synchronize to**
  - Amazon S3 (any storage classes - including Glacier).
  - Amazon EFS.
  - Amazon FSx (Windows, Lustre, NetApp, OpenZFS...).
- Replication tasks can be scheduled hourly, daily, weekly.
- File permissions and metadata are preserved (NFS POSIX, SMB...).
- One agent task can use 10 Gbps, can setup a bandwidth limit.
  ![AWS DataSync](/Images/Migration%20&%20Transfer/AwsDataSyncDiagram.png)

# 2. Transfer between AWS storage services

![Transfer between AWS storage services](/Images/Migration%20&%20Transfer/AwsDataSyncTransferStorageServices.png)

# 3. Data Transfer from On-Premises NFS to Amazon EFS using AWS DataSync

![Data Transfer from On-Premises NFS to Amazon EFS using AWS DataSync](/Images/Migration%20&%20Transfer/AwsDataSyncTransferEFS.png)

# 4. AWS DataSync vs AWS Storage Gateway

## 4.1. AWS DataSync

- **Purpose:** Data **transfer/migration service**.
- **Use Case:** Move large amounts of data **between on-premises and AWS** or between AWS storage services.
- **Direction:** Primarily **one-way or scheduled transfers** (on-prem <-> AWS, AWS <-> AWS).
- **Best for**
  - Data migration to the cloud.
  - Periodic synchronization of datasets.
  - Moving files to Amazon S3, EFS, FSx.
- **Key Point:** Optimized for **high-speed, secure, and automated transfers**.

## 4.2. AWS Storage Gateway

- **Purpose: Hybrid cloud storage integration.**
- **Use Case:** Provide on-premises applications with **seamless access to AWS storage**.
- **Direction:** Continuous integration (on-prem apps interact with AWS storage like a local system).
- **Types**
  - File Gateway (NFS/SMB to S3).
  - Volume Gateway (block storage with EBS snapshots).
  - Tape Gateway (VTL for backup to S3/Glacier).
- **Best for**
  - Extending on-prem storage into AWS.
  - Backup/archival with cloud.
  - Hybrid workloads needing **local + cloud** access.
- **Key Point:** Acts like a **bridge** between on-premises systems and AWS storage.

---

## 4.3. 🔑 Main Differences

| Feature             | AWS DataSync                       | AWS Storage Gateway                    |
| ------------------- | ---------------------------------- | -------------------------------------- |
| **Primary Purpose** | Data transfer/migration            | Hybrid storage access                  |
| **Usage Model**     | Bulk or scheduled sync jobs        | Continuous, transparent integration    |
| **Best For**        | Migration, replication, sync tasks | Backup, hybrid apps, extending storage |
| **Access**          | Move data in/out of AWS storage    | Present AWS storage as local storage   |
| **Services**        | S3, EFS, FSx                       | S3, EBS (snapshots), Glacier           |
