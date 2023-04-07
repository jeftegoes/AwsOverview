# Amazon Managed Streaming for Apache Kafka (Amazon MSK)<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Kinesis Data Streams vs. Amazon MSK](#2-kinesis-data-streams-vs-amazon-msk)

# 1. Introduction

- Alternative to Amazon Kinesis.
- Fully managed Apache Kafka on AWS.
  - Allow you to create, update, delete clusters.
  - MSK creates & manages Kafka brokers nodes & Zookeeper nodes for you.
  - Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA).
  - Automatic recovery from common Apache Kafka failures.
  - Data is stored on EBS volumes **for as long as you want**.
- **MSK Serverless**
  - Run Apache Kafka on MSK without managing the capacity.
  - MSK automatically provisions resources and scales compute & storage.

# 2. Kinesis Data Streams vs. Amazon MSK

| Kinesis Data Streams      | Amazon MSK                                   |
| ------------------------- | -------------------------------------------- |
| 1 MB message size limit   | 1MB default, configure for higher (ex: 10MB) |
| Data Streams with Shards  | Kafka Topics with Partitions                 |
| Shard Splitting & Merging | Can only add partitions to a topic           |
| TLS In-flight encryption  | PLAINTEXT or TLS In-flight Encryption        |
| KMS at-rest encryption    | KMS at-rest encryption                       |
