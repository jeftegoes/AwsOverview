# AWS X-Ray<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Visual analysis of our applications](#2-visual-analysis-of-our-applications)
- [3. Advantages](#3-advantages)
- [4. Compatibility](#4-compatibility)
- [5. Leverages Tracing](#5-leverages-tracing)
- [6. How to enable it?](#6-how-to-enable-it)
- [7. X-Ray magic](#7-x-ray-magic)
- [8. Troubleshooting](#8-troubleshooting)
- [9. Instrumentation in your code](#9-instrumentation-in-your-code)
- [10. X-Ray Concepts](#10-x-ray-concepts)
- [11. Sampling Rules](#11-sampling-rules)
- [12. Custom Sampling Rules](#12-custom-sampling-rules)
- [13. X-Ray Write and Read APIs](#13-x-ray-write-and-read-apis)
  - [13.1. Write APIs (used by the X-Ray daemon)](#131-write-apis-used-by-the-x-ray-daemon)
  - [13.2. Read APIs](#132-read-apis)
- [14. X-Ray with Elastic Beanstalk](#14-x-ray-with-elastic-beanstalk)
- [15. AWS Distro for OpenTelemetry](#15-aws-distro-for-opentelemetry)
- [16. Running the X-Ray daemon on Amazon ECS](#16-running-the-x-ray-daemon-on-amazon-ecs)

# 1. Introduction

- **AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture.**
- Debugging in Production, the good old way:
  - Test locally.
  - Add log statements everywhere.
  - Re-deploy in production.
- Log formats differ across applications using CloudWatch and analytics is hard.
- Debugging: monolith "easy", distributed services "hard".
- No common views of your entire architecture!

# 2. Visual analysis of our applications

![AWS X-Ray Service Map](/Images/AWSXRayServiceMap.PNG)

# 3. Advantages

- Troubleshooting performance (bottlenecks).
- Understand dependencies in a microservice architecture.
- Pinpoint service issues.
- Review request behavior.
- Find errors and exceptions.
- Are we meeting time SLA?
- Where I am throttled?
- Identify users that are impacted.

# 4. Compatibility

- AWS Lambda.
- Elastic Beanstalk.
- ECS.
- ELB.
- API Gateway.
- EC2 Instances or any application server (even on premise).

# 5. Leverages Tracing

- Tracing is an end to end way to following a "request".
- Each component dealing with the request adds its own "trace".
- Tracing is made of segments (+ sub segments).
- Annotations can be added to traces to provide extra-information
- Ability to trace:
  - Every request.
  - Sample request (as a % for example or a rate per minute).
- X-Ray Security:
  - IAM for authorization.
  - KMS for encryption at rest.

# 6. How to enable it?

1. Your code (Java, Python, Go, Node.js, .NET) must import the AWS X-Ray SDK:

- Very little code modification needed.
- The application SDK will then capture:
  - Calls to AWS services.
  - HTTP / HTTPS requests.
  - Database Calls (MySQL, PostgreSQL, DynamoDB).
  - Queue calls (SQS).

2. Install the X-Ray daemon or enable X-Ray AWS Integration:

- X-Ray daemon works as a low level UDP packet interceptor (Linux / Windows / Mac...).
- AWS Lambda / other AWS services already run the X-Ray daemon for you.
- Each application must have the IAM rights to write data to X-Ray.

# 7. X-Ray magic

- X-Ray service collects data from all the different services.
- Service map is computed from all the segments and traces.
- X-Ray is graphical, so even non technical people can help troubleshoot.

# 8. Troubleshooting

- If X-Ray is not working on EC2:
  - Ensure the EC2 IAM Role has the proper permissions.
  - Ensure the EC2 instance is running the X-Ray Daemon.
- To enable on AWS Lambda:
  - Ensure it has an IAM execution role with proper policy (AWSX-RayWriteOnlyAccess).
  - Ensure that X-Ray is imported in the code.

# 9. Instrumentation in your code

- **Instrumentation** means the measure of product's performance, diagnose errors, and to write trace information.
- To instrument your application code, you use the **X-Ray SDK**.
- Many SDK require only configuration changes.
- You can modify your application code to customize and annotation the data that the SDK sends to X- Ray, using **interceptors, filters, handlers, middleware...**

# 10. X-Ray Concepts

- **Segments:** each application / service will send them.
- **Subsegments:** if you need more details in your segment.
- **Trace:** segments collected together to form an end-to-end trace.
- **Sampling:** decrease the amount of requests sent to X-Ray, reduce cost.
- **Annotations:** Key Value pairs used to **index** traces and use with **filters**.
- **Metadata:** Key Value pairs, **not indexed**, not used for searching.
- The X-Ray daemon / agent has a config to send traces cross account:
  - Make sure the IAM permissions are correct - the agent will assume the role.
  - This allows to have a central account for all your application tracing.

# 11. Sampling Rules

- With sampling rules, you control the amount of data that you record.
- You can modify sampling rules without changing your code.
- By default, the **X-Ray SDK** records the **first request** each second, and _five percent_ of any additional requests.
  - **One request per second is the reservoir**, which ensures that at least one trace is recorded each second as long the service is serving requests.
  - _Five percent is the rate_ at which additional requests beyond the reservoir size are sampled.

# 12. Custom Sampling Rules

- You can create your own rules with the **reservoir** and _rate_.

# 13. X-Ray Write and Read APIs

## 13.1. Write APIs (used by the X-Ray daemon)

- **PutTraceSegments:** Uploads segment documents to AWS X-Ray.
- **PutTelemetryRecords:** Used by the AWS X-Ray daemon to upload telemetry
  - SegmentsReceivedCount
  - SegmentsRejectedCounts
  - BackendConnectionErrors...
- **GetSamplingRules:** Retrieve all sampling rules (to know what/when to send).
- **GetSamplingTargets & GetSamplingStatisticSummaries:** advanced.
- The X-Ray daemon needs to have an IAM policy authorizing the correct API calls to function correctly.

## 13.2. Read APIs

- **GetServiceGraph:** main graph.
- **BatchGetTraces:** Retrieves a list of traces specified by ID. Each trace is a collection of segment documents that originates from a single request.
- **GetTraceSummaries:** Retrieves IDs and annotations for traces available for a specified time frame using an optional filter. To get the full traces, pass the trace IDs to BatchGetTraces.
- **GetTraceGraph:** Retrieves a service graph for one or more specific trace IDs.

# 14. X-Ray with Elastic Beanstalk

- AWS Elastic Beanstalk platforms include the X-Ray daemon.
- You can run the daemon by setting an option in the Elastic Beanstalk console or with a configuration file (in .ebextensions/xray-daemon.config).
- Make sure to give your instance profile the correct IAM permissions so that the X-Ray daemon can function correctly.
- Then make sure your application code is instrumented with the X-Ray SDK.
- Note: The X-Ray daemon is not provided for Multicontainer Docker.

# 15. AWS Distro for OpenTelemetry

- Secure, production-ready AWS-supported distribution of the open-source project OpenTelemetry project.
- Provides a single set of APIs, libraries, agents, and collector services.
- Collects distributed traces and metrics from your apps.
- Collects metadata from your AWS resources and services.
- Auto-instrumentation Agents to collect traces without changing your code.
- Send traces and metrics to multiple AWS services and partner solutions.
- X-Ray, CloudWatch, Prometheus...
- Instrument your apps running on AWS (e.g., [ECS](AWS%20EC2.md), [ECS](AWS%20ECS.md), EKS, Fargate, [Lambda](AWS%20Lambda.md)) as well as on-premises.
- Migrate from X-Ray to AWS Distro for Temeletry if you want to standardize with open-source APIs from Telemetry or send traces to multiple destinations simultaneously.

# 16. Running the X-Ray daemon on Amazon ECS

- In Amazon ECS, create a Docker image that runs the X-Ray daemon, upload it to a Docker image repository, and then deploy it to your Amazon ECS cluster.
- You can use port mappings and network mode settings in your task definition file to allow your application to communicate with the daemon container.
