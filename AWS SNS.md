# AWS SNS<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. How to publish](#2-how-to-publish)
- [3. Security](#3-security)
- [4. SNS + SQS: Fan Out](#4-sns--sqs-fan-out)
- [5. Applications](#5-applications)
  - [5.1. S3 Events to multiple queues](#51-s3-events-to-multiple-queues)
  - [5.2. SNS to Amazon S3 through](#52-sns-to-amazon-s3-through)
- [6. FIFO Topic](#6-fifo-topic)
- [7. SNS FIFO + SQS FIFO: Fan Out](#7-sns-fifo--sqs-fifo-fan-out)
- [8. Message Filtering](#8-message-filtering)

# 1. Introduction

- What if you want to send one message to many receivers?
- The "event producer" only sends message to one SNS topic.
- As many "event receivers" (subscriptions) as we want to listen to the SNS topic notifications.
- Each subscriber to the topic will get all the messages (note: new feature to filter messages).
- Up to 12,500,000 subscriptions per topic.
- 100,000 topics limit.
- Many AWS services can send data directly to SNS for notifications.

# 2. How to publish

- Topic Publish (using the SDK)
  - Create a topic.
  - Create a subscription (or many).
  - Publish to the topic.
- Direct Publish (for mobile apps SDK)
  - Create a platform application.
  - Create a platform endpoint.
  - Publish to the platform endpoint.
  - Works with Google GCM, Apple APNS, Amazon ADM...

# 3. Security

- **Encryption:**
  - In-flight encryption using HTTPS API.
  - At-rest encryption using KMS keys.
  - Client-side encryption if the client wants to perform encryption/decryption itself.
- **Access Controls:** IAM policies to regulate access to the SNS API.
- **SNS Access Policies** (similar to S3 bucket policies)
  - Useful for cross-account access to SNS topics.
  - Useful for allowing other services ( S3...) to write to an SNS topic.

# 4. SNS + SQS: Fan Out

- Push once in SNS, receive in all SQS queues that are subscribers.
- Fully decoupled, no data loss.
- SQS allows for: data persistence, delayed processing and retries of work.
- Ability to add more SQS subscribers over time.
- Make sure your SQS queue **access policy** allows for SNS to write.
- Cross-Region Delivery: works with SQS Queues in other regions.

# 5. Applications

## 5.1. S3 Events to multiple queues

- For the same combination of: **event type** (e.g. object create) and **prefix** (e.g. images/) you can only have one S3 Event rule.
- If you want to send the same S3 event to many SQS queues, use fan-out.

## 5.2. SNS to Amazon S3 through

- SNS can send to Kinesis.

# 6. FIFO Topic

- FIFO = First In First Out (ordering of messages in the topic).
- Similar features as SQS FIFO:
  - **Ordering** by Message Group ID (all messages in the same group are ordered).
  - **Deduplication** using a Deduplication ID or Content Based Deduplication.
- _Can only have SQS FIFO queues as subscribers._
- Limited throughput (same throughput as SQS FIFO).

# 7. SNS FIFO + SQS FIFO: Fan Out

- In case you need fan out + ordering + deduplication.

# 8. Message Filtering

- JSON policy used to filter messages sent to SNS topic's subscriptions.
- If a subscription doesn't have a filter policy, it receives every message.
