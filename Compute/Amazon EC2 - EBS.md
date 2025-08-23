# Amazon EC2 EBS - Elastic Block Storage <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. EBS Volume](#2-ebs-volume)
- [3. Delete on Termination attribute](#3-delete-on-termination-attribute)
- [4. Snapshots](#4-snapshots)
  - [4.1. Features](#41-features)
- [5. EC2 Instance Store](#5-ec2-instance-store)
- [6. EBS Volume Types](#6-ebs-volume-types)
  - [6.1. EBS Volume Types Use cases](#61-ebs-volume-types-use-cases)
    - [6.1.1. General Purpose SSD](#611-general-purpose-ssd)
    - [6.1.2. Provisioned IOPS (PIOPS) SSD](#612-provisioned-iops-piops-ssd)
    - [6.1.3. Hard Disk Drives (HDD)](#613-hard-disk-drives-hdd)
    - [6.1.4. EBS Multi-Attach - io1/io2 family](#614-ebs-multi-attach---io1io2-family)
- [7. EBS Encryption](#7-ebs-encryption)
  - [7.1. Encryption: encrypt an unencrypted EBS volume](#71-encryption-encrypt-an-unencrypted-ebs-volume)
- [8. Shared Responsibility Model for EC2 Storage](#8-shared-responsibility-model-for-ec2-storage)

# 1. Introduction

- An **EBS (Elastic Block Store) Volume** is a **network drive** we can attach to your instances while they run.
- It allows your instances to persist data, even after their termination.
- **They can only be mounted to one instance at a time (at the CCP level).**
- They are bound to a specific **availability zone (AZ)**.
- Analogy: Think of them as a "network USB stick".
- **Free tier:** 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month.

# 2. EBS Volume

- It's a network drive (i.e. not a physical drive).
  - It uses the network to communicate the instance, which means there might be a bit of latency.
  - It can be detached from an EC2 instance and attached to another one quickly.
- It's locked to an Availability Zone (AZ).
  - An EBS Volume in `sa-east-1a` cannot be attached to `sa-east-1b`.
  - To move a volume across, we first need to **snapshot** it.
- Have a provisioned capacity (size in GBs, and IOPS).
  - We get billed for all the provisioned capacity.
  - We can increase the capacity of the drive over time.

![Amazon EC2 - EBS Volume diagram](/Images/Compute/AmazonEC2EBSVolumeDiagram.png)

# 3. Delete on Termination attribute

- Controls the EBS behaviour when an EC2 instance terminates.
  - By default, the root EBS volume is deleted (attribute enabled).
  - By default, any other attached EBS volume is not deleted (attribute disabled).
- This can be controlled by the AWS console / AWS CLI.
- **Use case: Preserve root volume when instance is terminated.**
- We can **prevent automatic deletion** by setting the `DeleteOnTermination` attribute to `false`.

# 4. Snapshots

- Make a backup (snapshot) of your EBS volume at a point in time.
- Not necessary to detach volume to do snapshot, **but recommended**.
- Can copy snapshots across AZ or Region.

![Amazon EC2 - EBS Snapshot diagram](/Images/Compute/AmazonEC2EBSSnapshotDiagram.png)

## 4.1. Features

- **EBS Snapshot Archive**
  - Move a Snapshot to an "archive tier" that is 75% cheaper.
  - Takes within 24 to 72 hours for restoring the archive.
- **Recycle Bin for EBS Snapshots**
  - Setup rules to retain deleted snapshots so we can recover them after an accidental deletion.
  - Specify retention (from 1 day to 1 year).
- **Fast Snapshot Restore (FSR)**
  - Force full initialization of snapshot to have no latency on the first use ($$$).

# 5. EC2 Instance Store

- EBS volumes are **network drives** with good but "limited" performance.
- **If we need a high-performance hardware disk, use EC2 Instance Store**.
- Better I/O performance.
- EC2 Instance Store **LOSE** their storage if they're stopped (ephemeral).
- Good for buffer / cache / scratch data / temporary content.
- Risk of data loss if hardware fails.
- Backups and Replication are your responsibility.
- They **cannot be detached or reattached** to another instance.
- When we **create an AMI** from an instance, **data on instance store volumes is not preserved**.

# 6. EBS Volume Types

- **EBS Volumes come in 6 types**
  - **gp2 / gp3 (SSD)**: General purpose SSD volume that balances price and performance for a wide variety of workloads.
  - **io1 / io2 (SSD)**: Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads.
  - **st1 (HDD):** Low cost HDD volume designed for frequently accessed, throughput-intensive workloads.
  - **sc1 (HDD):** Lowest cost HDD volume designed for less frequently accessed workloads.
- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec).
- When in doubt always consult the AWS documentation - it's good!
- **Only gp2/gp3 and io1/io2 can be used as boot volumes.**

## 6.1. EBS Volume Types Use cases

### 6.1.1. General Purpose SSD

- Cost effective storage, low-latency.
- System boot volumes, Virtual desktops, Development and test environments.
- 1 GiB - 16 TiB.
- **gp3**
  - Baseline of 3,000 IOPS and throughput of 125 MiB/s.
  - Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s independently.
- **gp2**
  - Small gp2 volumes can burst IOPS to 3,000.
  - Size of the volume and IOPS are linked, max IOPS is 16,000.
  - 3 IOPS per GB, means at 5,334 GB we are at the max IOPS.

### 6.1.2. Provisioned IOPS (PIOPS) SSD

- Critical business applications with sustained IOPS performance.
- Or applications that need more than 16,000 IOPS.
- Great for **databases workloads** (sensitive to storage perf and consistency).
- **io1/io2 (4 GiB - 16 TiB)**
  - Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other.
  - Can increase PIOPS independently from storage size.
  - io2 have more durability and more IOPS per GiB (at the same price as io1).
- **io2 Block Express (4 GiB - 64 TiB)**
  - Sub-millisecond latency.
  - Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1.
- **Supports EBS Multi-attach.**

### 6.1.3. Hard Disk Drives (HDD)

- **IMPORTANT! CANNOT** be a boot volume.
- 125 GiB to 16 TiB.
- **Throughput Optimized HDD (st1)**
  - Big Data, Data Warehouses, Log Processing.
  - **Max throughput** 500 MiB/s - max IOPS 500.
- **Cold HDD (sc1)**
  - For data that is infrequently accessed.
  - Scenarios where lowest cost is important.
  - **Max throughput** 250 MiB/s - max IOPS 250.

### 6.1.4. EBS Multi-Attach - io1/io2 family

- Attach the same EBS volume to multiple EC2 instances in the same AZ.
- Each instance has full read & write permissions to the volume.
- **Use case**
  - Achieve **higher application availability** in clustered Linux applications (ex: Teradata).
  - Applications must manage concurrent write operations.
- **Up to 16 EC2 Instances at a time.**
- Must use a file system that's cluster-aware (not XFS, EX4, etc...).

# 7. EBS Encryption

- When we create an encrypted EBS volume, we get the following:
  - Data at rest is encrypted inside the volume.
  - All the data in flight moving between the instance and the volume is encrypted.
  - All snapshots are encrypted.
  - All volumes created from the snapshot.
- Encryption and decryption are handled transparently (we have nothing to do).
- Encryption has a minimal impact on latency.
- EBS Encryption leverages keys from KMS (AES-256).
- Copying an unencrypted snapshot allows encryption.
- Snapshots of encrypted volumes are encrypted.

## 7.1. Encryption: encrypt an unencrypted EBS volume

1. Create an EBS snapshot of the volume.
2. Encrypt the EBS snapshot (using copy).
3. Create new ebs volume from the snapshot (the volume will also be encrypted).
4. Now we can attach the encrypted volume to the original instance.

# 8. Shared Responsibility Model for EC2 Storage

- **AWS**
  - Infrastructure.
  - Replication for data for EBS volumes & EFS drives.
  - Replacing faulty hardware.
  - Ensuring their employees cannot access your data.
- **You**
  - Setting up backup / snapshot procedures.
  - Setting up data encryption.
  - Responsibility of any data on the drives.
  - Understanding the risk of using EC2 Instance Store.
