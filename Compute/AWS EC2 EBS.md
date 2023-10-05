# AWS EC2 EBS - Elastic Block Storage<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. EBS Volume](#2-ebs-volume)
- [3. EBS - Delete on Termination attribute](#3-ebs---delete-on-termination-attribute)
- [4. EBS Snapshots](#4-ebs-snapshots)
  - [4.1. Features](#41-features)
- [5. AMI Overview](#5-ami-overview)
  - [5.1. AMI Process (from an EC2 instance)](#51-ami-process-from-an-ec2-instance)
  - [5.2. EC2 Image Builder](#52-ec2-image-builder)
  - [5.3. Cross-Account AMI Sharing](#53-cross-account-ami-sharing)
  - [5.4. Cross-Account AMI Copy](#54-cross-account-ami-copy)
- [6. EC2 Instance Store](#6-ec2-instance-store)
- [7. EBS Volume Types](#7-ebs-volume-types)
  - [7.1. EBS Volume Types Use cases](#71-ebs-volume-types-use-cases)
    - [7.1.1. General Purpose SSD](#711-general-purpose-ssd)
    - [7.1.2. Provisioned IOPS (PIOPS) SSD](#712-provisioned-iops-piops-ssd)
    - [7.1.3. Hard Disk Drives (HDD)](#713-hard-disk-drives-hdd)
    - [7.1.4. EBS Multi-Attach - io1/io2 family](#714-ebs-multi-attach---io1io2-family)
- [8. EFS - Elastic File System](#8-efs---elastic-file-system)
  - [8.1. EFS - Performance and Storage Classes](#81-efs---performance-and-storage-classes)
  - [8.2. EBS vs EFS - Elastic Block Storage](#82-ebs-vs-efs---elastic-block-storage)
  - [8.3. EBS vs EFS - Elastic File System](#83-ebs-vs-efs---elastic-file-system)
- [9. Shared Responsibility Model for EC2 Storage](#9-shared-responsibility-model-for-ec2-storage)
- [10. Amazon FSx - Overview](#10-amazon-fsx---overview)
- [11. Amazon FSx for Windows File Server](#11-amazon-fsx-for-windows-file-server)
- [12. Amazon FSx for Lustre](#12-amazon-fsx-for-lustre)

# 1. Introduction

- An **EBS (Elastic Block Store) Volume** is a **network drive** you can attach to your instances while they run.
- It allows your instances to persist data, even after their termination.
- **They can only be mounted to one instance at a time (at the CCP level).**
- They are bound to a specific **availability zone (AZ)**.
- Analogy: Think of them as a "network USB stick".
- Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month.

# 2. EBS Volume

- It's a network drive (i.e. not a physical drive).
  - It uses the network to communicate the instance, which means there might be a bit of latency.
  - It can be detached from an EC2 instance and attached to another one quickly.
- It's locked to an Availability Zone (AZ).
  - An EBS Volume in us-east-1a cannot be attached to us-east-1b.
  - To move a volume across, you first need to **snapshot** it.
- Have a provisioned capacity (size in GBs, and IOPS).
  - You get billed for all the provisioned capacity.
  - You can increase the capacity of the drive over time.

![EBS Volume diagram](/Images/EBSVolumeDiagram.png)

# 3. EBS - Delete on Termination attribute

- Controls the EBS behaviour when an EC2 instance terminates.
  - By default, the root EBS volume is deleted (attribute enabled).
  - By default, any other attached EBS volume is not deleted (attribute disabled).
- This can be controlled by the AWS console / AWS CLI.
- **Use case: preserve root volume when instance is terminated.**

# 4. EBS Snapshots

- Make a backup (snapshot) of your EBS volume at a point in time.
- Not necessary to detach volume to do snapshot, **but recommended**.
- Can copy snapshots across AZ or Region.

![EBS Snapshot diagram](/Images/EBSSnapshotDiagram.png)

## 4.1. Features

- **EBS Snapshot Archive:**
  - Move a Snapshot to an "archive tier" that is 75% cheaper.
  - Takes within 24 to 72 hours for restoring the archive.
- **Recycle Bin for EBS Snapshots:**
  - Setup rules to retain deleted snapshots so you can recover them after an accidental deletion.
  - Specify retention (from 1 day to 1 year).
- **Fast Snapshot Restore (FSR)**
  - Force full initialization of snapshot to have no latency on the first use ($$$).

# 5. AMI Overview

- AMI = Amazon Machine Image.
  - **Golden AMI** is an AMI that you standardize through configuration, consistent security patching, and hardening.
    - It also contains agents you approve for logging, security, performance monitoring, etc.
- AMI are a **customization** of an EC2 instance.
  - You add your own software, configuration, operating system, monitoring...
  - Faster boot / configuration time because all your software is pre-packaged.
- AMI are built for a **specific region** (and can be copied across regions).
- You can launch EC2 instances from:
  - **A Public AMI:** AWS provided.
  - **Your own AMI:** you make and maintain them yourself.
  - **An AWS Marketplace AMI:** an AMI someone else made (and potentially sells).

## 5.1. AMI Process (from an EC2 instance)

- Start an EC2 instance and customize it.
- Stop the instance (for data integrity).
- Build an AMI - this will also create EBS snapshots.
- Launch instances from other AMIs.

## 5.2. EC2 Image Builder

- Used to automate the creation of Virtual Machines or container images.
- Automate the creation, maintain, validate and test **EC2 AMIs**.
- Can be run on a schedule (weekly, whenever packages are updated, etc...).
- Free service (only pay for the underlying resources).
- Steps:
  1. EC2 Image Builder
     1. Create
  2. Builder EC2 Instance
     1. Create
  3. New AMI
  4. Test EC2 Instance

## 5.3. Cross-Account AMI Sharing

- You can share an AMI with another AWS account.
- Sharing an AMI does not affect the ownership of the AMI.
- You can only share AMIs that have unencrypted volumes and volumes that are encrypted with a customer managed key.
- If you share an AMI with encrypted volumes, you must also share any customer managed keys used to encrypt them.

## 5.4. Cross-Account AMI Copy

- If you copy an AMI that has been shared with your account, you are the owner of the target AMI in your account.
- The owner of the source AMI must grant you read permissions for the storage that backs the AMI (EBS Snapshot).
- If the shared AMI has encrypted snapshots, the owner must share the key or keys with you as well.
- Can encrypt the AMI with your own CMK while copying.

# 6. EC2 Instance Store

- EBS volumes are **network drives** with good but "limited" performance.
- **If you need a high-performance hardware disk, use EC2 Instance Store**.
- Better I/O performance.
- EC2 Instance Store lose their storage if they're stopped (ephemeral).
- Good for buffer / cache / scratch data / temporary content.
- Risk of data loss if hardware fails.
- Backups and Replication are your responsibility.

# 7. EBS Volume Types

- **EBS Volumes come in 6 types:**
  - **gp2 / gp3 (SSD)**: General purpose SSD volume that balances price and performance for a wide variety of workloads.
  - **io1 / io2 (SSD)**: Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads.
  - **st1 (HDD):** Low cost HDD volume designed for frequently accessed, throughput-intensive workloads.
  - **sc1 (HDD):** Lowest cost HDD volume designed for less frequently accessed workloads.
- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec).
- When in doubt always consult the AWS documentation - it's good!
- **Only gp2/gp3 and io1/io2 can be used as boot volumes.**

## 7.1. EBS Volume Types Use cases

### 7.1.1. General Purpose SSD

- Cost effective storage, low-latency.
- System boot volumes, Virtual desktops, Development and test environments.
- 1 GiB - 16 TiB.
- gp3:
  - Baseline of 3,000 IOPS and throughput of 125 MiB/s.
  - Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s independently.
- gp2:
  - Small gp2 volumes can burst IOPS to 3,000.
  - Size of the volume and IOPS are linked, max IOPS is 16,000.
  - 3 IOPS per GB, means at 5,334 GB we are at the max IOPS.

### 7.1.2. Provisioned IOPS (PIOPS) SSD

- Critical business applications with sustained IOPS performance.
- Or applications that need more than 16,000 IOPS.
- Great for **databases workloads** (sensitive to storage perf and consistency).
- io1/io2 (4 GiB - 16 TiB):
  - Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other.
  - Can increase PIOPS independently from storage size.
  - io2 have more durability and more IOPS per GiB (at the same price as io1).
- io2 Block Express (4 GiB - 64 TiB):
  - Sub-millisecond latency.
  - Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1.
- Supports EBS Multi-attach.

### 7.1.3. Hard Disk Drives (HDD)

- Cannot be a boot volume.
- 125 GiB to 16 TiB.
- Throughput Optimized HDD (st1):
  - Big Data, Data Warehouses, Log Processing.
  - **Max throughput** 500 MiB/s - max IOPS 500.
- Cold HDD (sc1):
  - For data that is infrequently accessed.
  - Scenarios where lowest cost is important.
  - **Max throughput** 250 MiB/s - max IOPS 250.

### 7.1.4. EBS Multi-Attach - io1/io2 family

- Attach the same EBS volume to multiple EC2 instances in the same AZ.
- Each instance has full read & write permissions to the volume.
- Use case:
  - Achieve **higher application availability** in clustered Linux applications (ex: Teradata).
  - Applications must manage concurrent write operations.
- **Up to 16 EC2 Instances at a time.**
- Must use a file system that's cluster-aware (not XFS, EX4, etc...).

# 8. EFS - Elastic File System

- Managed NFS (network file system) that **can be mounted on many EC2**.
- EFS works with EC2 instances in **multi-AZ**.
- Highly available, scalable, expensive (3x gp2), pay per use, no capacity planning.
- Use cases: content management, web serving, data sharing, Wordpress.
- Uses NFSv4.1 protocol.
- Uses security group to control access to EFS.
- **Compatible with Linux based AMI (not Windows).**
- Encryption at rest using KMS.
- POSIX file system (~Linux) that has a standard file API.
- File system scales automatically, pay-per-use, no capacity planning!

![EFS diagram](/Images/EFSDiagram.png)

## 8.1. EFS - Performance and Storage Classes

- **EFS Scale:**
  - 1000s of concurrent NFS clients, 10 GB+ /s throughput.
  - Grow to Petabyte-scale network file system, automatically.
- **Performance mode (set at EFS creation time):**
  - General purpose (default): latency-sensitive use cases (web server, CMS, etc...).
  - Max I/O - higher latency, throughput, highly parallel (big data, media processing).
- **Throughput mode:**
  - Bursting (1 TB = 50MiB/s + burst of up to 100MiB/s).
  - Provisioned: set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage.
- **Storage Tiers (lifecycle management feature - move file after N days):**
  - Standard: for frequently accessed files.
  - Infrequent access (EFS-IA): cost to retrieve files, lower price to store. Enable EFS-IA with a Lifecycle Policy.
- **Availability and durability:**
  - Regional: Multi-AZ, great for prod.
  - One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA).
- Over 90% in cost savings.

## 8.2. EBS vs EFS - Elastic Block Storage

- **EBS volumes:**
  - Can be attached to only one instance at a time.
  - Are locked at the Availability Zone (AZ) level.
  - gp2: IO increases if the disk size increases.
  - io1: can increase IO independently.
- **To migrate an EBS volume across AZ:**
  - Take a snapshot.
  - Restore the snapshot to another AZ.
  - EBS backups use IO and you shouldn't run them while your application is handling a lot of traffic.
- Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated (you can disable that).

## 8.3. EBS vs EFS - Elastic File System

- Mounting 100s of instances across AZ.
- EFS share website files (WordPress).
- Only for Linux Instances (POSIX).
- EFS has a higher price point than EBS.
- Can leverage EFS-IA for cost savings.
- Remember: EFS vs EBS vs Instance Store.

# 9. Shared Responsibility Model for EC2 Storage

- AWS:
  - Infrastructure.
  - Replication for data for EBS volumes & EFS drives.
  - Replacing faulty hardware.
  - Ensuring their employees cannot access your data.
- You:
  - Setting up backup / snapshot procedures.
  - Setting up data encryption.
  - Responsibility of any data on the drives.
  - Understanding the risk of using EC2 Instance Store.

# 10. Amazon FSx - Overview

- Launch 3rd party high-performance file systems on AWS.
- Fully managed service.
- Products:
  - FSx for Lustre.
  - FSx for Windows File Server.
  - FSx for NetApp ONTAP.

# 11. Amazon FSx for Windows File Server

- A fully managed, highly reliable, and scalable Windows native shared file system.
- Built on Windows File Server.
- Supports SMB protocol & Windows NTFS.
- Integrated with Microsoft Active Directory.
- Can be accessed from AWS or your on-premise infrastructure.

# 12. Amazon FSx for Lustre

- A fully managed, high-performance, scalable file storage for **High Performance Computing (HPC)**.
- The name Lustre is derived from "Linux" and "cluster".
- Machine Learning, Analytics, Video Processing, Financial Modeling, ...
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies.
