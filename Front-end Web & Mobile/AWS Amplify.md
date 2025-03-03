# AWS Amplify <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. AWS Amplify](#2-aws-amplify)
- [3. Important Features](#3-important-features)
- [4. AWS Amplify Hosting](#4-aws-amplify-hosting)
- [5. AWS Amplify - End-to-End (E2E) Testing](#5-aws-amplify---end-to-end-e2e-testing)

# 1. Introduction

- Create mobile and web applications:
  - **Amplify Studio:** Visually build a full-stack app, both front-end UI and a backend.
  - **Amplify Libraries:** Connect your app to existing AWS Services (Cognito, S3 and more).
  - **Amplify CLI:** Configure an Amplify backend, with a guided CLI workflow.
  - **Amplify Hosting:** Host secure, reliable, fast web apps or websites via the AWS content delivery network.

# 2. AWS Amplify

- A set of tools to get started with creating **mobile and web applications**.
  - **Elastic Beanstalk for mobile and web applications.**
- Must-have features such as **data storage, authentication, storage, and machine-learning**, all powered by AWS services.
- **Front-end libraries** with ready-to-use components for React.js, Vue, Javascript, iOS, Android, Flutter, etc...
- Incorporates AWS best practices to for reliability, security, scalability.
- Build and deploy with the **Amplify CLI** or **Amplify Studio**.

# 3. Important Features

- **AUTHENTICATION**
  - Leverages Amazon Cognito.
  - User registration, authentication, account recovery & other operations.
  - Support MFA, Social Sign-in, etc...
  - Pre-built UI components.
  - Fine-grained authorization.
- **DATASTORE**
  - Leverages Amazon AppSync and Amazon DynamoDB.
  - Work with local data and have automatic synchronization to the cloud without complex code.
  - Powered by GraphQL.
  - Offline and real-time capabilities.
  - Visual data modeling w/ Amplify Studio.

# 4. AWS Amplify Hosting

- Build and Host Modern Web Apps.
- CICD (build, test, deploy).
- Pull Request Previews.
- Custom Domains.
- Monitoring.
- Redirect and Custom Headers.
- Password protection.

# 5. AWS Amplify - End-to-End (E2E) Testing

- Run end-to-end (E2E) tests in the test phase in Amplify.
- Catch regressions before pushing code to production.
- Use the test step to run any test commands at build time `amplify.yml `.
- **Integrated with Cypress testing framework**.
  - Allows you to generate UI report for your tests.
