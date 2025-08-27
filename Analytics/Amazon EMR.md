# Amazon EMR <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Node types \& purchasing](#2-node-types--purchasing)
- [3. Amazon EMR and Amazon Redshift](#3-amazon-emr-and-amazon-redshift)

# 1. Introduction

- EMR stands for "Elastic MapReduce".
- EMR helps creating Hadoop clusters (Big Data) to analyze and process vast amount of data.
- The clusters can be made of hundreds of EC2 instances.
- EMR comes bundled with Apache Spark, HBase, Presto, Flink...
- EMR takes care of all the provisioning and configuration.
- Auto-scaling and integrated with Spot instances.
- **Use cases:** Data processing, machine learning, web indexing, big data...

# 2. Node types & purchasing

- **Master Node:** Manage the cluster, coordinate, manage health - long running.
- **Core Node:** Run tasks and store data - long running.
- **Task Node (optional):** Just to run tasks - usually Spot.
- **Purchasing options**
  - **On-demand:** Reliable, predictable, won't be terminated.
  - **Reserved (min 1 year):** Cost savings (EMR will automatically use if available).
  - **Spot Instances:** Cheaper, can be terminated, less reliable.
- Can have long-running cluster, or transient (temporary) cluster.

# 3. Amazon EMR and Amazon Redshift

- Use **Amazon EMR** for big data processing and transformations (ETL).
- Load the transformed data into **Amazon Redshift**.
- Perform **analytics and BI queries** using SQL and BI tools.
  ![Amazon EMR and Amazon Redshift](/Images/Analytics/AmazonEMRRedshift.png)