# AWS DataSync <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Transfer between AWS storage services](#2-transfer-between-aws-storage-services)

# 1. Introduction

- **Move large amount of data to and from**
  - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API...) - needs agent.
  - AWS to AWS (different storage services) - no agent needed.
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
