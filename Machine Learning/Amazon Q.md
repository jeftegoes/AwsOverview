# Amazon Q <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon Q Business](#1-amazon-q-business)
  - [1.1. Amazon Q Business + IAM Identity Center](#11-amazon-q-business--iam-identity-center)
  - [1.2. Admin Controls](#12-admin-controls)
- [2. Amazon Q Apps (Q Business)](#2-amazon-q-apps-q-business)
- [3. Amazon Q Developer](#3-amazon-q-developer)
- [4. Amazon Q Developer](#4-amazon-q-developer)
  - [4.1. IDE Extensions](#41-ide-extensions)
- [5. Amazon Q for QuickSight](#5-amazon-q-for-quicksight)
- [6. Amazon Q for EC2](#6-amazon-q-for-ec2)
- [7. Amazon Q for AWS Chatbot](#7-amazon-q-for-aws-chatbot)
- [8. Amazon Q for Glue](#8-amazon-q-for-glue)
- [9. PartyRock](#9-partyrock)

# 1. Amazon Q Business

- Fully managed Gen-AI assistant for your employees.
- Based on your company's knowledge and data.
  - Answer questions, provide summaries, generate content, automate tasks.
  - Perform routine actions (e.g., submit time-off requests, send meeting invites).
- Built on Amazon Bedrock (but you can't choose the underlying FM).
- Data Connectors (fully managed RAG) – Connects to 40+ popular enterprise data sources.
  - Amazon S3, RDS, Aurora, WorkDocs...
  - Microsoft 365, Salesforce, GDrive, Gmail, Slack, Sharepoint...
- **Plugins** – Allows you to interact with 3rd party services.
  - Jira, ServiceNow, Zendesk, Salesforce...
- Custom Plugins – Connects to any 3rd party application using APIs.

## 1.1. Amazon Q Business + IAM Identity Center

- Users can be authenticated through **IAM Identity Center (IAM)**.
- Users receive responses generated only from the documents they have access to.
  - IAM Identity Center can be configured with external Identity Providers.
  - IdP: Google Login, Microsoft Active Directory...

    todo: diagram

## 1.2. Admin Controls

- Controls and customize responses to your organizational needs.
- Admin controls == Guardrails.
- Block specific words or topics.
- Respond only with internal information (vs using external knowledge).
- Global controls & topic-level controls (more granular rules).

# 2. Amazon Q Apps (Q Business)

- Create Gen AI-powered apps without coding by using natural language.
- Leverages your company's internal data.
- Possibility to leverage plugins (Jira, etc...).

# 3. Amazon Q Developer

- Answer questions about the AWS documentation and AWS service selection.
- Answer questions about resources in your AWS account.
- Suggest CLI (Command Line Interface) to run to make changes to your account.
- Helps you do bill analysis, resolve errors, troubleshooting...

# 4. Amazon Q Developer

- AI code companion to help you code new applications (similar to GitHub Copilot).
- Supports many languages: Java, JavaScript, Python, TypeScript, C#...
- Real-time code suggestions and security scans
- Software agent to implement features, generate documentation, bootstrapping new projects.

## 4.1. IDE Extensions

- Integrates with IDE (Integrated Development Environment) to help with your software development needs.
  - Answer questions about AWS developmet.
  - Code completions and code generation.
  - Scan your code for security vulnerabilities.
  - Debugging, optimizations, improvements.

# 5. Amazon Q for QuickSight

- [Amazon QuickSight](/Analytics/Amazon%20QuickSight.md) is used to visualize your data and create dashboards about them.
- Amazon Q understands natural language that you use to ask questions about your data.
- Create executive summaries of your data.
- Ask and answer questions of data.
- Generate and edit visuals for your dashboards.

# 6. Amazon Q for EC2

- EC2 instances are the virtual servers you can start in AWS.
- Amazon Q for EC2 provides guidance and suggestions for EC2 instance types that are best suited to your new workload.
- Can provide requirements using natural language to get even more suggestions or ask for advice by providing other workload requirements.

# 7. Amazon Q for AWS Chatbot

- AWS Chatbot is a way for you to deploy an AWS Chatbot in a Slack or Microsoft Teams channel that knows about your AWS account.
- Troubleshoot issues, receive notifications for alarms, security findings, billing alerts, create support request.
- You can access Amazon Q directly in AWS Chatbot to accelerate understanding of the AWS services, troubleshoot issues, and identify remediation paths.

# 8. Amazon Q for Glue

- AWS Glue is an "ETL" (Extract Transform and Load) service used to move data across places
- Amazon Q for Glue can help with...
- **Chat**
  - Answer general questions about Glue
  - Provide links to the documentation
- **Data integration code generation**
  - Answer questions about AWS Glue ETL scripts
  - Generate new code
- **Troubleshoot**
  - Understand errors in AWS Glue jobs
  - Provide step-by-step instructions, to root cause and resolve your issues.

# 9. PartyRock

- GenAI app-building playground (powered by Amazon Bedrock).
- Allows you to experiment creating GenAI apps with various FMs (no coding or AWS account required).
- UI is similar to Amazon Q Apps (with less setup and no AWS account required).
