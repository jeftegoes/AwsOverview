# AWS SQS - Simple Queue Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. What's a queue?](#2-whats-a-queue)
  - [2.1. Standard Queue](#21-standard-queue)
- [3. Producing Messages](#3-producing-messages)
- [4. Consuming Messages](#4-consuming-messages)
- [5. Multiple EC2 Instances Consumers](#5-multiple-ec2-instances-consumers)
- [6. Security](#6-security)
- [7. Message Visibility Timeout](#7-message-visibility-timeout)
- [8. FIFO Queue](#8-fifo-queue)
- [9. Dead Letter Queue (DLQ)](#9-dead-letter-queue-dlq)
  - [9.1. Redrive to Source](#91-redrive-to-source)
- [10. Delay Queue](#10-delay-queue)
- [11. Long Polling](#11-long-polling)
- [12. SQS Extended Client](#12-sqs-extended-client)
- [13. Must know API](#13-must-know-api)
- [14. SQS FIFO](#14-sqs-fifo)
  - [14.1. Deduplication](#141-deduplication)
  - [14.2. Message Grouping](#142-message-grouping)

# 1. Introduction

- When we start deploying multiple applications, they will inevitably need to communicate with one another.
- There are two patterns of application communication.

1. Synchronous communications (application to application).
2. Asynchronous / Event based (application to queue to application).

- Synchronous between applications can be problematic if there are sudden spikes of traffic.
- What if you need to suddenly encode 1000 videos but usually it's 10?
- In that case, it's better to **decouple** your applications using:
  - SQS: queue model.
  - SNS: pub/sub model.
  - Kinesis: real-time streaming model.
- These services can scale independently from our application!

# 2. What's a queue?

Producer > Send messages > SQS Queue < Poll messages < Consumer

## 2.1. Standard Queue

- Oldest offering (over 10 years old).
- Fully managed service, used to **decouple applications**.
- Attributes:
  - Unlimited throughput, unlimited number of messages in queue.
  - Default retention of messages: 4 days, maximum of 14 days.
  - Low latency (<10 ms on publish and receive).
  - Limitation of 256KB per message sent.
- Can have duplicate messages (at least once delivery, occasionally).
- Can have out of order messages (best effort ordering).

# 3. Producing Messages

- Produced to SQS using the SDK (SendMessage API).
- The message is **persisted** in SQS until a consumer deletes it.
- Message retention: default 4 days, up to 14 days.
- Example: send an order to be processed
  - Order id
  - Customer id
  - Any attributes you want
- SQS standard: unlimited throughput.

# 4. Consuming Messages

- Consumers (running on [EC2 instances](AWS%20EC2.md), servers, or [AWS Lambda](AWS%20Lambda.md))...
- Poll SQS for messages (receive up to 10 messages at a time).
- Process the messages (example: insert the message into an [RDS database](AWS%20RDS.md)).
- Delete the messages using the DeleteMessage API.

# 5. Multiple EC2 Instances Consumers

- Consumers receive and process messages in parallel.
- At least once delivery.
- Best-effort message ordering.
- Consumers delete messages after processing them.
- We can scale consumers horizontally to improve throughput of processing.

# 6. Security

- **Encryption:**
  - In-flight encryption using HTTPS API.
  - At-rest encryption using KMS keys.
  - Client-side encryption if the client wants to perform encryption/decryption itself.
- **Access Controls:** IAM policies to regulate access to the SQS API.
- **SQS Access Policies** (similar to S3 bucket policies)
  - Useful for cross-account access to SQS queues.
  - Useful for allowing other services (SNS, S3...) to write to an SQS queue.

# 7. Message Visibility Timeout

- After a message is polled by a consumer, it becomes invisible to other consumers.
- By default, the "message visibility timeout" is 30 seconds.
- That means the message has 30 seconds to be processed.
- After the message visibility timeout is over, the message is "visible" in SQS.

![SQS Visibility Timeout](Images/AWSSQSMessageVisibilityTimeout.png)

- If a message is not processed within the visibility timeout, it will be processed **twice**.
- A consumer could call the `ChangeMessageVisibility` API to get more time.
- If visibility timeout is high (hours), and consumer crashes, re-processing will take time.
- If visibility timeout is too low (seconds), we may get duplicates.

# 8. FIFO Queue

- FIFO = First In First Out (ordering of messages in the queue).
- Limited throughput: 300 msg/s without batching, 3000 msg/s with.
- Exactly-once send capability (by removing duplicates).
- Messages are processed in order by the consumer.

# 9. Dead Letter Queue (DLQ)

- If a consumer fails to process a message within the Visibility Timeout... the message goes back to the queue!
- We can set a threshold of how many times a message can go back to the queue.
- After the `MaximumReceives` threshold is exceeded, the message goes into a Dead Letter Queue (DLQ).
- Useful for debugging!
- **DLQ of a FIFO queue must also be a FIFO queue.**
- **DLQ of a Standard queue must also be a Standard queue.**
- Make sure to process the messages in the DLQ before they expire:
  - Good to set a retention of 14 days in the DLQ.

## 9.1. Redrive to Source

- Feature to help consume messages in the DLQ to understand what is wrong with them.
- When our code is fixed, we can redrive the messages from the DLQ back into the source queue (or any other queue) in batches without writing custom code.

# 10. Delay Queue

- Delay a message (consumers don't see it immediately) up to 15 minutes.
- Default is 0 seconds (message is available right away).
- Can set a default at queue level.
- Can override the default on send using the `DelaySeconds` parameter.

![SQS Delay Queue](Images/AWSSQSDelayQueue.png)

# 11. Long Polling

- When a consumer requests messages from the queue, it can optionally "wait" for messages to arrive if there are none in the queue.
- This is called Long Polling.
- **LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application**.
- The wait time can be between 1 sec to 20 sec (20 sec preferable).
- Long Polling is preferable to Short Polling.
- Long polling can be enabled at the queue level or at the API level using `ReceiveMessageWaitTimeSeconds`.

# 12. SQS Extended Client

- Message size limit is 256KB, how to send large messages, e.g. 1GB?
  - Using the SQS Extended Client (Java Library).

# 13. Must know API

- `CreateQueue` (MessageRetentionPeriod), `DeleteQueue`.
- `PurgeQueue` - Delete all the messages in queue.
- `SendMessage` (DelaySeconds), `ReceiveMessage`, `DeleteMessage`.
- `MaxNumberOfMessages` - Default 1, max 10 (for ReceiveMessage API).
- `ReceiveMessageWaitTimeSeconds` - Long Polling.
- `ChangeMessageVisibility` - Change the message timeout.
- Batch APIs for `SendMessage`, `DeleteMessage`, `ChangeMessageVisibility` helps decrease your costs.

# 14. SQS FIFO

## 14.1. Deduplication

- De-duplication interval is 5 minutes.
- Two de-duplication methods:
  - Content-based deduplication: will do a SHA-256 hash of the message body.
  - `MessageDeduplicationId` - Explicitly provide a Message Deduplication ID.

![MessageDeduplicationId](Images/AWSSQSDeduplicationID.png)

## 14.2. Message Grouping

- If you specify the same value of MessageGroupID in an SQS FIFO queue, you can only have one consumer, and all the messages are in order.
- To get ordering at the level of a subset of messages, specify different values for MessageGroupID.
  - Messages that share a common Message Group ID will be in order within the group.
  - Each Group ID can have a different consumer (parallel processing!).
  - Ordering across groups is not guaranteed.
