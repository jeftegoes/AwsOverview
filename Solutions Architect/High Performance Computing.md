# High Performance Computing (HPC) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. High Performance Computing (HPC)](#1-high-performance-computing-hpc)
  - [1.1. Data Management \& Transfer](#11-data-management--transfer)
  - [1.2. Compute and Networking](#12-compute-and-networking)
  - [1.3. Compute and Networking](#13-compute-and-networking)
  - [1.4. Storage](#14-storage)
  - [1.5. Automation and Orchestration](#15-automation-and-orchestration)
- [2. Creating a highly available EC2 instance](#2-creating-a-highly-available-ec2-instance)
  - [2.1. With ASG](#21-with-asg)
  - [2.2. With ASG + EBS](#22-with-asg--ebs)

# 1. High Performance Computing (HPC)

- The cloud is the perfect place to perform HPC.
- We can create a very high number of resources in no time.
- We can speed up time to results by adding more resources.
- We can pay only for the systems you have used.
- Perform genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving.
- Which services help perform HPC?

## 1.1. Data Management & Transfer

- **AWS Direct Connect**
  - Move GB/s of data to the cloud, over a private secure network.
- **Snowball & Snowmobile**
  - Move PB of data to the cloud.
- **AWS DataSync**
  - Move large amount of data between on-premises and S3, EFS, FSx for Windows.

## 1.2. Compute and Networking

- **EC2 Instances**
  - CPU optimized, GPU optimized.
  - Spot Instances / Spot Fleets for cost savings + Auto Scaling.
- EC2 Placement Groups: **Cluster** for good network performance.

## 1.3. Compute and Networking

- **EC2 Enhanced Networking (SR-IOV)**
  - Higher bandwidth, higher PPS (packet per second), lower latency.
  - **Option 1:** **Elastic Network Adapter (ENA)** up to 100 Gbps.
  - **Option 2:** Intel 82599 VF up to 10 Gbps - LEGACY.
- **Elastic Fabric Adapter (EFA)**
  - Improved ENA for HPC, only works for Linux.
  - Great for inter-node communications, tightly coupled workloads.
  - Leverages Message Passing Interface (MPI) standard.
  - Bypasses the underlying Linux OS to provide low-latency, reliable transport.

## 1.4. Storage

- **Instance-attached storage**
  - **EBS:** Scale up to 256,000 IOPS with io2 Block Express.
  - **Instance Store:** Scale to millions of IOPS, linked to EC2 instance, low latency.
- **Network storage**
  - **Amazon S3:** Large blob, not a file system.
  - **Amazon EFS:** Scale IOPS based on total size, or use provisioned IOPS.
  - **Amazon FSx for Lustre**
    - HPC optimized distributed file system, millions of IOPS.
    - Backed by S3.

## 1.5. Automation and Orchestration

- **AWS Batch**
  - **AWS Batch** supports multi-node parallel jobs, which enables you to run single jobs that span multiple **EC2** instances.
- Easily schedule jobs and launch EC2 instances accordingly.
- **AWS ParallelCluster**
  - Open-source cluster management tool to deploy HPC on AWS.
  - Configure with text files.
  - Automate creation of VPC, Subnet, cluster type and instance types.
  - **Ability to enable EFA on the cluster (improves network performance).**

# 2. Creating a highly available EC2 instance

![Highly available EC2 instance](/Images/HighlyAvailableEC2Instance.png)

## 2.1. With ASG

![Highly available EC2 instance with ASG ](/Images/HighlyAvailableEC2InstanceASG.png)

## 2.2. With ASG + EBS

TODO: DIAGRAM
