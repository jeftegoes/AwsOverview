# AWS Snow Family <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. AWS Snow Family](#1-aws-snow-family)
- [2. Data Migrations with AWS Snow Family](#2-data-migrations-with-aws-snow-family)
  - [2.1. AWS Snowcone](#21-aws-snowcone)
  - [2.2. Snowball Edge (for data transfers)](#22-snowball-edge-for-data-transfers)
  - [2.3. AWS Snowmobile](#23-aws-snowmobile)
- [3. Snow Family - Usage Process](#3-snow-family---usage-process)
- [4. What is Edge Computing?](#4-what-is-edge-computing)
  - [4.1. Snow Family - Edge Computing](#41-snow-family---edge-computing)
- [5. Solution Architecture: Snowball into Glacier](#5-solution-architecture-snowball-into-glacier)
- [6. AWS OpsHub](#6-aws-opshub)

# 1. AWS Snow Family

- Highly-secure, portable devices to **collect and process data at the edge**, **and migrate data into and out of AWS**.
- Helps migrate up to Petabytes of data.
- **Data migration**
  - Snowcon.
  - Snowball Edge.
  - Snowmobile.
- **Edge computing**
  - Snowcone.
  - Snowball Edge.

# 2. Data Migrations with AWS Snow Family

- **Challenges**
  - Limited connectivity.
  - Limited bandwidth.
  - High network cost.
  - Shared bandwidth (can't maximize the line).
  - Connection stability.
- **Time to Transfer**
  | | 100 Mbps | 1Gbps | 10Gbps |
  | ------ | -------- | -------- | -------- |
  | 10 TB | 12 days | 30 hours | 3 hours |
  | 100 TB | 124 days | 12 days | 30 hours |
  | 1 PB | 3 years | 124 days | 12 days |
- AWS Snow Family: offline devices to perform data migrations.
  - If it takes more than a week to transfer over the network, use Snowball devices!

## 2.1. AWS Snowcone

- Small, portable computing, anywhere, rugged & secure, withstands harsh environments.
- Light (4.5 pounds, 2.1 kg).
- Device used for edge computing, storage, and data transfer.
- 8 TBs of usable storage.
- Use Snowcone where Snowball does not fit (space-constrained environment).
- Must provide your own battery / cables.
- Can be sent back to AWS offline, or connect it to internet and use AWS DataSync to send data.
  ![AWS Snowcone](/Images/Storage/AWSSnowcone.jpg)

## 2.2. Snowball Edge (for data transfers)

- Physical data transport solution: move TBs or PBs of data in or out of AWS.
- Alternative to moving data over the network (and paying network fees).
- Pay per data transfer job.
- Provide block storage and Amazon S3-compatible object storage.
- Snowball Edge Storage Optimized.
  - 80 TB of HDD capacity for block volume and S3 compatible object storage.
- Snowball Edge Compute Optimized.
  - 42 TB of HDD capacity for block volume and S3 compatible object storage.
- **Use cases:** Large data cloud migrations, DC decommission, disaster recovery.
- **Snowball Edge is best-suited to move petabytes of data and offers computing capabilities. Be careful, it's recommended to use a fleet of Snowballs to move less than 10PBs of data. Over this quantity, it's better-suited to use Snowmobile.**
  ![AWS Snowball Edge](/Images/Storage/AWSSnowballEdge.jpg)

## 2.3. AWS Snowmobile

- Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs).
- Each Snowmobile has 100 PB of capacity (use multiple in parallel).
- High security: temperature controlled, GPS, 24/7 video surveillance.
- Better than Snowball if you transfer more than 10 PB.

# 3. Snow Family - Usage Process

1. Request Snowball devices from the AWS console for delivery.
2. Install the snowball client / AWS OpsHub on your servers.
3. Connect the snowball to your servers and copy files using the client.
4. Ship back the device when you're done (goes to the right AWS facility).
5. Data will be loaded into an S3 bucket.
6. Snowball is completely wiped.

# 4. What is Edge Computing?

- Process data while it's being created on an edge location.
  - A truck on the road, a ship on the sea, a mining station underground...
- **These locations may have**
  - Limited / no internet access.
  - Limited / no easy access to computing power.
- We setup a **Snowball Edge** / **Snowcone** device to do edge computing.
- **Use cases of Edge Computing**
  - Preprocess data.
  - Machine learning at the edge.
  - Transcoding media streams.
- Eventually (if need be) we can ship back the device to AWS (for transferring data for example).

## 4.1. Snow Family - Edge Computing

- **Snowcone (smaller)**
  - 2 CPUs, 4 GB of memory, wired or wireless access.
  - USB-C power using a cord or the optional battery.
- **Snowball Edge - Compute Optimized**
  - 52 vCPUs, 208 GiB of RAM.
  - Optional GPU (useful for video processing or machine learning).
  - 42 TB usable storage.
- **Snowball Edge - Storage Optimized**
  - Up to 40 vCPUs, 80 GiB of RAM.
  - Object storage clustering available.
- All: Can run EC2 Instances & AWS Lambda functions (using AWS IoT Greengrass).
- Long-term deployment options: 1 and 3 years discounted pricing.

# 5. Solution Architecture: Snowball into Glacier

- **Snowball cannot import to Glacier directly.**
- You must use Amazon S3 first, in combination with an S3 lifecycle policy.

# 6. AWS OpsHub

- Historically, to use Snow Family devices, you needed a CLI (Command Line Interface tool).
- Today, you can use AWS OpsHub (a software you install on your computer / laptop) to manage your Snow Family Device.
  - Unlocking and configuring single or clustered devices.
  - Transferring files.
  - Launching and managing instances running on Snow Family Devices.
- Monitor device metrics (storage capacity, active instances on your device).
- Launch compatible AWS services on your devices (ex: Amazon EC2 instances, AWS DataSync, Network File System (NFS)).
