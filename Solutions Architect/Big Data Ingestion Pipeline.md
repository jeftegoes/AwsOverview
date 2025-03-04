# Big Data Infestion Architectures <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Big Data Ingestion Pipeline](#1-big-data-ingestion-pipeline)
  - [1.1. Requirements](#11-requirements)
  - [1.2. Solution](#12-solution)
  - [1.3. Diagram](#13-diagram)

# 1. Big Data Ingestion Pipeline

## 1.1. Requirements

- We want the ingestion pipeline to be fully serverless.
- We want to collect data in real time.
- We want to transform the data.
- We want to query the transformed data using SQL.
- The reports created using the queries should be in S3.
- We want to load that data into a warehouse and create dashboards.

## 1.2. Solution

- IoT Core allows you to harvest data from IoT devices.
- Kinesis is great for real-time data collection.
- Firehose helps with data delivery to S3 in near real-time (1 minute).
- Lambda can help Firehose with data transformations.
- Amazon S3 can trigger notifications to SQS.
- Lambda can subscribe to SQS (we could have connecter S3 to Lambda).
- Athena is a serverless SQL service and results are stored in S3.
- The reporting bucket contains analyzed data and can be used by reporting tool such as AWS QuickSight, Redshift, etc...

## 1.3. Diagram

![Big Data Ingestion Pipeline](/Images/Solution%20Architect/BigDataIngestionPipeline.png)
