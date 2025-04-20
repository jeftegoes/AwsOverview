# Amazon EC2 - Elastic Compute Cloud<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Sizing \& configuration options](#2-sizing--configuration-options)
- [3. User Data](#3-user-data)
- [4. EC2 Instance Types](#4-ec2-instance-types)
  - [4.1. General Purpose - m](#41-general-purpose---m)
  - [4.2. Compute Optimized - c](#42-compute-optimized---c)
  - [4.3. Memory Optimized - r](#43-memory-optimized---r)
  - [4.4. Storage Optimized - i](#44-storage-optimized---i)
  - [4.5. Accelerated Computing - p](#45-accelerated-computing---p)
  - [4.6. EC2 Instance Types: example](#46-ec2-instance-types-example)
    - [4.6.1. Burstable performance instances](#461-burstable-performance-instances)
- [5. Security Groups](#5-security-groups)
  - [5.1. Security Groups Good to know](#51-security-groups-good-to-know)
  - [5.2. To access S3 into VPC](#52-to-access-s3-into-vpc)
  - [5.3. To acess S3 with IAM Roles](#53-to-acess-s3-with-iam-roles)
- [6. Classic Ports to know](#6-classic-ports-to-know)
- [7. How to SSH into your EC2 Instance](#7-how-to-ssh-into-your-ec2-instance)
- [8. Elastic IPs](#8-elastic-ips)
  - [8.1. Private vs Public IP (IPv4)](#81-private-vs-public-ip-ipv4)
- [9. Placement Groups (Topologies)](#9-placement-groups-topologies)
  - [9.1. Cluster](#91-cluster)
  - [9.2. Spread](#92-spread)
  - [9.3. Partition](#93-partition)
- [10. Elastic Network Interfaces (ENI)](#10-elastic-network-interfaces-eni)
- [11. EC2 Hibernate](#11-ec2-hibernate)
  - [11.1. Introducing EC2 Hibernate](#111-introducing-ec2-hibernate)
  - [11.2. Good to know](#112-good-to-know)
- [12. EC2 Instance Connect](#12-ec2-instance-connect)
- [13. EC2 Instances Purchasing Options](#13-ec2-instances-purchasing-options)
  - [13.1. On Demand](#131-on-demand)
  - [13.2. Reserved Instances](#132-reserved-instances)
  - [13.3. Savings Plans](#133-savings-plans)
  - [13.4. Spot Instances](#134-spot-instances)
    - [13.4.1. Requests](#1341-requests)
    - [13.4.2. Spot Fleets](#1342-spot-fleets)
  - [13.5. Dedicated Hosts](#135-dedicated-hosts)
  - [13.6. Dedicated Instances](#136-dedicated-instances)
  - [13.7. Capacity Reservations](#137-capacity-reservations)
  - [13.8. Which purchase option is better? (Correlation with Hotel)](#138-which-purchase-option-is-better-correlation-with-hotel)
  - [13.9. AWS License Manager](#139-aws-license-manager)
  - [13.10. Shared Responsibility Model for EC2](#1310-shared-responsibility-model-for-ec2)
- [14. VM Import/Export](#14-vm-importexport)
- [15. AMI](#15-ami)
  - [15.1. AMI Process (from an EC2 instance)](#151-ami-process-from-an-ec2-instance)
  - [15.2. EC2 Image Builder](#152-ec2-image-builder)
  - [15.3. Instance Migration between AZ](#153-instance-migration-between-az)
  - [15.4. Cross-Account AMI Sharing](#154-cross-account-ami-sharing)
    - [15.4.1. AMI Sharing with KMS Encryption](#1541-ami-sharing-with-kms-encryption)
  - [15.5. Cross-Account AMI Copy](#155-cross-account-ami-copy)
    - [15.5.1. AMI Copy with KMS Encryption](#1551-ami-copy-with-kms-encryption)
- [16. Instance Scheduler on AWS](#16-instance-scheduler-on-aws)

# 1. Introduction

- EC2 is one of the most popular of AWS' offering.
- EC2 = Elastic Compute Cloud = Infrastructure as a Service.
- It mainly consists in the capability of:
  - Renting virtual machines (EC2).
  - Storing data on virtual drives (EBS).
  - Distributing load across machines (ELB).
  - Scaling the services using an auto-scaling group (ASG).
- Knowing EC2 is fundamental to understand how the Cloud works.

# 2. Sizing & configuration options

- Operating System **(OS)**: Linux, Windows or Mac OS.
- How much compute power & cores **(CPU)**.
- How much random-access memory **(RAM)**.
- How much storage space:
  - Network-attached **(EBS & EFS)**.
  - Hardware **(EC2 Instance Store)**.
- **Network card:** Speed of the card, Public IP address.
- **Firewall rules: Security group.**
- Bootstrap script (configure at first launch): EC2 User Data.

# 3. User Data

- It is possible to bootstrap our instances using an **EC2 User Data** script.
- **Bootstrapping** means launching commands when a machine starts.
- That script is **only run once** at the instance **first start**.
- **EC2 User Data** is used to automate boot tasks such as:
  - Installing updates.
  - Installing software.
  - Downloading common files from the internet.
  - Anything you can think of.
- The **EC2 User Data** Script runs with the **root user**.

# 4. EC2 Instance Types

- You can use different types of EC2 instances that are optimised for different use cases (https://aws.amazon.com/ec2/instance-types/).
- AWS has the following naming convention:
  - **m**_5_.`2xlarge`
    - **m:** instance class.
    - _5:_ generation (AWS improves them over time).
    - `2xlarge:` size within the instance class.
- **General types**
  - General Purpose - m
  - Compute Optimized - c
  - Memory Optimized - r
  - Accelerated Computing - p
  - Storage Optimized - i
  - HPC Optimized - hpc

## 4.1. General Purpose - m

- Great for a diversity of workloads such as web servers or code repositories.
- Balance between:
  - Compute.
  - Memory.
  - Networking.

## 4.2. Compute Optimized - c

- **AWS Compute Optimizer recommends optimal AWS resources for your workloads to reduce costs and improve performance by using machine learning to analyze historical utilization metrics.**
- Great for compute-intensive tasks that require high performance processors:
  - Batch processing workloads.
  - Media transcoding.
  - High performance web servers.
  - High performance computing (HPC).
  - Scientific modeling & machine learning.
  - Dedicated gaming servers.

## 4.3. Memory Optimized - r

- Fast performance for workloads that process large data sets in memory.
- **Use cases**
  - High performance, relational/non-relational databases.
  - Distributed web scale cache stores.
  - In-memory databases optimized for BI (business intelligence).
  - Applications performing real-time processing of big unstructured data.

## 4.4. Storage Optimized - i

- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage.
- **Use cases**
  - High frequency online transaction processing (OLTP) systems.
  - Relational & NoSQL databases.
  - Cache for in-memory databases (for example, Redis).
  - Data warehousing applications.
  - Distributed file systems.

## 4.5. Accelerated Computing - p

- Accelerated computing instances use hardware accelerators, or co-processors, to perform functions, such:
  - Floating point number calculations.
  - Graphics processing.
  - Data pattern matching.
  - Scientific modeling & machine learning.
- More efficiently than is possible in software running on CPUs.

## 4.6. EC2 Instance Types: example

| Instance    | vCPU | Mem (GiB) | Storage          | Network performance | EBS Banwidth (Mbps) |
| ----------- | ---- | --------- | ---------------- | ------------------- | ------------------- |
| t2.micro    | 1    | 1         | EBS-Only         | Low to Moderate     |                     |
| t2.xlarge   | 4    | 16        | EBS-Only         | Moderate            |                     |
| c5d.4xlarge | 16   | 32        | 1 x 400 NVMe SSD | Up to 10 Gbps       | 4,750               |
| r5.16xlarge | 64   | 512       | EBS-Only         | 20 Gbps             | 13,600              |
| m5.8xlarge  | 32   | 128       | EBS-Only         | 10 Gbps             | 6,800               |

- **t2.micro is part of the AWS free tier (up to 750 hours per month).**
- https://instances.vantage.sh/

### 4.6.1. Burstable performance instances

- T3, T3a, and T2 instances, are designed to provide a baseline level of CPU performance with the ability to burst to a higher level when required by your workload.
  - Burstable performance instances are the only instance types that use credits for CPU usage.

# 5. Security Groups

- Security Groups are the fundamental of network security in AWS.
- They control how traffic is allowed into or out of our EC2 Instances.
  ![Security Group diagram](/Images/SecurityGroupBasicDiagram.png)
- Security groups only contain **allow** rules.
- Security groups rules can reference by IP or by security group.
- Security groups are acting as a **firewall** on EC2 instances.
- They regulate:
  - Access to Ports.
  - Authorised IP ranges - IPv4 and IPv6.
  - Control of inbound network (from other to the instance).
  - Control of outbound network (from the instance to other).
    ![Security Group Deeper Dive Diagram](/Images/SecurityGroupDeeperDiveDiagram.png)

## 5.1. Security Groups Good to know

- Can be attached to multiple instances.
- Locked down to a region / VPC combination.
- Does live "outside" the EC2 - if traffic is blocked the EC2 instance won't see it.
- **It's good to maintain one separate security group for SSH access.**
- **Troubleshoot**
  - If your application is not accessible (time out), then it's a security group issue.
  - If your application gives a "connection refused" error, then it's an application error or it's not launched.
- All inbound traffic is **blocked** by default.
- All outbound traffic is **authorised** by default.

## 5.2. To access S3 into VPC

![EC2 Access S3](/Images/AWSEC2AccessS3.png)

- In this scenario, **S3 is not part of your VPC**, unlike your EC2 instances, EBS volumes, ELBs, and other services that typically reside within your private network.
- An EC2 instance needs to have access to the Internet, via the Internet Gateway or a NAT Instance/Gateway in order to access S3.
- Alternatively, you can also create a **VPC endpoint** so your private subnet would be able to connect to S3.
- [Amazon VPC](/Networking%20&%20Content%20Delivery/Amazon%20VPC.md)

## 5.3. To acess S3 with IAM Roles

![Scenario IAM Role and EC2 Instance Profile](/Images/AWSCrendentialScenario.png)

# 6. Classic Ports to know

- 22 = SSH (Secure Shell) - Log into a Linux instance.
- 21 = FTP (File Transfer Protocol) - Upload files into a file share.
- 22 = SFTP (Secure File Transfer Protocol) - Upload files using SSH.
- 80 = HTTP - access unsecured websites.
- 443 = HTTPS - access secured websites.
- 3389 = RDP (Remote Desktop Protocol) - Log into a Windows instance.

# 7. How to SSH into your EC2 Instance

- Windows:

  - Configure pem file
    ![Permission Propertie Aws PemFile](/Images/PermissionPropertieAwsPemFile.png)
  - Command
    - ssh -i D:\MY_PENFILE.pem ec2-user@PUBLIC_IP.

- Generate a public SSH key from a private SSH key. Then, import the key into each of your AWS Regions.
- Here is the correct way of reusing SSH keys in your AWS Regions:
  1. Generate a public SSH key (.pub) file from the private SSH key (.pem) file.
  2. Set the AWS Region you wish to import to.
  3. Import the public SSH key into the new Region.

# 8. Elastic IPs

- When you stop and then start an EC2 instance, it can change its public IP.
- If you need to have a fixed public IP for your instance, you need an **Elastic IP**.
- An Elastic IP is a public IPv4 IP you own as long as you don't delete it.
- You can attach it to one instance at a time.
- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
- **You can only have 5 Elastic IP in your account (you can ask AWS to increase that).**
- Overall, try to avoid using Elastic IP:
  - They often reflect poor architectural decisions.
  - Instead, use a random public IP and register a DNS name to it.
  - Or, as we'll see later, use a Load Balancer and don't use a public IP.

## 8.1. Private vs Public IP (IPv4)

- By default, your EC2 machine comes with:
  - A private IP for the internal AWS Network.
  - A public IP, for the WWW.
- When we are doing SSH into our EC2 machines:
  - We can't use a private IP, because we are not in the same network.
  - We can only use the public IP.
- If your machine is stopped and then started, the public IP can change.

# 9. Placement Groups (Topologies)

- Sometimes you want control over the EC2 Instance placement strategy.
- That strategy can be defined using placement groups.
- When you create a placement group, you specify one of the following strategies for the group:
  - **Cluster:** Clusters instances into a low-latency group in a single Availability Zone.
  - **Spread:** Spreads instances across underlying hardware (max 7 instances per group per AZ).
  - **Partition:** Spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka).

## 9.1. Cluster

- **Pros:** Great network (10 Gbps bandwidth between instances with Enhanced Networking enabled - recommended).
- **Cons:** If the AZ fails, all instances fails at the same time.
- **Use case**
  - Big Data job that needs to complete fast.
  - Application that needs extremely low latency and high network throughput.
    ![Placement Group Cluster](/Images/Compute/AmazonEC2PlacementGroupCluster.png)

## 9.2. Spread

- **Pros**
  - Can span across Availability Zones (AZ).
  - Reduced risk is simultaneous failure.
  - EC2 Instances are on different physical hardware.
- **Cons**
  - Limited to 7 instances per AZ per placement group.
- **Use case**
  - Application that needs to maximize high availability.
  - Critical Applications where each instance must be isolated from failure from each other.
    ![Placement Group Spread](/Images/Compute/AmazonEC2PlacementGroupSpread.png)

## 9.3. Partition

- Up to 7 partitions per AZ.
- Can span across multiple AZs in the same region.
- Up to 100s of EC2 instances.
- The instances in a partition do not share racks with the instances in the other partitions.
- A partition failure can affect many EC2 but won't affect other partitions.
- EC2 instances get access to the partition information as metadata.
- **Use cases:** HDFS, HBase, Cassandra, Kafka.
  ![Placement Group Partition](/Images/Compute/AmazonEC2PlacementGroupPartition.png)

# 10. Elastic Network Interfaces (ENI)

- Logical component in a VPC that represents a virtual network card.
- The ENI can have the following attributes:
  - Primary private IPv4, one or more secondary IPv4.
  - One Elastic IP (IPv4) per private IPv4.
  - One Public IPv4.
  - One or more security groups.
  - A MAC address.
- You can create ENI independently and attach them on the fly (move them) on EC2 instances for failover.
- Bound to a specific availability zone (AZ).

# 11. EC2 Hibernate

- We know we can stop, terminate instances.
  - **Stop** - the data on disk (EBS) is kept intact in the next start.
  - **Terminate** - any EBS volumes (root) also set-up to be destroyed is lost.
- **On start, the following happens**
  - First start: the OS boots & the EC2 User Data script is run.
  - Following starts: the OS boots up.
  - Then your application starts, caches get warmed up, and that can take time!

## 11.1. Introducing EC2 Hibernate

- The in-memory (RAM) state is preserved
- The instance boot is much faster! (the OS is not stopped / restarted).
- Under the hood: the RAM state is written to a file in the root EBS volume.
- The root EBS volume must be encrypted.
- **Use cases:**
  - Long-running processing
  - Saving the RAM state
  - Services that take time to initialize

## 11.2. Good to know

- Supported Instance Families - C3, C4, C5, I3, M3, M4, R3, R4, T2, T3, ...
- Instance RAM Size - must be less than 150 GB.
- Instance Size - not supported for bare metal instances.
- AMI - Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows...
- Root Volume - must be EBS, encrypted, not instance store, and large.
- Available for On-Demand, Reserved and Spot Instances.
- **An instance can NOT be hibernated more than 60 days.**

# 12. EC2 Instance Connect

- Connect to your EC2 instance within your browser.
- No need to use your key file that was downloaded.
- The "magic" is that a temporary key is uploaded onto EC2 by AWS.
- **Works only out-of-the-box with Amazon Linux 2.**
- Need to make sure the port 22 is still opened!

# 13. EC2 Instances Purchasing Options

- **On-Demand Instances:** Short workload, predictable pricing, pay by second.
- **Reserved (1 & 3 years)**
  - **Reserved Instances:** Long workloads.
  - **Convertible Reserved Instances:** Long workloads with flexible instances.
  - **Scheduled Reserved Instances:** Example - every Thursday between 3 and 6 pm.
- **Savings Plans (1 & 3 years):** Commitment to an amount of usage, long workload.
- **Spot Instances:** Short workloads, cheap, can lose instances (less reliable).
- **Dedicated Hosts:** Book an entire physical server, control instance placement.
- **Dedicated Instances:** No other customers will share your hardware.
- **Capacity Reservations:** Reserve capacity in a specific AZ for any duration.

## 13.1. On Demand

- Pay for what you use:
  - Linux or Windows - billing per second, after the first minute.
  - All other operating systems - billing per hour.
- Has the highest cost but no upfront payment.
- No long-term commitment.
- Recommended for **short-term** and **un-interrupted workloads**, where you can't predict how the application will behave.

## 13.2. Reserved Instances

- Up to **72%** discount compared to On-demand.
- Your reserve a specific instance attributes **(Instance, type, region, tenancy, OS)**.
- Reservation period: **1 year** = + discount | **3 years** = +++ discount.
- Payment options: **No Upfront** = + | **partial upfront** = ++ | **All upfront** = +++ discount.
- **Reserved Instance's Scope:**
  | | Regional Reserved Instances | Zonal Reserved Instances |
  | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
  | Ability to reserve capacity | A regional Reserved Instance does not reserve capacity. | A zonal Reserved Instance reserves capacity in the specified Availability Zone. |
  | Availability Zone flexibility | The Reserved Instance discount applies to instance usage in any Availability Zone in the specified Region. | No Availability Zone flexibility: the Reserved Instance discount applies to instance usage in the specified Availability Zone only. |
  | Instance size flexibility | The Reserved Instance discount applies to instance usage within the instance family, regardless of size. Only supported on Amazon Linux/Unix Reserved Instances with default tenancy. For more information, see Instance size flexibility determined by normalization factor. | No instance size flexibility: the Reserved Instance discount applies to instance usage for the specified instance type and size only. |
  | Queuing a purchase | You can queue purchases for regional Reserved Instances. | You can't queue purchases for zonal Reserved Instances. |

- Recommended for steady-state usage applications (think database).
- You can buy and sell in the Reserved Instance Marketplace.
- **Convertible Reserved Instance**
  - Can change the EC2 instance type, instance family, OS, scope and tenancy.
  - Up to **66%** discount.
- Scheduled Reserved Instances
  - Launch within time window you reserve.
  - When you require a fraction of day / week / month.
  - Commitment for 1 year only.

## 13.3. Savings Plans

- Get a discount based on long-term usage (up to 72% - same as RIs).
- Commit to a certain type of usage ($10/hour for 1 or 3 years).
- Usage beyond EC2 Savings Plans is billed at the On-Demand price.
- Locked to a specific instance family & AWS region (e.g., M5 in us-east-1).
- Flexible across:
  - Instance Size (e.g., m5.xlarge, m5.2xlarge).
  - OS (e.g., Linux, Windows).
  - Tenancy (Host, Dedicated, Default).

## 13.4. Spot Instances

- Can get a **discount of up to 90%** compared to On-demand.
- Instances that you can "lose" at any point of time if your max price is less than the current spot price.
- The **MOST cost-efficient** instances in AWS.
- **Useful for workloads that are resilient to failure:**
  - Batch jobs.
  - Data analysis.
  - Image processing.
  - Any **distributed** workloads.
  - Workloads with a flexible start and end time.
- **Not suitable for critical jobs or databases.**

### 13.4.1. Requests

- Define **max spot price** and get the instance while **current spot price < max**.
  - The hourly spot price varies based on offer and capacity.
  - If the current spot price > your max price you can choose to **stop** or **terminate** your instance with a 2 minutes grace period.
- Other strategy: **Spot Block**
  - "block" spot instance during a specified time frame (1 to 6 hours) without interruptions.
  - In rare situations, the instance may be reclaimed.
- Used for batch jobs, data analysis, or workloads that are resilient to failures.

### 13.4.2. Spot Fleets

- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
- The Spot Fleet will try to meet the target capacity with price constraints.
  - Define possible launch pools: instance type (m5.large), OS, Availability Zone.
  - Can have multiple launch pools, so that the fleet can choose.
  - Spot Fleet stops launching instances when reaching capacity or max cost.
- **Strategies to allocate Spot Instances**
  - `lowestPrice`: from the pool with the lowest price (cost optimization, short workload).
  - `diversified`: distributed across all pools (great for availability, long workloads).
  - `capacityOptimized`: pool with the optimal capacity for the number of instances.
  - `priceCapacityOptimized` (recommended): pools with highest capacity available, then select the pool with the lowest price (best choice for most workloads).
- _Spot Fleets allow us to automatically request Spot Instances with the lowest price._

## 13.5. Dedicated Hosts

- **An Amazon EC2 Dedicated Host is a physical server with EC2 instance capacity fully dedicated to your use. Dedicated Hosts can help you address compliance requirements and reduce costs by allowing you to use your existing server-bound software licenses.**
- A physical server with EC2 instance capacity fully dedicated to your use.
- Allows you address **compliance requirements** and **use your existing server - bound software licenses** (per-socket, per-core, pe-VM software licenses).
- Purchasing Options:
  - **On-demand:** pay per second for active Dedicated Host.
  - **Reserved:** 1 or 3 years (No Upfront, Partial Upfront, All Upfront).
- **The most expensive option.**
- Useful for software that have complicated licensing model (BYOL - Bring Your Own License).
- Or for companies that have strong regulatory or compliance needs.

## 13.6. Dedicated Instances

- Instances running on hardware that's dedicated to you.
- May share hardware with other instances in same account.
- No control over instance placement (can move hardware after Stop / Start).

## 13.7. Capacity Reservations

- Reserve **On-Demand** instances capacity in a specific AZ for any duration.
- You always have access to EC2 capacity when you need it.
- **No time commitment** (create/cancel anytime), **no billing discounts**.
- Combine with **Regional Reserved Instances** and **Savings Plans** to benefit from billing discounts.
- You're charged at On-Demand rate whether you run instances or not.
- Suitable for short-term, uninterrupted workloads that needs to be in a specific AZ.

## 13.8. Which purchase option is better? (Correlation with Hotel)

- **On demand:** Coming and staying in resort whenever we like, we pay the full price.
- **Reserved:** Like planning ahead and if we plan to stay for a long time, we may get a good discount.
- **Savings Plans:** Pay a certain amount per hour for certain period and stay in any room type (e.g, King, Suite, Sea View, ...).
- **Spot instances:** The hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms.
  - You can get kicked out at any time.
- **Dedicated Hosts:** We book an entire building of the resort.
- **Capacity Reservations:** You book a room for a period with full price even you don't stay in it.

## 13.9. AWS License Manager

- AWS License Manager makes it easier to manage your software licenses from vendors such as Microsoft, SAP, Oracle, and IBM across AWS and on-premises environments.
- AWS License Manager lets administrators create customized licensing rules that mirror the terms of their licensing agreements.
- Administrators can use these rules to help prevent licensing violations, such as using more licenses than an agreement stipulates.
- Rules in AWS License Manager help prevent a licensing breach by stopping the instance from launching or by notifying administrators about the infringement.
- Administrators gain control and visibility of all their licenses with the AWS License Manager dashboard and reduce the risk of non-compliance, misreporting, and additional costs due to licensing overages.
- Independent software vendors (ISVs) can also use AWS License Manager to easily distribute and track licenses.

## 13.10. Shared Responsibility Model for EC2

- AWS:
  - Infrastructure (global network security)
  - Isolation on physical hosts
  - Replacing faulty hardware
  - Compliance validation
- You:
  - Security Groups rules
  - Operating-system patches and updates
  - Software and utilities installed on the EC2 instance
  - IAM Roles assigned to EC2ASDASD\_\_& IAM user access management
  - Data security on your instance

# 14. VM Import/Export

- The VM Import/Export enables you to easily import virtual machine images from your existing environment to Amazon EC2 instances and export them back to your on-premises environment.

![VM Import/Export](/Images/AmazonEC2VMImportExport.png)

# 15. AMI

- AMI = Amazon Machine Image.
  - **Golden AMI** is an AMI that you standardize through configuration, consistent security patching, and hardening.
    - It also contains agents you approve for logging, security, performance monitoring, etc.
- AMI are a **customization** of an EC2 instance.
  - You add your own software, configuration, operating system, monitoring...
  - Faster boot / configuration time because all your software is pre-packaged.
- AMI are built for a **specific region** (and can be copied across regions).
- You can launch EC2 instances from:
  - **A Public AMI:** AWS provided.
  - **Your own AMI:** You make and maintain them yourself.
  - **An AWS Marketplace AMI:** An AMI someone else made (and potentially sells).
- **ATTENTION!** When the new AMI is copied from Region A into Region B, it automatically creates a snapshot in Region B because AMIs are based on the underlying snapshots.

## 15.1. AMI Process (from an EC2 instance)

1. Start an EC2 instance and customize it.
2. Stop the instance (for data integrity).
3. Build an AMI - this will also create EBS snapshots.
4. Launch instances from other AMIs.

## 15.2. EC2 Image Builder

- Used to automate the creation of Virtual Machines or container images.
- Automate the creation, maintain, validate and test **EC2 AMIs**.
- Can be run on a schedule (weekly, whenever packages are updated, etc...).
- Free service (only pay for the underlying resources).
- **Steps**
  1. EC2 Image Builder
     1. Create
  2. Builder EC2 Instance
     1. Create
  3. New AMI
  4. Test EC2 Instance

## 15.3. Instance Migration between AZ

- Remember **between AZ**.
  ![EC2 Instance Migration between AZ](/Images/AmazonEC2MigrationBetweenAZ.png)

## 15.4. Cross-Account AMI Sharing

- You can share an AMI with another AWS account.
- Sharing an AMI does **not affect the ownership** of the AMI.
- You can only share AMIs that have:
  1. Unencrypted volumes.
  2. Volumes that are encrypted with a customer managed key.
- If you share an AMI with encrypted volumes,** you must also share any customer managed keys used to encrypt them**.

![Cross-Account AMI Sharing](/Images/AmazonEC2CrossAccountAMISharing.png)

### 15.4.1. AMI Sharing with KMS Encryption

![AMI Sharing with KMS Encryption](/Images/AmazonEC2AMISharingWithKMSEncryption.png)

## 15.5. Cross-Account AMI Copy

- If you copy an AMI that has been shared with your account, you are the owner of the target AMI in your account.
- The owner of the source AMI must grant you read permissions for the storage that backs the **AMI (EBS Snapshot)**.
- If the shared AMI has encrypted snapshots, the owner must share the key or keys with you as well.
- Can encrypt the AMI with your own CMK while copying.

![Cross-Account AMI Copy](/Images/AmazonEC2CrossAccountAMICopy.png)

### 15.5.1. AMI Copy with KMS Encryption

- Cross-Region / Cross-Account Encrypted AMI Copy
  ![AMI Copy with KMS Encryption](/Images/AmazonEC2CrossAccountAMICopyWithKMSEncryption.png)

# 16. Instance Scheduler on AWS

- AWS solution deployed through [CloudFormation](/Management%20&%20Governance/AWS%20CloudFormation.md) (not a service).
- Automatically start/stop your AWS services to reduce costs (up to 70%).
- Example: stop company's EC2 instances outside business hours.
- Supports EC2 instances, EC2 Auto Scaling Groups, and [RDS](/Database/Amazon%20RDS.md) instances.
- Schedules are managed in a DynamoDB table - Uses resources' tags and Lambda to stop/start instances.
- Supports cross-account and cross-region resources.
- https://aws.amazon.com/solutions/implementations/instance-scheduler-on-aws
