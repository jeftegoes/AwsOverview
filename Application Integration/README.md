# Application Integration <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon SQS - Standard Queue](#1-amazon-sqs---standard-queue)
- [2. Amazon SNS](#2-amazon-sns)
- [3. Summary](#3-summary)

# 1. Amazon SQS - Standard Queue

- **Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. It uses a pull-based system.** [Amazon SQS](/Application%20Integration/Amazon%20SQS.md)

# 2. Amazon SNS

- **Is a highly available, durable, secure, fully managed pub/sub messaging service that enables you to decouple microservices, distributed systems, and serverless applications. It uses a push-based system.** [Amazon SNS](/Application%20Integration/Amazon%20SNS.md.md)

# 3. Summary

- **SQS**
  - Queue service in AWS.
  - Multiple Producers, messages are kept up to 14 days.
  - Multiple Consumers share the read and delete messages when done.
  - Used to decouple applications in AWS.
- **SNS**
  - Notification service in AWS.
  - Subscribers: Email, Lambda, SQS, HTTP, Mobile...
  - Multiple Subscribers, send all messages to all of them.
  - No message retention.
