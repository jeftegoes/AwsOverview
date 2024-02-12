# Aws overview <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Traditionally, how to build infrastructure](#1-traditionally-how-to-build-infrastructure)
  - [1.1. How websites work](#11-how-websites-work)
  - [1.2. What is a server composed of?](#12-what-is-a-server-composed-of)
  - [1.3. IT Terminology](#13-it-terminology)
  - [1.4. Problems with traditional IT approach](#14-problems-with-traditional-it-approach)
- [2. What is Cloud Computing?](#2-what-is-cloud-computing)
  - [2.1. You've been using some Cloud services](#21-youve-been-using-some-cloud-services)
  - [2.2. The Deployment Models of the Cloud](#22-the-deployment-models-of-the-cloud)
    - [2.2.1. Private Cloud:](#221-private-cloud)
    - [2.2.2. Public Cloud:](#222-public-cloud)
    - [2.2.3. Hybrid Cloud:](#223-hybrid-cloud)
  - [2.3. The Five Characteristics of Cloud Computing](#23-the-five-characteristics-of-cloud-computing)
  - [2.4. Six Advantages of Cloud Computing](#24-six-advantages-of-cloud-computing)
  - [2.5. Problems solved by the Cloud](#25-problems-solved-by-the-cloud)
  - [2.6. Types of Cloud Computing](#26-types-of-cloud-computing)
  - [2.7. Example of Cloud Computing Types](#27-example-of-cloud-computing-types)
  - [2.8. Pricing of the Cloud - Quick Overview](#28-pricing-of-the-cloud---quick-overview)
  - [2.9. AWS Global Infrastructure](#29-aws-global-infrastructure)
  - [2.10. AWS Regions](#210-aws-regions)
  - [2.11. How to choose an AWS Region?](#211-how-to-choose-an-aws-region)
  - [2.12. AWS Availability Zones](#212-aws-availability-zones)
  - [2.13. AWS Points of Presence (Edge Locations)](#213-aws-points-of-presence-edge-locations)
  - [2.14. Tour of the AWS Console](#214-tour-of-the-aws-console)
  - [2.15. Shared Responsibility Model diagram](#215-shared-responsibility-model-diagram)
  - [2.16. AWS Acceptable Use Policy](#216-aws-acceptable-use-policy)
- [3. IAM - Identity and Access Management and AWS CLI](#3-iam---identity-and-access-management-and-aws-cli)
  - [3.1. IAM - Summary](#31-iam---summary)
- [4. Compute](#4-compute)
- [5. Storage](#5-storage)
- [6. Databases](#6-databases)
- [7. Other Compute Services: ECS, Batch, Lightsail](#7-other-compute-services-ecs-batch-lightsail)
  - [7.1. ECS - Elastic Container Service, Fargate and ECR - Elastic Container Registry](#71-ecs---elastic-container-service-fargate-and-ecr---elastic-container-registry)
  - [7.2. Amazon API Gateway](#72-amazon-api-gateway)
  - [7.3. AWS Batch](#73-aws-batch)
  - [7.4. Batch vs Lambda](#74-batch-vs-lambda)
  - [7.5. Amazon Lightsail](#75-amazon-lightsail)
  - [7.6. Other Compute - Summary](#76-other-compute---summary)
- [8. Deploying and Managing Infrastructure at Scale](#8-deploying-and-managing-infrastructure-at-scale)
  - [8.1. CloudFormation](#81-cloudformation)
  - [8.2. AWS Cloud Development Kit (CDK)](#82-aws-cloud-development-kit-cdk)
  - [8.3. AWS CI/CD](#83-aws-cicd)
  - [8.4. AWS Systems Manager (SSM)](#84-aws-systems-manager-ssm)
  - [8.5. AWS OpsWorks](#85-aws-opsworks)
  - [8.6. AWS Amplify](#86-aws-amplify)
  - [8.7. Deployment - Summary](#87-deployment---summary)
- [9. Route 53](#9-route-53)
- [10. Global Infrastructure](#10-global-infrastructure)
  - [10.1. Why make a global application?](#101-why-make-a-global-application)
  - [10.2. Global AWS Infrastructure](#102-global-aws-infrastructure)
  - [10.3. Global Applications in AWS](#103-global-applications-in-aws)
  - [10.4. AWS CloudFront](#104-aws-cloudfront)
  - [10.5. AWS Global Accelerator](#105-aws-global-accelerator)
  - [10.6. AWS Global Accelerator vs CloudFront](#106-aws-global-accelerator-vs-cloudfront)
  - [10.7. AWS Outposts](#107-aws-outposts)
  - [10.8. AWS WaveLength](#108-aws-wavelength)
  - [10.9. AWS Local Zones](#109-aws-local-zones)
  - [10.10. Global Applications Architecture](#1010-global-applications-architecture)
  - [10.11. Global Applications in AWS - Summary](#1011-global-applications-in-aws---summary)
  - [10.12. Global Applications in AWS - Summary](#1012-global-applications-in-aws---summary)
- [11. Cloud Integration](#11-cloud-integration)
  - [11.1. Amazon SQS - Standard Queue](#111-amazon-sqs---standard-queue)
  - [11.2. Amazon Kinesis](#112-amazon-kinesis)
  - [11.3. Amazon SNS](#113-amazon-sns)
  - [11.4. Amazon MQ](#114-amazon-mq)
  - [11.5. Integration - Summary](#115-integration---summary)
- [12. Cloud Monitoring](#12-cloud-monitoring)
  - [12.1. Amazon CloudWatch Metrics](#121-amazon-cloudwatch-metrics)
    - [12.1.1. Important Metrics](#1211-important-metrics)
  - [12.2. Amazon CloudWatch Alarms](#122-amazon-cloudwatch-alarms)
  - [12.3. Amazon CloudWatch Logs](#123-amazon-cloudwatch-logs)
  - [12.4. Amazon EventBridge](#124-amazon-eventbridge)
  - [12.5. AWS CloudTrail](#125-aws-cloudtrail)
  - [12.6. AWS X-Ray](#126-aws-x-ray)
  - [12.7. Amazon CodeGuru](#127-amazon-codeguru)
  - [12.8. AWS Health Dashboard](#128-aws-health-dashboard)
  - [12.9. Monitoring Summary](#129-monitoring-summary)
- [13. VPC](#13-vpc)
- [14. Security \& Compliance](#14-security--compliance)
  - [14.1. AWS Shared Responsibility Model](#141-aws-shared-responsibility-model)
    - [14.1.1. Example, for RDS](#1411-example-for-rds)
    - [14.1.2. Example, for S3](#1412-example-for-s3)
  - [14.2. DDOS Protection on AWS](#142-ddos-protection-on-aws)
  - [14.3. AWS Shield](#143-aws-shield)
  - [14.4. AWS WAF - Web Application Firewall](#144-aws-waf---web-application-firewall)
  - [14.5. Penetration Testing on AWS Cloud](#145-penetration-testing-on-aws-cloud)
  - [14.6. Data at rest vs. Data in transit](#146-data-at-rest-vs-data-in-transit)
  - [14.7. AWS KMS (Key Management Service)](#147-aws-kms-key-management-service)
  - [14.8. AWS Certificate Manager (ACM)](#148-aws-certificate-manager-acm)
  - [14.9. AWS Secrets Manager](#149-aws-secrets-manager)
  - [14.10. AWS Artifact (not really a service)](#1410-aws-artifact-not-really-a-service)
  - [14.11. Amazon GuardDuty](#1411-amazon-guardduty)
  - [14.12. Amazon Inspector](#1412-amazon-inspector)
  - [14.13. AWS Config](#1413-aws-config)
  - [14.14. Amazon Macie](#1414-amazon-macie)
  - [14.15. AWS Security Hub](#1415-aws-security-hub)
  - [14.16. Amazon Detective](#1416-amazon-detective)
  - [14.17. AWS Abuse](#1417-aws-abuse)
  - [14.18. Root user privileges](#1418-root-user-privileges)
  - [14.19. Summary: Security \& Compliance](#1419-summary-security--compliance)
- [15. Machine Learning](#15-machine-learning)
- [16. Account Management, Billing \& Support](#16-account-management-billing--support)
  - [16.1. AWS Organizations](#161-aws-organizations)
  - [16.2. AWS Control Tower](#162-aws-control-tower)
  - [16.3. Pricing Models in AWS](#163-pricing-models-in-aws)
    - [16.3.1. Free services and free tier in AWS](#1631-free-services-and-free-tier-in-aws)
    - [16.3.2. Compute Pricing - EC2](#1632-compute-pricing---ec2)
    - [16.3.3. Compute Pricing - Lambda \& ECS](#1633-compute-pricing---lambda--ecs)
    - [16.3.4. Storage Pricing - S3](#1634-storage-pricing---s3)
    - [16.3.5. Storage Pricing - EBS](#1635-storage-pricing---ebs)
    - [16.3.6. Database Pricing - RDS](#1636-database-pricing---rds)
    - [16.3.7. Content Delivery - CloudFront](#1637-content-delivery---cloudfront)
      - [16.3.7.1. Networking Costs in AWS per GB - Simplified](#16371-networking-costs-in-aws-per-gb---simplified)
    - [16.3.8. Savings Plan](#1638-savings-plan)
  - [16.4. AWS Compute Optimizer](#164-aws-compute-optimizer)
  - [16.5. Billing and Costing Tools](#165-billing-and-costing-tools)
  - [16.6. AWS Pricing Calculator](#166-aws-pricing-calculator)
  - [16.7. AWS Billing Dashboard](#167-aws-billing-dashboard)
    - [16.7.1. Cost Allocation Tags](#1671-cost-allocation-tags)
    - [16.7.2. AWS Resource Groups and Tagging](#1672-aws-resource-groups-and-tagging)
    - [16.7.3. Cost and Usage Reports](#1673-cost-and-usage-reports)
    - [16.7.4. Cost Explorer](#1674-cost-explorer)
  - [16.8. Billing Alarms in CloudWatch](#168-billing-alarms-in-cloudwatch)
  - [16.9. AWS Budgets](#169-aws-budgets)
  - [16.10. AWS Trusted Advisor](#1610-aws-trusted-advisor)
  - [16.11. AWS Support Plans Pricing](#1611-aws-support-plans-pricing)
    - [16.11.1. AWS Basic Support Plan](#16111-aws-basic-support-plan)
    - [16.11.2. AWS Developer Support Plan](#16112-aws-developer-support-plan)
    - [16.11.3. AWS Business Support Plan (24/7)](#16113-aws-business-support-plan-247)
    - [16.11.4. AWS Enterprise On-Ramp Support Plan (24/7)](#16114-aws-enterprise-on-ramp-support-plan-247)
    - [16.11.5. AWS Enterprise Support Plan (24/7)](#16115-aws-enterprise-support-plan-247)
  - [16.12. Account Best Practices - Summary](#1612-account-best-practices---summary)
  - [16.13. Billing and Costing Tools - Summary](#1613-billing-and-costing-tools---summary)
- [17. AWS STS - Security Token Service](#17-aws-sts---security-token-service)
- [18. AWS Directory Services](#18-aws-directory-services)
- [19. Advanced Identity](#19-advanced-identity)
  - [19.1. Amazon Cognito](#191-amazon-cognito)
  - [19.2. AWS Organizations](#192-aws-organizations)
  - [19.3. AWS IAM Identity Center (Successor to AWS Single Sign-On)](#193-aws-iam-identity-center-successor-to-aws-single-sign-on)
  - [19.4. Advanced Identity - Summary](#194-advanced-identity---summary)
- [20. Other AWS Services](#20-other-aws-services)
  - [20.1. Amazon WorkSpaces](#201-amazon-workspaces)
  - [20.2. Amazon AppStream 2.0](#202-amazon-appstream-20)
    - [20.2.1. Amazon AppStream 2.0 vs WorkSpaces](#2021-amazon-appstream-20-vs-workspaces)
  - [20.3. Amazon Sumerian](#203-amazon-sumerian)
  - [20.4. AWS IoT Core](#204-aws-iot-core)
  - [20.5. Amazon Elastic Transcoder](#205-amazon-elastic-transcoder)
  - [20.6. AWS Device Farm](#206-aws-device-farm)
  - [20.7. AWS Backup](#207-aws-backup)
    - [20.7.1. Disaster Recovery Strategies](#2071-disaster-recovery-strategies)
  - [20.8. AWS Elastic Disaster Recovery (DRS)](#208-aws-elastic-disaster-recovery-drs)
  - [20.9. AWS DataSync](#209-aws-datasync)
  - [20.10. AWS Fault Injection Simulator (FIS)](#2010-aws-fault-injection-simulator-fis)
  - [20.11. AWS Step Functions](#2011-aws-step-functions)
  - [20.12. Amazon AppFlow](#2012-amazon-appflow)
  - [20.13. AWS CloudSearch](#2013-aws-cloudsearch)
  - [20.14. AWS Service Catalog](#2014-aws-service-catalog)
  - [20.15. Amazon SES - Simple Email Service](#2015-amazon-ses---simple-email-service)
- [21. AWS Architecting \& Ecosystem](#21-aws-architecting--ecosystem)
  - [21.1. Well Architected Framework General - Guiding Principles](#211-well-architected-framework-general---guiding-principles)
  - [21.2. AWS Cloud Best Practices - Design Principles](#212-aws-cloud-best-practices---design-principles)
  - [21.3. Well Architected Framework 6 Pillars](#213-well-architected-framework-6-pillars)
    - [21.3.1. Operational Excellence](#2131-operational-excellence)
      - [21.3.1.1. Operational Excellence AWS Services](#21311-operational-excellence-aws-services)
    - [21.3.2. Security](#2132-security)
      - [21.3.2.1. Security AWS Services](#21321-security-aws-services)
    - [21.3.3. Reliability](#2133-reliability)
      - [21.3.3.1. Reliability AWS Services](#21331-reliability-aws-services)
    - [21.3.4. Performance Efficiency](#2134-performance-efficiency)
      - [21.3.4.1. Performance Efficiency AWS Services](#21341-performance-efficiency-aws-services)
    - [21.3.5. Cost Optimization](#2135-cost-optimization)
      - [21.3.5.1. Cost Optimization AWS Services](#21351-cost-optimization-aws-services)
    - [21.3.6. Sustainability](#2136-sustainability)
      - [21.3.6.1. Sustainability AWS Services](#21361-sustainability-aws-services)
  - [21.4. AWS Well-Architected Tool](#214-aws-well-architected-tool)
  - [21.5. AWS Right Sizing](#215-aws-right-sizing)
  - [21.6. AWS Ecosystem - Free resources](#216-aws-ecosystem---free-resources)
  - [21.7. AWS Marketplace](#217-aws-marketplace)
  - [21.8. AWS Training](#218-aws-training)
  - [21.9. AWS Professional Services \& Partner Network](#219-aws-professional-services--partner-network)
  - [21.10. AWS Knowledge Center](#2110-aws-knowledge-center)
- [22. AWS Cloud Map](#22-aws-cloud-map)
- [23. AWS Limits (Quotas)](#23-aws-limits-quotas)
- [24. AWS related Abbreviations \& Acronyms](#24-aws-related-abbreviations--acronyms)
  - [24.1. A](#241-a)
  - [24.2. B](#242-b)
  - [24.3. C](#243-c)
  - [24.4. D](#244-d)
  - [24.5. E](#245-e)
  - [24.6. F](#246-f)
  - [24.7. H](#247-h)
  - [24.8. I](#248-i)
  - [24.9. J](#249-j)
  - [24.10. K](#2410-k)
  - [24.11. L](#2411-l)
  - [24.12. M](#2412-m)
  - [24.13. N](#2413-n)
  - [24.14. O](#2414-o)
  - [24.15. P](#2415-p)
  - [24.16. Q](#2416-q)
  - [24.17. R](#2417-r)
  - [24.18. S](#2418-s)
  - [24.19. T](#2419-t)
  - [24.20. V](#2420-v)
  - [24.21. W](#2421-w)
- [25. Commands AWS CLI](#25-commands-aws-cli)
  - [25.1. DynamoDB](#251-dynamodb)
  - [25.2. S3](#252-s3)
  - [25.3. Lambda](#253-lambda)
  - [25.4. ECS](#254-ecs)
  - [25.5. Systems Manager](#255-systems-manager)
- [26. Credits](#26-credits)

# 1. Traditionally, how to build infrastructure

## 1.1. How websites work

- Clients have IP addresses.
- Servers have IP addresses.

## 1.2. What is a server composed of?

- Compute: CPU.
- Memory: RAM.
- Storage: Data.
- Database: Store data in a structured way.
- Network: Routers, switch, DNS server.

## 1.3. IT Terminology

- Network: cables, routers and servers connected with each other.
- Router: A networking device that forwards data packets between computer
  networks. They know where to send your packets on the internet!
- Switch: Takes a packet and send it to the correct server / client on your network.

## 1.4. Problems with traditional IT approach

- Pay for the rent for the data center.
- Pay for power supply, cooling, maintenance.
- Adding and replacing hardware takes time.
- Scaling is limited.
- Hire 24/7 team to monitor the infrastructure.
- How to deal with disasters? (earthquake, power shutdown, fire...).
- Can we externalize all this? **CLOUD**

# 2. What is Cloud Computing?

- Cloud computing is the on-demand delivery of compute power, database storage, applications, and other IT resources.
- Through a cloud services platform with pay-as-you-go pricing.
- You can provision exactly the right type and size of computing resources you need.
- You can access as many resources as you need, almost instantly.
- Simple way to access servers, storage, databases and a set of application services.
- Amazon Web Services owns and maintains the network-connected hardware required for these application services, while you provision and use what you need via a web application.

## 2.1. You've been using some Cloud services

- Gmail
  - E-mail cloud service
  - Pay for ONLY your emails stored (no infrastructure, etc.)
- Dropbox
  - Cloud Storage Service
  - Originally built on AWS
- Netflix
  - Built on AWS
  - Video on Demand

## 2.2. The Deployment Models of the Cloud

### 2.2.1. Private Cloud:

- Cloud services used by a single organization, not exposed to the public.
- Complete control.
- Security for sensitive applications.
- Meet specific business needs.

### 2.2.2. Public Cloud:

- Cloud resources owned and operated by a third-party cloud service provider delivered over the Internet.

### 2.2.3. Hybrid Cloud:

- Keep some servers on premises and extend some capabilities to the Cloud.
- Control over sensitive assets in your private infrastructure.
- Flexibility and cost-effectiveness of the public cloud.

## 2.3. The Five Characteristics of Cloud Computing

- On-demand self service:
  - Users can provision resources and use them without human interaction from the service provider
- Broad network access:
  - Resources available over the network, and can be accessed by diverse client platforms
- Multi-tenancy and resource pooling:
  - Multiple customers can share the same infrastructure and applications with security and privacy
  - Multiple customers are serviced from the same physical resources
- Rapid elasticity and scalability:
  - Automatically and quickly acquire and dispose resources when needed
  - Quickly and easily scale based on demand
- Measured service:
  - Usage is measured, users pay correctly for what they have used

## 2.4. Six Advantages of Cloud Computing

- Trade capital expense (CAPEX) for operational expense (OPEX)
  - Pay On-Demand: don't own hardware
  - Reduced Total Cost of Ownership (TCO) & Operational Expense (OPEX)
- Benefit from massive economies of scale
  - Prices are reduced as AWS is more efficient due to large scale
- Stop guessing capacity
  - Scale based on actual measured usage
- Increase speed and agility
- Stop spending money running and maintaining data centers
- Go global in minutes: leverage the AWS global infrastructure

## 2.5. Problems solved by the Cloud

- Flexibility: change resource types when needed
- Cost-Effectiveness: pay as you go, for what you use
- Scalability: accommodate larger loads by making hardware stronger or adding additional nodes
- Elasticity: ability to scale out and scale-in when needed
- High-availability and fault-tolerance: build across data centers
- Agility: rapidly develop, test and launch software applications

## 2.6. Types of Cloud Computing

- Infrastructure as a Service (IaaS)
  - Provide building blocks for cloud IT.
  - Provides networking, computers, data storage space.
  - Highest level of flexibility.
  - Easy parallel with traditional on-premises IT.
- Platform as a Service (PaaS)
  - Removes the need for your organization to manage the underlying infrastructure.
  - Focus on the deployment and management of your applications.
- Software as a Service (SaaS)
  - Completed product that is run and managed by the service provider.

## 2.7. Example of Cloud Computing Types

- Infrastructure as a Service
  - Amazon EC2 (on AWS)
  - GCP, Azure, Rackspace, Digital Ocean, Linode
- Platform as a Service:
  - Elastic Beanstalk (on AWS)
  - Heroku, Google App Engine (GCP), Windows Azure (Microsoft)
- Software as a Service:
  - Many AWS services (ex: Rekognition for Machine Learning)
  - Google Apps (Gmail), Dropbox, Zoom

## 2.8. Pricing of the Cloud - Quick Overview

- AWS has 3 pricing fundamentals, following the pay-as-you-go pricing
  model
  - Compute:
    - Pay for compute time
  - Storage:
    - Pay for data stored in the Cloud
  - Data transfer **OUT** of the Cloud:
    - Data transfer IN is free
  - Solves the expensive issue of traditional IT

## 2.9. AWS Global Infrastructure

- AWS Regions.
- AWS Availability Zones.
- AWS Data Centers.
- AWS Edge Locations / Points of Presence.
- https://infrastructure.aws/

## 2.10. AWS Regions

- AWS has Regions all around the world.
- Names can be us-east-1, eu-west-3...
- A region is a cluster of data centers.
- Most AWS services are region-scoped.

## 2.11. How to choose an AWS Region?

- **Compliance** with data governance and legal requirements: data never leaves a region without your explicit permission.
- **Proximity** to customers: reduced latency.
- **Available services** within a Region: new services and new features aren't available in every Region.
- **Pricing** pricing varies region to region and is transparent in the service pricing page.
- **Capacity is unlimited in the cloud, you do not need to worry about it. The 4 points of considerations when choosing an AWS Region are: compliance with data governance and legal requirements, proximity to customers, available services and features within a Region, and pricing.**

## 2.12. AWS Availability Zones

- Each region has many availability zones (usually 3, min is 2, max is 6).
- Example:
  - ap-southeast-2a
  - ap-southeast-2b
  - ap-southeast-2c
- Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity.
- They're separate from each other, so that they're isolated from disasters.
- They're connected with high bandwidth, ultra-low latency networking.

## 2.13. AWS Points of Presence (Edge Locations)

- Amazon has 216 Points of Presence (205 Edge Locations & 11 Regional Caches) in 84 cities across 42 countries.
- Content is delivered to end users with lower latency.

## 2.14. Tour of the AWS Console

- AWS has Global Services:
  - Identity and Access Management (IAM)
  - Route 53 (DNS service)
  - CloudFront (Content Delivery Network)
  - WAF (Web Application Firewall)
- Most AWS services are Region-scoped:
  - Amazon EC2 (Infrastructure as a Service)
  - Elastic Beanstalk (Platform as a Service)
  - Lambda (Function as a Service)
  - Rekognition (Software as a Service)
- Region Table: https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services

## 2.15. Shared Responsibility Model diagram

- AWS = RESPONSIBILITY FOR THE SECURITY OF THE CLOUD
- CUSTOMER = RESPONSIBILITY FOR THE SECURITY IN THE CLOUD
- https://aws.amazon.com/compliance/shared-responsibility-model/

## 2.16. AWS Acceptable Use Policy

- https://aws.amazon.com/aup/
- No Illegal, Harmful, or Offensive Use or Content
- No Security Violations
- No Network Abuse
- No E-Mail or Other Message Abuse

# 3. IAM - Identity and Access Management and AWS CLI

[IAM - Identity and Access Management and AWS CLI](/Security,%20Identity,%20&%20Compliance/AWS%20IAM.md)

## 3.1. IAM - Summary

- **Users**: mapped to a physical user, has a password for AWS Console.
- **Groups**: contains users only.
- **Policies**: JSON document that outlines permissions for users or groups.
- **Roles**: for EC2 instances or AWS services.
- **Security**: MFA + Password Policy.
- **AWS CLI**: manage your AWS services using the command-line.
- **AWS SDK**: manage your AWS services using a programming language.
- **Access Keys**: access AWS using the CLI or SDK.
- **Audit**: IAM Credential Reports & IAM Access Advisor.

# 4. Compute

[Compute](Compute/README.md)

# 5. Storage

[Storage](Storage/README.md)

# 6. Databases

[Databases](Database/README.md)

# 7. Other Compute Services: ECS, Batch, Lightsail

## 7.1. ECS - Elastic Container Service, Fargate and ECR - Elastic Container Registry

[AWS ECS, Fargate and ECR](AWS%20ECS.md)

## 7.2. Amazon API Gateway

[AWS API Gateway](/AWS%20API%20Gateway.md)

## 7.3. AWS Batch

- **Fully managed** batch processing at **any scale**.
- Efficiently run 100,000s of computing batch jobs on AWS.
- A "batch" job is a job with a start and an end (opposed to continuous).
- Batch will dynamically launch **EC2 instances** or **Spot Instances**.
- AWS Batch provisions the right amount of compute / memory.
- You submit or schedule batch jobs and AWS Batch does the rest!
- Batch jobs are defined as **Docker images** and **run on ECS**.
- Helpful for cost optimizations and focusing less on the infrastructure.

## 7.4. Batch vs Lambda

- Lambda:
  - Time limit.
  - Limited runtimes.
  - Limited temporary disk space.
  - Serverless.
- Batch:
  - No time limit.
  - Any runtime as long as it's packaged as a Docker image.
  - Rely on EBS / instance store for disk space.
  - Relies on EC2 (can be managed by AWS).

## 7.5. Amazon Lightsail

- **Amazon Lightsail is designed to be the easiest way to launch and manage a virtual private server with AWS. Lightsail plans include everything you need to jumpstart your project - a virtual machine, SSD- based storage, data transfer, DNS management, and a static IP address - for a low, predictable price. It can be used to create a simple web application, a website or a dev/test environment.**
- Virtual servers, storage, databases, and networking.
- Low & predictable pricing.
- Simpler alternative to using EC2, RDS, ELB, EBS, Route 53...
- Great for people with little cloud experience!
- Can setup notifications and monitoring of your Lightsail resources.
- Use cases:
  - Simple web applications (has templates for LAMP, Nginx, MEAN, Node.js...).
  - Websites (templates for WordPress, Magento, Plesk, Joomla).
  - Dev / Test environment.
- Has high availability but no auto-scaling, limited AWS integrations.

## 7.6. Other Compute - Summary

- Docker: container technology to run applications.
- ECS: run Docker containers on EC2 instances.
- Fargate:
  - Run Docker containers without provisioning the infrastructure.
  - Serverless offering (no EC2 instances).
- ECR: Private Docker Images Repository.
- Batch: run batch jobs on AWS across managed EC2 instances.
- Lightsail: predictable & low pricing for simple application & DB stacks.

# 8. Deploying and Managing Infrastructure at Scale

## 8.1. CloudFormation

[AWS CloudFormation](AWS%20CloudFormation.md)

## 8.2. AWS Cloud Development Kit (CDK)

[AWS Cloud Development Kit](AWS%20Cloud%20Development%20Kit.md)

## 8.3. AWS CI/CD

[AWS CI/CD](AWS%20CICD.md)

## 8.4. AWS Systems Manager (SSM)

- **AWS Systems Manager gives you visibility and control of your infrastructure on AWS. It is used for patching systems at scale.** [AWS Systems Manager](AWS%20Systems%20Manager.md)

## 8.5. AWS OpsWorks

[AWS OpsWorks](Management%20&%20Governance/AWS%20OpsWorks.md)

## 8.6. AWS Amplify

- [AWS Amplify](AWS%20Amplify.md)

## 8.7. Deployment - Summary

- CloudFormation: (AWS only):
  - Infrastructure as Code, works with almost all of AWS resources.
  - Repeat across Regions & Accounts.
- Beanstalk: (AWS only):
  - Platform as a Service (PaaS), limited to certain programming languages or Docker.
  - Deploy code consistently with a known architecture: ex, ALB + EC2 + RDS.
- CodeDeploy (hybrid): deploy & upgrade any application onto servers.
- Systems Manager (hybrid): patch, configure and run commands at scale.
- OpsWorks (hybrid): managed Chef and Puppet in AWS.
- CodeCommit: Store code in private git repository (version controlled).
- CodeBuild: Build & test code in AWS.
- CodeDeploy: Deploy code onto servers.
- CodePipeline: Orchestration of pipeline (from code to build to deploy).
- CodeArtifact: Store software packages / dependencies on AWS.
- CodeStar: Unified view for allowing developers to do CICD and code.
- Cloud9: Cloud IDE (Integrated Development Environment) with collab.
- AWS CDK: Define your cloud infrastructure using a programming language.

# 9. Route 53

[AWS Route 53](AWS%20Route%2053.md)

# 10. Global Infrastructure

## 10.1. Why make a global application?

- A **global application** is an application deployed in **multiple geographies**.
- On AWS: this could be **Regions** and / or **Edge Locations**.
- Decreased Latency:
  - Latency is the time it takes for a network packet to reach a server.
  - It takes time for a packet from Asia to reach the US.
  - Deploy your applications closer to your users to decrease latency, better experience.
- Disaster Recovery (DR):
  - If an AWS region goes down (earthquake, storms, power shutdown, politics)...
  - You can fail-over to another region and have your application still working.
  - A DR plan is important to increase the availability of your application.
- Attack protection: distributed global infrastructure is harder to attack

## 10.2. Global AWS Infrastructure

- **Regions:** For deploying applications and infrastructure
- **Availability Zones:** Made of multiple data centers
- **Edge Locations (Points of Presence):** for content delivery as close as possible to users

## 10.3. Global Applications in AWS

- **Global DNS: Route 53**
  - Great to route users to the closest deployment with least latency
  - Great for disaster recovery strategies
- **Global Content Delivery Network (CDN): CloudFront**
  - Replicate part of your application to AWS Edge Locations - decrease latency
  - Cache common requests - improved user experience and decreased latency
- **S3 Transfer Acceleration**
  - Accelerate global uploads & downloads into Amazon S3
- **AWS Global Accelerator:**
  - Improve global application availability and performance using the AWS global network

## 10.4. AWS CloudFront

[AWS CloudFront](AWS%20CloudFront.md)

## 10.5. AWS Global Accelerator

- Improve global application availability and performance using the AWS global network.
- Leverage the AWS internal network to optimize the route to your application (60% improvement).
- 2 Anycast IP are created for your application and traffic is sent through Edge Locations.
- The Edge locations send the traffic to your application.

## 10.6. AWS Global Accelerator vs CloudFront

- They both use the AWS global network and its edge locations around the world.
- Both services integrate with AWS Shield for DDoS protection.
- CloudFront - Content Delivery Network:
  - Improves performance for your cacheable content (such as images and videos).
  - Content is served at the edge.
- Global Accelerator:
  - No caching, proxying packets at the edge to applications running in one or more AWS Regions.
  - Improves performance for a wide range of applications over TCP or UDP.
  - Good for HTTP use cases that require static IP addresses.
  - Good for HTTP use cases that required deterministic, fast regional failover.

## 10.7. AWS Outposts

- Hybrid Cloud: businesses that keep an on-premises infrastructure alongside a cloud infrastructure.
- Therefore, two ways of dealing with IT systems:
  - One for the AWS cloud (using the AWS console, CLI, and AWS APIs).
  - One for their on-premises infrastructure.
- AWS Outposts are "server racks" that offers the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud.
- AWS will setup and manage "Outposts Racks" within your on-premises infrastructure and you can start leveraging AWS services on-premises.
- You are responsible for the Outposts Rack physical security.
- Benefits:
  - Low-latency access to on-premises systems
  - Local data processing
  - Data residency
  - Easier migration from on-premises to the cloud
  - Fully managed service
- Some services that work on Outposts:
  - Amazon EC2
  - Amazon EBS
  - Amazon S3
  - Amazon EKS
  - Amazon ECS
  - Amazon RDS
  - Amazon EMR

## 10.8. AWS WaveLength

- **AWS Wavelength is an AWS Infrastructure offering optimized for mobile edge computing applications. Wavelength combines the high bandwidth and ultra-low latency of 5G networks with AWS compute and storage services to enable developers to innovate and build a whole new class of applications.**
- WaveLength Zones are infrastructure deployments embedded within the telecommunications providers datacenters at the edge of the 5G networks.
- Brings AWS services to the edge of the 5G networks.
- Example: EC2, EBS, VPC...
- Ultra-low latency applications through 5G networks.
- Traffic doesn't leave the Communication Service Provider's (CSP) network.
- High-bandwidth and secure connection to the parent AWS Region.
- No additional charges or service agreements.
- Use cases: Smart Cities, ML-assisted diagnostics, Connected Vehicles, Interactive Live Video Streams, AR/VR, Real-time Gaming, ...

## 10.9. AWS Local Zones

- Places AWS compute, storage, database, and other selected AWS services closer to end users to run latency-sensitive applications
- Extend your VPC to more locations - "Extension of an AWS Region"
- Compatible with EC2, RDS, ECS, EBS, ElastiCache, Direct Connect ...
- Example:
  - AWS Region: N. Virginia (us-east-1)
  - AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami, ...

## 10.10. Global Applications Architecture

- Single Region, Single AZ.
- Single Region, Multi AZ.
- Multi Region, Active-Passive.
- Multi Region, Active-Active.

## 10.11. Global Applications in AWS - Summary

- Global DNS: Route 53
  - Great to route users to the closest deployment with least latency.
  - Great for disaster recovery strategies.
- Global Content Delivery Network (CDN): CloudFront
  - Replicate part of your application to AWS Edge Locations - decrease latency.
  - Cache common requests - improved user experience and decreased latency.
- S3 Transfer Acceleration
  - Accelerate global uploads & downloads into Amazon S3.
- AWS Global Accelerator
  - Improve global application availability and performance using the AWS global network.

## 10.12. Global Applications in AWS - Summary

- AWS Outposts
  - Deploy Outposts Racks in your own Data Centers to extend AWS services
- AWS WaveLength
  - Brings AWS services to the edge of the 5G networks
  - Ultra-low latency applications
- AWS Local Zones
  - Bring AWS resources (compute, database, storage, ...) closer to your users
  - Good for latency-sensitive applications

# 11. Cloud Integration

- When we start deploying multiple applications, they will inevitably need to communicate with one another.
- There are two patterns of application communication:
  1. Synchronous communications (application to application).
  2. Asynchronous / Event based (application to queue to application).
- Synchronous between applications can be problematic if there are sudden spikes of traffic.
- What if you need to suddenly encode 1000 videos but usually it's 10?
- In that case, it's better to decouple your applications:
  - using SQS: queue model.
  - using SNS: pub/sub model.
  - using Kinesis: real-time data streaming model.
- These services can scale independently from our application!

## 11.1. Amazon SQS - Standard Queue

- **Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. It uses a pull-based system.** [AWS SQS](AWS%20SQS.md)

## 11.2. Amazon Kinesis

- **Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. Kinesis offers four services: Data Firehose, Data Analytics, Data Streams, Video Streams.** [AWS Kinesis](AWS%20Kinesis.md)

## 11.3. Amazon SNS

- **Is a highly available, durable, secure, fully managed pub/sub messaging service that enables you to decouple microservices, distributed systems, and serverless applications. It uses a push-based system.** [Amazon SNS](AWS%20SNS.md)

## 11.4. Amazon MQ

- **Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers in the cloud.**
- SQS, SNS are "cloud-native" services, and they're using proprietary protocols from AWS.
- Traditional applications running from on-premise may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS.
- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ.
- Amazon MQ = managed Apache ActiveMQ.
- Amazon MQ doesn't "scale" as much as SQS / SNS.
- Amazon MQ runs on a dedicated machine (not serverless).
- Amazon MQ has both queue feature (SQS) and topic features (SNS).

## 11.5. Integration - Summary

- SQS:
  - Queue service in AWS.
  - Multiple Producers, messages are kept up to 14 days.
  - Multiple Consumers share the read and delete messages when done.
  - Used to decouple applications in AWS.
- SNS:
  - Notification service in AWS.
  - Subscribers: Email, Lambda, SQS, HTTP, Mobile...
  - Multiple Subscribers, send all messages to all of them.
  - No message retention.
- Kinesis: real-time data streaming, persistence and analysis.
- Amazon MQ: managed Apache MQ in the cloud (MQTT, AMQP protocols).
- **When using SQS or SNS, you apply the "decouple your applications" principle. This means that IT systems should be designed in a way that reduces interdependencies-a change or a failure in one component should not cascade to other components.**

# 12. Cloud Monitoring

## 12.1. Amazon CloudWatch Metrics

[Amazon CloudWatch](Management%20&%20Governance/Amazon%20CloudWatch.md)

### 12.1.1. Important Metrics

- EC2 instances: CPU Utilization, Status Checks, Network (not RAM).
  - Default metrics every 5 minutes.
  - Option for Detailed Monitoring ($$$): metrics every 1 minute.
- EBS volumes: Disk Read/Writes.
- S3 buckets: BucketSizeBytes, NumberOfObjects, AllRequests.
- Billing:Total Estimated Charge (only in us-east-1).
- Service Limits: how much you've been using a service API.
- Custom metrics: push your own metrics.

## 12.2. Amazon CloudWatch Alarms

- **The CloudWatch Alarms feature allows you to watch CloudWatch metrics and to receive notifications when the metrics fall outside of the levels (high or low thresholds) that you configure.**
- Alarms are used to trigger notifications for any metric
- Alarms actions...
  - Auto Scaling: increase or decrease EC2 instances "desired" count
  - EC2 Actions: stop, terminate, reboot or recover an EC2 instance
  - SNS notifications: send a notification into an SNS topic
- Various options (sampling, %, max, min, etc...)
- Can choose the period on which to evaluate an alarm
- Example: create a billing alarm on the CloudWatch Billing metric
- Alarm States: OK. INSUFFICIENT_DATA, ALARM

## 12.3. Amazon CloudWatch Logs

- CloudWatch Logs can collect log from:
  - Elastic Beanstalk: collection of logs from application.
  - ECS: collection from containers.
  - AWS Lambda: collection from function logs.
  - CloudTrail based on filter.
  - CloudWatch log agents: on EC2 machines or on-premises servers.
  - Route53: Log DNS queries.
- Enables real-time monitoring of logs.
- Adjustable CloudWatch Logs retention.

## 12.4. Amazon EventBridge

[Amazon EventBridge](Application%20Integration/Amazon%20EventBridge.md)

## 12.5. AWS CloudTrail

- **Is a web service that records activity made on your account and delivers log files to your Amazon S3 bucket. AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account.**
- **Can record the history of events/API calls made within you AWS account, which will help determine who or what deleted the resource. You should investigate it first.** [AWS CloudTrail](Management%20&%20Governance/AWS%20CloudTrail.md)

## 12.6. AWS X-Ray

- **AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture.** [AWS X-Ray](Developer%20Tools/AWS%20X-Ray.md)

## 12.7. Amazon CodeGuru

[Amazon CodeGuru](Developer%20Tools/AWS%20CICD.md)

## 12.8. AWS Health Dashboard

[AWS Health Dashboard](Management%20&%20Governance/AWS%20Health%20Dashboard.md)

## 12.9. Monitoring Summary

- CloudWatch:
  - Metrics: monitor the performance of AWS services and billing metrics.
  - Alarms: automate notification, perform EC2 action, notify to SNS based on metric.
  - Logs: collect log files from EC2 instances, servers, Lambda functions...
  - Events (or EventBridge): react to events in AWS, or trigger a rule on a schedule.
- CloudTrail: audit API calls made within your AWS account.
- CloudTrail Insights: automated analysis of your CloudTrail Events.
- X-Ray: trace requests made through your distributed applications.
- Service Health Dashboard: status of all AWS services across all regions.
- Personal Health Dashboard: AWS events that impact your infrastructure.
- Amazon CodeGuru: automated code reviews and application performance recommendation.

# 13. VPC

![Amazon VPC](Networking%20&%20Content%20Delivery/Amazon%20VPC.md)

# 14. Security & Compliance

## 14.1. AWS Shared Responsibility Model

- AWS responsibility - Security of the Cloud
  - Protecting infrastructure (hardware, software, facilities, and networking) that runs all the AWS services.
  - Managed services like S3, DynamoDB, RDS, etc.
- Customer responsibility - Security in the Cloud
  - For EC2 instance, customer is responsible for management of the guest OS (including security patches and updates), firewall & network configuration, IAM.
  - Encrypting application data.
- Shared controls:
  - **AWS is responsible for patching and fixing flaws within the infrastructure, but customers are responsible for patching their guest OS and applications. Shared Controls also includes Configuration Management, and Awareness and Training.**
  - Patch Management, Configuration Management, Awareness & Training.

### 14.1.1. Example, for RDS

- AWS responsibility:
  - Manage the underlying EC2 instance, disable SSH access.
  - Automated DB patching.
  - Automated OS patching.
  - Audit the underlying instance and disks & guarantee it functions.
- Your responsibility:
  - Check the ports / IP / security group inbound rules in DB's SG.
  - In-database user creation and permissions.
  - Creating a database with or without public access.
  - Ensure parameter groups or DB is configured to only allow SSL connections.
  - Database encryption setting.

### 14.1.2. Example, for S3

- AWS responsibility:
  - Guarantee you get unlimited storage.
  - Guarantee you get encryption.
  - Ensure separation of the data between different customers.
  - Ensure AWS employees can't access your data.
- Your responsibility:
  - Bucket configuration.
  - Bucket policy / public setting.
  - IAM user and roles.
  - Enabling encryption.

## 14.2. DDOS Protection on AWS

- DDOS = Distributed Denial-of-Service
- **AWS Shield Standard:** protects against DDOS attack for your website and applications, for all customers at no additional costs
- **AWS Shield Advanced:** 24/7 premium DDoS protection
- **AWS WAF:** Filter specific requests based on rules
- **CloudFront and Route 53:**
  - Availability protection using global edge network
  - Combined with AWS Shield, provides attack mitigation at the edge
- Be ready to scale - leverage **AWS Auto Scaling**

## 14.3. AWS Shield

- **AWS Shield Standard:**
  - Free service that is activated for every AWS customer.
  - Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks.
- **AWS Shield Advanced:**
  - Optional DDoS mitigation service ($3,000 per month per organization).
  - Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53.
  - 24/7 access to AWS DDoS response team (DRP).
  - Protect against higher fees during usage spikes due to DDoS.

## 14.4. AWS WAF - Web Application Firewall

- **AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources.**
- Protects your web applications from common web exploits (Layer 7).
- **Layer 7 is HTTP** (vs Layer 4 is TCP).
- Deploy on **Application Load Balancer, API Gateway, CloudFront**.
- Define Web ACL (Web Access Control List):
  - Rules can include IP addresses, HTTP headers, HTTP body, or URI strings.
  - Protects from common attack - **SQL injection** and **Cross-Site Scripting (XSS)**.
  - Size constraints, **geo-match (block countries)**.
  - Rate-based rules (to count occurrences of events) - for DDoS protection.

[AWS WAF](AWS%20WAF.md)

## 14.5. Penetration Testing on AWS Cloud

- AWS customers are welcome to carry out security assessments or penetration tests against their AWS infrastructure **without prior approval for 8 services**:
  - Amazon EC2 instances, NAT Gateways, and Elastic Load Balancers.
  - Amazon RDS.
  - Amazon CloudFront.
  - Amazon Aurora.
  - Amazon API Gateways.
  - AWS Lambda and Lambda Edge functions.
  - Amazon Lightsail resources.
  - Amazon Elastic Beanstalk environments.
- List can increase over time...
- **Prohibited Activities**
  - DNS zone walking via Amazon Route 53 Hosted Zones.
  - Denial of Service (DoS), Distributed Denial of Service (DDoS), Simulated DoS, Simulated DDoS.
  - Port flooding.
  - Protocol flooding.
  - Request flooding (login request flooding, API request flooding).
- For any other simulated events, contact aws-security-simulated-event@amazon.com.
- Read more: https://aws.amazon.com/security/penetration-testing/.

## 14.6. Data at rest vs. Data in transit

- **At rest:** data stored or archived on a device.
  - On a hard disk, on a RDS instance, in S3 Glacier Deep Archive, etc.
- **In transit (in motion):** data being moved from one location to another.
  - Transfer from on-premises to AWS, EC2 to DynamoDB, etc.
  - Means data transferred on the network.
- We want to encrypt data in both states to protect it!
- For this we leverage **encryption keys**.

## 14.7. AWS KMS (Key Management Service)

- [AWS KMS](AWS%20KMS.md)

## 14.8. AWS Certificate Manager (ACM)

- **AWS Certificate Manager is a service that lets you easily provision, manage, and deploy public and private Secure Sockets Layer/Transport Layer Security (SSL/TLS) certificates for use with AWS services and your internal connected resources.**
  [AWS ACM](AWS%20ACM.md)

## 14.9. AWS Secrets Manager

- Newer service, meant for storing secrets.
- Capability to force **rotation of secrets** every X days.
- Automate generation of secrets on rotation (uses Lambda).
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora).
- Secrets are encrypted using KMS.
- Mostly meant for RDS integration.

## 14.10. AWS Artifact (not really a service)

- **AWS Artifact is your go-to, central resource for compliance-related information that matters to you.**
- Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements.
- **Artifact Reports** - Allows you to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports.
- **Artifact Agreements** - Allows you to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in your organization.
- Can be used to support internal audit or compliance.

## 14.11. Amazon GuardDuty

- **Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads.**
- Intelligent Threat discovery to Protect AWS Account.
- Uses Machine Learning algorithms, anomaly detection, 3rd party data.
- One click to enable (30 days trial), no need to install software.
- Input data includes:
  - CloudTrail Events Logs - unusual API calls, unauthorized deployments.
    - CloudTrail Management Events - create VPC subnet, create trail, ...
    - CloudTrail S3 Data Events - get object, list objects, delete object, ...
  - VPC Flow Logs - unusual internal traffic, unusual IP address.
  - DNS Logs - compromised EC2 instances sending encoded data within DNS queries.
  - Kubernetes Audit Logs - suspicious activities and potential EKS cluster compromises.
- Can setup CloudWatch Event rules to be notified in case of findings.
- CloudWatch Events rules can target AWS Lambda or SNS.
- Can protect against CryptoCurrency attacks (has a dedicated "finding" for it).

## 14.12. Amazon Inspector

- **Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS.**
- **It helps you test the network accessibility of your Amazon EC2 instances and the security state of your applications running on the instances.** [Amazon Inspector](Security,%20Identity,%20&%20Compliance/Amazon%20Inspector.md)

## 14.13. AWS Config

- **AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time.** [AWS Config](Management%20&%20Governance/AWS%20Config.md)

## 14.14. Amazon Macie

- Amazon Macie is a fully managed data security and data privacy service that uses **machine learning and pattern matching to discover and protect your sensitive data in AWS**.
- Macie helps identify and alert you to **sensitive data, such as personally identifiable information (PII)**.

## 14.15. AWS Security Hub

- **AWS Security Hub provides you with a comprehensive view of your security state within AWS and your compliance with security standards and best practices.**
- Central security tool to manage security across several AWS accounts and automate security checks.
- Integrated dashboards showing current security and compliance status to quickly take actions.
- Automatically aggregates alerts in predefined or personal findings formats from various AWS services & AWS partner tools:
  - GuardDuty.
  - Inspector.
  - Macie.
  - IAM Access Analyzer.
  - AWS Systems Manager.
  - AWS Firewall Manager.
  - AWS Partner Network Solutions.
- Must first enable the AWS Config Service.

## 14.16. Amazon Detective

- GuardDuty, Macie, and Security Hub are used to identify potential
  security issues, or findings.
- Sometimes security findings require deeper analysis to isolate the root cause and take action - it's a complex process.
- Amazon Detective **analyzes, investigates, and quickly identifies the root cause of security issues or suspicious activities using ML and graphs**.
- **Automatically collects and processes events** from VPC Flow Logs, CloudTrail, and GuardDuty to create a unified view.
- Produces visualizations with details and context to get to the root cause.

## 14.17. AWS Abuse

- **Report suspected AWS resources used for abusive or illegal purposes**.
- Abusive & prohibited behaviors are:
  - **Spam** - receving undesired emails from AWS-owned IP address, websites & forums spammed by AWS resources.
  - **Port scanning**- sending packets to your ports to discover the unsecured ones.
  - **DoS or DDoS attacks** - AWS-owned IP addresses attempting to overwhlem or crash your servers/softwares.
  - **Intrusion attempts** - logging in on your resources.
  - **Hosting objectionable** or copyrighted content - distributing illegal or copyrighted content without consent.
  - **Distributing malware** - AWS resources distributing softwares to harm computers or machines.
- Contact the AWS Abuse team: AWS abuse form, or abuse@amazonaws.com.

## 14.18. Root user privileges

- Root user = Account Owner (created when the account is created).
- Has complete access to all AWS services and resources.
- **Lock away your AWS account root user access keys!**.
- Do not use the root account for everyday tasks, even administrative tasks.
- Actions that can be performed only by the root user:
  - Change account settings (account name, email address, root user password, root user access keys).
  - View certain tax invoices.
  - Close your AWS account.
  - Restore IAM user permissions.
  - Change or cancel your AWS Support plan.
  - Register as a seller in the Reserved Instance Marketplace.
  - Configure an Amazon S3 bucket to enable MFA.
  - Edit or delete an Amazon S3 bucket policy that includes an invalid VPC ID or VPC endpoint ID.
  - Sign up for GovCloud.

## 14.19. Summary: Security & Compliance

- Shared Responsibility on AWS
- Shield: Automatic DDoS Protection + 24/7 support for advanced
- WAF: Firewall to filter incoming requests based on rules
- KMS: Encryption keys managed by AWS
- CloudHSM: Hardware encryption, we manage encryption keys
- AWS Certificate Manager: provision, manage, and deploy SSL/TLS Certificates
- Artifact: Get access to compliance reports such as PCI, ISO, etc...
- GuardDuty: Find malicious behavior with VPC, DNS & CloudTrail Logs
- Inspector: For EC2 only, install agent and find vulnerabilities
- Config: Track config changes and compliance against rules
- Macie: Find sensitive data (ex: PII data) in Amazon S3 buckets
- CloudTrail: Track API calls made by users within account
- AWS Security Hub: gather security findings from multiple AWS accounts
- Amazon Detective: find the root cause of security issues or suspicious activities
- AWS Abuse: Report AWS resources used for abusive or illegal purposes
- Root user privileges:
  - Change account settings
  - Close your AWS account
  - Change or cancel your AWS Support plan
  - Register as a seller in the Reserved Instance Marketplace

# 15. Machine Learning

[Machine Learning](Machine%20Learning/README.md)

# 16. Account Management, Billing & Support

## 16.1. AWS Organizations

[AWS Organizations](AWS%20Organizations.md)

## 16.2. AWS Control Tower

- **AWS Control Tower offers the easiest way to set up and govern a new, secure, multi-account AWS environment. It establishes a landing zone that is based on best-practices blueprints, and enables governance using guardrails you can choose from a pre-packaged list.** [AWS Control Tower](AWS%20Control%20Tower.md)

## 16.3. Pricing Models in AWS

- AWS has 4 pricing models:
  - **Pay as you go:** pay for what you use, remain agile, responsive, meet scale demands.
  - **Save when you reserve:** minimize risks, predictably manage budgets, comply with long-terms requirements.
    - Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElastiCache Reserved Nodes, RDS Reserved Instance, Redshift Reserved Nodes.
  - **Pay less by using more:** volume-based discounts
  - **Pay less as AWS grows**

### 16.3.1. Free services and free tier in AWS

- IAM.
- VPC.
- Consolidated Billing.
- **You do pay for the resources created:**
  - Elastic Beanstalk.
  - CloudFormation.
  - Auto Scaling Groups.
- Free Tier: https://aws.amazon.com/free/
  - EC2 t2.micro instance for a year.
  - S3, EBS, ELB, AWS Data transfer.

### 16.3.2. Compute Pricing - EC2

- Only charged for what you use.
- Number of instances.
- Instance configuration:
  - Physical capacity.
  - Region.
  - OS and software.
  - Instance type.
  - Instance size.
- ELB running time and amount of data processed.
- Detailed monitoring.
- On-demand instances:
  - Minimum of 60s.
    - **With Linux EC2 instances, you pay per second of compute capacity. There is also a minimum of 60s of use.**
  - Pay per second (Linux/Windows) or per hour (other).
- Reserved instances:
  - Up to 75% discount compared to On-demand on hourly rate.
  - 1 or 3 years commitment.
  - All upfront, partial upfront, no upfront.
- Spot instances:
  - Up to 90% discount compared to On-demand on hourly rate.
  - Bid for unused capacity.
- Dedicated Host:
  - On-demand.
  - Reservation for 1 year or 3 years commitment.
- **Savings plans as an alternative to save on sustained usage**.

### 16.3.3. Compute Pricing - Lambda & ECS

- Lambda:
  - Pay per call.
  - Pay per duration.
- ECS:
  - EC2 Launch Type Model: No additional fees, you pay for AWS resources stored and created in your application.
- Fargate:
  - Fargate Launch Type Model: Pay for vCPU and memory resources allocated to your applications in your containers.

### 16.3.4. Storage Pricing - S3

- Storage class: S3 Standard, S3 Infrequent Access, S3 One-Zone IA, S3 Intelligent Tiering, S3 Glacier and S3 Glacier Deep Archive.
- Number and size of objects: Price can be tiered (based on volume).
- Number and type of requests.
- Data transfer OUT of the S3 region.
- S3 Transfer Acceleration.
- Lifecycle transitions.
- Similar service: EFS (pay per use, has infrequent access & lifecycle rules).

### 16.3.5. Storage Pricing - EBS

- Volume type (based on performance).
- Storage volume in GB per month **provisionned**.
- IOPS:
  - General Purpose SSD: Included.
  - Provisioned IOPS SSD: Provisionned amount in IOPS.
  - Magnetic: Number of requests.
- Snapshots:
  - Added data cost per GB per month.
- Data transfer:
  - Outbound data transfer are tiered for volume discounts.
  - Inbound is free.

### 16.3.6. Database Pricing - RDS

- Per hour billing.
- Database characteristics:
  - Engine.
  - Size.
  - Memory class.
- Purchase type:
  - On-demand.
  - Reserved instances (1 or 3 years) with required up-front.
- Backup Storage: There is no additional charge for backup storage up to 100% of your total database storage for a region.
- Additional storage (per GB per month)
- Number of input and output requests per month
- Deployment type (storage and I/O are variable):
  - Single AZ
  - Multiple AZs
- Data transfer:
  - Outbound data transfer are tiered for volume discounts
  - Inbound is free

### 16.3.7. Content Delivery - CloudFront

- Pricing is different across different geographic regions.
- Aggregated for each edge location, then applied to your bill.
- Data Transfer Out (volume discount).
- Number of HTTP/HTTPS requests.

#### 16.3.7.1. Networking Costs in AWS per GB - Simplified

- Use Private IP instead of Public IP for good savings and better network performance.
- Use same AZ for maximum savings (at the cost of high availability).

### 16.3.8. Savings Plan

- Commit a certain $ amount per hour for 1 or 3 years
- Easiest way to setup long-term commitments on AWS
- EC2 Savings Plan
  - Up to 72% discount compared to On-Demand
  - Commit to usage of individual instance families in a region (e.g. C5 or M5)
  - Regardless of AZ, size (m5.xl to m5.4xl), OS (Linux/Windows) or tenancy
  - All upfront, partial upfront, no upfront
- Compute Savings Plan
  - Up to 66% discount compared to On-Demand
  - Regardless of Family, Region, size, OS, tenancy, compute options
  - Compute Options: EC2, Fargate, Lambda
  - **Compute Savings Plans provide the most flexibility and help to reduce your costs by up to 66% in exchange for a commitment to a consistent amount of usage for a 1 or 3 year term. These plans automatically apply to EC2 instance usage regardless of instance family, size, AZ, region, OS or tenancy, and also apply to Fargate or Lambda usage.**
- Setup from the AWS Cost Explorer console
- Estimate pricing at https://aws.amazon.com/savingsplans/pricing/

## 16.4. AWS Compute Optimizer

- Reduce costs and improve performance by recommending optimal AWS resources for your workloads.
- Helps you choose optimal configurations and right - size your workloads (over/under provisioned).
- Uses Machine Learning to analyze your resources, configurations and their utilization CloudWatch metrics.
- Supported resources:
  - EC2 instances.
  - EC2 Auto Scaling Groups.
  - EBS volumes.
  - Lambda functions.
- Lower your costs by up to 25%.
- Recommendations can be exported to S3.

## 16.5. Billing and Costing Tools

- **Estimating costs in the cloud:**
  - Pricing Calculator.
- **Tracking costs in the cloud:**
  - Billing Dashboard.
  - Cost Allocation Tags.
  - Cost and Usage Reports.
  - Cost Explorer.
- **Monitoring against costs plans:**
  - Billing Alarms.
  - Budgets.

## 16.6. AWS Pricing Calculator

- **The AWS Simple Monthly Calculator is an easy-to-use online tool that enables you to estimate their architecture solution monthly cost of AWS services for your use case based on your expected usage. It is being replaced by AWS Pricing Calculator.**
- **The TCO calculators allow you to estimate the cost savings when using AWS and provide a detailed set of reports that can be used in executive presentations.**
- Available at https://calculator.aws/.
- Estimate the cost for your solution architecture.

## 16.7. AWS Billing Dashboard

### 16.7.1. Cost Allocation Tags

- Use **cost allocation tags** to track your AWS costs on a detailed level.
- **AWS generated tags:**
  - Automatically applied to the resource you create.
  - Starts with Prefix aws: (e.g. aws: createdBy).
- **User-defined tags:**
  - Defined by the user.

### 16.7.2. AWS Resource Groups and Tagging

- [AWS Resource Groups and Tagging](AWS%20Resource%20Groups.md)

### 16.7.3. Cost and Usage Reports

- Dive deeper into your AWS costs and usage.
- The AWS Cost & Usage Report contains the most comprehensive set of AWS cost and usage data available, including additional metadata about AWS services, pricing, and reservations (e.g., Amazon EC2 Reserved Instances (RIs)).
- The AWS Cost & Usage Report lists AWS usage for each service category used by an account and its IAM users in hourly or daily line items, as well as any tags that you have activated for cost allocation purposes.
- Can be integrated with Athena, Redshift or QuickSight.

### 16.7.4. Cost Explorer

- **Cost Explorer can be used to forecast usage up to 12 months based on the previous usage. It can also be used to choose an optimal Savings Plan. Cost Explorer has an easy-to-use interface that lets you visualize, understand, and manage your AWS costs and usage over time.**
- Visualize, understand, and manage your AWS costs and usage over time.
- Create custom reports that analyze cost and usage data.
- Analyze your data at a high level: total costs and usage across all accounts.
- Or Monthly, hourly, resource level granularity.
- Choose an optimal Savings Plan (to lower prices on your bill).
- Forecast usage up to 12 months based on previous usage.

## 16.8. Billing Alarms in CloudWatch

- Billing data metric is stored in CloudWatch us-east-1.
- Billing data are for overall worldwide AWS costs.
- It's for actual cost, not for projected costs.
- Intended a simple alarm (not as powerful as AWS Budgets).

## 16.9. AWS Budgets

- **AWS Budgets gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount.**
- **Difference with CloudWatch Billing Alarms: CloudWatch Billing Alarms only send alerts when your costs and usage are exceeding your budget, not when it is forecasted to exceed your budget, while AWS Budgets does both.**
- Create budget and **send alarms when costs exceeds the budget**.
- 3 types of budgets: Usage, Cost, Reservation.
- For Reserved Instances (RI):
  - Track utilization.
  - Supports EC2, ElastiCache, RDS, Redshift.
- Up to 5 SNS notifications per budget.
- Can filter by: Service, Linked Account, Tag, Purchase Option, Instance Type, Region, Availability Zone, API Operation, etc...
- Same options as AWS Cost Explorer!
- 2 budgets are free, then $0.02/day/budget.

## 16.10. AWS Trusted Advisor

- **AWS Trusted Advisor is an online tool that provides you real time guidance to help you provision your resources following AWS best practices, including performance, security, and fault tolerance, but also cost optimization and service limits.**
  [AWS Trusted Advisor](AWS%20Trusted%20Advisor.md)

## 16.11. AWS Support Plans Pricing

### 16.11.1. AWS Basic Support Plan

- **Customer Service & Communities -** 24x7 access to customer service, documentation, whitepapers, and support forums.
- **AWS Trusted Advisor -** Access to the 7 core Trusted Advisor checks and guidance to provision your resources following best practices to increase performance and improve security.
- **AWS Personal Health Dashboard -** A personalized view of the health of AWS services, and alerts when your resources are impacted.

### 16.11.2. AWS Developer Support Plan

- All Basic Support Plan +
- **Business hours email access** to Cloud Support Associates.
- Unlimited cases / 1 primary contact.
- Case severity / response times:
  - General guidance: < 24 business hours.
  - System impaired: < 12 business hours.

### 16.11.3. AWS Business Support Plan (24/7)

- Intended to be used if you have **production workloads**.
- **Trusted Advisor** - Full set of checks + API access.
- **24x7 phone, email, and chat access** to Cloud Support Engineers.
- Unlimited cases / unlimited contacts.
- Access to Infrastructure Event Management **for additional fee**.
- Case severity / response times:
  - General guidance: < 24 business hours.
  - System impaired: < 12 business hours.
  - Production system impaired: < 4 hours.
  - Production system down: < 1 hour.

### 16.11.4. AWS Enterprise On-Ramp Support Plan (24/7)

- Intended to be used if you have production or business critical workloads.
- All of Business Support Plan +
- Access to a pool of **Technical Account Managers (TAM)**.
- **Concierge Support Team** (for billing and account best practices).
- **Infrastructure Event Management, Well-Architected & Operations Reviews**.
- Case severity / response times:
  - Production system impaired: < 4 hours.
  - Production system down: < 1 hour.
  - **Business-critical system down: < 30 minutes**.

### 16.11.5. AWS Enterprise Support Plan (24/7)

- Intended to be used if you have **mission critical workloads**.
- All of Business Support Plan +
- Access to a **designated** Technical Account Manager (TAM).
- **Concierge Support Team** (for billing and account best practices).
- **Infrastructure Event Management, Well-Architected & Operations Reviews**.
- Case severity / response times:
  - Production system impaired: < 4 hours.
  - Production system down: < 1 hour.
  - **Business-critical system down: < 15 minutes**.

## 16.12. Account Best Practices - Summary

- Operate multiple accounts using **Organizations**.
- Use **SCP** (service control policies) to restrict account power.
- Easily setup multiple accounts with best-practices with **AWS Control Tower**.
- **Use Tags & Cost Allocation Tags** for easy management & billing.
- **IAM guidelines**: MFA, least-privilege, password policy, password rotation.
- **Config** to record all resources configurations & compliance over time.
- **CloudFormation** to deploy stacks across accounts and regions.
- **Trusted Advisor** to get insights, Support Plan adapted to your needs.
- Send Service Logs and Access Logs to **S3 or CloudWatch Logs**.
- **CloudTrail** to record API calls made within your account.
- **If your Account is compromised:** change the root password, delete and rotate all passwords / keys, contact the AWS support.

## 16.13. Billing and Costing Tools - Summary

- **Compute Optimizer:** recommends resources, configurations to reduce cost.
- **Pricing Calculator:** cost of services on AWS.
- **Billing Dashboard:** high level overview + free tier dashboard.
- **Cost Allocation Tags:** tag resources to create detailed reports.
- **Cost and Usage Reports:** most comprehensive billing dataset.
- **Cost Explorer:** View current usage (detailed) and forecast usage.
- **Billing Alarms:** in us-east-1 - track overall and per-service billing.
- **Budgets:** more advanced - track usage, costs, RI, and get alerts.
- **Savings Plans:** easy way to save based on long-term usage of AWS.

# 17. AWS STS - Security Token Service

[AWS STS](AWS%20STS.md)

# 18. AWS Directory Services

[AWS Directory Services](AWS%20Directory%20Services.md)

# 19. Advanced Identity

## 19.1. Amazon Cognito

- **Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily.** [AWS Cognito](AWS%20Cognito.md)

## 19.2. AWS Organizations

- Organizations helps you to centrally manage billing; control access, compliance, and security; and share resources across your AWS accounts.

## 19.3. AWS IAM Identity Center (Successor to AWS Single Sign-On)

[AWS IAM Identity Center](AWS%20IAM%20Identity%20Center.md)

## 19.4. Advanced Identity - Summary

- IAM:
  - Identity and Access Management inside your AWS account.
  - For users that you trust and belong to your company.
  - **IAM Roles are sets of permissions making AWS service requests, which will be used by AWS services, but they do not provide temporary security credentials.**
- Organizations: manage multiple AWS accounts.
- Security Token Service (STS): temporary, limited-privileges credentials to access AWS resources.
- Cognito: create a database of users for your mobile & web applications.
- Directory Services: integrate Microsoft Active Directory in AWS.
- Single Sign-On (SSO): one login for multiple AWS accounts & applications.

# 20. Other AWS Services

## 20.1. Amazon WorkSpaces

- **Amazon WorkSpaces is a fully managed, secure cloud desktop service. You can use Amazon WorkSpaces to provision either Windows or Linux desktops in just a few minutes and quickly scale to provide thousands of desktops to workers across the globe.**
- Managed Desktop as a Service (DaaS) solution to easily provision Windows or Linux desktops.
- Great to eliminate management of on-premise VDI (Virtual Desktop Infrastructure).
- Fast and quickly scalable to thousands of users.
- Secured data - integrates with KMS.
- Pay-as-you-go service with monthly or hourly rates.

## 20.2. Amazon AppStream 2.0

- **Amazon AppStream 2.0 is a fully managed non-persistent application and desktop streaming service that provides users instant access to their desktop applications from anywhere.**
- Desktop Application Streaming Service.
- Deliver to any computer, without acquiring, provisioning infrastructure.
- The application is delivered from within a web browser.

### 20.2.1. Amazon AppStream 2.0 vs WorkSpaces

- **Workspaces:**
  - Fully managed VDI and desktop available.
  - The users connect to the VDI and open native or WAM applications.
  - Workspaces are on-demand or always on.
- **AppStream 2.0:**
  - Stream a desktop application to web browsers (no need to connect to a VDI).
  - Works with any device (that has a web browser).
  - Allow to configure an instance type per application type (CPU, RAM, GPU).

## 20.3. Amazon Sumerian

- **Amazon Sumerian is a managed service that lets you create and run 3D, Augmented Reality (AR) and Virtual Reality (VR) applications. You can build immersive and interactive scenes that run on AR and VR, mobile devices, and your web browser.**
- Create and run virtual reality (VR), augmented reality (AR), and 3D applications.
- Can be used to quickly create 3D models with animations.
- Ready-to-use templates and assets - no programming or 3D expertise required.
- Accessible via a web-browser URLs or on popular hardware for AR/VR.

## 20.4. AWS IoT Core

- **AWS IoT Core, is serverless and lets you connect billions of devices to the AWS Cloud, lets you securely connect IoT devices to the AWS Cloud and other devices without the need to provision or manage servers.**
- IoT stands for "Internet of Things" - the network of internet-connected devices that are able to collect and transfer data.
- AWS IoT Core allows you to easily connect IoT devices to the AWS Cloud.
- Serverless, secure & scalable to billions of devices and trillions of messages.
- Your applications can communicate with your devices even when they aren't connected.
- Integrates with a lot of AWS services (Lambda, S3, SageMaker, etc.).
- Build IoT applications that gather, process, analyze, and act on data.

## 20.5. Amazon Elastic Transcoder

- **Amazon Elastic Transcoder is media transcoding in the cloud. It is used to convert media files from their source format into versions that will play back on devices like smartphones, tablets, and PCs.**
- Elastic Transcoder is used to convert media files stored in S3 into media files in the formats required by consumer playback devices (phones etc..)
- Benefits:
  - Easy to use.
  - Highly scalable - can handle large volumes of media files and large file sizes.
  - Cost effective - duration-based pricing model.
  - Fully managed & secure, pay for what you use.

## 20.6. AWS Device Farm

- **AWS Device Farm is an application testing service that lets you improve the quality of your web and mobile apps by testing them across an extensive range of desktop browsers and real mobile devices; without having to provision and manage any testing infrastructure.**
- Fully-managed service that tests your web and mobile apps against desktop browsers, real mobile devices, and tablets.
- Run tests concurrently on multiple devices (speed up execution).
- Ability to configure device settings (GPS, language, Wi-Fi, Bluetooth, ...).

## 20.7. AWS Backup

- **AWS Backup is a centralized backup service that makes it easy and cost-effective for you to backup your application data across AWS services in the AWS Cloud**
- **CloudEndure Disaster Recovery minimizes downtime and data loss by providing fast, reliable recovery into AWS of your physical, virtual, and cloud-based servers.**
- Fully-managed service to centrally manage and automate backups across AWS services.
- On-demand and scheduled backups.
- Supports PITR (Point-in-time Recovery).
- Retention Periods, Lifecycle Management, Backup Policies, ...
- Cross-Region Backup.
- Cross-Account Backup (using AWS Organizations).

### 20.7.1. Disaster Recovery Strategies

- Backup and Restore (Cheapest)
- Pilot Light
- Multi-Site / Hot-Site
- Warm Standby

## 20.8. AWS Elastic Disaster Recovery (DRS)

- Used to be named "CloudEndure Disaster Recovery".
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS.
- Example: protect your most critical databases (including Oracle, MySQL, and SQL Server), enterprise apps (SAP), protect your data from ransomware attacks, ...
- Continuous block-level replication for your servers.

## 20.9. AWS DataSync

- Move large amount of data from on-premises to AWS.
  - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API) - needs agent.
  - AWS to AWS (different storage service) - no agent needed.
- Can synchronize to:
  - Amazon S3 (any storage classes - including Glacier).
  - Amazon EFS.
  - Amazon FSx for Windows.
- Replication tasks can be scheduled hourly, daily, weekly.
  - The replication tasks are **incremental** after the first full load.
- File permissions and metada are preserved (NFS POSIX, SMB).
- One agent task can use 10 Gbps, can setup a bandwidth limit.

![AWS DataSync](/Images/AwsDataSyncDiagram.png)

## 20.10. AWS Fault Injection Simulator (FIS)

- A fully managed service for running fault injection experiments on AWS workloads.
- Based on **Chaos Engineering** - stressing an application by creating disruptive events (e.g., sudden increase in CPU or memory), observing how the system responds, and implementing improvements.
- Helps you uncover hidden bugs and performance bottlenecks.
- Supports the following AWS services: EC2, ECS, EKS, RDS...
- Use pre-built templates that generate the desired disruptions.

## 20.11. AWS Step Functions

- **AWS Step Functions is a low-code visual workflow service used to orchestrate AWS services, automate business processes, and build Serverless applications. It manages failures, retries, parallelization, service integrations, ...** [AWS Step Functions](AWS%20Step%20Functions.md)

## 20.12. Amazon AppFlow

- Amazon AppFlow is a fully managed integration service that enables you to securely transfer data between Software-as-a-Service (SaaS) applications like Salesforce, SAP, Zendesk, Slack, and ServiceNow, and AWS services like Amazon S3 and Amazon Redshift, in just a few clicks.
- With AppFlow, you can run data flows at enterprise scale at the frequency you choose - on a schedule, in response to a business event, or on demand.
- You can configure data transformation capabilities like filtering and validation to generate rich, ready-to-use data as part of the flow itself, without additional steps.
- AppFlow automatically encrypts data in motion, and allows users to restrict data from flowing over the public Internet for SaaS applications that are integrated with AWS PrivateLink, reducing exposure to security threats.
- Benefits:
  - Integrate with a few clicks
    - Anyone can use AppFlow to integrate applications in a few minutes - no more waiting days or weeks to code custom connectors.
    - Features like data pagination, error logging, and network connection retries are included by default so there's no coding or management.
    - With Appflow, data flow quality is built in, and you can enrich the flow of data through mapping, merging, masking, filtering, and validation as part of the flow itself.
  - Transfer data at massive scale
    - AppFlow easily scales up without the need to plan or provision resources, so you can move large volumes of data without breaking it down into multiple batches.
    - AppFlow can run up to 100 GB per flow, which enables you to easily transfer millions of Salesforce records or Zendesk events or Marketo responses or other data - all while running a single flow.
  - Automate data security
    - All data flowing through AppFlow is encrypted at rest and in transit, and you can encrypt data with AWS keys, or bring your own custom keys.
    - With AppFlow, you can use your existing Identity and Access Management (IAM) policies to enforce fine-grained permissions, rather than creating new policies.
    - For SaaS integrations with AWS PrivateLink enabled, data is secured from the public internet by default.

## 20.13. AWS CloudSearch

- Amazon CloudSearch is a managed service in the AWS Cloud that makes it simple and cost-effective to set up, manage, and scale a search solution for your website or application.
- Amazon CloudSearch supports 34 languages and popular search features such as highlighting, autocomplete, and geospatial search.

## 20.14. AWS Service Catalog

[AWS Service Catalog](AWS%20Service%20Catalog.md)

## 20.15. Amazon SES - Simple Email Service

- **Fully managed service to send emails securely, globally and at scale.** [AWS SES](Amazon%20SES.md)

# 21. AWS Architecting & Ecosystem

## 21.1. Well Architected Framework General - Guiding Principles

- Stop guessing your capacity needs.
- Test systems at production scale.
- Automate to make architectural experimentation easier.
- Allow for evolutionary architectures.
  - Design based on changing requirements.
- Drive architectures using data.
- Improve through game days.
  - Simulate applications for flash sale days.

## 21.2. AWS Cloud Best Practices - Design Principles

- **Scalability:** vertical & horizontal.
- **Disposable Resources:** servers should be disposable & easily configured.
- **Automation:** Serverless, Infrastructure as a Service, Auto Scaling...
- **Loose Coupling:**
  - Monolith are applications that do more and more over time, become bigger.
  - Break it down into smaller, loosely coupled components.
  - A change or a failure in one component should not cascade to other components.
- **Services, not Servers:**
  - Don't use just EC2.
  - Use managed services, databases, serverless, etc!

## 21.3. Well Architected Framework 6 Pillars

1. Operational Excellence.
2. Security.
3. Reliability.
4. Performance Efficiency.
5. Cost Optimization.
6. Sustainability.

- They are not something to balance, or trade-offs, they're a synergy.

### 21.3.1. Operational Excellence

- Includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures.
- Design Principles:
  - **Perform operations as code** - Infrastructure as code.
  - **Annotate documentation** - Automate the creation of annotated documentation after every build.
  - **Make frequent, small, reversible changes** - So that in case of any failure, you can reverse it.
  - **Refine operations procedures frequently** - And ensure that team members are familiar with it.
  - **Anticipate failure**.
  - **Learn from all operational failures**.

#### 21.3.1.1. Operational Excellence AWS Services

- Prepare:
  - AWS CloudFormation
  - AWS Config
- Operate:
  - AWS CloudFormation
  - AWS Config
  - AWS CloudTrail
  - Amazon CloudWatch
  - AWS X-Ray
- Evolve:
  - AWS CloudFormation
  - AWS CodeBuild
  - AWS CodeCommit
  - AWS CodeDeploy
  - AWS CodePipeline

### 21.3.2. Security

- Includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies.
- Design Principles:
  - **Implement a strong identity foundation** - Centralize privilege management and reduce (or even eliminate) reliance on long-term credentials - Principle of least privilege - IAM.
  - **Enable traceability** - Integrate logs and metrics with systems to automatically respond and take action.
  - **Apply security at all layers** - Like edge network, VPC, subnet, load balancer, every instance, operating system, and application.
  - **Automate security best practices**.
  - **Protect data in transit and at rest** - Encryption, tokenization, and access control.
  - **Keep people away from data** - Reduce or eliminate the need for direct access or manual processing of data.
  - **Prepare for security events** - Run incident response simulations and use tools with automation to increase your speed for detection, investigation, and recovery.
  - **Shared Responsibility Model**.

#### 21.3.2.1. Security AWS Services

- Identity and Access Management:
  - IAM
  - AWS-STS
  - MFA token
  - AWS Organizations
- Detective Controls:
  - AWS Config
  - AWS CloudTrail
  - Amazon CloudWatch
- Infrastructure Protection:
  - Amazon CloudFront
  - Amazon VPC
  - AWS Shield
  - AWS WAF
  - Amazon Inspector
- Data Protection:
  - KMS
  - S3
  - Elastic Load Balancing (ELB)
  - Amazon EBS
  - Amazon RDS
- Incident Response:
  - IAM
  - AWS CloudFormation
  - Amazon CloudWatch Events

### 21.3.3. Reliability

- Ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.
- Design Principles:
  - **Test recovery procedures** - Use automation to simulate different failures or to recreate scenarios that led to failures before.
  - **Automatically recover from failure** - Anticipate and remediate failures before they occur.
  - **Scale horizontally to increase aggregate system availability** - Distribute requests across multiple, smaller resources to ensure that they don't share a common point of failure.
  - **Stop guessing capacity** - Maintain the optimal level to satisfy demand without over or under provisioning - Use Auto Scaling.
  - **Manage change in automation** - Use automation to make changes to infrastructure.

#### 21.3.3.1. Reliability AWS Services

- Foundations:
  - IAM
  - Amazon VPC
  - Service Quotas
  - AWS Trusted Advisor
- Change Management:
  - AWS Auto Scaling
  - Amazon CloudWatch
  - AWS CloudTrail
  - AWS Config
- Failure Management:
  - Backups
  - AWS CloudFormation
  - Amazon S3
  - Amazon S3 Glacier
  - Amazon Route 53

### 21.3.4. Performance Efficiency

- Includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve.
- Design Principles:
  - **Democratize advanced technologies** - Advance technologies become services and hence you can focus more on product development.
  - **Go global in minutes** - Easy deployment in multiple regions.
  - **Use serverless architectures** - Avoid burden of managing servers.
  - **Experiment more often** - Easy to carry out comparative testing.
  - **Mechanical sympathy** - Be aware of all AWS services.

#### 21.3.4.1. Performance Efficiency AWS Services

- Selection:
  - AWS Auto Scaling
  - AWS Lambda
  - Amazon Elastic Block Store (EBS)
  - Amazon Simple Storage Service (S3)
  - Amazon RDS
- Review:
  - AWS CloudFormation
- Monitoring:
  - Amazon CloudWatch
  - AWS Lambda
- Tradeoffs:
  - Amazon RDS
  - Amazon ElastiCache
  - AWS Snowball
  - Amazon CloudFront

### 21.3.5. Cost Optimization

- Includes the ability to run systems to deliver business value at the lowest price point.
- Design Principles:
  - **Adopt a consumption mode** - Pay only for what you use.
  - **Measure overall efficiency** - Use CloudWatch.
  - **Stop spending money on data center operations** - AWS does the infrastructure part and enables customer to focus on organization projects.
  - **Analyze and attribute expenditure** - Accurate identification of system usage and costs, helps measure return on investment (ROI)- Make sure to use tags.
  - **Use managed and application level services to reduce cost of ownership** - As managed services operate at cloud scale, they can offer a lower cost per transaction or service.

#### 21.3.5.1. Cost Optimization AWS Services

- Expenditure Awareness:
  - AWS Budgets
  - AWS Cost and Usage Report
  - AWS Cost Explorer
  - Reserved Instance Reporting
- Cost-Effective Resources:
  - Spot instance
  - Reserved instance
  - Amazon S3 Glacier
- Matching supply and demand:
  - AWS Auto Scaling
  - AWS Lambda
- Optimizing Over Time:
  - AWS Trusted Advisor
  - AWS Cost and Usage Report

### 21.3.6. Sustainability

- The sustainability pillar focuses on minimizing the environmental impacts of running cloud workloads.
- Design Principles:
  - **Understand your impact** - establish performance indicators, evaluate improvements.
  - **Establish sustainability goals** - Set long-term goals for each workload, model return on investment (ROI).
  - **Maximize utilization** - Right size each workload to maximize the energy efficiency of the underlying hardware and minimize idle resources.
  - **Anticipate and adopt new, more efficient hardware and software offerings** - and design for flexibility to adopt new technologies over time.
  - **Use managed services** - Shared services reduce the amount of infrastructure; Managed services help automate sustainability best practices as moving infrequent accessed data to cold storage and adjusting compute capacity.
  - **Reduce the downstream impact of your cloud workloads** - Reduce the amount of energy or resources required to use your services and reduce the need for your customers to upgrade their devices.

#### 21.3.6.1. Sustainability AWS Services

- EC2 Auto Scaling, Serverless Offering (Lambda, Fargate).
- Cost Explorer, AWS Graviton 2, EC2 T instances, @Spot Instances.
- EFS-IA, Amazon S3 Glacier, EBS Cold HDD volumes.
- S3 Lifecycle Configurations, S3 Intelligent Tiering.
- Amazon Data Lifecycle Manager.
- Read Local, Write Global: RDS Read Replicas, Aurora Global DB, DynamoDB Global Table, CloudFront.

## 21.4. AWS Well-Architected Tool

- Free tool to **review your architectures** against the 6 pillars Well-Architected Framework and **adopt architectural best practices**.
- How does it work?
  - Select your workload and answer questions.
  - Review your answers against the 6 pillars.
  - Obtain advice: get videos and documentations, generate a report, see the results in a dashboard.
- Let's have a look: https://console.aws.amazon.com/wellarchitected
  - https://aws.amazon.com/blogs/aws/new-aws-well-architected-tool-review-workloads-against-best-practices/

## 21.5. AWS Right Sizing

- EC2 has many instance types, but choosing the most powerful instance type isn't the best choice, because the cloud is **elastic**.
- Right sizing is the process of matching instance types and sizes to your workload performance and capacity requirements **at the lowest possible cost**.
- **Scaling up is easy so always start small**.
- It's also the process of looking at deployed instances and identifying opportunities to eliminate or downsize without compromising capacity or other requirements, which results in lower costs.
- It's important to Right Size...
  - **Before a Cloud Migration**.
  - **Continuously after the cloud onboarding process (requirements change over time)**.
- CloudWatch, Cost Explorer, Trusted Advisor, 3rd party tools can help.

## 21.6. AWS Ecosystem - Free resources

- **AWS Blogs:** https://aws.amazon.com/blogs/aws/
- **AWS Forums (community):** https://forums.aws.amazon.com/index.jspa
- **AWS Whitepapers & Guides:** https://aws.amazon.com/whitepapers
- **AWS Quick Starts:** https://aws.amazon.com/quickstart/
  - Automated, gold-standard deployments in the AWS Cloud
  - Build your production environment quickly with templates
  - Example: WordPress on AWS https://fwd.aws/P3yyv?did=qs_card&trk=qs_card
  - Leverages CloudFormation
- **AWS Solutions:** https://aws.amazon.com/solutions/
  - Vetted Technology Solutions for the AWS Cloud.
  - Example - AWS Landing Zone: secure, multi-account AWS environment:
    - https://aws.amazon.com/solutions/implementations/aws-landing-zone/
    - "Replaced" by AWS Control Tower

## 21.7. AWS Marketplace

- Digital catalog with thousands of software listings from **independent software vendors** (3rd party).
- Example:
  - Custom AMI (custom OS, firewalls, technical solutions...).
  - CloudFormation templates.
  - Software as a Service.
  - Containers.
- If you buy through the AWS Marketplace, it goes into your AWS bill.
- You can **sell your own solutions** on the AWS Marketplace.

## 21.8. AWS Training

- AWS Digital (online) and Classroom Training (in-person or virtual).
- AWS Private Training (for your organization).
- Training and Certification for the U.S Government.
- Training and Certification for the Enterprise.
- AWS Academy: helps universities teach AWS.

## 21.9. AWS Professional Services & Partner Network

- The AWS Professional Services organization is a global team of experts
- They work alongside your team and a chosen member of the APN
- APN = AWS Partner Network
- APN Technology Partners: providing hardware, connectivity, and software
- APN Consulting Partners: professional services firm to help build on AWS
- APN Training Partners: find who can help you learn AWS
- AWS Competency Program: AWS Competencies are granted to APN Partners who have demonstrated technical proficiency and proven
  customer success in specialized solution areas.
- AWS Navigate Program: help Partners become better Partners

## 21.10. AWS Knowledge Center

- Contains the most frequent & common questions and requests
  - https://aws.amazon.com/premiumsupport/knowledge-center/

# 22. AWS Cloud Map

- A fully managed resource discovery service.
- Creates a map of the backend services/resources that your applications depend on
- You register your application components, their locations, attributes, and health status with AWS Cloud Map.
- Integrated health checking (stop sending traffic to unhealthy endpoints).
- Your applications can query AWS Cloud Map using AWS SDK, API, or DNS.

# 23. AWS Limits (Quotas)

[AWS Limits](AWS%20Limits.md)

# 24. AWS related Abbreviations & Acronyms

## 24.1. A

- AWS Amazon Web Services
- Amazon ES = Amazon Elasticsearch Service
- AMI Amazon Machine Image
- API Application Programming Interface
- AI Artificial Intelligence
- ACL Access Control List
- ALB Application Load Balancer
- ARN Amazon Resource Name
- AZ Availability Zone
- ACM AWS Certificate Management
- ASG Auto Scaling Group
- AES Advanced Encryption System
- ADFS Active Directory Federation Service
- AVX Advanced Vector Extensions

## 24.2. B

- BYOL Bring Your Own License

## 24.3. C

- CDN Content Delivery Network
- CRC Cyclic Redundancy Check
- CLI Command Line Interface
- CIDR Classless Inter-Domain Routing
- CORS Cross Origin Resource Sharing
- CRR Cross Region Replication
- CI/CD Continuous Integration/Continuous Deployment

## 24.4. D

- DMS Database Migration Service
- DNS Domain Name System
- DDoS Distributed Denial of Service
- DoS Denial of Service
- DaaS Desktop as-a-Service

## 24.5. E

- EC2 Elastic Compute Cloud
- ECS EC2 Container Service
- ECR Elastic Container Registry
- EFS Elastic File System
- EI Elastic Inference
- ENA Elastic Network Adapter
- EKS Elastic Kubernetes Service
- EBS Elastic Block Store
- EMR Elastic MapReduce
- ELB Elastic Load Balancing
- EFA Elastic Fabric Adapter
- EIP Elastic IP
- EDA Electronic Design Automation
- ENI Elastic Network Interface
- ECU EC2 Compute Unit

## 24.6. F

- FIFO First In First Out
- FaaS Function as-a-Service

## 24.7. H

- HPC High-Performance Compute
- HVM Hardware Virtual Machine
- HTTP Hypertext Transfer Protocol
- HTTPS HTTP Secure
- HDK Hardware Development Kit

## 24.8. I

- IAM Identity & Access Management
- iOT Internet Of Things
- S3 IA S3 Infrequent Access
- iSCSI Internet Small Computer Storage Interface
- IOPS Input/Output Operation Per Second
- IGW Internet Gateway
- ICMP Internet Control Message Protocol
- IP Internet Protocol
- IPSec Internet Protocol Security
- IaaS Infrastructure-as-a-Service

## 24.9. J

- JSON JavaScript Object Notation

## 24.10. K

- KMS Key Management Service
- KVM Kernel-based Virtual Machine
- KDS Kinesis Data Streams
- KDF Kinesis Data Firehose

## 24.11. L

- LB Load Balancer
- LCU Load Balancer Capacity Unit

## 24.12. M

- MFA Multi-Factor Authentication
- MSTSC Microsoft Terminal Service Client
- MPP Massive Parallel Processing
- MITM Man in the Middle Attack
- ML Machine Learning
- MPLS Multi Protocol Label Switching

## 24.13. N

- NACL Network Access Control List
- NLP Natural Language Processing
- NFS Network File System
- NS Name Server
- NAT Network Address Translation
- NVMe Non-Volatile Memory Express

## 24.14. O

- OLTP Online Transaction Processing
- OLAP Online Analytics Processing
- OCI Open Container Initiative
- OVA Open Virtualization Format

## 24.15. P

- PCI DSS Payment Card Industry Data Security Standard
- PVM Para Virtual Machine
- PV ParaVirtual
- PaaS Platform as a Service

## 24.16. Q

- QLDB Quantum Ledger Database

## 24.17. R

- RAIDRedundant Array of Independent Disk
- RDS Relational Database Service
- RRS Reduced Redundancy Storage
- RI Reserved Instance
- RAM Random-access Memory
- RIE Runtime Interface Emulator
- RCU Read Capacity Units

## 24.18. S

- SSEServer Side Encryption
- S3 Simple Storage Service
- S3 RTC S3 Replication Time Control
- SRR Same Region Replication
- SMS Server Migration Service
- SWF Simple Workflow Service
- SES Simple Email Service
- SNS Simple Notification Service
- SQS Simple Queue Service
- SES Simple Email Service
- SLA Service Level Agreement
- SSL Secure Socket Layer
- SOA Start of Authority
- SDK Software Development Kit
- SSH Secure Shell
- SAR Serverless Application Repository
- SRD Scalable Reliable Datagrams
- SSO Single Sign-On
- SAML Security Assertion Markup Language
- SaaS Software-as-a-Service
- SaaS Security-as-a-Service
- SCP Service Control Policies
- SCA Storage Class Analysis
- STS Security Token Service
- SNI Server Name Indication

## 24.19. T

- TAM Technical Account Managers
- TTL Time To Live
- TLS Transport Layer Security
- TPM Trusted Platform Module
- TME Total Memory Encryption
- TPM Technical Program Manager
- TPS Transaction Per Second
- TCP Transmission Control Protocol

## 24.20. V

- VPC Virtual Private Cloud
- VM Virtual Machine
- VTL Virtual Tape Library
- VPN Virtual Private Network
- VLAN Virtual Local Area Network
- VDI Virtual Desktop Infrastructure
- VPG Virtual Private Gateway

## 24.21. W

- WAFWeb Application Firewall
- WCU Write Capacity Units

# 25. Commands AWS CLI

- List of all profiles
  - aws configure list-profiles
- See specific profile
  - aws sts get-caller-identity --profile `<name_profile>`# name of the profile you want to view
- List of AWS Regions
  - aws ec2 describe-regions
- Find AWS availability zones using AWS CLI
  - aws ec2 describe-availability-zones --region `<region>`
- See encoded errors using STS command line:
  - aws sts decode-authorization-message --encoded-message `<code_encoded>`

## 25.1. DynamoDB

- List all itens of table (Projection expression)
  - aws dynamodb scan --table-name `<table_name>`
  - aws dynamodb scan --table-name `<table_name>` --page-size 1
  - aws dynamodb scan --table-name `<table_name>` --max-items 1
  - aws dynamodb scan --table-name `<table_name>` --projection-expression "`<attribute_fields_or_columns>`" # Filter attributes
- List all content of table (F ilter expression)
  - aws dynamodb scan --table-name DemoTTL --filter-expression "`<attribute_fields_or_columns>` = :u" --expression-attribute-values '{":u": {"S":"`<content>`"}}'

## 25.2. S3

- List all itens of bucket with pagination
  - aws s3api list-objects --bucket `<bucket_name>` --page-size 100 --max-items 5

## 25.3. Lambda

- List all funcions
  - aws lambda list-functions
- Invoke a synchronous or asynchronous lambda function.
  - aws lambda invoke --function-name `<lambda_name>` --invocation-type `<invocation_type>` response.json # `invocation_type` like: `Event` or `RequestResponse`

## 25.4. ECS

- There is no option to delete a task definition on the AWS console. But, you can deregister (delete) a task definition by executing the following command.
  - aws ecs deregister-task-definition --task-definition `<name_task_definition:"revision>`

## 25.5. Systems Manager

- Create parameter
  - aws ssm put-parameter --name myEC2TypeDev --type String --value "t2.small"

# 26. Credits

- Much of this content extracted from Stephane Maarek's courses, **for personal study**, at several points has personal considerations and comments.
