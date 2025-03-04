# Amazon Aurora <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Aurora High Availability and Read Scaling](#2-aurora-high-availability-and-read-scaling)
- [3. Auto Scaling](#3-auto-scaling)
- [4. Global Aurora](#4-global-aurora)
- [5. Unplanned Failover](#5-unplanned-failover)
- [6. Features of Aurora](#6-features-of-aurora)
- [7. Aurora Security](#7-aurora-security)
- [8. Amazon Aurora Serverless](#8-amazon-aurora-serverless)
- [9. MySQL error log](#9-mysql-error-log)
- [10. Amazon Aurora vs Amazon Aurora Serverless](#10-amazon-aurora-vs-amazon-aurora-serverless)
- [11. Summary](#11-summary)

# 1. Introduction

- **Amazon Aurora is a MySQL and PostgreSQL-compatible relational database built for the cloud, that combines the performance and availability of traditional enterprise databases with the simplicity and cost-effectiveness of open source databases. It is a proprietary technology from AWS.**
- Aurora is a proprietary technology from AWS (not open sourced).
- **PostgreSQL and MySQL** are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database).
- Aurora is "AWS cloud optimized" and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS.
- Aurora storage automatically grows in increments of 10GB, up to 128 TB.
- Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10 ms replica lag).
- Failover in Aurora is instantaneous. It's HA (High Availability) native.
- Aurora costs more than RDS (20% more) - but is more efficient.

# 2. Aurora High Availability and Read Scaling

- 6 copies of your data across 3 AZ:
  - 4 copies out of 6 needed for writes.
  - 3 copies out of 6 need for reads.
  - Self healing with peer-to-peer replication.
  - Storage is striped across 100s of volumes.
- One Aurora Instance takes writes (master).
- Automated failover for master in less than 30 seconds.
- Master + up to 15 Aurora Read Replicas serve reads.
- **Support for Cross Region Replication.**

# 3. Auto Scaling

![Auto Scaling](/Images/AmazonAuroraAutoScaling.png)

# 4. Global Aurora

- **Aurora Cross Region Read Replicas**
  - Useful for disaster recovery.
  - Simple to put in place.
- **Aurora Global Database (recommended)**
  - 1 Primary Region (read / write).
  - Up to 5 secondary (read-only) regions, replication lag is less than 1 second.
  - Up to 16 Read Replicas per secondary region.
  - Helps for decreasing latency.
  - Promoting another region (for disaster recovery) has an RTO of < 1 minute.
  - **Typical cross-region replication takes less than 1 second.**

![Global Aurora](/Images/AmazonAuroraGlobal.png)

# 5. Unplanned Failover

![Unplanned Failover](/Images/AmazonAuroraUnplannedFailover.png)

# 6. Features of Aurora

- Automatic fail-over.
- Backup and Recovery.
- Isolation and security.
- Industry compliance.
- Push-button scaling.
- Automated Patching with Zero Downtime.
- Advanced Monitoring.
- Routine Maintenance.
- Backtrack: restore data at any point of time without using backups.

# 7. Aurora Security

- Similar to RDS because uses the same engines.
- Encryption at rest using KMS.
- Automated backups, snapshots and replicas are also encrypted.
- Encryption in flight using SSL (same process as MySQL or Postgres).
- Possibility to authenticate using IAM token (same method as RDS).
- You are responsible for protecting the instance with security groups.
- You can't SSH.

# 8. Amazon Aurora Serverless

- The Aurora Serverless is an auto-scaling, on-demand configuration designed for Amazon Aurora RDS.
- It can start, shut and scale capacity automatically, according to individual application's requirements.
- This service allows you to run cloud-powered databases without the need to manage database capacity.

# 9. MySQL error log

- You can monitor the MySQL logs directly through the Amazon RDS console, Amazon RDS API, AWS CLI, or AWS SDKs.
- You can also access MySQL logs by directing the logs to a database table in the main database and querying that table.
- You can use the mysqlbinlog utility to download a binary log.

# 10. Amazon Aurora vs Amazon Aurora Serverless

| Amazon Aurora Highlights         | Amazon Aurora Serverless Highlights                       |
| -------------------------------- | --------------------------------------------------------- |
| MySQL Compatibility              | API Integration                                           |
| Better performance               | Compatible with cloud functions for Google, Azure and IBM |
| Easy read scalability            | Lower charges                                             |
| High speed                       | Auto-scaling                                              |
| Low latency read replica         | Openwhisk                                                 |
| Higher IOPS cost                 |                                                           |
| Better cost-to-performance ratio |                                                           |

# 11. Summary

- Compatible API for PostgreSQL / MySQL, separation of storage and compute.
- Storage: data is stored in 6 replicas, across 3 AZ - highly available, self-healing, auto-scaling.
- Compute: Cluster of DB Instance across multiple AZ, auto-scaling of Read Replicas.
- Cluster: Custom endpoints for writer and reader DB instances.
- Same security / monitoring / maintenance features as RDS.
- Know the backup & restore options for Aurora.
- **Aurora Serverless** - For unpredictable / intermittent workloads, no capacity planning.
- **Aurora Global:** Up to 16 DB Read Instances in each region, < 1 second storage replication.
- **Aurora Machine Learning:** Perform ML using SageMaker & Comprehend on Aurora.
- **Aurora Database Cloning:** New cluster from existing one, faster than restoring a snapshot.
- **Use case:** same as RDS, but with less maintenance / more flexibility / more performance / more features.
