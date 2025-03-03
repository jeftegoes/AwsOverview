# Amazon Redshift <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Redshift Cluster](#2-redshift-cluster)
- [3. Snapshots \& DR](#3-snapshots--dr)
- [4. Spectrum](#4-spectrum)

# 1. Introduction

- **Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud.**
- Redshift is based on PostgreSQL, but it's not used for OLTP.
- **It's OLAP - online analytical processing (analytics and data warehousing).**
- Load data once every hour, not every second.
- 10x better performance than other data warehouses, scale to PBs of data.
- **Columnar** storage of data (instead of row based) & parallel query engine.
- Two modes: Provisioned cluster or Serverless cluster.
- Has a SQL interface for performing the queries.
- BI tools such as AWS Quicksight or Tableau integrate with it.
- vs Athena: faster queries / joins / aggregations thanks to indexes.

# 2. Redshift Cluster

- Leader node: for query planning, results aggregation.
- Compute node: for performing the queries, send results to leader.
- **Provisioned mode**
  - Choose instance types in advance.
  - Can reserve instances for cost savings.

# 3. Snapshots & DR

- **Redshift has "Multi-AZ" mode for some clusters.**
- Snapshots are point-in-time backups of a cluster, stored internally in [Amazon S3](/Storage/Amazon%20S3.md).
- Snapshots are incremental (only what has changed is saved).
- You can restore a snapshot into a **new cluster**.
- Automated: every 8 hours, every 5 GB, or on a schedule. Set retention between 1 to 35 days.
- Manual: snapshot is retained until you delete it.
- **You can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region.**

# 4. Spectrum

- Query data that is already in S3 without loading it.
- **Must have a Redshift cluster available to start the query.**
- The query is then submitted to thousands of Redshift Spectrum nodes.
