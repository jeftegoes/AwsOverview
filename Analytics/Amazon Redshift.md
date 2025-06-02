# Amazon Redshift <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Redshift Cluster](#2-redshift-cluster)
- [3. Snapshots \& DR](#3-snapshots--dr)
- [4. Spectrum](#4-spectrum)
- [5. Massively parallel processing](#5-massively-parallel-processing)

# 1. Introduction

- **Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud.**
- Redshift is based on PostgreSQL, but it's not used for OLTP.
- **It's OLAP - online analytical processing (analytics and data warehousing).**
- Load data once every hour, not every second.
- 10x better performance than other data warehouses, scale to PBs of data.
- **Columnar** storage of data (instead of row based) & parallel query engine.
- **Two modes:** Provisioned cluster or Serverless cluster.
- Has a SQL interface for performing the queries.
- BI tools such as AWS Quicksight or Tableau integrate with it.
- **vs Athena:** Faster queries / joins / aggregations thanks to indexes.

# 2. Redshift Cluster

- **Leader node:** For query planning, results aggregation.
- **Compute node:** For performing the queries, send results to leader.
- **Provisioned mode**
  - Choose instance types in advance.
  - Can reserve instances for cost savings.
    ![Amazon Redshift Cluster](/Images/Analytics/AmazonRedshiftCluster.png)

# 3. Snapshots & DR

- **Redshift has "Multi-AZ" mode for some clusters.**
- Snapshots are point-in-time backups of a cluster, stored internally in [Amazon S3](/Storage/Amazon%20S3.md).
- Snapshots are incremental (only what has changed is saved).
- We can restore a snapshot into a **new cluster**.
- Automated: every 8 hours, every 5 GB, or on a schedule. Set retention between 1 to 35 days.
- **Manual:** Snapshot is retained until we delete it.
- **We can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region.**

# 4. Spectrum

- Query data that is already in S3 without loading it.
- **Must have a Redshift cluster available to start the query.**
- The query is then submitted to thousands of Redshift Spectrum nodes.

# 5. Massively parallel processing

- Massively parallel processing (MPP) enables fast run of the most complex queries operating on large amounts of data.
- Multiple compute nodes handle all query processing leading up to final result aggregation, with each core of each node running the same compiled query segments on portions of the entire data.
