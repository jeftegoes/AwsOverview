# AWS Glue <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Convert data into Parquet format](#2-convert-data-into-parquet-format)
- [3. Data Catalog](#3-data-catalog)
- [4. AWS Glue DataBrew](#4-aws-glue-databrew)
- [5. Things to know at a high-level](#5-things-to-know-at-a-high-level)

# 1. Introduction

- Managed **extract, transform, and load (ETL)** service.
- Useful to prepare and transform data for analytics.
- Fully **serverless** service.
  ![AWS Glue Diagram](/Images/Analytics/AWSGlueDiagram.png)

# 2. Convert data into Parquet format

![AWS Glue Convert Data into Parquet Format](/Images/Analytics/AWSGlueConvertDataParquetFormat.png)

# 3. Data Catalog

- **The AWS Glue Data Catalog is a central repository to store structural and operational metadata for all your data assets. For a given data set, we can store its table definition, physical location, add business relevant attributes, as well as track how this data has changed over time.**
- Catalog of datasets.
- Can be used by [Amazon Athena](Amazon%20Athena.md), [Amazon Redshift](Amazon%20Redshift.md), [Amazon EMR](Amazon%20EMR.md).
  ![AWS Glue Data Catalog](/Images/Analytics/AWSGlueDataCatalog.png)

# 4. AWS Glue DataBrew

- Is a fully managed, serverless data preparation tool that enables users to visually clean, transform, and normalize raw datasets—all **without writing any code**.
- **It is specifically designed for**
  - Data analysts.
  - Business users.
  - Data engineers.
- Who need to prepare data for analytics or machine learning workflows using an intuitive point-and-click interface.

# 5. Things to know at a high-level

- **Glue Job Bookmarks:** Prevent re-processing old data.
- **Glue Elastic Views**
  - Combine and replicate data across multiple data stores using SQL.
  - No custom code, Glue monitors for changes in the source data, serverless.
  - Leverages a "virtual table" (materialized view).
- **Glue Studio:** New GUI to create, run and monitor ETL jobs in Glue.
- **Glue Streaming ETL (built on Apache Spark Structured Streaming):** Compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka).
