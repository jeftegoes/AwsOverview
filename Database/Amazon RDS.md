# Amazon RDS - Relational Database Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Advantage over using RDS versus deploying DB on EC2](#11-advantage-over-using-rds-versus-deploying-db-on-ec2)
  - [1.2. Backups](#12-backups)
  - [1.3. Storage Auto Scaling](#13-storage-auto-scaling)
  - [1.4. Read Replicas for read scalability](#14-read-replicas-for-read-scalability)
    - [1.4.1. Use Cases](#141-use-cases)
    - [1.4.2. Network Cost](#142-network-cost)
  - [1.5. Multi AZ (Disaster Recovery)](#15-multi-az-disaster-recovery)
    - [1.5.1. From Single-AZ to Multi-AZ](#151-from-single-az-to-multi-az)
  - [1.6. Security - Encryption](#16-security---encryption)
    - [1.6.1. TDE - Transparent Data Encryption](#161-tde---transparent-data-encryption)
    - [1.6.2. Encryption Operations](#162-encryption-operations)
    - [1.6.3. Network \& IAM](#163-network--iam)
    - [1.6.4. IAM Authentication](#164-iam-authentication)
    - [1.6.5. Security - Summary](#165-security---summary)
  - [1.7. Monitoring](#17-monitoring)
  - [1.8. RDS Proxy](#18-rds-proxy)
  - [1.9. Enhanced Monitoring](#19-enhanced-monitoring)
- [2. Amazon Aurora](#2-amazon-aurora)
  - [2.1. Aurora High Availability and Read Scaling](#21-aurora-high-availability-and-read-scaling)
  - [2.2. Global Aurora](#22-global-aurora)
  - [2.3. Features of Aurora](#23-features-of-aurora)
  - [2.4. Aurora Security](#24-aurora-security)
- [3. Amazon Aurora Serverless](#3-amazon-aurora-serverless)
- [4. MySQL error log](#4-mysql-error-log)
- [5. Amazon Aurora vs Amazon Aurora Serverless](#5-amazon-aurora-vs-amazon-aurora-serverless)
- [6. CloudFormation](#6-cloudformation)

# 1. Introduction

- RDS stands for Relational Database Service.
- It's a managed DB service for DB use SQL as a query language.
- It allows you to create databases in the cloud that are managed by AWS:
  - Postgres.
  - MySQL.
  - MariaDB.
  - Oracle.
  - Microsoft SQL Server.
  - Aurora (AWS Proprietary database).

## 1.1. Advantage over using RDS versus deploying DB on EC2

- RDS is a managed service:
  - Automated provisioning, OS patching.
  - Continuous backups and restore to specific timestamp (Point in Time Restore)!
  - Monitoring dashboards.
  - Read replicas for improved read performance.
  - Multi AZ setup for DR (Disaster Recovery).
  - Maintenance windows for upgrades.
  - Scaling capability (vertical and horizontal).
  - Storage backed by EBS (gp2 or io1).
- BUT you can't SSH into your instances.

## 1.2. Backups

- Backups are automatically enabled in RDS.
- **Automated backups:**
  - Daily full backup of the database (during the maintenance window).
  - Transaction logs are backed-up by RDS every 5 minutes.
  - => ability to restore to any point in time (from oldest backup to 5 minutes ago).
  - 7 days retention (can be increased to 35 days).
- **DB Snapshots:**
  - Manually triggered by the user.
  - Retention of backup for as long as you want.

## 1.3. Storage Auto Scaling

- Helps you increase storage on your RDS DB instance dynamically.
- When RDS detects you are running out of free database storage, it scales automatically.
- Avoid manually scaling your database storage.
- You have to set **Maximum Storage Threshold** (maximum limit for DB storage).
- Automatically modify storage if:
  - Free storage is less than 10% of allocated storage.
  - Low-storage lasts at least 5 minutes.
  - 6 hours have passed since last modification.
- Useful for applications with **unpredictable workloads**.
- Supports all RDS database engines (MariaDB, MySQL, PostgreSQL, SQL Server, Oracle).

## 1.4. Read Replicas for read scalability

- Up to 5 Read Replicas.
- Within AZ, Cross AZ or Cross Region.
- Replication is **ASYNC**, so reads are eventually consistent.
- Replicas can be promoted to their own DB.
- Applications must update the connection string to leverage read replicas.

### 1.4.1. Use Cases

- You have a production database that is taking on normal load.
- You want to run a reporting application to run some analytics.
- You create a Read Replica to run the new workload there.
- The production application is unaffected.
- Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE).

### 1.4.2. Network Cost

- In AWS there's a network cost when data goes from one AZ to another.
- **For RDS Read Replicas within the same region, you don't pay that fee.**

## 1.5. Multi AZ (Disaster Recovery)

- **SYNC** replication.
- One DNS name - automatic app failover to standby.
- Increase **availability**.
- Failover in case of loss of AZ, loss of network, instance or storage failure.
- No manual intervention in apps.
- Not used for scaling.
- Multi-AZ replication is free.
- Note: The Read Replicas be setup as Multi-AZ for Disaster Recovery (DR).

### 1.5.1. From Single-AZ to Multi-AZ

- Zero downtime operation (no need to stop the DB).
- Just click on "modify" for the database.
- The following happens internally:
  - A snapshot is taken.
  - A new DB is restored from the snapshot in a new AZ.
  - Synchronization is established between the two databases.

## 1.6. Security - Encryption

- At rest encryption:
  - Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption.
  - Encryption has to be defined at launch time.
  - If the master is not encrypted, the read replicas cannot be encrypted.
- In-flight encryption:
  - SSL certificates to encrypt data to RDS in flight.
  - Provide SSL options with trust certificate when connecting to database.
  - To enforce SSL:
    - PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (Parameter Groups).
    - MySQL: Within the DB: `GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;`

### 1.6.1. TDE - Transparent Data Encryption

- Amazon RDS supports using Transparent Data Encryption (TDE) to encrypt stored data on your DB instances running Microsoft SQL Server or Oracle.
- TDE automatically encrypts data before it is written to storage, and automatically decrypts data when the data is read from storage.
- At rest encryption:
  - Transparent Data Encryption (TDE) available for Oracle and SQL Server.

### 1.6.2. Encryption Operations

- Encrypting RDS backups:
  - Snapshots of un-encrypted RDS databases are un-encrypted.
  - Snapshots of encrypted RDS databases are encrypted.
  - Can copy a snapshot into an encrypted one.
- To encrypt an un-encrypted RDS database:
  - Create a snapshot of the un-encrypted database.
  - Copy the snapshot and enable encryption for the snapshot.
  - Restore the database from the encrypted snapshot.
  - Migrate applications to the new database, and delete the old database.

### 1.6.3. Network & IAM

- Network Security:
  - RDS databases are usually deployed within a private subnet, not in a public one.
  - RDS security works by leveraging security groups (the same concept as for EC2 instances) - it controls which IP / security group can communicate with RDS.
- Access Management
  - IAM policies help control who can manage AWS RDS (through the RDS API).
  - Traditional Username and Password can be used to login into the database.
  - IAM-based authentication can be used to login into RDS MySQL & PostgreSQL.

### 1.6.4. IAM Authentication

- IAM database authentication works with:
  - MySQL.
  - PostgreSQL.
- You don't need a password, just an authentication token obtained through IAM & RDS API calls.
- Auth token has a lifetime of 15 minutes.
- Benefits:
  - Network in/out must be encrypted using SSL.
  - IAM to centrally manage users instead of DB.
  - Can leverage IAM Roles and EC2 Instance profiles for easy integration.

### 1.6.5. Security - Summary

- Encryption at rest:
  - Is done only when you first create the DB instance.
  - Or: unencrypted DB => snapshot => copy snapshot as encrypted => create DB from snapshot.
- Your responsibility:
  - Check the ports / IP / security group inbound rules in DB's SG.
  - In-database user creation and permissions or manage through IAM.
  - Creating a database with or without public access.
  - Ensure parameter groups or DB is configured to only allow SSL connections.
- AWS responsibility:
  - No SSH access.
  - No manual DB patching.
  - No manual OS patching.
  - No way to audit the underlying instance.

## 1.7. Monitoring

![RDS Monitoring log options](/Images/AWSRDSMonitoring.png)

## 1.8. RDS Proxy

- Fully managed database proxy for RDS.
- Allows apps to pool and share DB connections established with the database.
- Improving database efficiency by reducing the stress on database resources (e.g., CPU, RAM) and minimize open connections (and timeouts).
- Serverless, autoscaling, highly available (multi-AZ).
- Reduced RDS & Aurora failover time by up 66%.
- Supports RDS (MySQL, PostgreSQL, MariaDB) and Aurora (MySQL, PostgreSQL).
- No code changes required for most apps.
- Enforce IAM Authentication for DB, and securely store credentials in AWS Secrets Manager.
- RDS Proxy is never publicly accessible (must be accessed from VPC).

![RDS Proxy Diagram](/Images/AWSRDSProxyDiagram.png)

## 1.9. Enhanced Monitoring

- Amazon RDS provides metrics in real-time for the operating system (OS) that your DB instance runs on.
- You can view the metrics for your DB instance using the console or consume the Enhanced Monitoring JSON output from CloudWatch Logs in a monitoring system of your choice.
- By default, **Enhanced Monitoring metrics are stored in the CloudWatch Logs for 30 days**.
- To modify the amount of time the metrics are stored in the CloudWatch Logs, change the retention for the `RDSOSMetrics` log group in the CloudWatch console.

# 2. Amazon Aurora

- **Amazon Aurora is a MySQL and PostgreSQL-compatible relational database built for the cloud, that combines the performance and availability of traditional enterprise databases with the simplicity and cost-effectiveness of open source databases. It is a proprietary technology from AWS.**
- Aurora is a proprietary technology from AWS (not open sourced).
- **PostgreSQL and MySQL** are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database).
- Aurora is "AWS cloud optimized" and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS.
- Aurora storage automatically grows in increments of 10GB, up to 128 TB.
- Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10 ms replica lag).
- Failover in Aurora is instantaneous. It's HA (High Availability) native.
- Aurora costs more than RDS (20% more) - but is more efficient.

## 2.1. Aurora High Availability and Read Scaling

- 6 copies of your data across 3 AZ:
  - 4 copies out of 6 needed for writes.
  - 3 copies out of 6 need for reads.
  - Self healing with peer-to-peer replication.
  - Storage is striped across 100s of volumes.
- One Aurora Instance takes writes (master).
- Automated failover for master in less than 30 seconds.
- Master + up to 15 Aurora Read Replicas serve reads.
- **Support for Cross Region Replication.**

## 2.2. Global Aurora

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

## 2.3. Features of Aurora

- Automatic fail-over.
- Backup and Recovery.
- Isolation and security.
- Industry compliance.
- Push-button scaling.
- Automated Patching with Zero Downtime.
- Advanced Monitoring.
- Routine Maintenance.
- Backtrack: restore data at any point of time without using backups.

## 2.4. Aurora Security

- Similar to RDS because uses the same engines.
- Encryption at rest using KMS.
- Automated backups, snapshots and replicas are also encrypted.
- Encryption in flight using SSL (same process as MySQL or Postgres).
- Possibility to authenticate using IAM token (same method as RDS).
- You are responsible for protecting the instance with security groups.
- You can't SSH.

# 3. Amazon Aurora Serverless

- The Aurora Serverless is an auto-scaling, on-demand configuration designed for Amazon Aurora RDS.
- It can start, shut and scale capacity automatically, according to individual application's requirements.
- This service allows you to run cloud-powered databases without the need to manage database capacity.

# 4. MySQL error log

- You can monitor the MySQL logs directly through the Amazon RDS console, Amazon RDS API, AWS CLI, or AWS SDKs.
- You can also access MySQL logs by directing the logs to a database table in the main database and querying that table.
- You can use the mysqlbinlog utility to download a binary log.

# 5. Amazon Aurora vs Amazon Aurora Serverless

| Amazon Aurora Highlights         | Amazon Aurora Serverless Highlights                       |
| -------------------------------- | --------------------------------------------------------- |
| MySQL Compatibility              | API Integration                                           |
| Better performance               | Compatible with cloud functions for Google, Azure and IBM |
| Easy read scalability            | Lower charges                                             |
| High speed                       | Auto-scaling                                              |
| Low latency read replica         | Openwhisk                                                 |
| Higher IOPS cost                 |                                                           |
| Better cost-to-performance ratio |                                                           |

# 6. CloudFormation

- `RDS::DBInstance`
  - `EngineVersion`- The version number of the database engine to use.
  - `AutoMinorVersionUpgrade` - A value that indicates whether minor engine upgrades are applied automatically to the DB instance during the maintenance window.
    - By default, minor engine upgrades are applied automatically.
  - `AllowMajorVersionUpgrade` - A value that indicates whether major version upgrades are allowed.
    - Changing this parameter doesn't result in an outage and the change is asynchronously applied as soon as possible.
