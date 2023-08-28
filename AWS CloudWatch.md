# AWS Monitoring, Troubleshooting and Audit<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Why Monitoring is Important](#1-why-monitoring-is-important)
- [2. Monitoring in AWS](#2-monitoring-in-aws)
- [3. AWS CloudWatch Metrics](#3-aws-cloudwatch-metrics)
- [4. EC2 Detailed monitoring](#4-ec2-detailed-monitoring)
- [5. Custom Metrics](#5-custom-metrics)
- [6. Logs](#6-logs)
  - [6.1. Sources](#61-sources)
- [7. Logs Metric Filter and Insights](#7-logs-metric-filter-and-insights)
- [8. Logs - S3 Export](#8-logs---s3-export)
- [9. Logs for EC2](#9-logs-for-ec2)
- [10. Logs Agent \& Unified Agent](#10-logs-agent--unified-agent)
- [11. Unified Agent - Metrics](#11-unified-agent---metrics)
- [12. Logs Metric Filter](#12-logs-metric-filter)
- [13. CloudWatch Alarms](#13-cloudwatch-alarms)
  - [13.1. Alarm Targets](#131-alarm-targets)
  - [13.2. Composite Alarms](#132-composite-alarms)
  - [13.3. Evaluating an alarm](#133-evaluating-an-alarm)
  - [13.4. EC2 Instance Recovery](#134-ec2-instance-recovery)
  - [13.5. CloudWatch Alarm: good to know](#135-cloudwatch-alarm-good-to-know)
- [14. Synthetics Canary](#14-synthetics-canary)
  - [14.1. Canary Blueprints](#141-canary-blueprints)
- [15. CloudWatch Events (Will be replaced with EventBridge)](#15-cloudwatch-events-will-be-replaced-with-eventbridge)
- [18. CloudWatch Evidently](#18-cloudwatch-evidently)

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

- AWS CloudWatch:
  - Metrics: Collect and track key metrics.
  - Logs: Collect, monitor, analyze and store log files.
  - Events: Send notifications when certain events happen in your AWS.
  - Alarms: React in real-time to metrics / events.
- AWS X-Ray:
  - Troubleshooting application performance and errors.
  - Distributed tracing of microservices.
- AWS CloudTrail:
  - Internal monitoring of API calls being made.
  - Audit changes to AWS Resources by your users.

# 3. AWS CloudWatch Metrics

- CloudWatch provides metrics for every services in AWS.
- **Metric** is a variable to monitor (CPUUtilization, NetworkIn...).
- Metrics belong to **namespaces**.
- **Dimension** is an attribute of a metric (instance id, environment, etc...).
- Up to 10 dimensions per metric.
- Metrics have **timestamps**.
- Can create CloudWatch dashboards of metrics.

# 4. EC2 Detailed monitoring

- EC2 instance metrics have metrics "every 5 minutes".
- With **Detailed Monitoring** (for a cost), you get data "every 1 minute".
- Use **Detailed Monitoring** if you want to scale faster for your ASG!
- The AWS Free Tier allows us to have 10 detailed monitoring metrics.
- Note: EC2 Memory usage is by default not pushed (must be pushed from inside the instance as a custom metric).

# 5. Custom Metrics

- Possibility to define and send your own custom metrics to CloudWatch.
- Example: memory (RAM) usage, disk space, number of logged in users...
- Use API call `PutMetricData`.
- Ability to use dimensions (attributes) to segment metrics.
  - Instance.id
  - Environment.name
- Metric resolution (StorageResolution API parameter - two possible value):
  - Standard: 1 minute (60 seconds).
  - High Resolution: 1/5/10/30 second(s) - **Higher cost**.
- Important: Accepts metric data points two weeks in the past and two hours in the future (make sure to configure your EC2 instance time correctly).

# 6. Logs

- **Log groups:** arbitrary name, usually representing an application.
- **Log stream:** instances within application / log files / containers.
- Can define log expiration policies (never expire, 30 days, etc..).
- **CloudWatch Logs can send logs to:**
  - Amazon S3 (exports).
  - Kinesis Data Streams.
  - Kinesis Data Firehose.
  - AWS Lambda.
  - OpenSearch.

## 6.1. Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent.
- Elastic Beanstalk: collection of logs from application.
- ECS: collection from containers.
- AWS Lambda: collection from function logs.
- VPC Flow Logs: VPC specific logs.
- API Gateway.
- CloudTrail based on filter.
- Route53: Log DNS queries.

# 7. Logs Metric Filter and Insights

- CloudWatch Logs can use filter expressions:
  - For example, find a specific IP inside of a log.
  - Or count occurrences of "ERROR" in your logs.
- Metric filters can be used to trigger CloudWatch alarms.
- CloudWatch Logs Insights can be used to query logs and add queries to CloudWatch Dashboards.

# 8. Logs - S3 Export

- Log data can take up to 12 hours to become available for export.
- The API call is CreateExportTask.
- Not near-real time or real-time... use Logs Subscriptions instead.

# 9. Logs for EC2

- By default, no logs from your EC2 machine will go to CloudWatch.
- You need to run a CloudWatch agent on EC2 to push the log files you want.
- Make sure IAM permissions are correct.
- The CloudWatch log agent can be setup on-premises too.

# 10. Logs Agent & Unified Agent

- For virtual servers (EC2 instances, on-premise servers...)
- **CloudWatch Logs Agent:**
  - Old version of the agent.
  - Can only send to CloudWatch Logs.
- **CloudWatch Unified Agent:**
  - Collect additional system-level metrics such as RAM, processes, etc...
  - Collect logs to send to CloudWatch Logs.
  - Centralized configuration using SSM Parameter Store.

# 11. Unified Agent - Metrics

- Collected directly on your Linux server / EC2 instance:
  - **CPU:** (active, guest, idle, system, user, steal).
  - **Disk metrics:** (free, used, total), Disk IO (writes, reads, bytes, iops).
  - **RAM:** (free, inactive, used, total, cached).
  - **Netstat:** (number of TCP and UDP connections, net packets, bytes).
  - **Processes:** (total, dead, bloqued, idle, running, sleep).
  - **Swap Space:** (free, used, used %).
  - Reminder: out-of-the box metrics for EC2 - disk, CPU, network (high level).

# 12. Logs Metric Filter

- CloudWatch Logs can use filter expressions.
  - For example, find a specific IP inside of a log.
  - Or count occurrences of "ERROR" in your logs.
  - Metric filters can be used to trigger alarms.
- Filters do not retroactively filter data. Filters only publish the metric data points for events that happen after the filter was created.

# 13. CloudWatch Alarms

- Alarms are used to trigger notifications for any metric.
- Various options (sampling, %, max, min, etc...).
- Alarm States:
  - OK
  - INSUFFICIENT_DATA
  - ALARM
- Period:
  - Length of time in seconds to evaluate the metric.
  - **High-Resolution Custom Metrics:** 10 sec, 30 sec or multiples of 60 sec.

## 13.1. Alarm Targets

- Stop, Terminate, Reboot, or Recover an EC2 Instance.
- Trigger Auto Scaling Action.
- Send notification to SNS (from which you can do pretty much anything).

## 13.2. Composite Alarms

- CloudWatch Alarms are on a single metric.
- Composite Alarms are monitoring the states of multiple other alarms.
- AND and OR conditions.
- Helpful to reduce "alarm noise" by creating complex composite alarms.

## 13.3. Evaluating an alarm

- When you create an alarm, you specify three settings to enable CloudWatch to evaluate when to change the alarm state:
  - **Period:** Is the length of time to evaluate the metric or expression to create each individual data point for an alarm. It is expressed in seconds.
  - **Evaluation Periods:** Is the number of the most recent periods, or data points, to evaluate when determining alarm state.
  - **Datapoints to Alarm:** Is the number of data points within the Evaluation Periods that must be breaching to cause the alarm to go to the ALARM state.
    - The breaching data points don't have to be consecutive, but they must all be within the last number of data points equal to **Evaluation Period**.

![Evaluating an alarm](Images/CloudWatchEvaluatingAlarm.png)

## 13.4. EC2 Instance Recovery

- Status Check:
  - Instance status = check the EC2 VM.
  - System status = check the underlying hardware.
- Recovery: Same Private, Public, Elastic IP, metadata, placement group

## 13.5. CloudWatch Alarm: good to know

- Alarms can be created based on CloudWatch Logs Metrics Filters.
- To test alarms and notifications, set the alarm state to Alarm using CLI: `aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"`

# 14. Synthetics Canary

- Configurable script that monitor your APIs, URLs, Websites, ...
- Reproduce what your customers do programmatically to find issues before customers are impacted.
- Checks the availability and latency of your endpoints and can store load time data and screenshots of the UI.
- Integration with CloudWatch Alarms.
- Scripts written in Node.js or Python.
- Programmatic access to a headless Google Chrome browser.
- Can run once or on a regular schedule.

## 14.1. Canary Blueprints

- **Heartbeat Monitor:** Load URL, store screenshot and an HTTP archive file.
- **API Canary:** Test basic read and write functions of REST APIs.
- **Broken Link Checker:** Check all links inside the URL that you are testing.
- **Visual Monitoring":** Compare a screenshot taken during a canary run with a baseline screenshot.
- **Canary Recorder:** Used with CloudWatch Synthetics Recorder (record your actions on a website and automatically generates a script for that).
- **GUI Workflow Builder:** Verifies that actions can be taken on your webpage (e.g., test a webpage with a login form).

# 15. CloudWatch Events (Will be replaced with EventBridge)

- Event Pattern: Intercept events from AWS services (Sources):
  - Example sources: EC2 Instance Start, CodeBuild Failure, S3, Trusted Advisor.
  - Can intercept any API call with CloudTrail integration.
- Schedule or Cron (example: create an event every 4 hours).
- A JSON payload is created from the event and passed to a target...
  - Compute: Lambda, Batch, ECS task.
  - Integration: SQS, SNS, Kinesis Data Streams, Kinesis Data Firehose.
  - Orchestration: Step Functions, CodePipeline, CodeBuild.
  - Maintenance: SSM, EC2 Actions.

# 18. CloudWatch Evidently

- Safely validate new features by serving them to a specified % of your users.
- Reduce risk and identify unintended consequences.
- Collect experiment data, analyze using stats, monitor performance.
- Launches (= feature flags): enable and disable features for a subset of users.
- Experiments (= A/B testing): compare multiple versions of the same feature.
- Overrides: pre-define a variation for a specific user.
- Store evaluation events in CloudWatch Logs or S3.
