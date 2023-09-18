# AWS AppSync <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Security](#2-security)

# 1. Introduction


- AWS AppSync simplifies application development by letting you create a flexible API to securely access, manipulate, and combine data from one or more data sources.
- AppSync is a managed service that uses **GraphQL** to make it easy for applications to get exactly the data they need.
- With AppSync, you can build scalable applications, including those **requiring real-time updates**, on a range of data sources such as NoSQL data stores, relational databases, HTTP APIs, and your custom data sources with AWS Lambda.
- For mobile and web apps, AppSync additionally provides local data access when devices go offline, and data synchronization with customizable conflict resolution, when they are back online.
- **GraphQL** makes it easy for applications to get exactly the data they need.
- This includes combining data from **one or more sources**:
  - NoSQL data stores, Relational databases, HTTP APIs...
  - Integrates with DynamoDB, Aurora, OpenSearch & others.
  - Custom sources with AWS Lambda.
- Retrieve data in real-time with WebSocket or MQTT on WebSocket.
- For mobile apps: local data access & data synchronization.
- It all starts with uploading one GraphQL schema.

# 2. Security

- There are four ways you can authorize applications to interact with your AWS AppSync GraphQL API:
  - `API_KEY`
  - `AWS_IAM`: IAM users / roles / cross-account access.
  - `OPENID_CONNECT`: OpenID Connect provider / JSON Web Token.
  - `AMAZON_COGNITO_USER_POOLS`
- For custom domain and HTTPS, use CloudFront in front of AppSync.
