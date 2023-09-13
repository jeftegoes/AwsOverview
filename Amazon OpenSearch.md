# Amazon OpenSearch Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)

# 1. Introduction

- **Amazon OpenSearch is successor to Amazon ElasticSearch.**
- In [DynamoDB](AWS%20DynamoDB.md), queries only exist by primary key or indexes...
- **With OpenSearch, you can search any field, even partially matches.**
- It's common to use OpenSearch as a complement to another database.
- OpenSearch requires a cluster of instances (not serverless).
- Does not natively support SQL (can be enabled via a plugin).
- Ingestion from Kinesis Data Firehose, AWS IoT, and [Amazon CloudWatch Logs](Amazon%20CloudWatch.md).
- Security through Cognito & IAM, KMS encryption, TLS.
- Comes with OpenSearch Dashboards (visualization).
