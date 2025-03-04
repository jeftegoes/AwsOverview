# Amazon Neptune <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Streams](#2-streams)

# 1. Introduction

- Fully managed **graph** database.
- A popular **graph dataset** would be a social network.
  - Users have friends
  - Posts have comments
  - Comments have likes from users
  - Users share and like posts...
- Highly available across 3 AZ, with up to 15 read replicas.
- Build and run applications working with highly connected datasets - optimized for these complex and hard queries.
- Can store up to billions of relations and query the graph with milliseconds latency.
- Highly available with replications across multiple AZs.
- Great for knowledge graphs (Wikipedia), fraud detection, recommendation engines, social networking.

# 2. Streams

- **Real-time ordered** sequence of every change to your graph data.
- Changes are available immediately after writing.
- **No duplicates, strict order.**
- Streams data is accessible in an **HTTP REST API**.
- **Use cases**
  - Send notifications when certain changes are made.
  - Maintain your graph data synchronized in another data store (e.g., S3, OpenSearch, ElastiCache).
  - Replicate data across regions in Neptune.
