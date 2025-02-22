# Serverless Architectures <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. API Rate Limits](#1-api-rate-limits)
- [2. Service Quotas (Service Limits)](#2-service-quotas-service-limits)
- [3. Exponential Backoff (any AWS service)](#3-exponential-backoff-any-aws-service)

# 1. Mobile application: MyTodoList

## 1.1. Requirements

- We want to create a mobile application with the following requirements:
- Expose as REST API with HTTPS.
- Serverless architecture.
- Users should be able to directly interact with their own folder in [S3](/Storage/Amazon%20S3.md).
- Users should authenticate through a managed serverless service.
- The users can write and read to-dos, but they mostly read them.
- The database should scale, and have some high read throughput.

## 1.2. Solution

- Serverless REST API:
  - HTTPS
  - [API Gateway](/Networking%20&%20Content%20Delivery/Amazon%20API%20Gateway.md)
  - [Lambda](/Compute/AWS%20Lambda.md)
  - [DynamoDB](/Database/Amazon%20DynamoDB.md)
- Using Cognito to generate temporary credentials to access S3 bucket with restricted policy. App users can directly access AWS resources this way. Pattern can be applied to [DynamoDB](/Database/Amazon%20DynamoDB.md), [Lambda](/Compute/AWS%20Lambda.md)...
- Caching the reads on [DynamoDB](/Database/Amazon%20DynamoDB.md) using DAX.
- Caching the REST requests at the API Gateway level.
- Security for authentication and authorization with Cognito.
