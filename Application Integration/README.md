# Application Integration <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Amazon SQS](#2-amazon-sqs)
- [3. Amazon SNS](#3-amazon-sns)
- [4. Amazon MQ](#4-amazon-mq)
- [5. AWS Step Functions](#5-aws-step-functions)
- [6. Amazon AppFlow](#6-amazon-appflow)
- [7. Summary](#7-summary)

# 1. Introduction

- When we start deploying multiple applications, they will inevitably need to communicate with one another.
- There are two patterns of application communication.
  1. Synchronous communications (application to application).
  2. Asynchronous / Event based (application to queue to application).
- Synchronous between applications can be problematic if there are sudden spikes of traffic.
- What if you need to suddenly encode 1000 videos but usually it's 10?
- In that case, it's better to **decouple** your applications using:
  - **SQS:** Queue model.
  - **SNS:** Pub/sub model.
  - **Kinesis:** Real-time streaming model.
- These services can scale independently from our application!

# 2. Amazon SQS

- **Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. It uses a pull-based system.** [Amazon SQS](Amazon%20SQS.md)

# 3. Amazon SNS

- **Is a highly available, durable, secure, fully managed pub/sub messaging service that enables you to decouple microservices, distributed systems, and serverless applications. It uses a push-based system.** [Amazon SNS](Amazon%20SNS.md)

# 4. Amazon MQ

- **Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers in the cloud.** [Amazon MQ](Amazon%20MQ.md)

# 5. AWS Step Functions

- **AWS Step Functions is a low-code visual workflow service used to orchestrate AWS services, automate business processes, and build Serverless applications. It manages failures, retries, parallelization, service integrations, ...** [AWS Step Functions](AWS%20Step%20Functions.md)

# 6. Amazon AppFlow

[Amazon AppFlow](Amazon%20AppFlow.md)

# 7. Summary

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
- **Kinesis:** Real-time data streaming, persistence and analysis.
- **Amazon MQ:** Managed Apache MQ in the cloud (MQTT, AMQP protocols).
- **When using SQS or SNS, you apply the "decouple your applications" principle. This means that IT systems should be designed in a way that reduces interdependencies-a change or a failure in one component should not cascade to other components.**
