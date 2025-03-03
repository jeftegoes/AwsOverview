# AWS Glue<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Things to know at a high-level](#2-things-to-know-at-a-high-level)

# 1. Introduction

- Managed **extract, transform, and load (ETL)** service.
- Useful to prepare and transform data for analytics.
- Fully **serverless** service.
- Glue Data Catalog: catalog of datasets.
  - **The AWS Glue Data Catalog is a central repository to store structural and operational metadata for all your data assets. For a given data set, you can store its table definition, physical location, add business relevant attributes, as well as track how this data has changed over time.**
  - Can be used by [Amazon Athena](Amazon%20Athena.md), [Amazon Redshift](Amazon%20Redshift.md), [Amazon EMR](Amazon%20EMR.md).

# 2. Things to know at a high-level

- **Glue Job Bookmarks:** Prevent re-processing old data.
- **Glue Elastic Views**
  - Combine and replicate data across multiple data stores using SQL.
  - No custom code, Glue monitors for changes in the source data, serverless.
  - Leverages a "virtual table" (materialized view).
- **Glue DataBrew:** Clean and normalize data using pre-built transformation.
- **Glue Studio:** New GUI to create, run and monitor ETL jobs in Glue.
- **Glue Streaming ETL** (built on Apache Spark Structured Streaming): Compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka).
