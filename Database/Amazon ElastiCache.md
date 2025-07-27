# Amazon ElastiCache <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Solution Architecture](#2-solution-architecture)
  - [2.1. DB Cache](#21-db-cache)
  - [2.2. User Session Store](#22-user-session-store)
- [3. Redis vs Memcached](#3-redis-vs-memcached)
- [4. Cache Security](#4-cache-security)
- [5. Replication](#5-replication)
  - [5.1. ElastiCache Replication: Cluster Mode Disabled](#51-elasticache-replication-cluster-mode-disabled)
  - [5.2. Redis Scaling - Cluster Mode Disabled](#52-redis-scaling---cluster-mode-disabled)
  - [5.3. Cluster Mode ENABLED](#53-cluster-mode-enabled)
  - [5.4. ElastiCache for Redis - Auto Scaling](#54-elasticache-for-redis---auto-scaling)
  - [5.5. ElastiCache - Redis Connection Endpoints](#55-elasticache---redis-connection-endpoints)
- [6. Caching Implementation Considerations](#6-caching-implementation-considerations)
  - [6.1. Patterns for ElastiCache](#61-patterns-for-elasticache)
  - [6.2. Lazy Loading / Cache-Aside / Lazy Population](#62-lazy-loading--cache-aside--lazy-population)
  - [6.3. Write Through - Add or Update cache when database is updated](#63-write-through---add-or-update-cache-when-database-is-updated)
- [7. Cache Evictions and Time-to-live (TTL)](#7-cache-evictions-and-time-to-live-ttl)
- [8. Final words of wisdom](#8-final-words-of-wisdom)
- [9. Use Case](#9-use-case)
- [10. Geospatial Support](#10-geospatial-support)
- [11. Summary](#11-summary)

# 1. Introduction

- The same way RDS is to get managed Relational Databases...
- ElastiCache is to get managed Redis or Memcached.
- Caches are in-memory databases with really high performance, low latency.
- Helps **reduce load off databases for read intensive workloads**.
- Helps make your application stateless.
- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups.
- **Using ElastiCache involves heavy application code changes.**

# 2. Solution Architecture

## 2.1. DB Cache

- Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache.
- Helps relieve load in RDS.
- Cache must have an invalidation strategy to make sure only the most current data is used in there.
  ![Amazon ElastiCache Solution Architecture - DB Cache](/Images/Database/AmazonElastiCacheSolutionArchitectureDbCache.png)

## 2.2. User Session Store

- User logs into any of the application.
- The application writes the session data into ElastiCache.
- The user hits another instance of our application.
- The instance retrieves the data and the user is already logged in.

![Amazon ElastiCache Solution Architecture User Session Store](/Images/Database/AmazonElastiCacheSolutionArchitectureUserSessionStore.png)

# 3. Redis vs Memcached

- **Redis**
  - **Multi-AZ** with Auto-Failover.
  - **Read Replicas** to scale reads and have **high availability**.
  - Data Durability using AOF persistence.
  - **Backup and restore features.**
  - Supports Sets and Sorted Sets.
- **Memcached**
  - Multi-node for partitioning of data (sharding).
  - **No high availability (replication).**
  - **Non persistent.**
  - **No backup and restore.**
  - Multi-threaded architecture.

# 4. Cache Security

- ElastiCache supports **IAM Authentication for Redis**.
- IAM policies on ElastiCache are only used for AWS API-level security.
- **Redis AUTH**
  - We can set a "password/token" when we create a Redis cluster.
  - This is an extra level of security for your cache (on top of security groups).
  - Support SSL in flight encryption.
- **Memcached**
  - Supports SASL-based authentication (advanced).

# 5. Replication

## 5.1. ElastiCache Replication: Cluster Mode Disabled

- One primary node, up to 5 replicas.
- Asynchronous Replication.
- The primary node is used for read/write.
- The other nodes are read-only.
- **One shard, all nodes have all the data.**
- Guard against data loss if node failure.
- Multi-AZ enabled by default for failover.
- Helpful to scale read performance.

## 5.2. Redis Scaling - Cluster Mode Disabled

- **Horizontal**
  - Scale out/in by adding/removing read replicas (max. 5 replicas).
- **Vertical**
  - Scale up/down to larger/smaller node type.
  - ElastiCache will internally create a new node group, then data replication and DNS update.

## 5.3. Cluster Mode ENABLED

- **Data is partitioned across shards (helpful to scale writes).**
- Each shard has a primary and up to 5 replica nodes (same concept as before).
- Multi-AZ capability.
- Up to 500 nodes per cluster:
  - 500 shards with single master.
  - 250 shards with 1 master and 1 replica.
  - ...
  - 83 shards with one master and 5 replicas.

## 5.4. ElastiCache for Redis - Auto Scaling

- Automatically increase/decrease the desired shards or replicas.
- Supports both **Target Tracking and Scheduled** Scaling Policies.
- Works only for Redis with **Cluster Mode Enabled**.

## 5.5. ElastiCache - Redis Connection Endpoints

- **Standalone Node**
  - One endpoint for read and write operations.
- **Cluster Mode Disabled Cluster**
  - **Primary Endpoint:** For all write operations.
  - **Reader Endpoint:** Evenly split read operations across all read replicas.
  - **Node Endpoint:** For read operations.
- **Cluster Mode Enabled Cluster**
  - **Configuration Endpoint:** For all read/write operations that support Cluster Mode Enabled commands.
  - **Node Endpoint:** For read operations.

# 6. Caching Implementation Considerations

- Read more at: https://aws.amazon.com/caching/implementation-considerations/
- **Is it safe to cache data?** Data may be out of date, eventually consistent.
- **Is caching effective for that data?**
  - Pattern: Data changing slowly, few keys are frequently needed.
  - Anti patterns: data changing rapidly, all large key space frequently needed.
- **Is data structured well for caching?**
  - Example: Key value caching, or caching of aggregations results.
- Which caching design pattern is the most appropriate?

## 6.1. Patterns for ElastiCache

- **Lazy Loading**
  - All the read data is cached, data can become stale in cache.
- **Write Through**
  - Adds or update data in the cache when written to a DB (no stale data).
- **Session Store**
  - Store temporary session data in a cache (using TTL features).
- **Quote: There are only two hard things in Computer Science: Cache invalidation and naming things.**

## 6.2. Lazy Loading / Cache-Aside / Lazy Population

- Lazy Loading would load data into the cache only when necessary (actively requested data from the database).
- Pros:
  - Only requested data is cached (the cache isn't filled up with unused data).
  - Node failures are not fatal (just increased latency to warm the cache).
- Cons
  - Cache miss penalty that results in 3 round trips, noticeable delay for that request.
  - Stale data: data can be updated in the database and outdated in the cache.

![Amazon ElastiCache Solution Architecture - DB Cache](/Images/Database/AmazonElastiCacheSolutionArchitectureDbCache.png)

```
  // Pseudo code

  if (record_value == NULL)
  {
    record_value = db.query("SELECT Details FROM Records WHERE ID == {0}", record_key)
    cache.set (record_key, record_value)
  }
```

## 6.3. Write Through - Add or Update cache when database is updated

- Pros:
  - Data in cache is never stale, reads are quick.
  - Write penalty vs Read penalty (each write requires 2 calls).
- Cons:
  - Missing Data until it is added / updated in the DB. Mitigation is to implement Lazy Loading strategy as well.
  - Cache churn - a lot of the data will never be read.

# 7. Cache Evictions and Time-to-live (TTL)

- Cache eviction can occur in three ways:
  - We delete the item explicitly in the cache.
  - Item is evicted because the memory is full and it's not recently used (LRU).
  - We set an item **time-to-live (or TTL)**.
- TTL are helpful for any kind of data:
  - Leaderboards.
  - Comments.
  - Activity streams.
- TTL can range from few seconds to hours or days.
- If too many evictions happen due to memory, we should scale up or out.

# 8. Final words of wisdom

- Lazy Loading / Cache aside is easy to implement and works for many situations as a foundation, especially on the read side.
- Write-through is usually combined with Lazy Loading as targeted for the queries or workloads that benefit from this optimization.
- Setting a TTL is usually not a bad idea, except when we're using Write- through. Set it to a sensible value for your application.
- Only cache the data that makes sense (user profiles, blogs, etc...).
- Quote: There are only two hard things in Computer Science: cache invalidation and naming things.

# 9. Use Case

- Gaming Leaderboards are computationally complex.
- **Redis Sorted sets** guarantee both uniqueness and element ordering.
- Each time a new element added, it's ranked in real time, then added in correct order.
  ![Amazon ElastiCache Use Case](/Images/Database/AmazonElastiCacheUseCase.png)

# 10. Geospatial Support

- Redis includes commands for **real-time geospatial operations** like:
  - Calculating distances between locations.
  - Finding all elements within a certain radius.

# 11. Summary

- Managed Redis / Memcached (similar offering as RDS, but for caches).
- In-memory data store, sub-millisecond latency.
- Select an ElastiCache instance type (e.g., cache.m6g.large).
- Support for Clustering (Redis) and Multi-AZ, Read Replicas (sharding).
- Security through IAM, Security Groups, KMS, Redis Auth.
- Backup / Snapshot / Point in time restore feature.
- Managed and Scheduled maintenance.
- **Requires some application code changes to be leveraged.**
- **Use Case:** Key/Value store, Frequent reads, less writes, cache results for DB queries, store session data for websites, cannot use SQL.
