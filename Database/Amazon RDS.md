# Amazon RDS - Relational Database Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Advantage over using RDS versus deploying DB on EC2](#2-advantage-over-using-rds-versus-deploying-db-on-ec2)
- [3. Backups](#3-backups)
- [4. Storage Auto Scaling](#4-storage-auto-scaling)
- [5. Read Replicas for read scalability](#5-read-replicas-for-read-scalability)
  - [5.1. Use Cases](#51-use-cases)
  - [5.2. Network Cost](#52-network-cost)
- [6. Disaster Recovery](#6-disaster-recovery)
  - [6.1. Multi AZ](#61-multi-az)
  - [6.2. From Single-AZ to Multi-AZ](#62-from-single-az-to-multi-az)
- [7. Security - Encryption](#7-security---encryption)
  - [7.1. TDE - Transparent Data Encryption](#71-tde---transparent-data-encryption)
  - [7.2. Encryption Operations](#72-encryption-operations)
  - [7.3. Network \& IAM](#73-network--iam)
  - [7.4. IAM Authentication](#74-iam-authentication)
  - [7.5. Security - Summary](#75-security---summary)
- [8. Monitoring](#8-monitoring)
- [9. RDS Proxy](#9-rds-proxy)
- [10. Enhanced Monitoring](#10-enhanced-monitoring)
- [11. CloudFormation](#11-cloudformation)
- [12. Summary](#12-summary)

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

# 2. Advantage over using RDS versus deploying DB on EC2

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

# 3. Backups

- Backups are automatically enabled in RDS.
- **Automated backups:**
  - Daily full backup of the database (during the maintenance window).
  - Transaction logs are backed-up by RDS every 5 minutes.
  - => ability to restore to any point in time (from oldest backup to 5 minutes ago).
  - 7 days retention (can be increased to 35 days).
- **DB Snapshots:**
  - Manually triggered by the user.
  - Retention of backup for as long as you want.

# 4. Storage Auto Scaling

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

# 5. Read Replicas for read scalability

- Up to 5 Read Replicas.
- Within AZ, Cross AZ or Cross Region.
- Replication is **ASYNC**, so reads are eventually consistent.
- Replicas can be promoted to their own DB.
- Applications must update the connection string to leverage read replicas.

## 5.1. Use Cases

- You have a production database that is taking on normal load.
- You want to run a reporting application to run some analytics.
- You create a Read Replica to run the new workload there.
- The production application is unaffected.
- Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE).

## 5.2. Network Cost

- In AWS there's a network cost when data goes from one AZ to another.
- **For RDS Read Replicas within the same region, you don't pay that fee.**

# 6. Disaster Recovery

|                   | RTO    | RPO    | COST   | SCOPE         |
| ----------------- | ------ | ------ | ------ | ------------- |
| Automated Backups | Good   | Better | Low    | Single Region |
| Manual Snapshots  | Better | Good   | Medium | Cross-Region  |
| Read Replicas     | Best   | Best   | High   | Cross-Region  |

## 6.1. Multi AZ

- **SYNC** replication.
- One DNS name - automatic app failover to standby.
- Increase **availability**.
- Failover in case of loss of AZ, loss of network, instance or storage failure.
- No manual intervention in apps.
- Not used for scaling.
- Multi-AZ replication is free.
- Note: The Read Replicas be setup as Multi-AZ for Disaster Recovery (DR).

  ![Multi AZ](/Images/AmazonRDSMultiAZ.png)

## 6.2. From Single-AZ to Multi-AZ

- Zero downtime operation (no need to stop the DB).
- Just click on "modify" for the database.
- The following happens internally:
  - A snapshot is taken.
  - A new DB is restored from the snapshot in a new AZ.
  - Synchronization is established between the two databases.
    ![Single AZ](/Images/AmazonRDSSingleAZ.png)

# 7. Security - Encryption

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

## 7.1. TDE - Transparent Data Encryption

- Amazon RDS supports using Transparent Data Encryption (TDE) to encrypt stored data on your DB instances running Microsoft SQL Server or Oracle.
- TDE automatically encrypts data before it is written to storage, and automatically decrypts data when the data is read from storage.
- At rest encryption:
  - Transparent Data Encryption (TDE) available for Oracle and SQL Server.

## 7.2. Encryption Operations

- Encrypting RDS backups:
  - Snapshots of un-encrypted RDS databases are un-encrypted.
  - Snapshots of encrypted RDS databases are encrypted.
  - Can copy a snapshot into an encrypted one.
- To encrypt an un-encrypted RDS database:
  - Create a snapshot of the un-encrypted database.
  - Copy the snapshot and enable encryption for the snapshot.
  - Restore the database from the encrypted snapshot.
  - Migrate applications to the new database, and delete the old database.

## 7.3. Network & IAM

- Network Security:
  - RDS databases are usually deployed within a private subnet, not in a public one.
  - RDS security works by leveraging security groups (the same concept as for EC2 instances) - it controls which IP / security group can communicate with RDS.
- Access Management
  - IAM policies help control who can manage AWS RDS (through the RDS API).
  - Traditional Username and Password can be used to login into the database.
  - IAM-based authentication can be used to login into RDS MySQL & PostgreSQL.

## 7.4. IAM Authentication

- IAM database authentication works with:
  - MySQL.
  - PostgreSQL.
- You don't need a password, just an authentication token obtained through IAM & RDS API calls.
- Auth token has a lifetime of 15 minutes.
- Benefits:
  - Network in/out must be encrypted using SSL.
  - IAM to centrally manage users instead of DB.
  - Can leverage IAM Roles and EC2 Instance profiles for easy integration.

## 7.5. Security - Summary

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

# 8. Monitoring

![RDS Monitoring log options](/Images/AWSRDSMonitoring.png)

# 9. RDS Proxy

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

# 10. Enhanced Monitoring

- Amazon RDS provides metrics in real-time for the operating system (OS) that your DB instance runs on.
- You can view the metrics for your DB instance using the console or consume the Enhanced Monitoring JSON output from CloudWatch Logs in a monitoring system of your choice.
- By default, **Enhanced Monitoring metrics are stored in the CloudWatch Logs for 30 days**.
- To modify the amount of time the metrics are stored in the CloudWatch Logs, change the retention for the `RDSOSMetrics` log group in the CloudWatch console.

# 11. CloudFormation

- `RDS::DBInstance`
  - `EngineVersion`- The version number of the database engine to use.
  - `AutoMinorVersionUpgrade` - A value that indicates whether minor engine upgrades are applied automatically to the DB instance during the maintenance window.
    - By default, minor engine upgrades are applied automatically.
  - `AllowMajorVersionUpgrade` - A value that indicates whether major version upgrades are allowed.
    - Changing this parameter doesn't result in an outage and the change is asynchronously applied as soon as possible.

# 12. Summary

- Managed PostgreSQL / MySQL / Oracle / SQL Server / DB2 / MariaDB / Custom.
- Provisioned RDS Instance Size and EBS Volume Type & Size.
- Auto-scaling capability for Storage.
- Support for Read Replicas and Multi AZ.
- Security through IAM, Security Groups, KMS , SSL in transit.
- Automated Backup with Point in time restore feature (up to 35 days).
- Manual DB Snapshot for longer-term recovery.
- Managed and Scheduled maintenance (with downtime).
- Support for IAM Authentication, integration with Secrets Manager.
- RDS Custom for access to and customize the underlying instance (Oracle & SQL Server).
- **Use case:** Store relational datasets (RDBMS / OLTP), perform SQL queries, transactions.
