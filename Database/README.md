# Storage<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Relational Databases](#2-relational-databases)
- [3. NoSQL Databases](#3-nosql-databases)
  - [3.1. NoSQL data example: JSON](#31-nosql-data-example-json)
- [4. Databases and Shared Responsibility on AWS](#4-databases-and-shared-responsibility-on-aws)
- [5. Amazon RDS - Relational Database Service](#5-amazon-rds---relational-database-service)
- [6. Amazon DynamoDB](#6-amazon-dynamodb)
- [7. ElastiCache](#7-elasticache)
- [8. Redshift Overview](#8-redshift-overview)
- [9. Amazon EMR](#9-amazon-emr)
- [10. Amazon Athena](#10-amazon-athena)
- [11. Amazon QuickSight](#11-amazon-quicksight)
- [12. DocumentDB](#12-documentdb)
- [13. Amazon Neptune](#13-amazon-neptune)
- [14. Amazon QLDB](#14-amazon-qldb)
- [15. Amazon Managed Blockchain](#15-amazon-managed-blockchain)
- [16. AWS Glue](#16-aws-glue)
- [17. AWS Database Migration Service (DMS)](#17-aws-database-migration-service-dms)
- [18. Databases \& Analytics Summary in AWS](#18-databases--analytics-summary-in-aws)

# 1. Introduction

- Storing data on disk (EFS, EBS, EC2 Instance Store, S3) can have its limits.
- Sometimes, you want to store data in a database...
- You can **structure** the data.
- You build **indexes** to efficiently **query / search** through the data.
- You define **relationships** between your **datasets**.
- Databases are **optimized for a purpose** and come with different features, shapes and constraints.

# 2. Relational Databases

- Looks just like Excel spreadsheets, with links between them!
- Can use the SQL language to perform queries / lookups.

# 3. NoSQL Databases

- NoSQL = non-SQL = non relational databases
- NoSQL databases are purpose built for specific data models and have flexible schemas for building modern applications.
- Benefits:
  - Flexibility: easy to evolve data model.
  - Scalability: designed to scale-out by using distributed clusters.
  - High-performance: optimized for a specific data model.
  - Highly functional: types optimized for the data model.
- Examples: Key-value, document, graph, in-memory, search databases.

## 3.1. NoSQL data example: JSON

- JSON = JavaScript Object Notation.
- JSON is a common form of data that fits into a NoSQL model.
- Data can be nested.
- Fields can change over time.
- Support for new types: arrays, etc...

```
{
   "name":"John",
   "age":30,
   "cars":[
      "Ford",
      "BMW",
      "Fiat"
   ],
   "address":{
      "type":"house",
      "number":23,
      "street":"Dream Road"
   }
}
```

# 4. Databases and Shared Responsibility on AWS

- AWS offers use to **manage** different databases.
- **Databases** & Shared Responsibility on AWS include:
  - Quick Provisioning, High Availability, Vertical and Horizontal Scaling.
  - Automated Backup & Restore, Operations, Upgrades.
  - Operating System Patching is handled by AWS.
  - Monitoring, alerting.
- Note: many databases technologies could be run on EC2, but you must handle yourself the resiliency, backup, patching, high availability, fault tolerance, scaling...

# 5. Amazon RDS - Relational Database Service

- **Amazon Relational Database Service (Amazon RDS) is a SQL managed service that makes it easy to set up, operate, and scale a relational database in the cloud. It is suited for OLTP workloads.** [Amazon RDS](Amazon%20RDS.md)

# 6. Amazon DynamoDB

- **DynamoDB is a fast and flexible non-relational database service for any scale. It can scale with no downtime, it can process millions of requests per second, and is fast and consistent in performance.** [Amazon DynamoDB](Amazon%20DynamoDB.md)

# 7. ElastiCache

- **Amazon ElastiCache is a web service that makes it easy to deploy and run Memcached or Redis protocol-compliant server nodes in the cloud. ElastiCache caches are in-memory databases with high performance, low latency. They help reduce load off databases for read intensive workloads.** [Amazon ElastiCache](AWS%20ElastiCache.md)

# 8. Redshift Overview

- **Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud.**
- Redshift is based on PostgreSQL, but it's not used for OLTP.
- **It's OLAP - online analytical processing (analytics and data warehousing).**
- Load data once every hour, not every second.
- 10x better performance than other data warehouses, scale to PBs of data.
- **Columnar** storage of data (instead of row based).
- Massively Parallel Query Execution (MPP), highly available.
- Pay as you go based on the instances provisioned.
- Has a SQL interface for performing the queries.
- BI tools such as AWS Quicksight or Tableau integrate with it.
- **Manage their data warehouse.**

# 9. Amazon EMR

- EMR stands for "Elastic MapReduce"
- **Amazon EMR is a web service that enables businesses, researchers, data analysts, and developers to easily and cost-effectively process vast amounts of data. EMR helps creating Apache Hadoop clusters (Big Data) to analyze and process vast amount of data.**
- The clusters can be made of hundreds of EC2 instances.
- Also supports Apache Spark, HBase, Presto, Flink...
- EMR takes care of all the provisioning and configuration.
- Auto-scaling and integrated with Spot instances.
- Use cases: data processing, machine learning, web indexing, big data...

# 10. Amazon Athena

- **Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to manage, and you pay only for the queries that you run.** [AWS Athena](AWS%20Athena.md)

# 11. Amazon QuickSight

- **Amazon QuickSight is a fast, cloud-powered business intelligence (BI) service that makes it easy for you to deliver insights to everyone in your organization. You can create and publish interactive dashboards.**
- Serverless machine learning-powered business intelligence service to create interactive dashboards.
- Fast, automatically scalable, embeddable, with per-session pricing.
- Use cases:
  - Business analytics
  - Building visualizations
  - Perform ad-hoc analysis
  - Get business insights using data
- Integrated with RDS, Aurora, Athena, Redshift, S3...

# 12. DocumentDB

- **Amazon DocumentDB (with MongoDB compatibility) is a fast, calable, highly available, and fully managed document database service that supports MongoDB workloads.**
- Aurora is an "AWS-implementation" of PostgreSQL / MySQL...
- **DocumentDB is the same for MongoDB (which is a NoSQL database).**
- MongoDB is used to store, query, and index JSON data.
- Similar "deployment concepts" as Aurora.
- Fully Managed, highly available with replication across 3 AZ.
- Aurora storage automatically grows in increments of 10GB, up to 64 TB.
- Automatically scales to workloads with millions of requests per seconds.

# 13. Amazon Neptune

- **Amazon Neptune is a fast, reliable, fully-managed graph database service that makes it easy to build and run applications that work with highly connected datasets. It can be used for knowledge graphs, fraud detection, recommendations engines, social networking, etc.**
- Fully managed graph database.
- A popular graph dataset would be a social network.
  - Users have friends
  - Posts have comments
  - Comments have likes from users
  - Users share and like posts...
- Highly available across 3 AZ, with up to 15 read replicas.
- Build and run applications working with highly connected datasets - optimized for these complex and hard queries.
- Can store up to billions of relations and query the graph with milliseconds latency.
- Highly available with replications across multiple AZs.

# 14. Amazon QLDB

- QLDB stands for "Quantum Ledger Database".
- **Amazon QLDB is a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log owned by a central trusted authority. Amazon QLDB tracks each and every application data change and maintains a complete and verifiable history of changes over time.**
- A ledger is a book recording financial transactions.
- Fully Managed, Serverless, High available, Replication across 3 AZ.
- Used to review history of all the changes made to your application data over time.
- Immutable system: no entry can be removed or modified, cryptographically verifiable.
- 2-3x better performance than common ledger blockchain frameworks, manipulate data using SQL.
- Difference with Amazon Managed Blockchain: no decentralization component, in accordance with financial regulation rules.

# 15. Amazon Managed Blockchain

- **Amazon Managed Blockchain is a fully managed service that makes it easy to create and manage scalable blockchain networks using the popular open source frameworks Hyperledger Fabric and Ethereum. It allows multiple parties to execute transactions without the need of a trusted, central authority.**
- Blockchain makes it possible to build applications where multiple parties can execute transactions without the need for a trusted, central authority.
- Amazon Managed Blockchain is a managed service to:
  - Join public blockchain networks
  - Or create your own scalable private network
- Compatible with the frameworks Hyperledger Fabric & Ethereum

# 16. AWS Glue

- **AWS Glue is a fully managed extract, transform, and load (ETL) service that makes it easy for customers to prepare and load their data for analytics.** [AWS Glue](AWS%20Glue.md)

# 17. AWS Database Migration Service (DMS)

- **AWS Database Migration Service helps you migrate databases to AWS quickly and securely. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database.** [AWS Database Migration Service](AWS%20Database%20Migration%20Service.md)

# 18. Databases & Analytics Summary in AWS

- Relational Databases: OLTP, RDS and Aurora (SQL).
- Differences between Multi-AZ, Read Replicas, Multi-Region.
- In-memory Database: ElastiCache.
- Key/Value Database: DynamoDB (serverless) & DAX (cache for DynamoDB).
- Warehouse - OLAP: Redshift (SQL).
- Apache Hadoop Cluster: EMR.
- Athena: query data on Amazon S3 (serverless & SQL).
- QuickSight: dashboards on your data (serverless).
- DocumentDB: "Aurora for MongoDB" (JSON - NoSQL database).
- Amazon QLDB: Financial Transactions Ledger (immutable journal, cryptographically verifiable).
- Amazon Managed Blockchain: managed Hyperledger Fabric & Ethereum blockchains.
- Glue: Managed ETL (Extract Transform Load) and Data Catalog service.
- Database Migration Service: DMS.
- Neptune: Graph database.
