# Serverless Architectures <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Mobile application: MyTodoList](#1-mobile-application-mytodolist)
  - [1.1. Requirements](#11-requirements)
  - [1.2. Solution](#12-solution)
  - [1.3. Diagram](#13-diagram)
- [2. Serverless hosted website: MyBlog.com](#2-serverless-hosted-website-myblogcom)
  - [2.1. Requirements](#21-requirements)
  - [2.2. Solution](#22-solution)
  - [2.3. Diagram](#23-diagram)
- [3. Micro Services architecture](#3-micro-services-architecture)
  - [3.1. Requirements](#31-requirements)
  - [3.2. Discussions](#32-discussions)
- [4. 4.Software updates offloading](#4-4software-updates-offloading)
  - [4.1. Requirements](#41-requirements)
  - [4.2. Solution](#42-solution)
  - [4.3. Why CloudFront?](#43-why-cloudfront)

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

## 1.3. Diagram

![MyTodoList](/Images/Solution%20Architect/MobileApplicationMyTodoList.png)

# 2. Serverless hosted website: MyBlog.com

## 2.1. Requirements

- This website should scale globally.
- Blogs are rarely written, but often read.
- Some of the website is purely static files, the rest is a dynamic REST API.
- Caching must be implement where possible.
- Any new users that subscribes should receive a welcome email.
- Any photo uploaded to the blog should have a thumbnail generated.

## 2.2. Solution

- We've seen static content being distributed using CloudFront with S3.
- The REST API was serverless, didn't need Cognito because public.
- We leveraged a Global DynamoDB table to serve the data globally.
  - (we could have used Aurora Global Database).
- We enabled DynamoDB streams to trigger a Lambda function.
- The lambda function had an IAM role which could use SES.
- SES (Simple Email Service) was used to send emails in a serverless way.
- S3 can trigger SQS / SNS / Lambda to notify of events.

## 2.3. Diagram

![MyBlog](/Images/Solution%20Architect/WebsiteMyBlog.png)

# 3. Micro Services architecture

## 3.1. Requirements

- We want to switch to a micro service architecture.
- Many services interact with each other directly using a REST API.
- Each architecture for each micro service may vary in form and shape.
- We want a micro-service architecture so we can have a leaner development lifecycle for each service.

## 3.2. Discussions

- We are free to design each micro-service the way we want.
- **Synchronous patterns**
  - API Gateway.
  - Load Balancers.
- **Asynchronous patterns**
  - SQS.
  - Kinesis.
  - SNS.
  - Lambda triggers (S3).
- **Challenges with micro-services**
  - Repeated overhead for creating each new microservice.
  - Issues with optimizing server density/utilization.
  - Complexity of running multiple versions of multiple microservices simultaneously.
  - Proliferation of client-side code requirements to integrate with many separate services.
- Some of the challenges are solved by Serverless patterns:
  - API Gateway, Lambda scale automatically and we pay per usage.
  - We can easily clone API, reproduce environments.
  - Generated client SDK through Swagger integration for the API Gateway.

# 4. 4.Software updates offloading

## 4.1. Requirements

- We have an application running on EC2, that distributes software updates once in a while.
- When a new software update is out, we get a lot of request and the content is distributed in mass over the network. It's very costly.
- We don't want to change our application, but want to optimize our cost and CPU, how can we do it?

## 4.2. Solution

![Software Updates Offloading](/Images/Solution%20Architect/SoftwareUpdatesOffloading.png)

## 4.3. Why CloudFront?

- No changes to architecture.
- Will cache software update files at the edge.
- Software update files are not dynamic, they're static (never changing).
- Our EC2 instances aren't serverless.
- But CloudFront is, and will scale for us.
- Our ASG will not scale as much, and we'll save tremendously in EC2.
- We'll also save in availability, network bandwidth cost, etc.
- Easy way to make an existing application more scalable and cheaper!
