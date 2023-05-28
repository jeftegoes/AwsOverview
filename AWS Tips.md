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
- [9. PartiQL](#9-partiql)
- [10. AWS CodeBuild](#10-aws-codebuild)
- [11. CodePipeline](#11-codepipeline)
- [12. ALG - Auto Scaling Group](#12-alg---auto-scaling-group)
- [13. CloudWatch Events](#13-cloudwatch-events)
- [14. VPC Endpoints](#14-vpc-endpoints)
- [15. Elastic Beanstalk](#15-elastic-beanstalk)
- [16. API Gateway - Errors](#16-api-gateway---errors)
- [17. ALB - HealthCheck](#17-alb---healthcheck)

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

- 1 Shard = Capture, production or ingestion 1 MB / Distribution or consume 2 MB.

# 4. KMS

- Envelope Encryption
  - KMS Encrypt API call has a limit of 4 KB.
  - If you want to encrypt >4 KB, we need to use Envelope Encryption.

# 5. General tips

- Resource-based = AWS Cross account.

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

- buildspec.yml - CodeBuild.
- appspec.yml - CodeDeploy.
- env.yml - Elastic Beanstalk.
  - .config - Configuration files are YAML- or JSON-formatted documents with a **.config** file extension that you place in a folder named **.ebextensions**.

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

# 10. AWS CodeBuild

- **Source:** CodeCommit, S3, Bitbucket, GitHub.

# 11. CodePipeline

- InProgress, Stopping, Stopped, Succeeded, Superseded e Failed.

# 12. ALG - Auto Scaling Group

- Lauch template = Lauch onfiguration.

# 13. CloudWatch Events

- CloudWatch Events = Amazon EventBridge.

# 14. VPC Endpoints

- Endpoints allow you to connect to AWS Services **using a private network** instead of the public www network.
- This gives you enhanced security and lower latency to access AWS services.
- **VPC Endpoint Gateway**: S3 and DynamoDB.
- **VPC Endpoint Interface**: The rest.

# 15. Elastic Beanstalk

![Elastic Beanstalk Workflow](Images/ElasticBeanstalkWorkflow.png)

# 16. API Gateway - Errors

- 4xx means Client errors:
  - **400:** Bad Request.
  - **403:** Access Denied, WAF filtered.
  - **429:**
    - Quota exceeded
    - Throttle.
- 5xx means Server errors:
  - **502:** Bad Gateway Exception, usually for an incompatible output returned from a Lambda proxy integration backend and occasionally for out-of-order invocations due to heavy loads.
  - **503:** Service Unavailable Exception.
  - **504:** Integration Failure - ex Endpoint Request Timed-out Exception API Gateway requests time out after 29 second maximum.

# 17. ALB - HealthCheck

- `HealthyThresholdCount` - The number of consecutive successful health checks required before considering an unhealthy target healthy. The range is 2–10. The default is 5.
- `UnhealthyThresholdCount` - The number of consecutive failed health checks required before considering a target unhealthy. The range is 2–10. The default is 2.
