# AWS SDK - Software Development Kit <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What's the AWS SDK?](#1-whats-the-aws-sdk)
- [2. AWS SDK](#2-aws-sdk)

# 1. What's the AWS SDK?

- What if you want to perform actions on AWS directly from your applications code? (without using the CLI).
  - You can use an AWS SDK (software development kit)!
- Language-specific APIs (set of libraries).
- Enables you to access and manage AWS services programmatically.
- Embedded within your application.
- **Supports**
  - SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++).
  - Mobile SDKs (Android, iOS, ...)
  - IoT Device SDKs (Embedded C, Arduino, ...)
- **Example:** AWS CLI is built on AWS SDK for Python.

# 2. AWS SDK

- We have to use the AWS SDK when coding against AWS Services such as DynamoDB.
- Fun fact... the AWS CLI uses the Python SDK (boto3).
- We'll practice the AWS SDK when we get to the Lambda functions.
- Good to know: if you don't specify or configure a default region, then `sa-east-1` will be chosen by default.
