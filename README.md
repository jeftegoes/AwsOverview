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
  - [7.2. Amazon Lightsail](#72-amazon-lightsail)
  - [7.3. Other Compute - Summary](#73-other-compute---summary)
- [8. Global Infrastructure](#8-global-infrastructure)
  - [8.1. Why make a global application?](#81-why-make-a-global-application)
  - [8.2. Global AWS Infrastructure](#82-global-aws-infrastructure)
  - [8.3. Global Applications in AWS](#83-global-applications-in-aws)
  - [8.4. AWS Outposts](#84-aws-outposts)
  - [8.5. AWS WaveLength](#85-aws-wavelength)
  - [8.6. AWS Local Zones](#86-aws-local-zones)
  - [8.7. Global Applications Architecture](#87-global-applications-architecture)
  - [8.8. Global Applications in AWS - Summary](#88-global-applications-in-aws---summary)
  - [8.9. Global Applications in AWS - Summary](#89-global-applications-in-aws---summary)
- [9. Application Integration](#9-application-integration)
  - [9.1. Amazon MQ](#91-amazon-mq)
  - [9.2. Integration - Summary](#92-integration---summary)
- [10. Cloud Monitoring](#10-cloud-monitoring)
  - [10.1. Amazon CloudWatch Metrics](#101-amazon-cloudwatch-metrics)
    - [10.1.1. Important Metrics](#1011-important-metrics)
  - [10.2. Amazon CloudWatch Alarms](#102-amazon-cloudwatch-alarms)
  - [10.3. Amazon CloudWatch Logs](#103-amazon-cloudwatch-logs)
  - [10.4. Amazon EventBridge](#104-amazon-eventbridge)
  - [10.5. AWS CloudTrail](#105-aws-cloudtrail)
  - [10.6. AWS X-Ray](#106-aws-x-ray)
  - [10.7. Amazon CodeGuru](#107-amazon-codeguru)
  - [10.8. AWS Health Dashboard](#108-aws-health-dashboard)
  - [10.9. Monitoring Summary](#109-monitoring-summary)
- [11. VPC](#11-vpc)
- [12. Security \& Compliance](#12-security--compliance)
  - [12.1. AWS Shared Responsibility Model](#121-aws-shared-responsibility-model)
    - [12.1.1. Example, for RDS](#1211-example-for-rds)
    - [12.1.2. Example, for S3](#1212-example-for-s3)
  - [12.2. DDOS Protection on AWS](#122-ddos-protection-on-aws)
  - [12.3. AWS Shield](#123-aws-shield)
  - [12.4. Penetration Testing on AWS Cloud](#124-penetration-testing-on-aws-cloud)
  - [12.5. Data at rest vs. Data in transit](#125-data-at-rest-vs-data-in-transit)
  - [12.6. AWS Secrets Manager](#126-aws-secrets-manager)
  - [12.7. AWS Artifact (not really a service)](#127-aws-artifact-not-really-a-service)
  - [12.8. AWS Config](#128-aws-config)
  - [12.9. Amazon Macie](#129-amazon-macie)
  - [12.10. AWS Security Hub](#1210-aws-security-hub)
  - [12.11. Amazon Detective](#1211-amazon-detective)
  - [12.12. AWS Abuse](#1212-aws-abuse)
  - [12.13. Root user privileges](#1213-root-user-privileges)
  - [12.14. Summary: Security \& Compliance](#1214-summary-security--compliance)
- [13. Machine Learning](#13-machine-learning)
- [14. Account Management, Billing \& Support](#14-account-management-billing--support)
  - [14.1. AWS Organizations](#141-aws-organizations)
  - [14.2. AWS Control Tower](#142-aws-control-tower)
  - [14.3. Pricing Models in AWS](#143-pricing-models-in-aws)
    - [14.3.1. Free services and free tier in AWS](#1431-free-services-and-free-tier-in-aws)
    - [14.3.2. Compute Pricing - EC2](#1432-compute-pricing---ec2)
    - [14.3.3. Compute Pricing - Lambda \& ECS](#1433-compute-pricing---lambda--ecs)
    - [14.3.4. Storage Pricing - S3](#1434-storage-pricing---s3)
    - [14.3.5. Storage Pricing - EBS](#1435-storage-pricing---ebs)
    - [14.3.6. Database Pricing - RDS](#1436-database-pricing---rds)
    - [14.3.7. Content Delivery - CloudFront](#1437-content-delivery---cloudfront)
      - [14.3.7.1. Networking Costs in AWS per GB - Simplified](#14371-networking-costs-in-aws-per-gb---simplified)
    - [14.3.8. Savings Plan](#1438-savings-plan)
  - [14.4. AWS Compute Optimizer](#144-aws-compute-optimizer)
  - [14.5. Billing and Costing Tools](#145-billing-and-costing-tools)
- [15. AWS Directory Services](#15-aws-directory-services)
- [16. Security, Identity, \& Compliance](#16-security-identity--compliance)
- [17. Other AWS Services](#17-other-aws-services)
  - [17.1. Amazon WorkSpaces](#171-amazon-workspaces)
  - [17.2. Amazon AppStream 2.0](#172-amazon-appstream-20)
    - [17.2.1. Amazon AppStream 2.0 vs WorkSpaces](#1721-amazon-appstream-20-vs-workspaces)
  - [17.3. Amazon Sumerian](#173-amazon-sumerian)
  - [17.4. AWS IoT Core](#174-aws-iot-core)
  - [17.5. Amazon Elastic Transcoder](#175-amazon-elastic-transcoder)
  - [17.6. AWS Device Farm](#176-aws-device-farm)
  - [17.7. AWS Backup](#177-aws-backup)
    - [17.7.1. Disaster Recovery Strategies](#1771-disaster-recovery-strategies)
  - [17.8. AWS Elastic Disaster Recovery (DRS)](#178-aws-elastic-disaster-recovery-drs)
  - [17.9. AWS DataSync](#179-aws-datasync)
  - [17.10. AWS Fault Injection Simulator (FIS)](#1710-aws-fault-injection-simulator-fis)
  - [17.11. AWS Step Functions](#1711-aws-step-functions)
  - [17.12. Amazon AppFlow](#1712-amazon-appflow)
  - [17.13. AWS CloudSearch](#1713-aws-cloudsearch)
  - [17.14. Amazon SES - Simple Email Service](#1714-amazon-ses---simple-email-service)
- [18. AWS Architecting \& Ecosystem](#18-aws-architecting--ecosystem)
  - [18.1. Well Architected Framework General - Guiding Principles](#181-well-architected-framework-general---guiding-principles)
  - [18.2. AWS Cloud Best Practices - Design Principles](#182-aws-cloud-best-practices---design-principles)
    - [18.2.1. Operational Excellence](#1821-operational-excellence)
      - [18.2.1.1. Operational Excellence AWS Services](#18211-operational-excellence-aws-services)
    - [18.2.2. Security](#1822-security)
      - [18.2.2.1. Security AWS Services](#18221-security-aws-services)
    - [18.2.3. Reliability](#1823-reliability)
      - [18.2.3.1. Reliability AWS Services](#18231-reliability-aws-services)
    - [18.2.4. Performance Efficiency](#1824-performance-efficiency)
      - [18.2.4.1. Performance Efficiency AWS Services](#18241-performance-efficiency-aws-services)
    - [18.2.5. Cost Optimization](#1825-cost-optimization)
      - [18.2.5.1. Cost Optimization AWS Services](#18251-cost-optimization-aws-services)
    - [18.2.6. Sustainability](#1826-sustainability)
      - [18.2.6.1. Sustainability AWS Services](#18261-sustainability-aws-services)
  - [18.3. AWS Right Sizing](#183-aws-right-sizing)
  - [18.4. AWS Ecosystem - Free resources](#184-aws-ecosystem---free-resources)
  - [18.5. AWS Marketplace](#185-aws-marketplace)
  - [18.6. AWS Training](#186-aws-training)
  - [18.7. AWS Professional Services \& Partner Network](#187-aws-professional-services--partner-network)
  - [18.8. AWS Knowledge Center](#188-aws-knowledge-center)
- [19. AWS Cloud Map](#19-aws-cloud-map)
- [20. AWS Limits (Quotas)](#20-aws-limits-quotas)
- [21. Private vs Public IP (IPv4)](#21-private-vs-public-ip-ipv4)
  - [21.1. Private vs Public IP (IPv4) Fundamental Differences](#211-private-vs-public-ip-ipv4-fundamental-differences)
- [22. AWS charges for IPv4 addresses](#22-aws-charges-for-ipv4-addresses)
- [23. Commands AWS CLI](#23-commands-aws-cli)
  - [23.1. DynamoDB](#231-dynamodb)
  - [23.2. S3](#232-s3)
  - [23.3. Lambda](#233-lambda)
  - [23.4. ECS](#234-ecs)
  - [23.5. Systems Manager](#235-systems-manager)
- [24. Credits](#24-credits)

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

[AWS ECS, Fargate and ECR](/Containers/Amazon%20ECS.md)

## 7.2. Amazon Lightsail

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

## 7.3. Other Compute - Summary

- Docker: container technology to run applications.
- ECS: run Docker containers on EC2 instances.
- Fargate:
  - Run Docker containers without provisioning the infrastructure.
  - Serverless offering (no EC2 instances).
- ECR: Private Docker Images Repository.
- Lightsail: predictable & low pricing for simple application & DB stacks.

# 8. Global Infrastructure

## 8.1. Why make a global application?

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

## 8.2. Global AWS Infrastructure

- **Regions:** For deploying applications and infrastructure
- **Availability Zones:** Made of multiple data centers
- **Edge Locations (Points of Presence):** for content delivery as close as possible to users

## 8.3. Global Applications in AWS

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

## 8.4. AWS Outposts

- **Hybrid Cloud:** Businesses that keep an on-premises infrastructure alongside a cloud infrastructure.
- Therefore, two ways of dealing with IT systems:
  - One for the AWS cloud (using the AWS console, CLI, and AWS APIs).
  - One for their on-premises infrastructure.
- **AWS Outposts are "server racks"** that offers the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud.
- **AWS will setup and manage "Outposts Racks"** within your on-premises infrastructure and you can start leveraging AWS services on-premises.
- _You are responsible for the Outposts Rack physical security._
- **Benefits**
  - Low-latency access to on-premises systems.
  - Local data processing.
  - Data residency.
  - Easier migration from on-premises to the cloud.
  - Fully managed service.
- **Some services that work on Outposts**
  - Amazon EC2
  - Amazon EBS
  - Amazon S3
  - Amazon EKS
  - Amazon ECS
  - Amazon RDS
  - Amazon EMR

## 8.5. AWS WaveLength

- **AWS Wavelength is an AWS Infrastructure offering optimized for mobile edge computing applications. Wavelength combines the high bandwidth and ultra-low latency of 5G networks with AWS compute and storage services to enable developers to innovate and build a whole new class of applications.**
- WaveLength Zones are infrastructure deployments embedded within the telecommunications providers datacenters at the edge of the 5G networks.
- Brings AWS services to the edge of the 5G networks.
- Example: EC2, EBS, VPC...
- Ultra-low latency applications through 5G networks.
- Traffic doesn't leave the Communication Service Provider's (CSP) network.
- High-bandwidth and secure connection to the parent AWS Region.
- No additional charges or service agreements.
- Use cases: Smart Cities, ML-assisted diagnostics, Connected Vehicles, Interactive Live Video Streams, AR/VR, Real-time Gaming, ...

## 8.6. AWS Local Zones

- Places AWS compute, storage, database, and other selected AWS services closer to end users to run latency-sensitive applications
- Extend your VPC to more locations - "Extension of an AWS Region"
- Compatible with EC2, RDS, ECS, EBS, ElastiCache, Direct Connect ...
- Example:
  - AWS Region: N. Virginia (us-east-1)
  - AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami, ...

## 8.7. Global Applications Architecture

- Single Region, Single AZ.
- Single Region, Multi AZ.
- Multi Region, Active-Passive.
- Multi Region, Active-Active.

## 8.8. Global Applications in AWS - Summary

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

## 8.9. Global Applications in AWS - Summary

- AWS Outposts
  - Deploy Outposts Racks in your own Data Centers to extend AWS services
- AWS WaveLength
  - Brings AWS services to the edge of the 5G networks
  - Ultra-low latency applications
- AWS Local Zones
  - Bring AWS resources (compute, database, storage, ...) closer to your users
  - Good for latency-sensitive applications

# 9. Application Integration

- [Application Integration](Application%20Integration/README.md)
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

## 9.1. Amazon MQ

- **Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers in the cloud.**
- SQS, SNS are "cloud-native" services, and they're using proprietary protocols from AWS.
- Traditional applications running from on-premise may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS.
- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ.
- Amazon MQ = managed Apache ActiveMQ.
- Amazon MQ doesn't "scale" as much as SQS / SNS.
- Amazon MQ runs on a dedicated machine (not serverless).
- Amazon MQ has both queue feature (SQS) and topic features (SNS).

## 9.2. Integration - Summary

- Kinesis: real-time data streaming, persistence and analysis.
- Amazon MQ: managed Apache MQ in the cloud (MQTT, AMQP protocols).
- **When using SQS or SNS, you apply the "decouple your applications" principle. This means that IT systems should be designed in a way that reduces interdependencies-a change or a failure in one component should not cascade to other components.**

# 10. Cloud Monitoring

## 10.1. Amazon CloudWatch Metrics

[Amazon CloudWatch](Management%20&%20Governance/Amazon%20CloudWatch.md)

### 10.1.1. Important Metrics

- EC2 instances: CPU Utilization, Status Checks, Network (not RAM).
  - Default metrics every 5 minutes.
  - Option for Detailed Monitoring ($$$): metrics every 1 minute.
- EBS volumes: Disk Read/Writes.
- S3 buckets: BucketSizeBytes, NumberOfObjects, AllRequests.
- Billing:Total Estimated Charge (only in us-east-1).
- Service Limits: how much you've been using a service API.
- Custom metrics: push your own metrics.

## 10.2. Amazon CloudWatch Alarms

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

## 10.3. Amazon CloudWatch Logs

- CloudWatch Logs can collect log from:
  - Elastic Beanstalk: collection of logs from application.
  - ECS: collection from containers.
  - AWS Lambda: collection from function logs.
  - CloudTrail based on filter.
  - CloudWatch log agents: on EC2 machines or on-premises servers.
  - Route53: Log DNS queries.
- Enables real-time monitoring of logs.
- Adjustable CloudWatch Logs retention.

## 10.4. Amazon EventBridge

[Amazon EventBridge](Application%20Integration/Amazon%20EventBridge.md)

## 10.5. AWS CloudTrail

- **Is a web service that records activity made on your account and delivers log files to your Amazon S3 bucket. AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account.**
- **Can record the history of events/API calls made within you AWS account, which will help determine who or what deleted the resource. You should investigate it first.** [AWS CloudTrail](Management%20&%20Governance/AWS%20CloudTrail.md)

## 10.6. AWS X-Ray

- **AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture.** [AWS X-Ray](Developer%20Tools/AWS%20X-Ray.md)

## 10.7. Amazon CodeGuru

[Amazon CodeGuru](Developer%20Tools/AWS%20CICD.md)

## 10.8. AWS Health Dashboard

[AWS Health Dashboard](Management%20&%20Governance/AWS%20Health%20Dashboard.md)

## 10.9. Monitoring Summary

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

# 11. VPC

![Amazon VPC](Networking%20&%20Content%20Delivery/Amazon%20VPC.md)

# 12. Security & Compliance

## 12.1. AWS Shared Responsibility Model

- AWS responsibility - Security of the Cloud
  - Protecting infrastructure (hardware, software, facilities, and networking) that runs all the AWS services.
  - Managed services like S3, DynamoDB, RDS, etc.
- Customer responsibility - Security in the Cloud
  - For EC2 instance, customer is responsible for management of the guest OS (including security patches and updates), firewall & network configuration, IAM.
  - Encrypting application data.
- Shared controls:
  - **AWS is responsible for patching and fixing flaws within the infrastructure, but customers are responsible for patching their guest OS and applications. Shared Controls also includes Configuration Management, and Awareness and Training.**
  - Patch Management, Configuration Management, Awareness & Training.

### 12.1.1. Example, for RDS

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

### 12.1.2. Example, for S3

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

## 12.2. DDOS Protection on AWS

- DDOS = Distributed Denial-of-Service
- **AWS Shield Standard:** protects against DDOS attack for your website and applications, for all customers at no additional costs
- **AWS Shield Advanced:** 24/7 premium DDoS protection
- **AWS WAF:** Filter specific requests based on rules
- **CloudFront and Route 53:**
  - Availability protection using global edge network
  - Combined with AWS Shield, provides attack mitigation at the edge
- Be ready to scale - leverage **AWS Auto Scaling**

## 12.3. AWS Shield

- **AWS Shield Standard:**
  - Free service that is activated for every AWS customer.
  - Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks.
- **AWS Shield Advanced:**
  - Optional DDoS mitigation service ($3,000 per month per organization).
  - Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53.
  - 24/7 access to AWS DDoS response team (DRP).
  - Protect against higher fees during usage spikes due to DDoS.

## 12.4. Penetration Testing on AWS Cloud

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

## 12.5. Data at rest vs. Data in transit

- **At rest:** data stored or archived on a device.
  - On a hard disk, on a RDS instance, in S3 Glacier Deep Archive, etc.
- **In transit (in motion):** data being moved from one location to another.
  - Transfer from on-premises to AWS, EC2 to DynamoDB, etc.
  - Means data transferred on the network.
- We want to encrypt data in both states to protect it!
- For this we leverage **encryption keys**.

## 12.6. AWS Secrets Manager

- Newer service, meant for storing secrets.
- Capability to force **rotation of secrets** every X days.
- Automate generation of secrets on rotation (uses Lambda).
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora).
- Secrets are encrypted using KMS.
- Mostly meant for RDS integration.

## 12.7. AWS Artifact (not really a service)

- **AWS Artifact is your go-to, central resource for compliance-related information that matters to you.**
- Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements.
- **Artifact Reports** - Allows you to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports.
- **Artifact Agreements** - Allows you to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in your organization.
- Can be used to support internal audit or compliance.

## 12.8. AWS Config

- **AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time.** [AWS Config](Management%20&%20Governance/AWS%20Config.md)

## 12.9. Amazon Macie

- Amazon Macie is a fully managed data security and data privacy service that uses **machine learning and pattern matching to discover and protect your sensitive data in AWS**.
- Macie helps identify and alert you to **sensitive data, such as personally identifiable information (PII)**.

## 12.10. AWS Security Hub

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

## 12.11. Amazon Detective

- GuardDuty, Macie, and Security Hub are used to identify potential
  security issues, or findings.
- Sometimes security findings require deeper analysis to isolate the root cause and take action - it's a complex process.
- Amazon Detective **analyzes, investigates, and quickly identifies the root cause of security issues or suspicious activities using ML and graphs**.
- **Automatically collects and processes events** from VPC Flow Logs, CloudTrail, and GuardDuty to create a unified view.
- Produces visualizations with details and context to get to the root cause.

## 12.12. AWS Abuse

- **Report suspected AWS resources used for abusive or illegal purposes**.
- Abusive & prohibited behaviors are:
  - **Spam** - receving undesired emails from AWS-owned IP address, websites & forums spammed by AWS resources.
  - **Port scanning**- sending packets to your ports to discover the unsecured ones.
  - **DoS or DDoS attacks** - AWS-owned IP addresses attempting to overwhlem or crash your servers/softwares.
  - **Intrusion attempts** - logging in on your resources.
  - **Hosting objectionable** or copyrighted content - distributing illegal or copyrighted content without consent.
  - **Distributing malware** - AWS resources distributing softwares to harm computers or machines.
- Contact the AWS Abuse team: AWS abuse form, or abuse@amazonaws.com.

## 12.13. Root user privileges

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

## 12.14. Summary: Security & Compliance

- Shared Responsibility on AWS
- Shield: Automatic DDoS Protection + 24/7 support for advanced
- CloudHSM: Hardware encryption, we manage encryption keys
- AWS Certificate Manager: provision, manage, and deploy SSL/TLS Certificates
- Artifact: Get access to compliance reports such as PCI, ISO, etc...
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

# 13. Machine Learning

[Machine Learning](Machine%20Learning/README.md)

# 14. Account Management, Billing & Support

## 14.1. AWS Organizations

[AWS Organizations](AWS%20Organizations.md)

## 14.2. AWS Control Tower

- **AWS Control Tower offers the easiest way to set up and govern a new, secure, multi-account AWS environment. It establishes a landing zone that is based on best-practices blueprints, and enables governance using guardrails you can choose from a pre-packaged list.** [AWS Control Tower](AWS%20Control%20Tower.md)

## 14.3. Pricing Models in AWS

- AWS has 4 pricing models:
  - **Pay as you go:** pay for what you use, remain agile, responsive, meet scale demands.
  - **Save when you reserve:** minimize risks, predictably manage budgets, comply with long-terms requirements.
    - Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElastiCache Reserved Nodes, RDS Reserved Instance, Redshift Reserved Nodes.
  - **Pay less by using more:** volume-based discounts
  - **Pay less as AWS grows**

### 14.3.1. Free services and free tier in AWS

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

### 14.3.2. Compute Pricing - EC2

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

### 14.3.3. Compute Pricing - Lambda & ECS

- Lambda:
  - Pay per call.
  - Pay per duration.
- ECS:
  - EC2 Launch Type Model: No additional fees, you pay for AWS resources stored and created in your application.
- Fargate:
  - Fargate Launch Type Model: Pay for vCPU and memory resources allocated to your applications in your containers.

### 14.3.4. Storage Pricing - S3

- Storage class: S3 Standard, S3 Infrequent Access, S3 One-Zone IA, S3 Intelligent Tiering, S3 Glacier and S3 Glacier Deep Archive.
- Number and size of objects: Price can be tiered (based on volume).
- Number and type of requests.
- Data transfer OUT of the S3 region.
- S3 Transfer Acceleration.
- Lifecycle transitions.
- Similar service: EFS (pay per use, has infrequent access & lifecycle rules).

### 14.3.5. Storage Pricing - EBS

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

### 14.3.6. Database Pricing - RDS

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

### 14.3.7. Content Delivery - CloudFront

- Pricing is different across different geographic regions.
- Aggregated for each edge location, then applied to your bill.
- Data Transfer Out (volume discount).
- Number of HTTP/HTTPS requests.

#### 14.3.7.1. Networking Costs in AWS per GB - Simplified

- Use Private IP instead of Public IP for good savings and better network performance.
- Use same AZ for maximum savings (at the cost of high availability).

### 14.3.8. Savings Plan

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

## 14.4. AWS Compute Optimizer

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

## 14.5. Billing and Costing Tools

[Billing and Costing Tools](Cloud%20Financial%20Management/README.md)

# 15. AWS Directory Services

[AWS Directory Services](/Security,%20Identity,%20&%20Compliance/AWS%20Directory%20Services.md)

# 16. Security, Identity, & Compliance

[Security, Identity, & Compliance](/Security,%20Identity,%20&%20Compliance/README.md)

# 17. Other AWS Services

## 17.1. Amazon WorkSpaces

- **Amazon WorkSpaces is a fully managed, secure cloud desktop service. You can use Amazon WorkSpaces to provision either Windows or Linux desktops in just a few minutes and quickly scale to provide thousands of desktops to workers across the globe.**
- Managed Desktop as a Service (DaaS) solution to easily provision Windows or Linux desktops.
- Great to eliminate management of on-premise VDI (Virtual Desktop Infrastructure).
- Fast and quickly scalable to thousands of users.
- Secured data - integrates with KMS.
- Pay-as-you-go service with monthly or hourly rates.

## 17.2. Amazon AppStream 2.0

- **Amazon AppStream 2.0 is a fully managed non-persistent application and desktop streaming service that provides users instant access to their desktop applications from anywhere.**
- Desktop Application Streaming Service.
- Deliver to any computer, without acquiring, provisioning infrastructure.
- The application is delivered from within a web browser.

### 17.2.1. Amazon AppStream 2.0 vs WorkSpaces

- **Workspaces:**
  - Fully managed VDI and desktop available.
  - The users connect to the VDI and open native or WAM applications.
  - Workspaces are on-demand or always on.
- **AppStream 2.0:**
  - Stream a desktop application to web browsers (no need to connect to a VDI).
  - Works with any device (that has a web browser).
  - Allow to configure an instance type per application type (CPU, RAM, GPU).

## 17.3. Amazon Sumerian

- **Amazon Sumerian is a managed service that lets you create and run 3D, Augmented Reality (AR) and Virtual Reality (VR) applications. You can build immersive and interactive scenes that run on AR and VR, mobile devices, and your web browser.**
- Create and run virtual reality (VR), augmented reality (AR), and 3D applications.
- Can be used to quickly create 3D models with animations.
- Ready-to-use templates and assets - no programming or 3D expertise required.
- Accessible via a web-browser URLs or on popular hardware for AR/VR.

## 17.4. AWS IoT Core

- **AWS IoT Core, is serverless and lets you connect billions of devices to the AWS Cloud, lets you securely connect IoT devices to the AWS Cloud and other devices without the need to provision or manage servers.**
- IoT stands for "Internet of Things" - the network of internet-connected devices that are able to collect and transfer data.
- AWS IoT Core allows you to easily connect IoT devices to the AWS Cloud.
- Serverless, secure & scalable to billions of devices and trillions of messages.
- Your applications can communicate with your devices even when they aren't connected.
- Integrates with a lot of AWS services (Lambda, S3, SageMaker, etc.).
- Build IoT applications that gather, process, analyze, and act on data.

## 17.5. Amazon Elastic Transcoder

- **Amazon Elastic Transcoder is media transcoding in the cloud. It is used to convert media files from their source format into versions that will play back on devices like smartphones, tablets, and PCs.**
- Elastic Transcoder is used to convert media files stored in S3 into media files in the formats required by consumer playback devices (phones etc..)
- Benefits:
  - Easy to use.
  - Highly scalable - can handle large volumes of media files and large file sizes.
  - Cost effective - duration-based pricing model.
  - Fully managed & secure, pay for what you use.

## 17.6. AWS Device Farm

- **AWS Device Farm is an application testing service that lets you improve the quality of your web and mobile apps by testing them across an extensive range of desktop browsers and real mobile devices; without having to provision and manage any testing infrastructure.**
- Fully-managed service that tests your web and mobile apps against desktop browsers, real mobile devices, and tablets.
- Run tests concurrently on multiple devices (speed up execution).
- Ability to configure device settings (GPS, language, Wi-Fi, Bluetooth, ...).

## 17.7. AWS Backup

[AWS Backup](/Storage/AWS%20Backup.md)

### 17.7.1. Disaster Recovery Strategies

[Disaster Recovery Strategies](Disaster%20Recovery.md)

## 17.8. AWS Elastic Disaster Recovery (DRS)

- Used to be named "CloudEndure Disaster Recovery".
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS.
- Example: protect your most critical databases (including Oracle, MySQL, and SQL Server), enterprise apps (SAP), protect your data from ransomware attacks, ...
- Continuous block-level replication for your servers.

## 17.9. AWS DataSync

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

## 17.10. AWS Fault Injection Simulator (FIS)

- A fully managed service for running fault injection experiments on AWS workloads.
- Based on **Chaos Engineering** - stressing an application by creating disruptive events (e.g., sudden increase in CPU or memory), observing how the system responds, and implementing improvements.
- Helps you uncover hidden bugs and performance bottlenecks.
- Supports the following AWS services: EC2, ECS, EKS, RDS...
- Use pre-built templates that generate the desired disruptions.

## 17.11. AWS Step Functions

- **AWS Step Functions is a low-code visual workflow service used to orchestrate AWS services, automate business processes, and build Serverless applications. It manages failures, retries, parallelization, service integrations, ...** [AWS Step Functions](AWS%20Step%20Functions.md)

## 17.12. Amazon AppFlow

[Amazon AppFlow](/Application%20Integration/Amazon%20AppFlow.md)

## 17.13. AWS CloudSearch

- Amazon CloudSearch is a managed service in the AWS Cloud that makes it simple and cost-effective to set up, manage, and scale a search solution for your website or application.
- Amazon CloudSearch supports 34 languages and popular search features such as highlighting, autocomplete, and geospatial search.

## 17.14. Amazon SES - Simple Email Service

- **Fully managed service to send emails securely, globally and at scale.** [AWS SES](/Business%20Applications/Amazon%20SES.md)

# 18. AWS Architecting & Ecosystem

## 18.1. Well Architected Framework General - Guiding Principles

[White Papers & Architectures](White%20Papers%20&%20Architectures.md)

## 18.2. AWS Cloud Best Practices - Design Principles

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

### 18.2.1. Operational Excellence

- Includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures.
- Design Principles:
  - **Perform operations as code** - Infrastructure as code.
  - **Annotate documentation** - Automate the creation of annotated documentation after every build.
  - **Make frequent, small, reversible changes** - So that in case of any failure, you can reverse it.
  - **Refine operations procedures frequently** - And ensure that team members are familiar with it.
  - **Anticipate failure**.
  - **Learn from all operational failures**.

#### 18.2.1.1. Operational Excellence AWS Services

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

### 18.2.2. Security

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

#### 18.2.2.1. Security AWS Services

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

### 18.2.3. Reliability

- Ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.
- Design Principles:
  - **Test recovery procedures** - Use automation to simulate different failures or to recreate scenarios that led to failures before.
  - **Automatically recover from failure** - Anticipate and remediate failures before they occur.
  - **Scale horizontally to increase aggregate system availability** - Distribute requests across multiple, smaller resources to ensure that they don't share a common point of failure.
  - **Stop guessing capacity** - Maintain the optimal level to satisfy demand without over or under provisioning - Use Auto Scaling.
  - **Manage change in automation** - Use automation to make changes to infrastructure.

#### 18.2.3.1. Reliability AWS Services

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

### 18.2.4. Performance Efficiency

- Includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve.
- Design Principles:
  - **Democratize advanced technologies** - Advance technologies become services and hence you can focus more on product development.
  - **Go global in minutes** - Easy deployment in multiple regions.
  - **Use serverless architectures** - Avoid burden of managing servers.
  - **Experiment more often** - Easy to carry out comparative testing.
  - **Mechanical sympathy** - Be aware of all AWS services.

#### 18.2.4.1. Performance Efficiency AWS Services

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

### 18.2.5. Cost Optimization

- Includes the ability to run systems to deliver business value at the lowest price point.
- Design Principles:
  - **Adopt a consumption mode** - Pay only for what you use.
  - **Measure overall efficiency** - Use CloudWatch.
  - **Stop spending money on data center operations** - AWS does the infrastructure part and enables customer to focus on organization projects.
  - **Analyze and attribute expenditure** - Accurate identification of system usage and costs, helps measure return on investment (ROI)- Make sure to use tags.
  - **Use managed and application level services to reduce cost of ownership** - As managed services operate at cloud scale, they can offer a lower cost per transaction or service.

#### 18.2.5.1. Cost Optimization AWS Services

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

### 18.2.6. Sustainability

- The sustainability pillar focuses on minimizing the environmental impacts of running cloud workloads.
- Design Principles:
  - **Understand your impact** - establish performance indicators, evaluate improvements.
  - **Establish sustainability goals** - Set long-term goals for each workload, model return on investment (ROI).
  - **Maximize utilization** - Right size each workload to maximize the energy efficiency of the underlying hardware and minimize idle resources.
  - **Anticipate and adopt new, more efficient hardware and software offerings** - and design for flexibility to adopt new technologies over time.
  - **Use managed services** - Shared services reduce the amount of infrastructure; Managed services help automate sustainability best practices as moving infrequent accessed data to cold storage and adjusting compute capacity.
  - **Reduce the downstream impact of your cloud workloads** - Reduce the amount of energy or resources required to use your services and reduce the need for your customers to upgrade their devices.

#### 18.2.6.1. Sustainability AWS Services

- EC2 Auto Scaling, Serverless Offering (Lambda, Fargate).
- Cost Explorer, AWS Graviton 2, EC2 T instances, @Spot Instances.
- EFS-IA, Amazon S3 Glacier, EBS Cold HDD volumes.
- S3 Lifecycle Configurations, S3 Intelligent Tiering.
- Amazon Data Lifecycle Manager.
- Read Local, Write Global: RDS Read Replicas, Aurora Global DB, DynamoDB Global Table, CloudFront.

## 18.3. AWS Right Sizing

- EC2 has many instance types, but choosing the most powerful instance type isn't the best choice, because the cloud is **elastic**.
- Right sizing is the process of matching instance types and sizes to your workload performance and capacity requirements **at the lowest possible cost**.
- **Scaling up is easy so always start small**.
- It's also the process of looking at deployed instances and identifying opportunities to eliminate or downsize without compromising capacity or other requirements, which results in lower costs.
- It's important to Right Size...
  - **Before a Cloud Migration**.
  - **Continuously after the cloud onboarding process (requirements change over time)**.
- CloudWatch, Cost Explorer, Trusted Advisor, 3rd party tools can help.

## 18.4. AWS Ecosystem - Free resources

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

## 18.5. AWS Marketplace

- Digital catalog with thousands of software listings from **independent software vendors** (3rd party).
- Example:
  - Custom AMI (custom OS, firewalls, technical solutions...).
  - CloudFormation templates.
  - Software as a Service.
  - Containers.
- If you buy through the AWS Marketplace, it goes into your AWS bill.
- You can **sell your own solutions** on the AWS Marketplace.

## 18.6. AWS Training

- AWS Digital (online) and Classroom Training (in-person or virtual).
- AWS Private Training (for your organization).
- Training and Certification for the U.S Government.
- Training and Certification for the Enterprise.
- AWS Academy: helps universities teach AWS.

## 18.7. AWS Professional Services & Partner Network

- The AWS Professional Services organization is a global team of experts
- They work alongside your team and a chosen member of the APN
- APN = AWS Partner Network
- APN Technology Partners: providing hardware, connectivity, and software
- APN Consulting Partners: professional services firm to help build on AWS
- APN Training Partners: find who can help you learn AWS
- AWS Competency Program: AWS Competencies are granted to APN Partners who have demonstrated technical proficiency and proven
  customer success in specialized solution areas.
- AWS Navigate Program: help Partners become better Partners

## 18.8. AWS Knowledge Center

- Contains the most frequent & common questions and requests
  - https://aws.amazon.com/premiumsupport/knowledge-center/

# 19. AWS Cloud Map

- A fully managed resource discovery service.
- Creates a map of the backend services/resources that your applications depend on
- You register your application components, their locations, attributes, and health status with AWS Cloud Map.
- Integrated health checking (stop sending traffic to unhealthy endpoints).
- Your applications can query AWS Cloud Map using AWS SDK, API, or DNS.

# 20. AWS Limits (Quotas)

[AWS Limits](AWS%20Limits.md)

# 21. Private vs Public IP (IPv4)

- Networking has two sorts of IPs. IPv4 and IPv6:
  - IPv4: 1.160.10.240
  - IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
- IPv4 is still the most common format used online.
- IPv6 is newer and solves problems for the Internet of Things (IoT).
- IPv4 allows for **3.7 billion** different addresses in the public space.
- IPv4: [0-255].[0-255].[0-255].[0-255].

## 21.1. Private vs Public IP (IPv4) Fundamental Differences

- **Public IP**
  - Public IP means the machine can be identified on the internet (WWW).
  - Must be unique across the whole web (not two machines can have the same public IP).
  - Can be geo-located easily.
- **Private IP**
  - Private IP means the machine can only be identified on a private network only.
  - The IP must be unique across the private network.
  - BUT two different private networks (two companies) can have the same IPs.
  - Machines connect to WWW using a NAT + internet gateway (a proxy).
  - Only a specified range of IPs can be used as private IP.

# 22. AWS charges for IPv4 addresses

- Starting February 1st 2024, there's a charge for all Public IPv4 created in your account.
  - $0.005 per hour of Public IPv4 (~ $3.6 per month).
- For new accounts in AWS, you have a free tier for the EC2 service: 750 hours of Public IPv4 per month for the first 12 months.
- **For all other services there is no free tier.**
- **What about IPv6?**
  - Unfortunately, many Internet Service Provider (ISP) around the world don't support IPv6.
  - You can test IPv6 by going to https://test-ipv6.com/
  - If you use IPv6, you're on your own (security groups, networking...) but you can do it!
- **How to troubleshoot charges?**
  - Go into your AWS Bill
  - Look into the AWS Public IP Insights service
  - Nice article here: https://repost.aws/articles/ARknH_OR0cTvqoTfJrVGaB8A/why-am-i-seeing-charges-for-public-ipv4-addresses-when-i-am-under-the-aws-free-tier

# 23. Commands AWS CLI

- Configure AWS CLI to access the environment
  - aws configure
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

## 23.1. DynamoDB

- List all itens of table (Projection expression)
  - aws dynamodb scan --table-name `<table_name>`
  - aws dynamodb scan --table-name `<table_name>` --page-size 1
  - aws dynamodb scan --table-name `<table_name>` --max-items 1
  - aws dynamodb scan --table-name `<table_name>` --projection-expression "`<attribute_fields_or_columns>`" # Filter attributes
- List all content of table (F ilter expression)
  - aws dynamodb scan --table-name DemoTTL --filter-expression "`<attribute_fields_or_columns>` = :u" --expression-attribute-values '{":u": {"S":"`<content>`"}}'

## 23.2. S3

- List all itens of bucket with pagination
  - aws s3api list-objects --bucket `<bucket_name>` --page-size 100 --max-items 5

## 23.3. Lambda

- List all funcions
  - aws lambda list-functions
- Invoke a synchronous or asynchronous lambda function.
  - aws lambda invoke --function-name `<lambda_name>` --invocation-type `<invocation_type>` response.json # `invocation_type` like: `Event` or `RequestResponse`

## 23.4. ECS

- There is no option to delete a task definition on the AWS console. But, you can deregister (delete) a task definition by executing the following command.
  - aws ecs deregister-task-definition --task-definition `<name_task_definition:"revision>`

## 23.5. Systems Manager

- Create parameter
  - aws ssm put-parameter --name myEC2TypeDev --type String --value "t2.small"

# 24. Credits

- Much of this content extracted from Stephane Maarek's courses, **for personal study**, at several points has personal considerations and comments.
