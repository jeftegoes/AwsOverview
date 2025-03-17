# Amazon SQS - Simple Queue Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What's a queue?](#1-whats-a-queue)
  - [1.1. Standard Queue](#11-standard-queue)
- [2. Producing Messages](#2-producing-messages)
- [3. Consuming Messages](#3-consuming-messages)
- [4. Multiple EC2 Instances Consumers](#4-multiple-ec2-instances-consumers)
- [5. Security](#5-security)
- [6. Message Visibility Timeout](#6-message-visibility-timeout)
- [7. FIFO Queue](#7-fifo-queue)
- [8. Dead Letter Queue (DLQ)](#8-dead-letter-queue-dlq)
  - [8.1. Redrive to Source](#81-redrive-to-source)
- [9. Delay Queue](#9-delay-queue)
- [10. Long Polling](#10-long-polling)
- [11. SQS Extended Client](#11-sqs-extended-client)
- [12. Must know API](#12-must-know-api)
- [13. SQS FIFO](#13-sqs-fifo)
  - [13.1. Deduplication](#131-deduplication)
  - [13.2. Message Grouping](#132-message-grouping)

# 1. What's a queue?

Producer > Send messages > SQS Queue < Poll messages < Consumer

## 1.1. Standard Queue

- Oldest offering (over 10 years old).
- Fully managed service, used to **decouple applications**.
- **Attributes**
  - Unlimited throughput, unlimited number of messages in queue.
  - Default retention of messages: 4 days, maximum of 14 days.
  - Low latency (<10 ms on publish and receive).
  - Limitation of 256KB per message sent.
- Can have duplicate messages (at least once delivery, occasionally).
- Can have out of order messages (best effort ordering).

# 2. Producing Messages

- Produced to SQS using the SDK (SendMessage API).
- The message is **persisted** in SQS until a consumer deletes it.
- Message retention: default 4 days, up to 14 days.
- **Example:** Send an order to be processed.
  - Order id.
  - Customer id.
  - Any attributes you want.
- **SQS standard:** Unlimited throughput.

# 3. Consuming Messages

- Consumers (running on [EC2 instances](/Compute/Amazon%20EC2.md), servers, or [AWS Lambda](/Compute/AWS%20Lambda.md))...
- Poll SQS for messages (receive up to 10 messages at a time).
- Process the messages (example: insert the message into an [RDS database](/Database/Amazon%20RDS.md)).
- Delete the messages using the DeleteMessage API.

# 4. Multiple EC2 Instances Consumers

- Consumers receive and process messages in parallel.
- At least once delivery.
- Best-effort message ordering.
- Consumers delete messages after processing them.
- We can scale consumers horizontally to improve throughput of processing.

# 5. Security

- **Encryption**
  - In-flight encryption using HTTPS API.
  - At-rest encryption using KMS keys.
  - Client-side encryption if the client wants to perform encryption/decryption itself.
- **Access Controls:** IAM policies to regulate access to the SQS API.
- **SQS Access Policies** (similar to S3 bucket policies)
  - Useful for cross-account access to SQS queues.
  - Useful for allowing other services (SNS, S3...) to write to an SQS queue.

# 6. Message Visibility Timeout

- After a message is polled by a consumer, it becomes **invisible** to other consumers.
- By default, the "message visibility timeout" is **30 seconds**.
- That means the message has 30 seconds to be processed.
- After the message visibility timeout is over, the message is "visible" in SQS.
  ![SQS Visibility Timeout](/Images/AWSSQSMessageVisibilityTimeout.png)
- If a message is not processed within the visibility timeout, it will be processed **twice**.
- A consumer could call the `ChangeMessageVisibility` API to get more time.
- If visibility timeout is high (hours), and consumer crashes, re-processing will take time.
- If visibility timeout is too low (seconds), we may get duplicates.

# 7. FIFO Queue

- FIFO = First In First Out (ordering of messages in the queue).
- Limited throughput: 300 msg/s without batching, 3000 msg/s with.
- Exactly-once send capability (by removing duplicates using Deduplication ID).
- Messages are processed in order by the consumer.
- Ordering by Message Group ID (all messages in the same group are ordered) - mandatory parameter.

# 8. Dead Letter Queue (DLQ)

- If a consumer fails to process a message within the Visibility Timeout... the message goes back to the queue!
- We can set a threshold of how many times a message can go back to the queue.
- After the `MaximumReceives` threshold is exceeded, the message goes into a Dead Letter Queue (DLQ).
- Useful for **debugging**!
- **DLQ of a FIFO queue must also be a FIFO queue.**
- **DLQ of a Standard queue must also be a Standard queue.**
- Make sure to process the messages in the DLQ before they expire:
  - Good to set a retention of 14 days in the DLQ.

## 8.1. Redrive to Source

- Feature to help consume messages in the DLQ to understand what is wrong with them.
- When our code is fixed, we can redrive the messages from the DLQ back into the source queue (or any other queue) in batches without writing custom code.

# 9. Delay Queue

- Delay a message (consumers don't see it immediately) up to 15 minutes.
- Default is 0 seconds (message is available right away).
- Can set a default at queue level.
- Can override the default on send using the `DelaySeconds` parameter.

![SQS Delay Queue](/Images/AWSSQSDelayQueue.png)

# 10. Long Polling

- When a consumer requests messages from the queue, it can optionally "wait" for messages to arrive if there are none in the queue.
- This is called Long Polling.
- **LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application.**
- The wait time can be between 1 sec to 20 sec (20 sec preferable).
- Long Polling is preferable to Short Polling.
- Long polling can be enabled at the queue level or at the API level using `ReceiveMessageWaitTimeSeconds`.

# 11. SQS Extended Client

- Message size limit is 256KB, how to send large messages, e.g. 1GB?
  - Using the SQS Extended Client (Java Library).

# 12. Must know API

- `CreateQueue` - Creates a new standard or FIFO queue (MessageRetentionPeriod).
- `DeleteQueue` - Deletes the queue specified by the QueueUrl, regardless of the queue's contents. When you delete a queue, any messages in the queue are no longer available.
- `PurgeQueue` - Delete all the messages in queue.
- `SendMessage` (DelaySeconds), `ReceiveMessage`, `DeleteMessage`.
- `MaxNumberOfMessages` - Default 1, max 10 (for ReceiveMessage API).
- `ReceiveMessageWaitTimeSeconds` - Long Polling.
- `ChangeMessageVisibility` - Change the message timeout.
- `RemovePermission` - Revokes any permissions in the queue policy that matches the specified Label parameter.
- Batch APIs for `SendMessage`, `DeleteMessage`, `ChangeMessageVisibility` helps decrease your costs.

# 13. SQS FIFO

## 13.1. Deduplication

- De-duplication interval is 5 minutes.
- Two de-duplication methods:
  - Content-based deduplication: will do a SHA-256 hash of the message body.
  - `MessageDeduplicationId` - Explicitly provide a Message Deduplication ID.

![MessageDeduplicationId](/Images/AWSSQSDeduplicationID.png)

## 13.2. Message Grouping

- If you specify the same value of MessageGroupID in an SQS FIFO queue, you can only have one consumer, and all the messages are in order.
- To get ordering at the level of a subset of messages, specify different values for MessageGroupID.
  - Messages that share a common Message Group ID will be in order within the group.
  - Each Group ID can have a different consumer (parallel processing!).
  - Ordering across groups is not guaranteed.
