# Amazon FSx <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Amazon FSx](#2-amazon-fsx)
- [3. Amazon FSx for Windows (File Server)](#3-amazon-fsx-for-windows-file-server)
- [4. Amazon FSx for Lustre](#4-amazon-fsx-for-lustre)
  - [4.1. Hot \& Cold Data Handling](#41-hot--cold-data-handling)
- [5. FSx Lustre - File System Deployment Options](#5-fsx-lustre---file-system-deployment-options)
- [6. Amazon FSx for NetApp ONTAP](#6-amazon-fsx-for-netapp-ontap)
- [7. Amazon FSx for OpenZFS](#7-amazon-fsx-for-openzfs)

# 1. Introduction

- Amazon FSx for Lustre makes it easy and cost-effective to launch and run the world's most popular high-performance file system.
- It is used for workloads such as machine learning, high-performance computing (HPC), video processing, and financial modeling.
- The open-source Lustre file system is designed for applications that require fast storage - where we want our storage to keep up with our compute.
- FSx for Lustre integrates with Amazon S3, making it easy to process data sets with the Lustre file system.
  - When linked to an S3 bucket, an FSx for Lustre file system transparently presents S3 objects as files and allows we to write changed data back to S3.

# 2. Amazon FSx

- Launch 3rd party high-performance file systems on AWS.
- Fully managed service.
- **Products**
  - FSx for Lustre.
  - FSx for Windows File Server.
  - FSx for NetApp ONTAP.
  - FSx for OpenZFS.

# 3. Amazon FSx for Windows (File Server)

- **FSx for Windows** is a fully managed **Windows** file system share drive.
- Supports SMB (Server Message Block) protocol & Windows NTFS.
- Microsoft Active Directory integration, ACLs, user quotas.
- **Can be mounted on Linux EC2 instances.**
- Supports **Microsoft's Distributed File System (DFS) Namespaces** (group files across multiple FS).
- **Replication**: Uses **Distributed File System Replication (DFSR)** for multi-master folder synchronization across servers.
- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data.
- **Storage Options**
  - **SSD:** Latency sensitive workloads (databases, media processing, data analytics, ...).
  - **HDD:** Broad spectrum of workloads (home directory, CMS, ...).
- Can be accessed from our on-premises infrastructure (VPN or Direct Connect).
- Can be configured to be Multi-AZ (high availability).
- Data is backed-up daily to S3.

# 4. Amazon FSx for Lustre

- Lustre is a type of parallel distributed file system, for large-scale computing.
- The name Lustre is derived from "Linux" and "cluster.
- Machine Learning, **High Performance Computing (HPC)**.
- Video Processing, Financial Modeling, Electronic Design Automation.
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies.
- **Storage Options**
  - **SSD:** Low-latency, IOPS intensive workloads, small & random file operations.
  - **HDD:** Throughput-intensive workloads, large & sequential file operations.
- **Seamless integration with S3**
  - Can "read S3" as a file system (through FSx).
  - Can write the output of the computations back to S3 (through FSx).
- **Can be used from on-premises servers (VPN or Direct Connect).**

## 4.1. Hot & Cold Data Handling

- Process hot data in a parallel and distributed way.
- Store cold data efficiently in [Amazon S3](Amazon%20S3.md).

# 5. FSx Lustre - File System Deployment Options

- **Scratch File System**
  - Temporary storage.
  - Data is not replicated (doesn't persist if file server fails).
  - High burst (6x faster, 200MBps per TiB).
  - **Usage:** Short-term processing, optimize costs.
- **Persistent File System**
  - Long-term storage.
  - Data is replicated within same AZ.
  - Replace failed files within minutes.
  - **Usage:** Long-term processing, sensitive data.

# 6. Amazon FSx for NetApp ONTAP

- Managed NetApp ONTAP on AWS.
- **File System compatible with NFS, SMB, iSCSI (block storage) protocol.**
- Move workloads running on ONTAP or NAS to AWS.
- **Works with**
  - Linux.
  - Windows.
  - MacOS.
  - VMware Cloud on AWS.
  - Amazon Workspaces & AppStream 2.0.
  - Amazon EC2, ECS and EKS.
- Storage shrinks or grows automatically.
- Snapshots, replication, low-cost, compression and data de-duplication.
- **Point-in-time instantaneous cloning (helpful for testing new workloads).**
  ![Amazon FSx for NetApp ONTAP](/Images/Storage/AmazonFSxNetAppONTAP.png)
- Does **SUPPORT** Multi-AZ.

# 7. Amazon FSx for OpenZFS

- Managed OpenZFS file system on AWS.
- File System compatible with NFS (v3, v4, v4.1, v4.2).
- Move workloads running on ZFS to AWS.
- **Works with**
  - Linux.
  - Windows.
  - MacOS.
  - VMware Cloud on AWS.
  - Amazon Workspaces & AppStream 2.0.
  - Amazon EC2, ECS and EKS.
- Up to 1,000,000 IOPS with < 0.5ms latency.
- Snapshots, compression and low-cost.
- **Point-in-time instantaneous cloning (helpful for testing new workloads).**
  ![Amazon FSx for NetApp ONTAP](/Images/Storage/AmazonFSxOpenZFS.png)
