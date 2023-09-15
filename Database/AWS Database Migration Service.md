# AWS Database Migration Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. DMS Sources and Targets](#2-dms-sources-and-targets)
- [3. AWS Schema Conversion Tool (SCT)](#3-aws-schema-conversion-tool-sct)
- [4. Multi-AZ Deployment](#4-multi-az-deployment)
- [5. Replication Task Monitoring](#5-replication-task-monitoring)
- [6. CloudWatch Metrics](#6-cloudwatch-metrics)

# 1. Introduction

- Quickly and securely migrate databases to AWS, resilient, self healing.
- The source database remains available during the migration.
- Supports:
  - Homogeneous migrations: ex Oracle to Oracle.
  - Heterogeneous migrations: ex Microsoft SQL Server to Aurora.
- Continuous Data Replication using CDC.
- You must create an EC2 instance to perform the replication tasks.

# 2. DMS Sources and Targets

- **SOURCES**
  - On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2.
  - Azure: Azure SQL Database.
  - Amazon RDS: all including Aurora.
  - [Amazon S3](Amazon%20S3.md)
  - DocumentDB
- **TARGETS**
  - On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP.
  - Amazon RDS.
  - Redshift, DynamoDB, [Amazon S3](Amazon%20S3.md)
  - OpenSearch Service.
  - Kinesis Data Streams.
  - Apache Kafka.
  - DocumentDB & Amazon Neptune.
  - Redis & Babelfish.

# 3. AWS Schema Conversion Tool (SCT)

- Convert your Database's Schema from one engine to another.
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora.
- Example OLAP: (Teradata or Oracle) to Amazon Redshift.
- Prefer compute-intensive instances to optimize data conversions.
- **You do not need to use SCT if you are migrating the same DB engine.**
  - Ex: On-Premise PostgreSQL => RDS PostgreSQL.
  - The DB engine is still PostgreSQL (RDS is the platform).

# 4. Multi-AZ Deployment

- When Multi-AZ Enabled, DMS provisions and maintains a synchronously stand replica in a different AZ.
- Advantages:
  - Provides Data Redundancy.
  - Eliminates I/O freezes.
  - Minimizes latency spikes.

# 5. Replication Task Monitoring

- **Task Status**
  - Indicates the condition of the Task (e.g., Creating, Running, Stopped...).
  - **Task Status Bar:** Gives an estimation of the Task's progress.
- **Table State**
  - Includes the current state of the tables (e.g., Before load, Table completed...).
  - Number of inserts, deletions, and updates for each table.

# 6. CloudWatch Metrics

- **Host Metrics**
  - Performance and utilization statistics for the replication host.
  - CPUUtilization, FreeableMemory, FreeStorageSpace, WriteIOPS...
- **Replication Task Metrics**
  - Statistics for Replication Task including incoming and committed changes, latency between the Replication Host and both source and target databases.
  - FullLoadThroughputRowsSource, FullLoadThroughputRowsTarget, CDCThroughputRowsSource, CDCThroughputRowsTarget...
- **Table Metrics**
  - Statistics for tables that are in the process of being migrated, including the number of insert, update, delete, and DDL statements completed.
