# Amazon RDS - Relational Database Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Advantage over using RDS versus deploying DB on EC2](#2-advantage-over-using-rds-versus-deploying-db-on-ec2)
- [3. Backups](#3-backups)
  - [3.1. Restore options](#31-restore-options)
- [4. Storage Auto Scaling](#4-storage-auto-scaling)
- [5. Read Replicas for read scalability](#5-read-replicas-for-read-scalability)
  - [5.1. Use Cases](#51-use-cases)
  - [5.2. Network Cost](#52-network-cost)
- [6. Disaster Recovery](#6-disaster-recovery)
  - [6.1. Multi-AZ](#61-multi-az)
  - [6.2. From Single-AZ to Multi-AZ](#62-from-single-az-to-multi-az)
- [7. RDS Custom](#7-rds-custom)
- [8. Security - Encryption](#8-security---encryption)
  - [8.1. TDE - Transparent Data Encryption](#81-tde---transparent-data-encryption)
  - [8.2. Encryption Operations](#82-encryption-operations)
  - [8.3. Network \& IAM](#83-network--iam)
  - [8.4. IAM DB Authentication](#84-iam-db-authentication)
  - [8.5. Security - Summary](#85-security---summary)
- [9. Monitoring](#9-monitoring)
- [10. Amazon RDS Proxy](#10-amazon-rds-proxy)
- [11. Enhanced Monitoring](#11-enhanced-monitoring)
- [12. CloudFormation](#12-cloudformation)
- [13. RDS](#13-rds)
- [14. Maintenance Behavior](#14-maintenance-behavior)
  - [14.1. DB Engine Upgrades](#141-db-engine-upgrades)
  - [14.2. Amazon RDS Multi-AZ Maintenance Behavior](#142-amazon-rds-multi-az-maintenance-behavior)
- [15. Multi-AZ Failover](#15-multi-az-failover)
- [16. Summary](#16-summary)

# 1. Introduction

- RDS stands for Relational Database Service.
- It's a managed DB service for DB use SQL as a query language.
- It allows we to create databases in the cloud that are managed by AWS:
  - Postgres.
  - MySQL.
  - MariaDB.
  - Oracle.
  - Microsoft SQL Server.
  - IBM DB2.
  - Aurora (AWS Proprietary database).

# 2. Advantage over using RDS versus deploying DB on EC2

- **RDS is a managed service**
  - Automated provisioning, OS patching.
  - Continuous backups and restore to specific timestamp (Point in Time Restore)!
  - Monitoring dashboards.
  - Read replicas for improved read performance.
  - Multi-AZ setup for DR (Disaster Recovery).
  - Maintenance windows for upgrades.
  - Scaling capability (vertical and horizontal).
  - Storage backed by EBS (gp2 or io1).
- BUT we can't SSH into our instances.

# 3. Backups

- Backups are automatically enabled in RDS.
- **Automated backups**
  - Daily full backup of the database (during the maintenance window).
  - Transaction logs are backed-up by RDS every 5 minutes.
  - => ability to restore to any point in time (from oldest backup to 5 minutes ago).
  - 1** to 35 days of retention,** set 0 to disable automated backups.
- **Manual DB Snapshots**
  - Manually triggered by the user.
  - Retention of backup for as long as we want.
- **Trick**
  - In a stopped RDS database, we will still pay for storage.
  - If we plan on stopping it for a long time, we should snapshot & restore instead.

## 3.1. Restore options

- Restoring a RDS backup or a snapshot creates a new database.
- **Restoring MySQL RDS database from S3**
  - Create a backup of our on-premises database.
  - Store it on Amazon S3 (object storage).
  - Restore the backup file onto a new RDS instance running MySQL.

# 4. Storage Auto Scaling

- Helps we increase storage on our RDS DB instance dynamically.
- When RDS detects we are running out of free database storage, it scales automatically.
- Avoid manually scaling our database storage.
- We have to set **Maximum Storage Threshold** (maximum limit for DB storage).
- **Automatically modify storage if**
  - Free storage is less than 10% of allocated storage.
  - Low-storage lasts at least 5 minutes.
  - 6 hours have passed since last modification.
- Useful for applications with **unpredictable workloads**.
- Supports all RDS database engines (MariaDB, MySQL, PostgreSQL, SQL Server, Oracle).

# 5. Read Replicas for read scalability

- Up to 15 Read Replicas.
- Within AZ, Cross AZ or Cross-Region.
- Replication is **ASYNC**, so reads are eventually consistent.
- Replicas can be promoted to their own DB.
- Applications must update the connection string to leverage read replicas.

## 5.1. Use Cases

- We have a production database that is taking on normal load.
- We want to run a reporting application to run some analytics.
- We create a Read Replica to run the new workload there.
- The production application is unaffected.
- Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE).

## 5.2. Network Cost

- In AWS there's a network cost when data goes from one AZ to another.
- **For RDS Read Replicas within the same region, we don't pay that fee.**

# 6. Disaster Recovery

|                   | RTO    | RPO    | COST   | SCOPE         |
| ----------------- | ------ | ------ | ------ | ------------- |
| Automated Backups | Good   | Better | Low    | Single Region |
| Manual Snapshots  | Better | Good   | Medium | Cross-Region  |
| Read Replicas     | Best   | Best   | High   | Cross-Region  |

## 6.1. Multi-AZ

- **SYNC** replication.
- One DNS name - automatic app failover to standby.
- Increase **availability**.
- Failover in case of loss of AZ, loss of network, instance or storage failure.
- No manual intervention in apps.
- Not used for scaling.
- Multi-AZ replication is free.
- **Note:** The Read Replicas be setup as Multi-AZ for Disaster Recovery (DR).
  ![Multi-AZ](/Images/Database/AmazonRDSMultiAZ.png)

## 6.2. From Single-AZ to Multi-AZ

- Zero downtime operation (no need to stop the DB).
- Just click on "modify" for the database.
- The following happens internally
  1. A snapshot is taken.
  2. A new DB is restored from the snapshot in a new AZ.
  3. Synchronization is established between the two databases.
     ![Single AZ](/Images/Database/AmazonRDSSingleAZ.png)

# 7. RDS Custom

- **Managed Oracle and Microsoft SQL Server Database with OS and database customization.**
- **RDS:** Automates setup, operation, and scaling of database in AWS.
- **Custom:** Access to the underlying database and OS so we can.
  - Configure settings.
  - Install patches.
  - Enable native features.
  - Access the underlying EC2 Instance using **SSH** or **SSM Session Manager**.
- **De-activate Automation Mode** to perform our customization, better to take a DB snapshot before.
- **RDS vs. RDS Custom**
  - **RDS:** Entire database and the OS to be managed by AWS.
  - **RDS Custom:** Full admin access to the underlying OS and the database.

# 8. Security - Encryption

- **At rest encryption**
  - Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption.
  - Encryption has to be defined at launch time.
  - If the master is not encrypted, the read replicas cannot be encrypted.
- **In-flight encryption**
  - SSL certificates to encrypt data to RDS in flight.
  - Provide SSL options with trust certificate when connecting to database.
  - **To enforce SSL**
    - **PostgreSQL:** `rds.force_ssl=1` in the AWS RDS Console (Parameter Groups).
    - **MySQL:** Within the DB: `GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;`

## 8.1. TDE - Transparent Data Encryption

- Amazon RDS supports using Transparent Data Encryption (TDE) to encrypt stored data on our DB instances running Microsoft SQL Server or Oracle.
- TDE automatically encrypts data before it is written to storage, and automatically decrypts data when the data is read from storage.
- **At rest encryption**
  - Transparent Data Encryption (TDE) available for Oracle and SQL Server.

## 8.2. Encryption Operations

- **Encrypting RDS backups**
  - Snapshots of un-encrypted RDS databases are un-encrypted.
  - Snapshots of encrypted RDS databases are encrypted.
  - Can copy a snapshot into an encrypted one.
- **To encrypt an un-encrypted RDS database**
  1. Create a snapshot of the un-encrypted database.
  2. Copy the snapshot and enable encryption for the snapshot.
  3. Restore the database from the encrypted snapshot.
  4. Migrate applications to the new database, and delete the old database.

## 8.3. Network & IAM

- Network Security:
  - RDS databases are usually deployed within a private subnet, not in a public one.
  - RDS security works by leveraging security groups (the same concept as for EC2 instances) - it controls which IP / security group can communicate with RDS.
- Access Management
  - IAM policies help control who can manage AWS RDS (through the RDS API).
  - Traditional Username and Password can be used to login into the database.
  - IAM-based authentication can be used to login into RDS MySQL & PostgreSQL.

## 8.4. IAM DB Authentication

- **IAM DB authentication works with**
  - MySQL.
  - PostgreSQL.
- We don't need a password, just an authentication token obtained through IAM & RDS API calls.
- Auth token has a lifetime of 15 minutes.
- **Benefits**
  - Network in/out must be encrypted using SSL.
  - IAM to centrally manage users instead of DB.
  - Can leverage IAM Roles and EC2 Instance profiles for easy integration.

## 8.5. Security - Summary

- **Encryption at rest**
  - Is done only when we first create the DB instance.
  - **Or:** Unencrypted DB => snapshot => copy snapshot as encrypted => create DB from snapshot.
- **Our responsibility**
  - Check the ports / IP / security group inbound rules in DB's SG.
  - In-database user creation and permissions or manage through IAM.
  - Creating a database with or without public access.
  - Ensure parameter groups or DB is configured to only allow SSL connections.
- **AWS responsibility**
  - No SSH access.
  - No manual DB patching.
  - No manual OS patching.
  - No way to audit the underlying instance.

# 9. Monitoring

![RDS Monitoring log options](/Images/Database/AmazonRDSMonitoring.png)

# 10. Amazon RDS Proxy

- Fully managed database proxy for RDS.
- Allows apps to pool and share DB connections established with the database.
- **Improving database efficiency by reducing the stress on database resources (e.g., CPU, RAM) and minimize open connections (and timeouts)**.
- Serverless, autoscaling, highly available (multi-AZ).
- **Reduced RDS & Aurora failover time by up 66%.**
- Supports RDS (MySQL, PostgreSQL, MariaDB) and Aurora (MySQL, PostgreSQL).
- No code changes required for most apps.
- **Enforce IAM Authentication for DB, and securely store credentials in AWS Secrets Manager.**
- **RDS Proxy is never publicly accessible (must be accessed from VPC).**
  ![RDS Proxy Diagram](/Images/Database/AmazonRDSProxyDiagram.png)

# 11. Enhanced Monitoring

- Amazon RDS provides metrics in real-time for the operating system (OS) that your DB instance runs on.
- We can view the metrics for your DB instance using the console or consume the Enhanced Monitoring JSON output from CloudWatch Logs in a monitoring system of your choice.
- By default, **Enhanced Monitoring metrics are stored in the CloudWatch Logs for 30 days**.
- To modify the amount of time the metrics are stored in the CloudWatch Logs, change the retention for the `RDSOSMetrics` log group in the CloudWatch console.

# 12. CloudFormation

- `RDS::DBInstance`
  - `EngineVersion`- The version number of the database engine to use.
  - `AutoMinorVersionUpgrade` - A value that indicates whether minor engine upgrades are applied automatically to the DB instance during the maintenance window.
    - By default, minor engine upgrades are applied automatically.
  - `AllowMajorVersionUpgrade` - A value that indicates whether major version upgrades are allowed.
    - Changing this parameter doesn't result in an outage and the change is asynchronously applied as soon as possible.

# 13. RDS

- **At-rest encryption**
  - Database master & replicas encryption using AWS KMS - must be defined as launch time.
  - If the master is not encrypted, the read replicas cannot be encrypted.
  - To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted.
- **In-flight encryption:** TLS-ready by default, use the AWS TLS root certificates client-side.
- **IAM Authentication:** IAM roles to connect to your database (instead of username/pw).
- **Security Groups:** Control Network access to your RDS / Aurora DB.
- **No SSH available** except on RDS Custom.
- **Audit Logs can be enabled** and sent to CloudWatch Logs for longer retention.

# 14. Maintenance Behavior

## 14.1. DB Engine Upgrades

- Upgrading the database engine version **requires downtime**.
  - This causes a **full shutdown** of the Multi-AZ deployment during the upgrade.
  - For **database engine upgrades**, both the **primary and standby** are updated **simultaneously**.
- In Multi-AZ deployments, both primary and standby instances are upgraded at the same time.
- Downtime lasts until the upgrade process is complete.
- The duration of downtime depends on the size of the DB instance.

## 14.2. Amazon RDS Multi-AZ Maintenance Behavior

- **Multi-AZ deployments** help reduce downtime during maintenance events.
- For **OS updates**, RDS follows this process:
  1. Applies updates to the **standby** instance.
  2. Promotes the standby to become the **new primary**.
  3. Updates the **old primary**, now acting as the new standby.

# 15. Multi-AZ Failover

- Amazon RDS provides high availability and failover using **Multi-AZ deployments**.
- Multi-AZ deploys a **synchronous standby replica** in a different Availability Zone for:
  - Data redundancy.
  - Minimizing latency spikes during backups.
  - Enhancing availability during maintenance or failures.
- Failover is **automatic**, requiring no administrative action.
- During failover, the **CNAME record is updated** to point to the standby, which becomes the new primary.
  - This ensures the **database URL remains the same** and failover is seamless.

# 16. Summary

- Managed PostgreSQL / MySQL / Oracle / SQL Server / DB2 / MariaDB / Custom.
- Provisioned RDS Instance Size and EBS Volume Type & Size.
- Auto-scaling capability for Storage.
- Support for Read Replicas and Multi-AZ.
- Security through IAM, Security Groups, KMS , SSL in transit.
- Automated Backup with Point in time restore feature (up to 35 days).
- Manual DB Snapshot for longer-term recovery.
- Managed and Scheduled maintenance (with downtime).
- Support for IAM Authentication, integration with Secrets Manager.
- RDS Custom for access to and customize the underlying instance (Oracle & SQL Server).
- **Use case:** Store relational datasets (RDBMS / OLTP), perform SQL queries, transactions.
