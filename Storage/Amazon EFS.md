# Amazon Elastic File System (Amazon EFS) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Performance and Storage Classes](#2-performance-and-storage-classes)
  - [2.1. EFS Scale](#21-efs-scale)
  - [2.2. Performance mode (set at EFS creation time)](#22-performance-mode-set-at-efs-creation-time)
  - [2.3. Throughput mode](#23-throughput-mode)
  - [2.4. Storage Tiers](#24-storage-tiers)
  - [2.5. Availability and durability](#25-availability-and-durability)
- [3. Inter-region VPC peering connection](#3-inter-region-vpc-peering-connection)
- [4. EFS vs EBS - Elastic Block Storage](#4-efs-vs-ebs---elastic-block-storage)
- [5. EBS vs EFS - Elastic File System](#5-ebs-vs-efs---elastic-file-system)

# 1. Introduction

- Amazon EFS = Amazon Elastic File System.
- Managed NFS (network file system) that **can be mounted on many EC2**.
- EFS works with EC2 instances in **multi-AZ**.
- Highly available, scalable, expensive (3x gp2), pay per use, no capacity planning.
  ![Amazon Elastic File System Diagram](/Images/Storage/AmazonEFSDiagram.png)
- **Use cases:** Content management, web serving, data sharing, Wordpress.
- Uses NFSv4.1 protocol.
- Uses security group to control access to EFS.
- **Compatible with Linux based AMI (not Windows).**
- Encryption at rest using KMS.
- POSIX file system (~Linux) that has a standard file API.
- File system scales automatically, pay-per-use, no capacity planning!

# 2. Performance and Storage Classes

## 2.1. EFS Scale

- 1000s of concurrent NFS clients, 10 GB+ /s throughput.
- Grow to Petabyte-scale network file system, automatically.

## 2.2. Performance mode (set at EFS creation time)

- **General purpose (default):** Latency-sensitive use cases (web server, CMS, etc...).
- **Max I/O:** Higher latency, throughput, highly parallel (big data, media processing).

## 2.3. Throughput mode

- **Bursting:** (1 TB = 50MiB/s + burst of up to 100MiB/s).
- **Provisioned:** Set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage.
- **Elastic:** Automatically scales throughput up or down based on your workloads.
  - Up to 3GiB/s for reads and 1GiB/s for writes.
  - Used for unpredictable workloads.

## 2.4. Storage Tiers

- Lifecycle management feature - move file after N days.
- **Standard:** For frequently accessed files.
- **Infrequent access (EFS-IA):** Cost to retrieve files, lower price to store. Enable EFS-IA with a Lifecycle Policy.
  ![Amazon EFS Infrequent Access](/Images/Storage/AmazonEFSInfrequentAccess.png)
- **Archive:** Rarely accessed data (few times each year), 50% cheaper.
- Implement **lifecycle policies** to move files between storage tiers.

## 2.5. Availability and durability

- **Regional:** Multi-AZ, great for prod.
- **One Zone:** One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA).
- Over 90% in cost savings.

# 3. Inter-region VPC peering connection

- We can connect to Amazon EFS file systems from EC2 instances in other AWS regions using an inter-region VPC peering connection, and from on-premises servers using an AWS VPN connection.

# 4. EFS vs EBS - Elastic Block Storage

- **EBS volumes**
  - Can be attached to only one instance (except multi-attach `io1/io2`).
  - Are locked at the Availability Zone (AZ) level.
  - **gp2:** IO increases if the disk size increases.
  - **gp3 & io1:** Can increase IO independently
- **To migrate an EBS volume across AZ**
  1. Take a snapshot.
  2. Restore the snapshot to another AZ.
  3. EBS backups use IO and we shouldn't run them while your application is handling a lot of traffic.
- Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated (we can disable that).

# 5. EBS vs EFS - Elastic File System

- Mounting 100s of instances across AZ.
- EFS share website files (WordPress).
- Only for Linux Instances (POSIX).
- EFS has a higher price point than EBS.
- Can leverage EFS-IA for cost savings.
- Remember: EFS vs EBS vs Instance Store.
