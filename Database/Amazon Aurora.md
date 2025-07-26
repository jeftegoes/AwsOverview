# Amazon Aurora <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. High Availability and Read Scaling](#2-high-availability-and-read-scaling)
- [3. Auto Scaling](#3-auto-scaling)
- [4. Custom Endpoints](#4-custom-endpoints)
- [5. Global Aurora](#5-global-aurora)
- [6. Machine Learning](#6-machine-learning)
- [7. Unplanned Failover](#7-unplanned-failover)
- [8. Additional Failover Control](#8-additional-failover-control)
- [9. Features of Aurora](#9-features-of-aurora)
- [10. Aurora Security](#10-aurora-security)
- [11. Amazon Aurora Serverless](#11-amazon-aurora-serverless)
- [12. MySQL error log](#12-mysql-error-log)
- [13. Amazon Aurora vs Amazon Aurora Serverless](#13-amazon-aurora-vs-amazon-aurora-serverless)
- [14. Backups](#14-backups)
  - [14.1. Restore options](#141-restore-options)
- [15. Database Cloning](#15-database-cloning)
- [16. Aurora Security](#16-aurora-security)
- [17. Babelfish for Aurora PostgreSQL](#17-babelfish-for-aurora-postgresql)
  - [17.1. What is Babelfish?](#171-what-is-babelfish)
  - [17.2. Key Capabilities](#172-key-capabilities)
  - [17.3. Benefits](#173-benefits)
  - [17.4. Common Use Cases](#174-common-use-cases)
  - [17.5. Limitations](#175-limitations)
- [18. Amazon Aurora I/O-Optimized Summary](#18-amazon-aurora-io-optimized-summary)
  - [18.1. Key Benefits](#181-key-benefits)
- [19. Summary](#19-summary)

# 1. Introduction

- Aurora is a proprietary technology from AWS (not open sourced).
- **PostgreSQL and MySQL** are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database).
- Aurora is "AWS cloud optimized" and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS.
- Aurora storage automatically grows in increments of 10GB, up to 128 TB.
- Aurora can have up to 15 replicas and the replication process is faster than MySQL (sub 10 ms replica lag).
- Failover in Aurora is instantaneous. It's HA (High Availability) native.
- Aurora costs more than RDS (20% more), but is more efficient.

# 2. High Availability and Read Scaling

- **6 copies of your data across 3 AZ**
  - 4 copies out of 6 needed for writes.
  - 3 copies out of 6 need for reads.
  - Self healing with peer-to-peer replication.
  - Storage is striped across 100s of volumes.
- One Aurora Instance takes writes (master).
- Automated failover for master in less than 30 seconds.
- Master + up to 15 Aurora Read Replicas serve reads.
- **Support for Cross-Region Replication.**

# 3. Auto Scaling

![Auto Scaling](/Images/Database/AmazonAuroraAutoScaling.png)

# 4. Custom Endpoints

- Define a subset of Aurora Instances as a Custom Endpoint.
- **Example:** Run analytical queries on specific replicas.
- The Reader Endpoint is generally not used after defining Custom Endpoints.

# 5. Global Aurora

- **Aurora Cross-Region Read Replicas**
  - Useful for disaster recovery.
  - Simple to put in place.
- **Aurora Global Database (recommended)**
  - 1 Primary Region (read / write).
  - Up to 5 secondary (read-only) regions, replication lag is less than 1 second.
  - Up to 16 Read Replicas per secondary region.
  - Helps for decreasing latency.
  - Promoting another region (for disaster recovery) has an RTO of < 1 minute.
  - **Typical cross-region replication takes less than 1 second.**
    ![Global Aurora](/Images/Database/AmazonAuroraGlobal.png)

# 6. Machine Learning

- Enables we to add ML-based predictions to your applications via SQL.
- Simple, optimized, and secure integration between Aurora and AWS ML services.
- Supported services.
  - [Amazon SageMaker](/Machine%20Learning/README.md) (use with any ML model).
  - [Amazon Comprehend](/Machine%20Learning/README.md) (for sentiment analysis).
- We don't need to have ML experience.
- **Use cases:** fraud detection, ads targeting, sentiment analysis, product recommendations.

# 7. Unplanned Failover

![Unplanned Failover](/Images/Database/AmazonAuroraUnplannedFailover.png)

# 8. Additional Failover Control

- Each read replica is now associated with a priority tier (0-15).
- In the event of a failover, Amazon Aurora will promote the Read Replica that has the highest priority (the lowest numbered tier).
  - If two or more Aurora Replicas share the **same priority**, then Amazon RDS promotes the replica that is **largest in size**.
  - If two or more Aurora Replicas share the **same priority and size**, then Amazon Aurora promotes an arbitrary replica in the **same promotion tier**.

# 9. Features of Aurora

- Automatic fail-over.
- Backup and Recovery.
- Isolation and security.
- Industry compliance.
- Push-button scaling.
- Automated Patching with Zero Downtime.
- Advanced Monitoring.
- Routine Maintenance.
- **Backtrack:** Restore data at any point of time without using backups.

# 10. Aurora Security

- Similar to RDS because uses the same engines.
- Encryption at rest using KMS.
- Automated backups, snapshots and replicas are also encrypted.
- Encryption in flight using SSL (same process as MySQL or Postgres).
- Possibility to authenticate using IAM token (same method as RDS).
- We are responsible for protecting the instance with security groups.
- We can't SSH.

# 11. Amazon Aurora Serverless

- The Aurora Serverless is an auto-scaling, on-demand configuration designed for Amazon Aurora RDS.
- It can start, shut and scale capacity automatically, according to individual application's requirements.
- This service allows we to run cloud-powered databases without the need to manage database capacity.
- Automated database instantiation and auto.
- Scaling based on actual usage.
- Good for infrequent, intermittent or unpredictable workloads.
- No capacity planning needed.
- Pay per second, can be more cost-effective.

# 12. MySQL error log

- We can monitor the MySQL logs directly through the Amazon RDS console, Amazon RDS API, AWS CLI, or AWS SDKs.
- We can also access MySQL logs by directing the logs to a database table in the main database and querying that table.
- We can use the mysqlbinlog utility to download a binary log.

# 13. Amazon Aurora vs Amazon Aurora Serverless

| Amazon Aurora Highlights         | Amazon Aurora Serverless Highlights                       |
| -------------------------------- | --------------------------------------------------------- |
| MySQL Compatibility              | API Integration                                           |
| Better performance               | Compatible with cloud functions for Google, Azure and IBM |
| Easy read scalability            | Lower charges                                             |
| High speed                       | Auto-scaling                                              |
| Low latency read replica         | Openwhisk                                                 |
| Higher IOPS cost                 |                                                           |
| Better cost-to-performance ratio |                                                           |

# 14. Backups

- **Automated backups**
  - 1 to 35 days (cannot be disabled).
  - Point-in-time recovery in that timeframe.
- **Manual DB Snapshots**
  - Manually triggered by the user.
  - Retention of backup for as long as we want.

## 14.1. Restore options

- Aurora backup or a snapshot creates a new database.
- **Restoring MySQL Aurora cluster from S3**
  1. Create a backup of your on-premises database using Percona XtraBackup.
  2. Store the backup file on Amazon S3.
  3. Restore the backup file onto a new Aurora cluster running MySQL.

# 15. Database Cloning

- Create a new Aurora DB Cluster from an existing one.
- Faster than snapshot & restore.
- Uses **copy-on-write** protocol.
  - Initially, the new DB cluster uses the same data volume as the original DB cluster (fast and efficient - no copying is needed).
  - When updates are made to the new DB cluster data, then additional storage is allocated and data is copied to be separated
- Very fast & cost-effective.
- **Useful to create a "staging" database from a "production" database without impacting the production database.**

# 16. Aurora Security

- **At-rest encryption**
  - Database master & replicas encryption using AWS KMS - must be defined as launch time.
  - If the master is not encrypted, the read replicas cannot be encrypted.
  - To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted.
- **In-flight encryption:** TLS-ready by default, use the AWS TLS root certificates client-side.
- **IAM Authentication:** IAM roles to connect to your database (instead of username/pw).
- **Security Groups:** Control Network access to your RDS / Aurora DB.
- **No SSH available** except on RDS Custom.
- **Audit Logs can be enabled** and sent to CloudWatch Logs for longer retention.

# 17. Babelfish for Aurora PostgreSQL

## 17.1. What is Babelfish?

- Feature for Amazon Aurora PostgreSQL.
- Allows Aurora to understand SQL Server (T-SQL) commands.
- Enables migration with minimal code changes.

## 17.2. Key Capabilities

- Supports both T-SQL and PostgreSQL dialects.
- Uses dual-port setup:
  - Port 1433: T-SQL (SQL Server)
  - Port 5432: PostgreSQL
- Supports TDS protocol (versions 7.1 to 7.4).

## 17.3. Benefits

- Reduces application migration complexity.
- No need to replace SQL Server database drivers.
- Maintains SQL Server compatibility at the protocol and syntax level.

## 17.4. Common Use Cases

- Migrating from SQL Server to Aurora PostgreSQL.
- Running legacy applications without rewriting them for PostgreSQL.

## 17.5. Limitations

- Not all SQL Server features are supported.
- Review AWS documentation for detailed limitations.

# 18. Amazon Aurora I/O-Optimized Summary

- **Aurora I/O-Optimized** is a new cluster configuration designed for **I/O-intensive applications** like:
  - E-commerce platforms.
  - Payment processing systems.
  - High-throughput databases.

## 18.1. Key Benefits

- **Improved price/performance** for workloads with heavy I/O usage.
- **Higher throughput** and **lower latency**.
- **Predictable pricing** (eliminates per-I/O charges).

# 19. Summary

- Compatible API for PostgreSQL / MySQL, separation of storage and compute.
- **Storage:** Data is stored in 6 replicas, across 3 AZ - highly available, self-healing, auto-scaling.
- **Compute:** Cluster of DB Instance across multiple AZ, auto-scaling of Read Replicas.
- **Cluster:** Custom endpoints for writer and reader DB instances.
- Same security / monitoring / maintenance features as RDS.
- Know the backup & restore options for Aurora.
- **Aurora Serverless:** For unpredictable / intermittent workloads, no capacity planning.
- **Aurora Global:** Up to 16 DB Read Instances in each region, < 1 second storage replication.
- **Aurora Machine Learning:** Perform ML using SageMaker & Comprehend on Aurora.
- **Aurora Database Cloning:** New cluster from existing one, faster than restoring a snapshot.
- **Use case:** Same as RDS, but with less maintenance / more flexibility / more performance / more features.
