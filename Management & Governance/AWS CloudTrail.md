# AWS CloudTrail <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Events](#2-events)
- [3. Insights](#3-insights)
- [4. Events Retention](#4-events-retention)
- [5. DynamoDB information in CloudTrail](#5-dynamodb-information-in-cloudtrail)
- [6. Amazon EventBridge - Intercept API Calls](#6-amazon-eventbridge---intercept-api-calls)
- [7. Amazon EventBridge + CloudTrail](#7-amazon-eventbridge--cloudtrail)
- [8. Log file Integrity Validation](#8-log-file-integrity-validation)
  - [8.1. Digest file](#81-digest-file)
- [9. CloudTrail vs CloudWatch vs X-Ray](#9-cloudtrail-vs-cloudwatch-vs-x-ray)

# 1. Introduction

- **Provides governance, compliance and audit for your AWS Account.**
- CloudTrail is enabled by default!
- Get **an history of events / API calls made within your AWS Account** by:
  - Console.
  - SDK.
  - CLI.
  - AWS Services.
- Can put logs from CloudTrail into CloudWatch Logs or S3.
- **A trail can be applied to All Regions (default) or a single Region.**
- If a resource is deleted in AWS, investigate CloudTrail first!
  ![AWS CloudTrail diagram](/Images/Management%20&%20Governance/AWSCloudTrailDiagram.png)

# 2. Events

- Three kinds of events in AWS CloudTrail:
  - **Management Events**
    - Operations that are performed on resources in your AWS account.
    - **Examples**
      - Configuring security (IAM `AttachRolePolicy`).
      - Configuring rules for routing data (Amazon EC2 `CreateSubnet`).
      - Setting up logging (AWS CloudTrail `CreateTrail`).
    - **By default, trails are configured to log management events.**
    - Can separate **Read Events** (that don't modify resources) from **Write Events** (that may modify resources).
  - **Data Events**
    - **By default, data events are not logged (because high volume operations).**
    - Amazon S3 object-level activity (ex: `GetObject`, `DeleteObject`, `PutObject`): Can separate Read and Write Events.
    - AWS Lambda function execution activity (the Invoke API).
  - **Insights**
    - Next topic.

# 3. Insights

- **AWS CloudTrail Insights helps AWS users identify and respond to unusual activity associated with write API calls by continuously analyzing CloudTrail management events.**
- Enable **CloudTrail Insights to detect unusual activity** in your account:
  - Inaccurate resource provisioning.
  - Hitting service limits.
  - Bursts of AWS IAM actions.
  - Gaps in periodic maintenance activity.
- CloudTrail Insights analyzes normal management events to create a baseline.
- And then **continuously analyzes write events to detect unusual patterns**:
  - Anomalies appear in the CloudTrail console.
  - Event is sent to Amazon S3.
  - An EventBridge event is generated (for automation needs).
    ![AWS CloudTrail Insights Events](/Images/Management%20&%20Governance/AWSCloudTrailInsights.png)

# 4. Events Retention

- Events are stored for 90 days in CloudTrail.
- To keep events beyond this period, log them to [Amazon S3](/Storage/Amazon%20S3.md) and use [Amazon Athena](/Analytics/Amazon%20Athena.md).
  ![Events Retention](/Images/Management%20&%20Governance/AWSCloudTrailEventsRetention.png)

# 5. DynamoDB information in CloudTrail

- When supported event activity occurs in DynamoDB, that activity is recorded in a CloudTrail event along with other AWS service events in Event history.
- You can view, search, and download recent events in your AWS account.

# 6. Amazon EventBridge - Intercept API Calls

![Intercept API Calls](/Images/AmazonEventBridgeInterceptAPICalls.png)

# 7. Amazon EventBridge + CloudTrail

- User -> `AssumeRole` -> `API Call logs` -> CloudTrail -> `event` -> EventBridge -> SNS
- User -> `Edit SG Inbound Rules` -> EC2 -> `API Call logs` -> CloudTrail -> `event` -> EventBridge -> SNS

# 8. Log file Integrity Validation

- To determine whether a log file was modified, deleted, or unchanged after CloudTrail delivered it, you can use CloudTrail **Log File Integrity Validation**.
- This feature is built using industry-standard algorithms: SHA-256 for hashing and SHA-256 with RSA for digital signing.
- This makes it computationally infeasible to modify, delete or forge CloudTrail log files without detection. You can use the AWS CLI to validate the files in the location where CloudTrail delivered them.

![Log file validation parameter](/Images/Management%20&%20Governance/AWSCloudTrailLogFileValidationParameter.png)

## 8.1. Digest file

- When you enable log file integrity validation, CloudTrail creates a hash for every log file that it delivers.
- Every hour, CloudTrail also creates and delivers a file that references the log files for the last hour and contains a hash of each.
- This file is called a **digest file**.
- CloudTrail signs each digest file using the private key of a public and private key pair.
- After delivery, you can use the public key to validate the digest file.
- CloudTrail uses different key pairs for each AWS region.
- **The digest files are delivered to the same Amazon S3 bucket associated with your trail as your CloudTrail log files.**

# 9. CloudTrail vs CloudWatch vs X-Ray

- **CloudTrail**
  - Audit API calls made by users / services / AWS console.
  - Useful to detect unauthorized calls or root cause of changes.
- **CloudWatch**
  - CloudWatch Metrics over time for monitoring.
  - CloudWatch Logs for storing application log.
  - CloudWatch Alarms to send notifications in case of unexpected metrics.
- **X-Ray**
  - Automated Trace Analysis & Central Service Map Visualization.
  - Latency, Errors and Fault analysis.
  - Request tracking across distributed systems.
