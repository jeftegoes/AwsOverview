# Storage <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Choosing the Right Database](#2-choosing-the-right-database)
- [3. Database Types](#3-database-types)
- [4. Relational Databases](#4-relational-databases)
- [5. NoSQL Databases](#5-nosql-databases)
  - [5.1. NoSQL data example: JSON](#51-nosql-data-example-json)
- [6. Databases and Shared Responsibility on AWS](#6-databases-and-shared-responsibility-on-aws)
- [7. Amazon RDS - Relational Database Service](#7-amazon-rds---relational-database-service)
- [8. Amazon DynamoDB](#8-amazon-dynamodb)
- [9. ElastiCache](#9-elasticache)
- [10. DocumentDB](#10-documentdb)
- [11. Amazon Neptune](#11-amazon-neptune)
- [12. Amazon QLDB](#12-amazon-qldb)
- [13. Amazon Managed Blockchain](#13-amazon-managed-blockchain)
- [14. AWS Database Migration Service (DMS)](#14-aws-database-migration-service-dms)
- [15. Databases Summary in AWS](#15-databases-summary-in-aws)

# 1. Introduction

- Storing data on disk (EFS, EBS, EC2 Instance Store, S3) can have its limits.
- Sometimes, you want to store data in a database...
- You can **structure** the data.
- You build **indexes** to efficiently **query / search** through the data.
- You define **relationships** between your **datasets**.
- Databases are **optimized for a purpose** and come with different features, shapes and constraints.

# 2. Choosing the Right Database

- We have a lot of managed databases on AWS to choose from.
- Questions to choose the right database based on your architecture:
  - Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it change, does it need to scale or fluctuate during the day?
  - How much data to store and for how long? Will it grow? Average object size? How are they accessed?
  - Data durability? Source of truth for the data?
  - Latency requirements? Concurrent users?
  - Data model? How will you query the data? Joins? Structured? Semi-Structured?
  - Strong schema? More flexibility? Reporting? Search? RDBMS / NoSQL?
  - License costs? Switch to Cloud Native DB such as Aurora?

# 3. Database Types

- RDBMS (= SQL / OLTP): RDS, Aurora - great for joins.
- NoSQL database - no joins, no SQL : DynamoDB (~JSON), ElastiCache (key / value pairs), Neptune (graphs), DocumentDB (for MongoDB), Keyspaces (for Apache Cassandra).
- Object Store: S3 (for big objects) / Glacier (for backups / archives).
- Data Warehouse (= SQL Analytics / BI): Redshift (OLAP), Athena, EMR.
- Search: OpenSearch (JSON) - free text, unstructured searches.
- Graphs: Amazon Neptune - displays relationships between data.
- Ledger: Amazon Quantum Ledger Database.
- Time series: Amazon Timestrea.

# 4. Relational Databases

- Looks just like Excel spreadsheets, with links between them!
- Can use the SQL language to perform queries / lookups.

# 5. NoSQL Databases

- NoSQL = non-SQL = non relational databases
- NoSQL databases are purpose built for specific data models and have flexible schemas for building modern applications.
- Benefits:
  - Flexibility: easy to evolve data model.
  - Scalability: designed to scale-out by using distributed clusters.
  - High-performance: optimized for a specific data model.
  - Highly functional: types optimized for the data model.
- Examples: Key-value, document, graph, in-memory, search databases.

## 5.1. NoSQL data example: JSON

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

# 6. Databases and Shared Responsibility on AWS

- AWS offers use to **manage** different databases.
- **Databases** & Shared Responsibility on AWS include:
  - Quick Provisioning, High Availability, Vertical and Horizontal Scaling.
  - Automated Backup & Restore, Operations, Upgrades.
  - Operating System Patching is handled by AWS.
  - Monitoring, alerting.
- Note: many databases technologies could be run on EC2, but you must handle yourself the resiliency, backup, patching, high availability, fault tolerance, scaling...

# 7. Amazon RDS - Relational Database Service

- **Amazon Relational Database Service (Amazon RDS) is a SQL managed service that makes it easy to set up, operate, and scale a relational database in the cloud. It is suited for OLTP workloads.** [Amazon RDS](Amazon%20RDS.md)

# 8. Amazon DynamoDB

- **DynamoDB is a fast and flexible non-relational database service for any scale. It can scale with no downtime, it can process millions of requests per second, and is fast and consistent in performance.** [Amazon DynamoDB](Amazon%20DynamoDB.md)

# 9. ElastiCache

- **Amazon ElastiCache is a web service that makes it easy to deploy and run Memcached or Redis protocol-compliant server nodes in the cloud. ElastiCache caches are in-memory databases with high performance, low latency. They help reduce load off databases for read intensive workloads.** [Amazon ElastiCache](Amazon%20ElastiCache.md)

# 10. DocumentDB

- **Amazon DocumentDB (with MongoDB compatibility) is a fast, calable, highly available, and fully managed document database service that supports MongoDB workloads.** [Amazon DocumentDB](Amazon%20DocumentDB.md)

# 11. Amazon Neptune

- **Amazon Neptune is a fast, reliable, fully-managed graph database service that makes it easy to build and run applications that work with highly connected datasets. It can be used for knowledge graphs, fraud detection, recommendations engines, social networking, etc.** [Amazon Netptune](Amazon%20Neptune.md)

# 12. Amazon QLDB

- **Amazon QLDB is a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log owned by a central trusted authority. Amazon QLDB tracks each and every application data change and maintains a complete and verifiable history of changes over time.** [Amazon QLDB](Amazon%20QLDB.md)

# 13. Amazon Managed Blockchain

- **Amazon Managed Blockchain is a fully managed service that makes it easy to create and manage scalable blockchain networks using the popular open source frameworks Hyperledger Fabric and Ethereum. It allows multiple parties to execute transactions without the need of a trusted, central authority.**
- Blockchain makes it possible to build applications where multiple parties can execute transactions without the need for a trusted, central authority.
- Amazon Managed Blockchain is a managed service to:
  - Join public blockchain networks
  - Or create your own scalable private network
- Compatible with the frameworks Hyperledger Fabric & Ethereum

# 14. AWS Database Migration Service (DMS)

- **AWS Database Migration Service helps you migrate databases to AWS quickly and securely. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database.** [AWS Database Migration Service](AWS%20Database%20Migration%20Service.md)

# 15. Databases Summary in AWS

- Relational Databases: OLTP, RDS and Aurora (SQL).
- Differences between Multi-AZ, Read Replicas, Multi-Region.
- In-memory Database: ElastiCache.
- Key/Value Database: DynamoDB (serverless) & DAX (cache for DynamoDB).
- Warehouse - OLAP: Redshift (SQL).
- DocumentDB: "Aurora for MongoDB" (JSON - NoSQL database).
- Amazon QLDB: Financial Transactions Ledger (immutable journal, cryptographically verifiable).
- Amazon Managed Blockchain: managed Hyperledger Fabric & Ethereum blockchains.
- Database Migration Service: DMS.
- Neptune: Graph database.
