# Amazon MQ <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)

# 1. Introduction

- SQS, SNS are "cloud-native" services: Proprietary protocols from AWS.
- Traditional applications running from on-premises may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS.
- **When migrating to the cloud**, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ.
- **Amazon MQ is a managed message broker service for: RabbitMQ and ActiveMQ.**
- Amazon MQ doesn't "scale" as much as SQS / SNS.
- Amazon MQ runs on servers, can run in Multi-AZ with failover.
- Amazon MQ has both queue feature (~SQS) and topic features (~SNS).
