# AWS CloudTrail<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Events](#2-events)
- [3. Insights](#3-insights)
- [4. Events Retention](#4-events-retention)
- [5. DynamoDB information in CloudTrail](#5-dynamodb-information-in-cloudtrail)
- [6. Amazon EventBridge + CloudTrail](#6-amazon-eventbridge--cloudtrail)
- [7. CloudTrail vs CloudWatch vs X-Ray](#7-cloudtrail-vs-cloudwatch-vs-x-ray)

# 1. Introduction

- **Is a web service that records activity made on your account and delivers log files to your Amazon S3 bucket. AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account.**
- **Can record the history of events/API calls made within you AWS account, which will help determine who or what deleted the resource. You should investigate it first.**
- Provides governance, compliance and audit for your AWS Account.
- CloudTrail is enabled by default!
- Get **an history of events / API calls made within your AWS Account** by:
  - Console.
  - SDK.
  - CLI.
  - AWS Services.
- Can put logs from CloudTrail into CloudWatch Logs or S3.
- **A trail can be applied to All Regions (default) or a single Region.**
- If a resource is deleted in AWS, investigate CloudTrail first!

![AWS CloudTrail diagram](/Images/AWSCloudTrailDiagram.png)

# 2. Events

- **Management Events**
  - Operations that are performed on resources in your AWS account.
  - Examples:
    - Configuring security (IAM AttachRolePolicy).
    - Configuring rules for routing data (Amazon EC2 CreateSubnet).
    - Setting up logging (AWS CloudTrail CreateTrail).
- **By default, trails are configured to log management events.**
- Can separate **Read Events** (that don't modify resources) from Write Events (that may modify resources).
- **Data Events**
  - **By default, data events are not logged (because high volume operations).**
  - Amazon S3 object-level activity (ex: `GetObject`, `DeleteObject`, `PutObject`): can separate Read and Write Events.
  - AWS Lambda function execution activity (the Invoke API).

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

# 4. Events Retention

- Events are stored for 90 days in CloudTrail.
- To keep events beyond this period, log them to S3 and use Athena.

# 5. DynamoDB information in CloudTrail

- When supported event activity occurs in DynamoDB, that activity is recorded in a CloudTrail event along with other AWS service events in Event history.
- You can view, search, and download recent events in your AWS account.

# 6. Amazon EventBridge + CloudTrail

- User -> `AssumeRole` -> `API Call logs` -> CloudTrail -> `event` -> EventBridge -> SNS
- User -> `Edit SG Inbound Rules` -> EC2 -> `API Call logs` -> CloudTrail -> `event` -> EventBridge -> SNS

# 7. CloudTrail vs CloudWatch vs X-Ray

- CloudTrail:
  - Audit API calls made by users / services / AWS console.
  - Useful to detect unauthorized calls or root cause of changes.
- CloudWatch:
  - CloudWatch Metrics over time for monitoring.
  - CloudWatch Logs for storing application log.
  - CloudWatch Alarms to send notifications in case of unexpected metrics.
- X-Ray:
  - Automated Trace Analysis & Central Service Map Visualization.
  - Latency, Errors and Fault analysis.
  - Request tracking across distributed systems.
