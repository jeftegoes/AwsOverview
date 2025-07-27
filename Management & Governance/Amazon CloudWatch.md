# Amazon CloudWatch <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Why Monitoring is Important](#1-why-monitoring-is-important)
- [2. Monitoring in AWS](#2-monitoring-in-aws)
- [3. Amazon CloudWatch Metrics](#3-amazon-cloudwatch-metrics)
  - [3.1. Metric Streams](#31-metric-streams)
  - [3.2. Custom Metrics](#32-custom-metrics)
- [4. EC2 Detailed monitoring](#4-ec2-detailed-monitoring)
- [5. CloudWatch Anomaly Detection](#5-cloudwatch-anomaly-detection)
- [6. Amazon Lookout for Metrics](#6-amazon-lookout-for-metrics)
- [7. Logs](#7-logs)
  - [7.1. Sources](#71-sources)
- [8. Logs Insights](#8-logs-insights)
- [9. Logs - S3 Export](#9-logs---s3-export)
- [10. Subscription Filter](#10-subscription-filter)
  - [10.1. Scenario with HIDS](#101-scenario-with-hids)
  - [10.2. Cross-Account Subscription](#102-cross-account-subscription)
- [11. CloudWatch Logs Aggregation Multi-Account \& Multi Region](#11-cloudwatch-logs-aggregation-multi-account--multi-region)
- [12. CloudWatch Logs Metric Filter](#12-cloudwatch-logs-metric-filter)
- [13. CloudWatch cross-account observability](#13-cloudwatch-cross-account-observability)
- [14. All kind of Logs](#14-all-kind-of-logs)
  - [14.1. AWS Managed Logs](#141-aws-managed-logs)
- [15. Logs for EC2](#15-logs-for-ec2)
  - [15.1. Logs Agent \& Unified Agent](#151-logs-agent--unified-agent)
    - [15.1.1. Unified Agent - Metrics](#1511-unified-agent---metrics)
- [16. Logs Metric Filter](#16-logs-metric-filter)
- [17. CloudWatch Alarms](#17-cloudwatch-alarms)
  - [17.1. Alarm Targets](#171-alarm-targets)
  - [17.2. Composite Alarms](#172-composite-alarms)
  - [17.3. Evaluating an alarm](#173-evaluating-an-alarm)
  - [17.4. EC2 Instance Recovery](#174-ec2-instance-recovery)
  - [17.5. CloudWatch Alarm: good to know](#175-cloudwatch-alarm-good-to-know)
- [18. Synthetics Canary](#18-synthetics-canary)
  - [18.1. Canary Blueprints](#181-canary-blueprints)
- [19. CloudWatch Events (Will be replaced with Amazon EventBridge)](#19-cloudwatch-events-will-be-replaced-with-amazon-eventbridge)
- [20. CloudWatch Evidently](#20-cloudwatch-evidently)
- [21. Insights](#21-insights)
  - [21.1. Container](#211-container)
  - [21.2. Lambda](#212-lambda)
  - [21.3. Contributor](#213-contributor)
  - [21.4. Application](#214-application)
  - [21.5. CloudWatch Insights and Operational Visibility](#215-cloudwatch-insights-and-operational-visibility)

# 1. Why Monitoring is Important

- We know how to deploy applications:
  - Safely.
  - Automatically.
  - Using Infrastructure as Code (IaC).
  - Leveraging the best AWS components!
- Our applications are deployed, and our users don't care how we did it...
- Our users only care that the application is working!
  - Application latency: Will it increase over time?
  - Application outages: Customer experience should not be degraded.
  - Users contacting the IT department or complaining is not a good outcome.
  - Troubleshooting and remediation.
- Internal monitoring:
  - Can we prevent issues before they happen?
  - Performance and Cost.
  - Trends (scaling patterns).
  - Learning and Improvement.

# 2. Monitoring in AWS

- **Amazon CloudWatch**
  - Metrics: Collect and track key metrics.
  - Logs: Collect, monitor, analyze and store log files.
  - Events: Send notifications when certain events happen in your AWS.
  - Alarms: React in real-time to metrics / events.
- **AWS X-Ray**
  - Troubleshooting application performance and errors.
  - Distributed tracing of microservices.
- **AWS CloudTrail**
  - Internal monitoring of API calls being made.
  - Audit changes to AWS Resources by your users.

# 3. Amazon CloudWatch Metrics

- CloudWatch provides metrics for every services in AWS.
- **Metric** is a variable to monitor (CPUUtilization, NetworkIn...).
- Metrics belong to **namespaces**.
- **Dimension** is an attribute of a metric (instance id, environment, etc...).
- Up to 10 dimensions per metric.
- Metrics have **timestamps**.
- Can create CloudWatch dashboards of metrics.
- Can create **CloudWatch Custom Metrics** (for the RAM for example).

## 3.1. Metric Streams

- Continually stream CloudWatch metrics to a destination of your choice, with **near-real-time delivery** and low latency.
  - Amazon Kinesis Data Firehose (and then its destinations).
  - **3rd party service provider:** Datadog, Dynatrace, New Relic, Splunk, Sumo Logic...
- Option to **filter metrics** to only stream a subset of them.
  ![Amazon CloudWatch Metric Streams](/Images/Management%20&%20Governance/AmazonCloudWatchMetricStreams.png)

## 3.2. Custom Metrics

- Possibility to define and send your own custom metrics to CloudWatch.
- **Example:** Memory (RAM) usage, disk space, number of logged in users...
- Use API call `PutMetricData`.
- Ability to use dimensions (attributes) to segment metrics.
  - `Instance.id`
  - `Environment.name`
- Metric resolution (`StorageResolution` API parameter - two possible value):
  - **Standard:** 1 minute (60 seconds).
  - **High Resolution:** 1/5/10/30 second(s) - **Higher cost**.
- **Important: Accepts metric data points two weeks in the past and two hours in the future (make sure to configure your EC2 instance time correctly).**

# 4. EC2 Detailed monitoring

- EC2 instance metrics have metrics "every 5 minutes".
- With **Detailed Monitoring** (for a cost), we get data "every 1 minute".
- Use **Detailed Monitoring** if you want to scale faster for your ASG!
- The AWS Free Tier allows us to have 10 detailed monitoring metrics.
- Note: EC2 Memory usage is by default not pushed (must be pushed from inside the instance as a custom metric).

# 5. CloudWatch Anomaly Detection

- Continuously analyze metrics to determine normal baselines and surface anomalies using ML algorithms.
- It creates a model of the metric's expected values (based on metric's past data).
- Shows you which values in the graph are out of the normal range.
- **Allows we to create Alarms based on metric's expected value (instead of Static Threshold).**
- Ability to exclude specified time periods or events from being trained.

# 6. Amazon Lookout for Metrics

- Automatically detect anomalies within metrics and identify their root causes using Machine Learning.
- It detects and diagnoses errors within your data with no manual intervention.
- Integrates with different AWS Services and 3rd party SaaS apps through AppFlow.
- Send alerts to SNS, Lambda, Slack, Webhooks...

# 7. Logs

- **Log groups:** Arbitrary name, usually representing an application.
- **Log stream:** Instances within application / log files / containers.
- Can define log expiration policies (never expire, 1 day to 10 years...)
- **CloudWatch Logs can send logs to**
  - Amazon S3 (exports).
  - Kinesis Data Streams.
  - Kinesis Data Firehose.
  - AWS Lambda.
  - OpenSearch.
- Logs are encrypted by default.
- Can setup KMS-based encryption with your own keys.

## 7.1. Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent.
- Elastic Beanstalk: collection of logs from application.
- **ECS:** Collection from containers.
- **AWS Lambda:** Collection from function logs.
- **VPC Flow Logs:** VPC specific logs.
- API Gateway.
- CloudTrail based on filter.
- **Route 53:** Log DNS queries.

# 8. Logs Insights

- Search and analyze log data stored in CloudWatch Logs.
- **Example:** Find a specific IP inside a log, count occurrences of "ERROR" in your logs...
- **Provides a purpose-built query language**
  - Automatically discovers fields from AWS services and JSON log events.
  - Fetch desired event fields, filter based on conditions, calculate aggregate statistics, sort events, limit number of events...
  - Can save queries and add them to CloudWatch Dashboards.
- Can query multiple Log Groups in different AWS accounts.
- It's a query engine, not a real-time engine.

# 9. Logs - S3 Export

- Log data can take **up to 12 hours** to become available for export.
- The API call is `CreateExportTask`.
- Not near-real time or real-time... use Logs Subscriptions instead.

# 10. Subscription Filter

- Get a real-time log events from CloudWatch Logs for processing and analysis.
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda.
- **Subscription Filter:** Filter which logs are events delivered to your destination.
  ![Amazon CloudWatch Logs Subscriptions](/Images/Management%20&%20Governance/AmazonCloudWatchLogsSubscriptions.png)
  ![Amazon CloudWatch Logs Subscriptions](/Images/Management%20&%20Governance/AmazonCloudWatchSubscriptionFilters.png)

## 10.1. Scenario with HIDS

- HIDS = Host-based intrusion detection system.
  ![Amazon CloudWatch Logs Subscriptions with HIDS](/Images/Management%20&%20Governance/AmazonCloudWatchLogsSubscriptionsHIDS.png)

## 10.2. Cross-Account Subscription

- Send log events to resources in a different AWS account (KDS, KDF).
  ![Cross-Account Subscription](/Images/Management%20&%20Governance/AmazonCloudWatchLogsSubscriptionsCrossAccount.png)

# 11. CloudWatch Logs Aggregation Multi-Account & Multi Region

![Logs Aggregation Multi-Account & Multi Region](/Images/Management%20&%20Governance/AmazonCloudWatchLogsAggregation.png)

# 12. CloudWatch Logs Metric Filter

- CloudWatch Logs can use filter expressions.
  - For example, find a specific IP inside of a log.
  - Or count occurrences of "ERROR" in your logs.
  - Metric filters can be used to trigger alarms.
- **Filters do not retroactively filter data. Filters only publish the metric data points for events that happen after the filter was created.**
  - Ability to specify up to 3 Dimensions for the Metric Filter (optional).
    ![CloudWatch Logs Metric Filter](/Images/Management%20&%20Governance/AmazonCloudWatchLogsMetricFilter.png)

# 13. CloudWatch cross-account observability

![CloudWatch cross-account observability](/Images/Management%20&%20Governance/AmazonCloudWatchCrossAccountObservability.png)

# 14. All kind of Logs

- **Application Logs**
  - Logs that are produced by your application code.
  - Contains custom log messages, stack traces, and so on.
  - Written to a local file on the filesystem.
  - Usually streamed to CloudWatch Logs using a **CloudWatch Agent** on [Amazon EC2](/Compute/Amazon%20EC2.md).
  - If using [AWS Lambda](/Compute/AWS%20Lambda.md), direct integration with CloudWatch Logs.
  - If using ECS or Fargate, direct integration with CloudWatch Logs.
  - If using [Amazon Elastic Beanstalk](/Compute/Amazon%20Elastic%20Beanstalk.md), direct integration with CloudWatch Logs.
- **Operating System Logs (Event Logs, System Logs)**
  - Logs that are generated by your operating system (EC2 or on-premise instance).
  - Informing you of system behavior (ex: /var/log/messages or /var/log/auth.log).
  - Usually streamed to CloudWatch Logs using a **CloudWatch Agent**.
- **Access Logs**
  - List of all the requests for individual files that people have requested from a website.
    - Example for httpd: /var/log/apache/access.log
  - Usually for load balancers, proxies, web servers, etc...

## 14.1. AWS Managed Logs

- Load Balancer Access Logs (ALB, NLB, CLB) => to S3.
  - Access logs for your Load Balancers.
- CloudTrail Logs => to S3 and CloudWatch Logs.
  - Logs for API calls made within your account.
- VPC Flow Logs => to S3 and CloudWatch Logs.
  - Information about IP traffic going to and from network interfaces in your VPC.
- Route 53 Access Logs => to CloudWatch Logs.
  - Log information about the queries that Route 53 receives.
- S3 Access Logs => to S3.
  - Server access logging provides detailed records for the requests that are made to a bucket.
- CloudFront Access Logs => to S3.
  - Detailed information about every user request that CloudFront receives.

# 15. Logs for EC2

- **By default, no logs from your EC2 machine will go to CloudWatch.**
- We need to run a **CloudWatch Agent** on EC2 to push the log files we want.
- Make sure IAM permissions are correct.
- The CloudWatch log agent can be setup on-premises too.
  ![Amazon CloudWatch Logs Agent](/Images/Management%20&%20Governance/AmazonCloudWatchLogsAgent.png)

## 15.1. Logs Agent & Unified Agent

- For virtual servers (EC2 instances, on-premise servers...).
- **CloudWatch Logs Agent**
  - Old version of the agent.
  - Can only send to CloudWatch Logs.
- **CloudWatch Unified Agent**
  - Collect additional system-level metrics such as RAM, processes, etc...
  - Collect logs to send to CloudWatch Logs.
  - Centralized configuration using SSM Parameter Store.

### 15.1.1. Unified Agent - Metrics

- Collected directly on your Linux server / EC2 instance:
  - **CPU:** (active, guest, idle, system, user, steal).
  - **Disk metrics:** (free, used, total), Disk IO (writes, reads, bytes, iops).
  - **RAM:** (free, inactive, used, total, cached).
  - **Netstat:** (number of TCP and UDP connections, net packets, bytes).
  - **Processes:** (total, dead, bloqued, idle, running, sleep).
  - **Swap Space:** (free, used, used %).
  - **Reminder:** Out-of-the box metrics for EC2 - disk, CPU, network (high level).

# 16. Logs Metric Filter

- CloudWatch Logs can use filter expressions.
  - For example, find a specific IP inside of a log.
  - Or count occurrences of "ERROR" in your logs.
  - Metric filters can be used to trigger alarms.
- Filters do not retroactively filter data. Filters only publish the metric data points for events that happen after the filter was created.

# 17. CloudWatch Alarms

- Alarms are used to trigger notifications for any metric.
- Various options (sampling, %, max, min, etc...).
- **Alarm States**
  - OK
  - INSUFFICIENT_DATA
  - ALARM
- **Period**
  - Length of time in seconds to evaluate the metric.
  - **High-Resolution Custom Metrics:** 10 sec, 30 sec or multiples of 60 sec.

## 17.1. Alarm Targets

- Stop, Terminate, Reboot, or Recover an EC2 Instance.
- Trigger Auto Scaling Action.
- Send notification to SNS (from which we can do pretty much anything).

## 17.2. Composite Alarms

- CloudWatch Alarms are on a single metric.
- **Composite Alarms are monitoring the states of multiple other alarms.**
- AND and OR conditions.
- Helpful to reduce "alarm noise" by creating complex composite alarms.

## 17.3. Evaluating an alarm

- When we create an alarm, we specify three settings to enable CloudWatch to evaluate when to change the alarm state:
  - **Period:** Is the length of time to evaluate the metric or expression to create each individual data point for an alarm. It is expressed in seconds.
  - **Evaluation Periods:** Is the number of the most recent periods, or data points, to evaluate when determining alarm state.
  - **Datapoints to Alarm:** Is the number of data points within the Evaluation Periods that must be breaching to cause the alarm to go to the ALARM state.
    - The breaching data points don't have to be consecutive, but they must all be within the last number of data points equal to **Evaluation Period**.

![Evaluating an alarm](/Images/Management%20&%20Governance/CloudWatchEvaluatingAlarm.png)

## 17.4. EC2 Instance Recovery

- **Status Check**
  - Instance status = check the EC2 VM.
  - System status = check the underlying hardware.
  - Attached EBS status = check attached EBS volumes.
- **Recovery:** Same Private, Public, Elastic IP, metadata, placement group.

## 17.5. CloudWatch Alarm: good to know

- Alarms can be created based on CloudWatch Logs Metrics Filters.
- To test alarms and notifications, set the alarm state to Alarm using CLI: `aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"`

# 18. Synthetics Canary

- Configurable script that monitor your APIs, URLs, Websites, ...
- Reproduce what your customers do programmatically to find issues before customers are impacted.
- Checks the availability and latency of your endpoints and can store load time data and screenshots of the UI.
- Integration with CloudWatch Alarms.
- Scripts written in Node.js or Python.
- Programmatic access to a headless Google Chrome browser.
- Can run once or on a regular schedule.

## 18.1. Canary Blueprints

- **Heartbeat Monitor:** Load URL, store screenshot and an HTTP archive file.
- **API Canary:** Test basic read and write functions of REST APIs.
- **Broken Link Checker:** Check all links inside the URL that we are testing.
- **Visual Monitoring":** Compare a screenshot taken during a canary run with a baseline screenshot.
- **Canary Recorder:** Used with CloudWatch Synthetics Recorder (record your actions on a website and automatically generates a script for that).
- **GUI Workflow Builder:** Verifies that actions can be taken on your webpage (e.g., test a webpage with a login form).

# 19. CloudWatch Events (Will be replaced with Amazon EventBridge)

- Event Pattern: Intercept events from AWS services (Sources):
  - Example sources: EC2 Instance Start, CodeBuild Failure, S3, Trusted Advisor.
  - Can intercept any API call with CloudTrail integration.
- Schedule or Cron (example: create an event every 4 hours).
- A JSON payload is created from the event and passed to a target...
  - Compute: Lambda, Batch, ECS task.
  - Integration: SQS, SNS, Kinesis Data Streams, Kinesis Data Firehose.
  - Orchestration: Step Functions, CodePipeline, CodeBuild.
  - Maintenance: SSM, EC2 Actions.

[Amazon EventBridge](/Application%20Integration/Amazon%20EventBridge.md)

# 20. CloudWatch Evidently

- Safely validate new features by serving them to a specified % of your users.
  - Reduce risk and identify unintended consequences.
  - Collect experiment data, analyze using stats, monitor performance.
- **Launches** (= feature flags): enable and disable features for a subset of users.
- **Experiments** (= A/B testing): compare multiple versions of the same feature.
- **Overrides:** Pre-define a variation for a specific user.
- Store evaluation events in CloudWatch Logs or S3.

# 21. Insights

## 21.1. Container

- Collect, aggregate, summarize metrics and logs from containers.
- Available for containers on...
  - Amazon Elastic Container Service (Amazon ECS).
  - Amazon Elastic Kubernetes Services (Amazon EKS).
  - Kubernetes platforms on EC2.
  - Fargate (both for ECS and EKS).
- **In Amazon EKS and Kubernetes, CloudWatch Insights is using a containerized version of the CloudWatch Agent to discover containers.**

## 21.2. Lambda

- Monitoring and troubleshooting solution for serverless applications running on AWS Lambda.
- Collects, aggregates, and summarizes system-level metrics including CPU time, memory, disk, and network.
- Collects, aggregates, and summarizes diagnostic information such as cold starts and Lambda worker shutdowns.
- Lambda Insights is provided as a Lambda Layer.

## 21.3. Contributor

- Analyze log data and create time series that display contributor data.
  - See metrics about the top-N contributors.
  - The total number of unique contributors, and their usage.
- This helps we find top talkers and understand who or what is impacting system performance.
- Works for any AWS-generated logs (VPC, VPC Flow Logs, DNS, etc..)
- For example, we can find bad hosts, **identify the heaviest network users**, or find the URLs that generate the most errors.
- We can build your rules from scratch, or we can also use sample rules that AWS has created - **leverages your CloudWatch Logs**.
- CloudWatch also provides built-in rules that we can use to analyze metrics from other AWS services.

## 21.4. Application

- **Provides automated dashboards that show potential problems with monitored applications, to help isolate ongoing issues.**
- Your applications run on Amazon EC2 Instances with select technologies only (Java, .NET, Microsoft IIS Web Server, databases...).
- And we can use other AWS resources such as Amazon EBS, RDS, ELB, ASG, Lambda, SQS, DynamoDB, S3 bucket, ECS, EKS, SNS, API Gateway...
- Powered by SageMaker.
- Enhanced visibility into your application health to reduce the time it will take we to troubleshoot and repair your applications.
- Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter.

## 21.5. CloudWatch Insights and Operational Visibility

- **CloudWatch Container Insights**
  - ECS, EKS, Kubernetes on EC2, Fargate, needs agent for Kubernetes.
  - Metrics and logs.
- **CloudWatch Lambda Insights**
  - Detailed metrics to troubleshoot serverless applications.
- **CloudWatch Contributors Insights**
  - Find "Top-N" Contributors through CloudWatch Logs.
- **CloudWatch Application Insights**
  - Automatic dashboard to troubleshoot your application and related AWS services.
