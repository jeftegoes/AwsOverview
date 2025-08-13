# Amazon Elastic File System (Amazon EFS) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Performance and Storage Classes](#2-performance-and-storage-classes)
  - [2.1. EFS Scale](#21-efs-scale)
  - [2.2. Performance mode (set at EFS creation time)](#22-performance-mode-set-at-efs-creation-time)
  - [2.3. Throughput mode](#23-throughput-mode)
    - [2.3.1. Comparison Table](#231-comparison-table)
  - [2.4. Storage Tiers](#24-storage-tiers)
  - [2.5. Availability and durability](#25-availability-and-durability)
- [3. Inter-region VPC peering connection](#3-inter-region-vpc-peering-connection)
- [4. EFS vs EBS - Elastic Block Storage](#4-efs-vs-ebs---elastic-block-storage)
- [5. EBS vs EFS - Elastic File System](#5-ebs-vs-efs---elastic-file-system)
- [6. Amazon EFS Mount Targets](#6-amazon-efs-mount-targets)
  - [6.1. Why Create a Mount Target in Each AZ?](#61-why-create-a-mount-target-in-each-az)
  - [6.2. How It Works](#62-how-it-works)
  - [6.3. High Availability Benefits](#63-high-availability-benefits)
  - [6.4. AWS Recommendation](#64-aws-recommendation)
- [7. Cross-Account Amazon EFS Access for AWS Lambda](#7-cross-account-amazon-efs-access-for-aws-lambda)

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

- **Bursting (burst throughput):** (1 TB = 50MiB/s + burst of up to 100MiB/s).
- **Provisioned:** Set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage.
- **Elastic:** Automatically scales throughput up or down based on your workloads.
  - Up to 3GiB/s for reads and 1GiB/s for writes.
  - Used for unpredictable workloads.

### 2.3.1. Comparison Table

| Feature               | Bursting Throughput            | Provisioned Throughput           |
| --------------------- | ------------------------------ | -------------------------------- |
| Default mode          | Yes                            | No                               |
| Throughput depends on | File system size               | User-specified throughput        |
| Cost basis            | Storage + usage                | Storage + provisioned throughput |
| Ideal for             | Spiky, unpredictable workloads | Consistent high-throughput needs |
| Throttling risk       | Yes, if credits run out        | No, as long as within limits     |
| Manual configuration  | No                             | Yes                              |

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
- **Remember:** EFS vs EBS vs Instance Store.

  ![Amazon EFS Mount Target](/Images/Storage/AmazonEFSMountTarget.png)

# 6. Amazon EFS Mount Targets

## 6.1. Why Create a Mount Target in Each AZ?

- **Amazon EFS (Elastic File System)** is a regional service, but EC2 instances connect to it through **MOUNT TARGETS**, which are specific to an **Availability Zone (AZ)**.
- Creating a **mount target in each AZ** where your EC2 instances run ensures:
  - **Lowest possible latency** - Traffic stays inside the same AZ.
  - **No inter-AZ data transfer costs** - You avoid paying for cross-zone network usage.
    ![Amazon EFS Mount Target](/Images/Storage/AmazonEFSMountTarget.png)

## 6.2. How It Works

- When an EC2 instance mounts EFS **through a target in the same AZ**, the network traffic:
  - Stays within the **local VPC infrastructure**.
  - Avoids slower cross-zone paths.
- **This results in**
  - Faster file access.
  - More predictable performance.

## 6.3. High Availability Benefits

- If **one AZ fails**, EC2 instances in other AZs:
  - Continue to access EFS **through their own local mount targets**.
  - Maintain application uptime without manual reconfiguration.

## 6.4. AWS Recommendation

- **Always** create a mount target in every AZ where EC2 instances require EFS access.
- **This approach delivers**
  - **Optimal performance** (low latency, high throughput).
  - **Resilience** against AZ outages.

# 7. Cross-Account Amazon EFS Access for AWS Lambda

- **Approach**
  - Use **Amazon EFS resource policies** to grant **cross-account access** to the central AWS account.
  - Attach the **EFS mount target** to a **shared VPC** or a **VPC peered** with the Lambda function's VPC.
  - Configure the Lambda function to **mount the file system via an EFS Access Point**.
- **Benefits**
  - **Native AWS support** for secure cross-account access without workarounds.
  - EFS **access points** provide fine-grained permissions and simplified mounting.
  - **No need for data duplication** or proxying through intermediate services.
  - Scales automatically with Lambda and supports **secure, performant** file access.
  - Most **cost-effective** and **operationally efficient** method for this scenario.
