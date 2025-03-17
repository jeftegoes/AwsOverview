# AWS DynamoDB <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Traditional Architecture](#1-traditional-architecture)
- [2. NoSQL databases](#2-nosql-databases)
- [3. Amazon DynamoDB](#3-amazon-dynamodb)
- [4. Details](#4-details)
- [5. Primary Keys](#5-primary-keys)
- [6. Read/Write Capacity Modes](#6-readwrite-capacity-modes)
  - [6.1. R/W Capacity Modes - Provisioned](#61-rw-capacity-modes---provisioned)
    - [6.1.1. Write Capacity Units (WCU)](#611-write-capacity-units-wcu)
    - [6.1.2. Strongly Consistent Read vs. Eventually Consistent Read](#612-strongly-consistent-read-vs-eventually-consistent-read)
    - [6.1.3. Read Capacity Units (RCU)](#613-read-capacity-units-rcu)
    - [6.1.4. Partitions Internal](#614-partitions-internal)
    - [6.1.5. Throttling](#615-throttling)
  - [6.2. R/W Capacity Modes - On-Demand](#62-rw-capacity-modes---on-demand)
- [7. DynamoDB Basic APIs](#7-dynamodb-basic-apis)
  - [7.1. Writing Data](#71-writing-data)
  - [7.2. Reading Data](#72-reading-data)
  - [7.3. Reading Data (Query)](#73-reading-data-query)
  - [7.4. Reading Data (Scan)](#74-reading-data-scan)
    - [7.4.1. Rate-limited parallel scan](#741-rate-limited-parallel-scan)
  - [7.5. Deleting Data](#75-deleting-data)
  - [7.6. Batch Operations](#76-batch-operations)
  - [7.7. Extra](#77-extra)
- [8. Indexes](#8-indexes)
  - [8.1. Local Secondary Index (LSI)](#81-local-secondary-index-lsi)
  - [8.2. Global Secondary Index (GSI)](#82-global-secondary-index-gsi)
  - [8.3. Indexes and Throttling](#83-indexes-and-throttling)
- [9. PartiQL](#9-partiql)
- [10. Conditional Writes](#10-conditional-writes)
  - [10.1. Example on Delete Item](#101-example-on-delete-item)
  - [10.2. Do Not Overwrite Elements](#102-do-not-overwrite-elements)
  - [10.3. Example of String Comparisons](#103-example-of-string-comparisons)
- [11. Optimistic Locking](#11-optimistic-locking)
- [12. Accelerator - DAX](#12-accelerator---dax)
  - [12.1. Use cases for DAX](#121-use-cases-for-dax)
- [13. Streams](#13-streams)
  - [13.1. DynamoDB Streams and AWS Lambda](#131-dynamodb-streams-and-aws-lambda)
- [14. Global Tables](#14-global-tables)
- [15. Time To Live (TTL)](#15-time-to-live-ttl)
- [16. DynamoDB CLI - Good to Know](#16-dynamodb-cli---good-to-know)
- [17. Transactions](#17-transactions)
  - [17.1. Capacity Computations](#171-capacity-computations)
- [18. Session State Cache](#18-session-state-cache)
- [19. Write Sharding](#19-write-sharding)
- [20. Write Types](#20-write-types)
- [21. Operations](#21-operations)
- [22. Security \& Other Features](#22-security--other-features)
  - [22.1. Server-side encryption at rest](#221-server-side-encryption-at-rest)
  - [22.2. DynamoDB Encryption Client](#222-dynamodb-encryption-client)
  - [22.3. Fine-Grained Access Control](#223-fine-grained-access-control)
- [23. Backups for disaster recovery](#23-backups-for-disaster-recovery)
- [24. Integration with Amazon S3](#24-integration-with-amazon-s3)
- [25. Summary](#25-summary)

# 1. Traditional Architecture

- Traditional applications leverage RDBMS databases.
- These databases have the SQL query language.
- Strong requirements about how the data should be modeled.
- Ability to do query joins, aggregations, complex computations.
- Vertical scaling (getting a more powerful CPU / RAM / IO).
- Horizontal scaling (increasing reading capability by adding EC2 / RDS Read Replicas).

# 2. NoSQL databases

- NoSQL databases are non-relational databases and are distributed.
- NoSQL databases include MongoDB, DynamoDB, ...
- NoSQL databases do not support query joins (or just limited support).
- All the data that is needed for a query is present in one row.
- NoSQL databases don't perform aggregations such as "SUM", "AVG", ...
- **NoSQL databases scale horizontally.**
- There's no "right or wrong" for NoSQL vs SQL, they just require to model the data differently and think about user queries differently

# 3. Amazon DynamoDB

- Fully managed, highly available with replication across multiple AZs.
- NoSQL database - not a relational database - with transaction support.
- Scales to massive workloads, distributed **serverless** database.
- Millions of requests per seconds, trillions of row, 100s of TB of storage.
- Fast and consistent in performance (low latency on retrieval).
- Integrated with IAM for security, authorization and administration.
- Enables event driven programming with DynamoDB Streams.
- Low cost and auto-scaling capabilities.
- No maintenance or patching, always available.
- Standard and Infrequent Access (IA) Table Class.

# 4. Details

- DynamoDB is made of **Tables**.
- Each table has a **Primary Key** (must be decided at creation time).
- Each table can have an infinite number of items (= rows).
- Each item has **attributes** (can be added over time - can be null).
- Maximum size of an item is **400KB**.
- Data types supported are:
  - **Scalar Types:** String, Number, Binary, Boolean, Null.
  - **Document Types:** List, Map.
  - **Set Types:** String Set, Number Set, Binary Set.t
- Therefore, in DynamoDB you can rapidly evolve schemas.

# 5. Primary Keys

- **Option 1: Partition Key (HASH)**
  - Partition key must be unique for each item.
  - Partition key must be "diverse" so that the data is distributed.
  - Example: "User_ID" for a users table.
- **Option 2: Partition Key + Sort Key (HASH + RANGE)**
  - The combination must be unique for each item.
  - Data is grouped by partition key.
  - Example: users-games table, "User_ID" for Partition Key and "Game_ID" for Sort Key.
- What is the best Partition Key to maximize data distribution?
  - movie_id
  - producer_name
  - leader_actor_name
  - movie_language
- "movie_id" has the highest cardinality so it's a good candidate.
- "movie_language" doesn't take many values and may be skewed towards English so it's not a great choice for the Partition Key.

# 6. Read/Write Capacity Modes

- Control how you manage your table's capacity (read/write throughput).
- **Provisioned Mode (default)**
  - You specify the number of reads/writes per second.
  - You need to plan capacity beforehand.
  - Pay for provisioned Read Capacity Units (RCU) & Write Capacity Units (WCU).
  - Possibility to add auto-scaling mode for RCU & WCU.
- **On-Demand Mode**
  - Read/writes automatically scale up/down with your workloads.
  - No capacity planning needed.
  - Pay for what you use, more expensive ($$$).
  - Great for unpredictable workloads, steep sudden spikes.
- You can switch between different modes once every 24 hours.

## 6.1. R/W Capacity Modes - Provisioned

- Table must have provisioned read and write capacity units.
- **Read Capacity Units (RCU)** - throughput for reads.
- **Write Capacity Units (WCU)** - throughput for writes.
- Option to setup auto-scaling of throughput to meet demand.
- Throughput can be exceeded temporarily using **"Burst Capacity"**.
- If Burst Capacity has been consumed, you'll get a `ProvisionedThroughputExceededException`.
- It's then advised to do an **exponential backoff** retry.

### 6.1.1. Write Capacity Units (WCU)

- One Write Capacity Unit (WCU) represents one write per second for an item up to 1 KB in size.

![WCU Formula](/Images/DynamoDBWCUFormula.png)

- If the items are larger than 1 KB, more WCUs are consumed:
  - Example 1: we write 10 items per second, with item size 2 KB:
    - We need 10 \* (2/1) = 20 WCUs.
  - Example 2: we write 6 items per second, with item size 4.5 KB
    - We need 6 \* (5/1) = 30 WCUs (4.5 gets rounded to the upper KB).
  - Example 3: we write 120 items per minute, with item size 2 KB
    - We need (120/60) \* (2/1) = 4 WCUs

### 6.1.2. Strongly Consistent Read vs. Eventually Consistent Read

- **Eventually Consistent Read (default)**
  - If we read just after a write, it's possible we'll get some stale data because of replication.
- **Strongly Consistent Read**
  - If we read just after a write, we will get the correct data.
  - Set `ConsistentRead` parameter to `TRUE` in API calls (`GetItem`, `BatchGetItem`, `Query`, `Scan`).
    - Consumes twice the RCU.

### 6.1.3. Read Capacity Units (RCU)

- One Read Capacity Unit (RCU) represents:
  - **One (1) Strongly Consistent Read** per second
  - **Two (2) Eventually Consistent Reads** per second
- For an item up to 4 **KB** in size.

![WCU Formula](/Images/DynamoDBRCUFormula.png)

- If the items are larger than 4 KB, more RCUs are consumed:
  - Example 1: 10 Strongly Consistent Reads per second, with item size 4 KB
    - We need 10 \* (4kb/4kb) = 10 RCUs.
  - Example 2: 16 Eventually Consistent Reads per second, with item size 12 KB
    - We need (16/2) \* (12kb/4kb) = 24 RCUs (4.5 gets rounded to the upper KB).
  - Example 3: 10 Strongly Consistent Reads per second, with item size 6 KB
    - We need 10 \* (8kb/4kb) = 20 RCUs (We must round up 6 KB to 8 KB)

### 6.1.4. Partitions Internal

- Data is stored in partitions.
- Partition Keys go through a hashing algorithm to know to which partition they go to.

### 6.1.5. Throttling

- If we exceed provisioned RCUs or WCUs, we get `ProvisionedThroughputExceededException`.
- Reasons:
  - **Hot Keys** - one partition key is being read too many times (e.g., popular item).
  - **Hot Partitions.**
  - **Very large items**, RCU and WCU depends on size of items.
- Solutions:
  - **Exponential backoff** when exception is encountered (already in SDK).
  - **Distribute partition keys** as much as possible.
  - If RCU issue, we can **use DynamoDB Accelerator (DAX)**.

## 6.2. R/W Capacity Modes - On-Demand

- Read/writes automatically scale up/down with your workloads.
- No capacity planning needed (WCU / RCU).
- Unlimited WCU and RCU, no throttle, more expensive.
- You're charged for reads/writes that you use in terms of **RRU** and **WRU**.
- **Read Request Units (RRU)** - throughput for reads (same as RCU).
- **Write Request Units (WRU)** - throughput for writes (same as WCU).
- 2.5x more expensive than provisioned capacity (use with care).
- **Use cases:** Unknown workloads, unpredictable application traffic, ...

# 7. DynamoDB Basic APIs

## 7.1. Writing Data

- `PutItem`
  - Creates a new item or fully replace an old item (same Primary Key).
  - Consumes WCUs.
- `UpdateItem`
  - Edits an existing item's attributes or adds a new item if it doesn't exist.
  - Can be used to implement **Atomic Counters** - A numeric attribute that's unconditionally incremented.
- **Conditional Writes:**
  - Accept a write/update/delete only if conditions are met, otherwise returns an error.
  - Helps with concurrent access to items.
  - No performance impact.

## 7.2. Reading Data

- `GetItem`
  - Read based on Primary key.
  - Primary Key can be HASH or HASH+RANGE.
  - Eventually Consistent Read (default).
  - Option to use Strongly Consistent Reads (more RCU - might take longer).
  - `ProjectionExpression` can be specified to retrieve only certain attributes.

## 7.3. Reading Data (Query)

- `Query` returns items based on:
  - **KeyConditionExpression:**
    - Partition Key value (must be = operator) - required.
    - Sort Key value (=, <, <=, >, >=, Between, Begins with) - optional.
  - **FilterExpression:**
    - Additional filtering after the Query operation (before data returned to you).
    - Use only with non-key attributes (does not allow HASH or RANGE attributes).
- Returns:
  - The number of items specified in **Limit**.
  - Or up to 1 MB of data.
- Ability to do pagination on the results.
- Can query table, a Local Secondary Index, or a Global Secondary Index.

## 7.4. Reading Data (Scan)

- `Scan` the entire table and then filter out data (inefficient).
- Returns up to 1 MB of data - use pagination to keep on reading.
- Consumes a lot of RCU.
- Limit impact using `Limit` or reduce the size of the result and pause.
- For faster performance, use **Rate-limited parallel `scan`**.
  - Multiple workers scan multiple data segments at the same time.
  - Increases the throughput and RCU consumed.
  - Limit the impact of parallel scans just like you would for Scans.
- Can use `ProjectionExpression` and `FilterExpression` (no changes to RCU).

### 7.4.1. Rate-limited parallel scan

- By default, the `Scan` operation processes **data sequentially**.
- Amazon DynamoDB returns data to the application in 1 MB increments, and an application performs additional `Scan` operations to retrieve the next 1 MB of data.
- To address these issues, the `Scan` operation can logically divide a table or secondary index into multiple segments, with multiple application workers scanning the segments in parallel.
- Each worker can be a thread (in programming languages that support multithreading) or an operating system process.

## 7.5. Deleting Data

- `DeleteItem`
  - Delete an individual item.
  - Ability to perform a conditional delete.
- `DeleteTable`
  - Delete a whole table and all its items.
  - Much quicker deletion than calling DeleteItem on all items.

## 7.6. Batch Operations

- Allows you to save in latency by reducing the number of API calls.
- Operations are done in parallel for better efficiency.
- Part of a batch can fail; in which case we need to try again for the failed items.
- `BatchWriteItem`
  - Up to 25 `PutItem` and/or `DeleteItem` in one call.
  - Up to 16 MB of data written, up to 400 KB of data per item.
  - Can't update items (use `UpdateItem`).
  - `UnprocessedItems` for failed write operations (exponential backoff or add WCU).
- `BatchGetItem`
  - Return items from one or more tables.
  - Up to 100 items, up to 16 MB of data.
  - Items are retrieved in parallel to minimize latency.
  - `UnprocessedKeys` for failed read operations (exponential backoff or add RCU).
    - For example, The DynamoDB API returns up to 100 items limited to 16MB per call. If there are still missing records to be returned, the API returns `UnprocessedKeys` until the process is finished.
      - In the present case, although we have 50 records, their total size is 50 x 2MB = 100MB, which exceeds the 16MB limit, so the message will be returned as follows:
        - Sixth call returns 16MB - 6 `UnprocessedKeys` (Total returned 92MB)
        - Seventh call returns 8MB - Query process terminated (Total returned 100MB)

## 7.7. Extra

- To return the number of write capacity units consumed by any of these operations (`PutItem`, `UpdateItem`, `DeleteItem`), set the `ReturnConsumedCapacity` parameter to one of the following:
  - `TOTAL` — returns the total number of write capacity units consumed.
  - `INDEXES` — returns the total number of write capacity units consumed, with subtotals for the table and any secondary indexes that were affected by the operation.
  - `NONE` — no write capacity details are returned. (This is the default.)

# 8. Indexes

## 8.1. Local Secondary Index (LSI)

- **Alternative Sort Key** for your table (same **Partition Key** as that of base table).
- The Sort Key consists of one scalar attribute (String, Number, or Binary).
- Up to 5 Local Secondary Indexes per table.
- **Must be defined at table creation time.**
- **Attribute Projections** - can contain some or all the attributes of the base table **(KEYS_ONLY, INCLUDE, ALL)**.
- When you query this index, you can choose **either eventual consistency or strong consistency**.
- For each partition key value, the total size of all indexed items must be **10 GB** or less.
- Queries or scans on this index consume read capacity units from the base table.

## 8.2. Global Secondary Index (GSI)

- **Alternative Primary Key (HASH or HASH+RANGE)** from the base table.
- Speed up queries on non-key attributes.
- The Index Key consists of scalar attributes (String, Number, or Binary).
- **Attribute Projections** - some or all the attributes of the base table **(KEYS_ONLY, INCLUDE, ALL)**.
- Must provision RCUs and WCUs for the index.
- **Can be added/modified after table creation.**
- `Queries` or `scans` on this index consume capacity units from the index, not from the base table.
- Queries on this index **support eventual consistency only**.

## 8.3. Indexes and Throttling

- Global Secondary Index (GSI):
  - **If the writes are throttled on the GSI, then the main table will be throttled!**
  - Even if the WCU on the main tables are fine.
  - Choose your GSI partition key carefully!
  - Assign your WCU capacity carefully!
- Local Secondary Index (LSI):
  - Uses the WCUs and RCUs of the main table.
  - No special throttling considerations.

# 9. PartiQL

- SQL-compatible query language for DynamoDB.
- Allows you to select, insert, update, and delete (but not all) data in DynamoDB using SQL.
- Run queries across multiple DynamoDB tables.
- Run PartiQL queries from:
  - AWS Management Console.
  - NoSQL Workbench for DynamoDB.
  - DynamoDB APIs.
  - AWS CLI.
  - AWS SDK.
- It supports Batch operations.

# 10. Conditional Writes

- For `PutItem`, `UpdateItem`, `DeleteItem`, and `BatchWriteItem`.
- You can specify a **Condition Expression** to determine which items should be modified:
  - **attribute_exists**
  - **attribute_not_exists**
  - **attribute_type**
  - **contains** (for string)
  - **begins_with** (for string)
  - ProductCategory **IN** (:cat1, :cat2) **and** Price **between** :low and :high
  - **size** (string length)
- **Note: Filter Expression filters the results of read queries, while Condition Expressions are for write operations.**

## 10.1. Example on Delete Item

- **attribute_not_exists**
  - Only succeeds if the attribute doesn't exist yet (no value).
- **attribute_exists**
  - Opposite of attribute_not_exists.

## 10.2. Do Not Overwrite Elements

- **attribute_not_exists** (partition_key)
  - Make sure the item isn't overwritten.
- **attribute_not_exists** (partition_key) and **attribute_not_exists** (sort_key)
  - Make sure the partition / sort key combination is not overwritten.

## 10.3. Example of String Comparisons

- **begins_with**
  - check if prefix matches.
- **contains**
  - check if string is contained in another string.

# 11. Optimistic Locking

- A strategy to ensure an item hasn't changed before you update/delete it.
- DynamoDB has a feature called **"Conditional Writes"**.
- Each item has an attribute that acts as a version number.

![Optimistic Locking](/Images/DynamoDBOptimisticLocking.png)

# 12. Accelerator - DAX

- **DynamoDB that delivers up to 10x performance improvement.**
- **It caches the most frequently used data, thus offloading the heavy reads on hot keys of your DynamoDB table, hence preventing the `ProvisionedThroughputExceededException` exception.**
- Fully-managed, highly available, seamless in-memory cache for DynamoDB.
- **Help solve read congestion by caching.**
- **Microseconds latency for cached data.**
- Doesn't require application logic modification (compatible with existing DynamoDB APIs).
- Solves the "Hot Key" problem (too many reads).
- 5 minutes TTL for cache (default).
- Up to 10 nodes in the cluster.
- Multi-AZ (3 nodes minimum recommended for production).
- Secure (Encryption at rest with KMS, VPC, IAM, CloudTrail, ...)

## 12.1. Use cases for DAX

- DAX is ideal for the following types of applications:
  - Applications that require the fastest possible response time for reads.
  - Applications that read a small number of items more frequently than others.
  - Applications that are read-intensive, but are also cost-sensitive.
  - Applications that require repeated reads against a large set of data.

# 13. Streams

- Ordered stream of item-level modifications (create/update/delete) in a table.
- Stream records can be:
  - Sent to **Kinesis Data Streams**.
  - Read by **AWS Lambda**.
  - Read by **Kinesis Client Library applications**.
- Data Retention for up to 24 hours.
- **Use cases:**
  - React to changes in real-time (welcome email to users).
  - Analytics.
  - Insert into derivative tables.
  - Insert into ElasticSearch.
  - Implement cross-region replication.
- Ability to choose the information that will be written to the stream:
  - **KEYS_ONLY** - only the key attributes of the modified item.
  - **NEW_IMAGE** - the entire item, as it appears after it was modified.
  - **OLD_IMAGE** - the entire item, as it appeared before it was modified.
  - **NEW_AND_OLD_IMAGES** - both the new and the old images of the item.
- DynamoDB Streams are made of shards, just like Kinesis Data Streams.
- You don't provision shards, this is automated by AWS.
- **Records are not retroactively populated in a stream after enabling it.**

## 13.1. DynamoDB Streams and AWS Lambda

- **DynamoDB Streams allows you to capture a time-ordered sequence of item-level modifications in a DynamoDB table.**
- **It's integrated with AWS Lambda so that you create triggers that automatically respond to events in real-time.**
- You need to define an **Event Source Mapping** to read from a DynamoDB Streams.
- You need to ensure the Lambda function has the appropriate permissions.
- **Your [AWS Lambda](AWS%20Lambda.md) function is invoked synchronously.**

# 14. Global Tables

- Make a DynamoDB table accessible with low latency in multiple-regions.
- Active-Active replication.
- Applications can READ and WRITE to the table in any region.
- Must enable DynamoDB Streams as a pre-requisite.

# 15. Time To Live (TTL)

- Automatically delete items after an expiry timestamp.
- Doesn't consume any WCUs (i.e., no extra cost).
- The TTL attribute must be a "Number" data type with "Unix Epoch timestamp" value.
- Expired items deleted within 48 hours of expiration.
- Expired items, that haven't been deleted, appears in reads/queries/scans (if you don't want them, filter them out).
- Expired items are deleted from both LSIs and GSIs.
- A delete operation for each expired item enters the DynamoDB Streams (can help recover expired items).
- **Use cases:** reduce stored data by keeping only current items, adhere to regulatory obligations, ...

# 16. DynamoDB CLI - Good to Know

- `--projection-expression` - One or more attributes to retrieve.
- `--filter-expression` - Filter items before returned to you.
- **General AWS CLI Pagination options (e.g., DynamoDB, S3, ...)**
  - `--page-size` - Specify that AWS CLI retrieves the full list of items but with a larger number of API calls instead of one API call (default: 1000 items).
  - `--max-items` - Max. number of items to show in the CLI (returns NextToken).
  - `--starting-token` - Specify the last NextToken to retrieve the next set of items.
    [Commands section](README.md)

# 17. Transactions

![Transactions formulas](/Images/DynamoDBTransactionsFormulas.png)

- Coordinated, all-or-nothing operations (add/update/delete) to multiple items across one or more tables.
- Provides Atomicity, Consistency, Isolation, and Durability (ACID).
- **Read Modes:** Eventual Consistency, Strong Consistency, Transactional.
- **Write Modes:** Standard, Transactional.
- **Consumes 2x WCUs & RCUs.**
  - DynamoDB performs 2 operations for every item (prepare & commit).
- Two operations: (up to 25 unique items or up to 4 MB of data):
  - `TransactGetItems` - one or more `GetItem` operations.
  - `TransactWriteItems` - one or more `PutItem`, `UpdateItem`, and `DeleteItem` operations.
- **Use cases:** financial transactions, managing orders, multiplayer games, ...

## 17.1. Capacity Computations

\*\*

- **Example 1:** 3 Transactional writes per second, with item size 5 KB:
  - We need 3 _(5kb / 1kb)_ 2 (transactional cost) = 30 WCUs.
- **Example 2:** 5 Transaction reads per second , with item size 5 KB:
  - We need 5 _(8gb / 4kb)_ 2 (transactional cost) = 20 RCUs.
  - (5 gets rounded to the upper 4 KB).

# 18. Session State Cache

- It's common to use DynamoDB to store session states.
- **vs. ElastiCache**
  - ElastiCache is in-memory, but DynamoDB is serverless.
  - Both are key/value stores.
- **vs. EFS**
  - EFS must be attached to EC2 instances as a network drive.
- **vs. EBS & Instance Store**
  - EBS & Instance Store can only be used for local caching, not shared caching.
- **vs. S3**
  - S3 is higher latency, and not meant for small objects.

# 19. Write Sharding

- Imagine we have a voting application with two candidates, **candidate A** and **candidate B**.
- If **Partition Key** is **"Candidate_ID"**, this results into two partitions, which will generate issues (e.g., Hot Partition).
- A strategy that allows better distribution of items evenly across partitions.
- **Add a suffix to Partition Key value.**
- Two methods:
  - Sharding Using Random Suffix.
  - Sharding Using Calculated Suffix.

# 20. Write Types

- Concurrent Writes
- Atomic Writes
- Conditional Writes
- Batch Write

# 21. Operations

- **Table Cleanup:**
  - **Option 1:** Scan + DeleteItem
    - Very slow, consumes RCU & WCU, expensive
  - **Option 2:** Drop Table + Recreate table
    - Fast, efficient, cheap
- **Copying a DynamoDB Table:**
  - **Option 1:** Using AWS Data Pipeline
  - **Option 2:** Backup and restore into a new table
    - Takes some time
  - **Option 3:** Scan + PutItem or BatchWriteItem
    - Write your own code

# 22. Security & Other Features

- When creating a new table, you can choose one of the following AWS KMS key types to encrypt your table. **You can switch between these key types at any time**.
  - **AWS owned key:** Default encryption type. The key is owned by DynamoDB (no additional charge)
  - **AWS managed key:** The key is stored in your account and is managed by AWS KMS (AWS KMS charges apply).
  - **Customer managed key:** The key is stored in your account and is created, owned, and managed by you. You have full control over the KMS key (AWS KMS charges apply).
- **Security:**
  - VPC Endpoints available to access DynamoDB without using the Internet.
  - Access fully controlled by IAM.
  - Encryption at rest using AWS KMS and in-transit using SSL/TLS.
- **Global Tables:**
  - Multi-region, multi-active, fully replicated, high performance.
- **DynamoDB Local:**
  - Develop and test apps locally without accessing the DynamoDB web service (without Internet).
- AWS Database Migration Service (AWS DMS) can be used to migrate to DynamoDB (from MongoDB, Oracle, MySQL, S3, ...).

## 22.1. Server-side encryption at rest

- DynamoDB supports encryption at rest, a server-side encryption feature in which DynamoDB transparently encrypts your tables for you when the table is persisted to disk, and decrypts them when you access the table data.
- When you use an **AWS SDK** to interact with DynamoDB, by default, your data is encrypted in transit over an HTTPS connection, decrypted at the DynamoDB endpoint, and then re-encrypted before being stored in DynamoDB.
  - Encryption by default.
  - DynamoDB creates and manages the cryptographic keys.
  - All table data is encrypted on disk.
  - Objects related to tables are encrypted, too.
  - Your items are decrypted when you access them.

## 22.2. DynamoDB Encryption Client

- Client-side encryption provides end-to-end protection for your data, in transit and at rest, from its source to storage in DynamoDB. Your plaintext data is never exposed to any third party, including AWS. However, the DynamoDB Encryption Client is designed to be implemented in new, unpopulated databases. You need to add the encryption features to your DynamoDB applications before you send any data to DynamoDB.
  - Your data is protected in transit and at rest.
  - You can sign your table Items.
  - You choose how your cryptographic keys are generated and protected.
  - You determine how your data is protected.
  - The DynamoDB Encryption Client doesn't encrypt the entire table.

## 22.3. Fine-Grained Access Control

- Using **Web Identity Federation** or **Cognito Identity Pools**, each user gets AWS credentials.
- You can assign an IAM Role to these users with a **Condition** to limit their API access to DynamoDB.
- `LeadingKeys` - Limit row-level access for users on the **Primary Key**.
- `Attributes` - Limit specific attributes the user can see.

# 23. Backups for disaster recovery

- DynamoDB offers two built-in backup methods:
  - **Continuous backups using point-in-time recovery (PITR)**
    - Optionally enabled for the last 35 days.
    - Point-in-time recovery to any time within the backup window.
    - The recovery process creates a new table.
  - **On-demand backups**
    - Full backups for long-term retention, until explicitely deleted.
    - Doesn't affect performance or latency.
    - Can be configured and managed in AWS Backup (enables cross-region copy).
    - The recovery process creates a new table.
- Both of these methods are suitable for backing up your tables for disaster recovery purposes.
- However, with these methods, you can't use the data for use cases involving data analysis or extract, transform, and load (ETL) jobs.
- The DynamoDB Export to S3 feature is the easiest way to create backups that you can download locally or use with another AWS service.
- To customize the process of creating backups, you can use Amazon EMR, AWS Glue, or AWS Data Pipeline.

# 24. Integration with Amazon S3

- **Export to S3 (must enable PITR)**
  - Works for any point of time in the last 35 days.
  - Doesn't affect the read capacity of your table.
  - Perform data analysis on top of DynamoDB.
  - Retain snapshots for auditing.
  - ETL on top of S3 data before importing back into DynamoDB.
  - Export in DynamoDB JSON or ION format.
- **Import from S3**
  - Import CSV, DynamoDB JSON or ION format.
  - Doesn't consume any write capacity.
  - Creates a new table.
  - Import errors are logged in CloudWatch Logs.

# 25. Summary

- AWS proprietary technology, managed serverless NoSQL database, millisecond latency.
- Capacity modes: provisioned capacity with optional auto-scaling or on-demand capacity.
- Can replace ElastiCache as a key/value store (storing session data for example, using TTL feature).
- Highly Available, Multi AZ by default, Read and Writes are decoupled, transaction capability.
- DAX cluster for read cache, microsecond read latency.
- Security, authentication and authorization is done through IAM.
- Event Processing: DynamoDB Streams to integrate with AWS Lambda, or Kinesis Data Streams.
- Global Table feature: active-active setup.
- Automated backups up to 35 days with PITR (restore to new table), or on-demand backups.
- Export to S3 without using RCU within the PITR window, import from S3 without using WCU.
- **Great to rapidly evolve schemas.**
- **Use Case:** Serverless applications development (small documents 100s KB), distributed serverless cache.
