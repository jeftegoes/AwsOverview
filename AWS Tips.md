# Tips to remember<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. S3](#1-s3)
- [2. DynamoDB](#2-dynamodb)
- [3. Kinesys](#3-kinesys)
- [4. KMS](#4-kms)
- [5. General tips](#5-general-tips)
- [6. ECS](#6-ecs)
- [7. X-Ray](#7-x-ray)
- [8. Files](#8-files)

# 1. S3

- Your application can achieve at least:
  - **3,500 PUT/COPY/POST/DELETE.**
  - **5,500 GET/HEAD requests per second per prefix in a bucket.**
- Object values are the content of the body:
  - Max Object Size is 5TB (5000GB).
  - If uploading more than 5GB, must use "multi-part upload".

# 2. DynamoDB

- **UnprocessedKeys** for failed read operations (exponential backoff or add RCU).
  - For example, The DynamoDB API returns up to 100 items limited to 16MB per call. If there are still missing records to be returned, the API returns UnprocessedKeys until the process is finished.
    - In the present case, although we have 50 records, their total size is 50 x 2MB = 100MB, which exceeds the 16MB limit, so the message will be returned as follows:
      - Sixth call returns 16MB - 6 UnprocessedKeys (Total returned 92MB)
      - Seventh call returns 8MB - Query process terminated (Total returned 100MB)

# 3. Kinesys

- 1 Shard = Capture, production or ingestion 1 MB / Distribution or consume 2 MB

# 4. KMS

- Envelope Encryption
  - KMS Encrypt API call has a limit of 4 KB
  - If you want to encrypt >4 KB, we need to use Envelope Encryption

# 5. General tips

- Resource-based = AWS Cross account

# 6. ECS

- Task Placement Strategies
  - **Binpack**
    - Place tasks based on the least available amount of CPU or memory.
  - **Random**
    - Tasks are placed randomly.
  - **Spread**
  - Tasks are placed evenly based on the specified value.
    - Example: **instanceId, attribute:ecs.availability-zone, ...**

# 7. X-Ray

- **Segments:** Each application / service will send them.
- **Subsegments:** If you need more details in your segment.
  - `namespace` - aws for AWS SDK calls; remote for other downstream calls.
- **Trace:** Segments collected together to form an end-to-end trace.
- **Sampling:** Decrease the amount of requests sent to X-Ray, reduce cost.
- **Annotations:** Key Value pairs used to **index** traces and use with **filters**.
- **Metadata:** Key Value pairs, **not indexed**, not used for searching.
- The X-Ray daemon / agent has a config to send traces cross account:
  - Make sure the IAM permissions are correct - the agent will assume the role.
  - This allows to have a central account for all your application tracing.

# 8. Files

- buildspec.yml - CodeBuild
- appspec.yml - CodeDeploy
