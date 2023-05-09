# Tips to remember<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. S3](#1-s3)
- [2. DynamoDB](#2-dynamodb)
- [3. Kinesys](#3-kinesys)

# 1. S3

- Your application can achieve at least:
  - **3,500 PUT/COPY/POST/DELETE.**
  - **5,500 GET/HEAD requests per second per prefix in a bucket.**

# 2. DynamoDB

- **UnprocessedKeys** for failed read operations (exponential backoff or add RCU).
  - For example, The DynamoDB API returns up to 100 items limited to 16MB per call. If there are still missing records to be returned, the API returns UnprocessedKeys until the process is finished.
    - In the present case, although we have 50 records, their total size is 50 x 2MB = 100MB, which exceeds the 16MB limit, so the message will be returned as follows:
      - Sixth call returns 16MB - 6 UnprocessedKeys (Total returned 92MB)
      - Seventh call returns 8MB - Query process terminated (Total returned 100MB)

# 3. Kinesys

- 1 Shard = Capture, production or ingestion 1 MB / Distribution or consume 2 MB
