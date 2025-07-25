# Amazon EventBridge (Default event bus) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Amazon EventBridge (formerly CloudWatch Events)](#2-amazon-eventbridge-formerly-cloudwatch-events)
- [3. EventBridge rules](#3-eventbridge-rules)
- [4. Event Bus](#4-event-bus)
- [5. Schema Registry](#5-schema-registry)
- [6. Resource-based Policy](#6-resource-based-policy)
- [7. Amazon EventBridge vs CloudWatch Events](#7-amazon-eventbridge-vs-cloudwatch-events)
- [8. Security](#8-security)

# 1. Introduction

- Is a serverless event bus that makes it easier to build event-driven applications at scale using events generated from your applications, integrated Software-as-a-Service (SaaS) applications, and AWS services.
- Delivers a stream of real-time data from event sources such as Zendesk or Shopify to targets like AWS Lambda and other SaaS applications.
- You can set up routing rules to determine where to send your data to build application architectures that react in real-time to your data sources with event publisher and consumer completely decoupled.

# 2. Amazon EventBridge (formerly CloudWatch Events)

- **Schedule:** Cron jobs (scheduled scripts).
  ![Cron jobs](/Images/Application%20Integration/AmazonEventBridgeCronJobs.png)
- **Event Pattern:** Event rules to react to a service doing something.
  ![Event Pattern](/Images/Application%20Integration/AmazonEventBridgeEventRules.png)
- Trigger Lambda functions, send SQS/SNS messages...

# 3. EventBridge rules

![EventBridge rules](/Images/Application%20Integration/AmazonEventBridgeRules.png)

# 4. Event Bus

- **Default event bus:** Generated by AWS services (CloudWatch Events).
- **Partner event bus:** Receive events from SaaS service or applications (Zendesk, DataDog, Segment, Auth0...).
- Custom Event buses: for your own applications.
  ![Event Bus](/Images/Application%20Integration/AmazonEventBridgeEventBus.png)
- Event buses can be accessed by other AWS accounts using Resource-based Policies.
- You can **archive events** (all/filter) sent to an event bus (indefinitely or set period).
- Ability to **replay archived events**.
- **Rules:** How to process the events (like CloudWatch Events).
- EventBridge has a different name to mark the new capabilities.

# 5. Schema Registry

- EventBridge can analyze the events in your bus and infer the **schema**.
- The **Schema Registry** allows you to generate code for your application, that will know in advance how data is structured in the event bus.
- Schema can be versioned.

# 6. Resource-based Policy

- Manage permissions for a specific Event Bus.
- **Example:** Allow/deny events from another AWS account or AWS region.
- **Use case:** Aggregate all events from your AWS Organization in a single AWS account or AWS region.

# 7. Amazon EventBridge vs CloudWatch Events

- Amazon EventBridge builds upon and extends CloudWatch Events.
- It uses the same service API and endpoint, and the same underlying service infrastructure.
- EventBridge allows extension to add event buses for your custom applications and your third-party SaaS apps.
- Event Bridge has the Schema Registry capability.
- EventBridge has a different name to mark the new capabilities.
- Over time, the CloudWatch Events name will be replaced with EventBridge.

# 8. Security

- When a rule runs, it needs permissions on the target.
- **Resource-based policy:** Lambda, SNS, SQS, S3 buckets, API Gateway...
- **IAM role:** EC2 Auto Scaling, Systems Manager Run Command, ECS task...
