# Aws overview <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Credits](#1-credits)
- [2. Traditionally, how to build infrastructure](#2-traditionally-how-to-build-infrastructure)
  - [2.1. How websites work](#21-how-websites-work)
  - [2.2. What is a server composed of?](#22-what-is-a-server-composed-of)
  - [2.3. IT Terminology](#23-it-terminology)
  - [2.4. Problems with traditional IT approach](#24-problems-with-traditional-it-approach)
- [3. What is Cloud Computing?](#3-what-is-cloud-computing)
  - [3.1. You've been using some Cloud services](#31-youve-been-using-some-cloud-services)
  - [3.2. The Deployment Models of the Cloud](#32-the-deployment-models-of-the-cloud)
    - [3.2.1. Private Cloud](#321-private-cloud)
    - [3.2.2. Public Cloud](#322-public-cloud)
    - [3.2.3. Hybrid Cloud](#323-hybrid-cloud)
  - [3.3. The Five Characteristics of Cloud Computing](#33-the-five-characteristics-of-cloud-computing)
  - [3.4. Six Advantages of Cloud Computing](#34-six-advantages-of-cloud-computing)
  - [3.5. Problems solved by the Cloud](#35-problems-solved-by-the-cloud)
  - [3.6. Types of Cloud Computing](#36-types-of-cloud-computing)
  - [3.7. Example of Cloud Computing Types](#37-example-of-cloud-computing-types)
  - [3.8. Pricing of the Cloud - Quick Overview](#38-pricing-of-the-cloud---quick-overview)
  - [3.9. AWS Global Infrastructure](#39-aws-global-infrastructure)
  - [3.10. AWS Regions](#310-aws-regions)
  - [3.11. How to choose an AWS Region?](#311-how-to-choose-an-aws-region)
  - [3.12. AWS Availability Zones](#312-aws-availability-zones)
  - [3.13. AWS Points of Presence (Edge Locations)](#313-aws-points-of-presence-edge-locations)
  - [3.14. Tour of the AWS Console](#314-tour-of-the-aws-console)
  - [3.15. Shared Responsibility Model diagram](#315-shared-responsibility-model-diagram)
  - [3.16. AWS Acceptable Use Policy](#316-aws-acceptable-use-policy)
- [4. IAM - Identity and Access Management and AWS CLI](#4-iam---identity-and-access-management-and-aws-cli)
  - [4.1. IAM - Summary](#41-iam---summary)
- [5. Compute](#5-compute)
- [6. Storage](#6-storage)
- [7. Databases](#7-databases)
- [8. Other Compute Services: ECS, Batch, Lightsail](#8-other-compute-services-ecs-batch-lightsail)
  - [8.1. ECS - Elastic Container Service, Fargate and ECR - Elastic Container Registry](#81-ecs---elastic-container-service-fargate-and-ecr---elastic-container-registry)
  - [8.2. Amazon Lightsail](#82-amazon-lightsail)
  - [8.3. Other Compute - Summary](#83-other-compute---summary)
- [9. Global Infrastructure](#9-global-infrastructure)
  - [9.1. Why make a global application?](#91-why-make-a-global-application)
  - [9.2. Global AWS Infrastructure](#92-global-aws-infrastructure)
  - [9.3. Global Applications in AWS](#93-global-applications-in-aws)
  - [9.4. AWS Outposts](#94-aws-outposts)
  - [9.5. AWS WaveLength](#95-aws-wavelength)
  - [9.6. AWS Local Zones](#96-aws-local-zones)
  - [9.7. Global Applications Architecture](#97-global-applications-architecture)
  - [9.8. Global Applications in AWS - Summary](#98-global-applications-in-aws---summary)
- [10. Application Integration](#10-application-integration)
  - [10.1. Integration - Summary](#101-integration---summary)
- [11. Cloud Monitoring](#11-cloud-monitoring)
  - [11.1. Amazon CloudWatch Logs](#111-amazon-cloudwatch-logs)
  - [11.2. Amazon EventBridge](#112-amazon-eventbridge)
  - [11.3. AWS CloudTrail](#113-aws-cloudtrail)
  - [11.4. AWS X-Ray](#114-aws-x-ray)
  - [11.5. Amazon CodeGuru](#115-amazon-codeguru)
  - [11.6. AWS Health Dashboard](#116-aws-health-dashboard)
  - [11.7. Monitoring Summary](#117-monitoring-summary)
- [12. VPC](#12-vpc)
- [13. Security \& Compliance](#13-security--compliance)
  - [13.1. AWS Shared Responsibility Model](#131-aws-shared-responsibility-model)
    - [13.1.1. Example, for RDS](#1311-example-for-rds)
    - [13.1.2. Example, for S3](#1312-example-for-s3)
  - [13.2. DDOS Protection on AWS](#132-ddos-protection-on-aws)
  - [13.3. Penetration Testing on AWS Cloud](#133-penetration-testing-on-aws-cloud)
  - [13.4. Data at rest vs. Data in transit](#134-data-at-rest-vs-data-in-transit)
  - [13.5. AWS Artifact (not really a service)](#135-aws-artifact-not-really-a-service)
  - [13.6. AWS Config](#136-aws-config)
  - [13.7. Amazon Macie](#137-amazon-macie)
  - [13.8. AWS Security Hub](#138-aws-security-hub)
  - [13.9. Amazon Detective](#139-amazon-detective)
  - [13.10. AWS Abuse](#1310-aws-abuse)
  - [13.11. Root user privileges](#1311-root-user-privileges)
  - [13.12. Summary: Security \& Compliance](#1312-summary-security--compliance)
- [14. Machine Learning](#14-machine-learning)
- [15. Account Management, Billing \& Support](#15-account-management-billing--support)
  - [15.1. AWS Organizations](#151-aws-organizations)
  - [15.2. AWS Control Tower](#152-aws-control-tower)
  - [15.3. Pricing Models in AWS](#153-pricing-models-in-aws)
    - [15.3.1. Free services and free tier in AWS](#1531-free-services-and-free-tier-in-aws)
    - [15.3.2. Compute Pricing - EC2](#1532-compute-pricing---ec2)
    - [15.3.3. Compute Pricing - Lambda \& ECS](#1533-compute-pricing---lambda--ecs)
    - [15.3.4. Storage Pricing - S3](#1534-storage-pricing---s3)
    - [15.3.5. Storage Pricing - EBS](#1535-storage-pricing---ebs)
    - [15.3.6. Database Pricing - RDS](#1536-database-pricing---rds)
    - [15.3.7. Content Delivery - CloudFront](#1537-content-delivery---cloudfront)
      - [15.3.7.1. Networking Costs in AWS per GB - Simplified](#15371-networking-costs-in-aws-per-gb---simplified)
    - [15.3.8. Savings Plan](#1538-savings-plan)
  - [15.4. AWS Compute Optimizer](#154-aws-compute-optimizer)
  - [15.5. Billing and Costing Tools](#155-billing-and-costing-tools)
- [16. AWS Directory Services](#16-aws-directory-services)
- [17. Security, Identity, \& Compliance](#17-security-identity--compliance)
- [18. Other AWS Services](#18-other-aws-services)
  - [18.1. Amazon WorkSpaces](#181-amazon-workspaces)
  - [18.2. Amazon AppStream 2.0](#182-amazon-appstream-20)
    - [18.2.1. Amazon AppStream 2.0 vs WorkSpaces](#1821-amazon-appstream-20-vs-workspaces)
  - [18.3. Amazon Sumerian](#183-amazon-sumerian)
  - [18.4. AWS IoT Core](#184-aws-iot-core)
  - [18.5. Amazon Elastic Transcoder](#185-amazon-elastic-transcoder)
  - [18.6. AWS Device Farm](#186-aws-device-farm)
  - [18.7. AWS Backup](#187-aws-backup)
    - [18.7.1. Disaster Recovery Strategies](#1871-disaster-recovery-strategies)
  - [18.8. AWS Elastic Disaster Recovery (DRS)](#188-aws-elastic-disaster-recovery-drs)
  - [18.9. AWS DataSync](#189-aws-datasync)
  - [18.10. AWS Fault Injection Simulator (FIS)](#1810-aws-fault-injection-simulator-fis)
  - [18.11. AWS CloudSearch](#1811-aws-cloudsearch)
  - [18.12. Amazon SES - Simple Email Service](#1812-amazon-ses---simple-email-service)
- [19. AWS Architecting \& Ecosystem](#19-aws-architecting--ecosystem)
  - [19.1. Well Architected Framework General - Guiding Principles](#191-well-architected-framework-general---guiding-principles)
  - [19.2. AWS Cloud Best Practices - Design Principles](#192-aws-cloud-best-practices---design-principles)
    - [19.2.1. Operational Excellence](#1921-operational-excellence)
      - [19.2.1.1. Operational Excellence AWS Services](#19211-operational-excellence-aws-services)
    - [19.2.2. Security](#1922-security)
      - [19.2.2.1. Security AWS Services](#19221-security-aws-services)
    - [19.2.3. Reliability](#1923-reliability)
      - [19.2.3.1. Reliability AWS Services](#19231-reliability-aws-services)
    - [19.2.4. Performance Efficiency](#1924-performance-efficiency)
      - [19.2.4.1. Performance Efficiency AWS Services](#19241-performance-efficiency-aws-services)
    - [19.2.5. Cost Optimization](#1925-cost-optimization)
      - [19.2.5.1. Cost Optimization AWS Services](#19251-cost-optimization-aws-services)
    - [19.2.6. Sustainability](#1926-sustainability)
      - [19.2.6.1. Sustainability AWS Services](#19261-sustainability-aws-services)
  - [19.3. AWS Right Sizing](#193-aws-right-sizing)
  - [19.4. AWS Ecosystem - Free resources](#194-aws-ecosystem---free-resources)
  - [19.5. AWS Marketplace](#195-aws-marketplace)
  - [19.6. AWS Training](#196-aws-training)
  - [19.7. AWS Professional Services \& Partner Network](#197-aws-professional-services--partner-network)
  - [19.8. AWS Knowledge Center](#198-aws-knowledge-center)
- [20. AWS Cloud Map](#20-aws-cloud-map)
- [21. AWS Limits (Quotas)](#21-aws-limits-quotas)
- [22. Private vs Public IP (IPv4)](#22-private-vs-public-ip-ipv4)
  - [22.1. Private vs Public IP (IPv4) Fundamental Differences](#221-private-vs-public-ip-ipv4-fundamental-differences)
- [23. AWS charges for IPv4 addresses](#23-aws-charges-for-ipv4-addresses)
- [24. Commands AWS CLI](#24-commands-aws-cli)
  - [24.1. DynamoDB](#241-dynamodb)
  - [24.2. S3](#242-s3)
  - [24.3. Lambda](#243-lambda)
  - [24.4. ECS](#244-ecs)
  - [24.5. Systems Manager](#245-systems-manager)

# 1. Credits

- Much of this content extracted from AWS and Stephane Maarek's courses, **for personal study**, at several points has personal considerations and comments.

# 2. Traditionally, how to build infrastructure

## 2.1. How websites work

- Clients have IP addresses.
- Servers have IP addresses.

## 2.2. What is a server composed of?

- Compute: CPU.
- Memory: RAM.
- Storage: Data.
- Database: Store data in a structured way.
- Network: Routers, switch, DNS server.

## 2.3. IT Terminology

- Network: cables, routers and servers connected with each other.
- Router: A networking device that forwards data packets between computer
  networks. They know where to send your packets on the internet!
- Switch: Takes a packet and send it to the correct server / client on your network.

## 2.4. Problems with traditional IT approach

- Pay for the rent for the data center.
- Pay for power supply, cooling, maintenance.
- Adding and replacing hardware takes time.
- Scaling is limited.
- Hire 24/7 team to monitor the infrastructure.
- How to deal with disasters? (earthquake, power shutdown, fire...).
- Can we externalize all this? **CLOUD**

# 3. What is Cloud Computing?

- Cloud computing is the on-demand delivery of compute power, database storage, applications, and other IT resources.
- Through a cloud services platform with pay-as-you-go pricing.
- We can provision exactly the right type and size of computing resources you need.
- We can access as many resources as you need, almost instantly.
- Simple way to access servers, storage, databases and a set of application services.
- Amazon Web Services owns and maintains the network-connected hardware required for these application services, while you provision and use what you need via a web application.

## 3.1. You've been using some Cloud services

- **Gmail**
  - E-mail cloud service.
  - Pay for ONLY your emails stored (no infrastructure, etc.).
- **Dropbox**
  - Cloud Storage Service.
  - Originally built on AWS.
- **Netflix**
  - Built on AWS
  - Video on Demand

## 3.2. The Deployment Models of the Cloud

### 3.2.1. Private Cloud

- Cloud services used by a single organization, not exposed to the public.
- Complete control.
- Security for sensitive applications.
- Meet specific business needs.

### 3.2.2. Public Cloud

- Cloud resources owned and operated by a third-party cloud service provider delivered over the Internet.

### 3.2.3. Hybrid Cloud

- Keep some servers on premises and extend some capabilities to the Cloud.
- Control over sensitive assets in your private infrastructure.
- Flexibility and cost-effectiveness of the public cloud.

## 3.3. The Five Characteristics of Cloud Computing

- **On-demand self service**
  - Users can provision resources and use them without human interaction from the service provider.
- **Broad network access**
  - Resources available over the network, and can be accessed by diverse client platforms.
- **Multi-tenancy and resource pooling**
  - Multiple customers can share the same infrastructure and applications with security and privacy.
  - Multiple customers are serviced from the same physical resources.
- **Rapid elasticity and scalability**
  - Automatically and quickly acquire and dispose resources when needed.
  - Quickly and easily scale based on demand.
- **Measured service**
  - Usage is measured, users pay correctly for what they have used

## 3.4. Six Advantages of Cloud Computing

- Trade capital expense (CAPEX) for operational expense (OPEX)
  - **Pay On-Demand:** Don't own hardware.
  - Reduced Total Cost of Ownership (TCO) & Operational Expense (OPEX).
- Benefit from massive economies of scale
  - Prices are reduced as AWS is more efficient due to large scale.
- Stop guessing capacity
  - Scale based on actual measured usage.
- Increase speed and agility.
- Stop spending money running and maintaining data centers.
- Go global in minutes: leverage the AWS global infrastructure.

## 3.5. Problems solved by the Cloud

- **Flexibility:** Change resource types when needed.
- **Cost-Effectiveness:** Pay as you go, for what you use.
- **Scalability:** Accommodate larger loads by making hardware stronger or adding additional nodes.
- **Elasticity:** Ability to scale-out and scale-in when needed.
- **High-availability and fault-tolerance:** Build across data centers.
- **Agility:** Rapidly develop, test and launch software applications.

## 3.6. Types of Cloud Computing

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

## 3.7. Example of Cloud Computing Types

- Infrastructure as a Service
  - Amazon EC2 (on AWS)
  - GCP, Azure, Rackspace, Digital Ocean, Linode
- Platform as a Service:
  - Elastic Beanstalk (on AWS)
  - Heroku, Google App Engine (GCP), Windows Azure (Microsoft)
- Software as a Service:
  - Many AWS services (ex: Rekognition for Machine Learning)
  - Google Apps (Gmail), Dropbox, Zoom

## 3.8. Pricing of the Cloud - Quick Overview

- AWS has 3 pricing fundamentals, following the pay-as-you-go pricing
  model
  - **Compute**
    - Pay for compute time
  - **Storage**
    - Pay for data stored in the Cloud
  - Data transfer **OUT** of the Cloud:
    - Data transfer IN is free
  - Solves the expensive issue of traditional IT

## 3.9. AWS Global Infrastructure

- AWS Regions.
- AWS Availability Zones.
- AWS Data Centers.
- AWS Edge Locations / Points of Presence.
- https://infrastructure.aws/

## 3.10. AWS Regions

- AWS has Regions all around the world.
- Names can be `us-east-1`, `eu-west-3`...
- A region is a cluster of data centers.
- Most AWS services are region-scoped.

## 3.11. How to choose an AWS Region?

- **Compliance** with data governance and legal requirements: data never leaves a region without your explicit permission.
- **Proximity** to customers: reduced latency.
- **Available services** within a Region: new services and new features aren't available in every Region.
- **Pricing** pricing varies region to region and is transparent in the service pricing page.
- **Capacity is unlimited in the cloud, you do not need to worry about it. The 4 points of considerations when choosing an AWS Region are: compliance with data governance and legal requirements, proximity to customers, available services and features within a Region, and pricing.**

## 3.12. AWS Availability Zones

- Each region has many availability zones (usually 3, min is 2, max is 6).
- Example:
  - ap-southeast-2a
  - ap-southeast-2b
  - ap-southeast-2c
- Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity.
- They're separate from each other, so that they're isolated from disasters.
- They're connected with high bandwidth, ultra-low latency networking.

## 3.13. AWS Points of Presence (Edge Locations)

- Amazon has 216 Points of Presence (205 Edge Locations & 11 Regional Caches) in 84 cities across 42 countries.
- Content is delivered to end users with lower latency.

## 3.14. Tour of the AWS Console

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

## 3.15. Shared Responsibility Model diagram

- AWS = RESPONSIBILITY FOR THE SECURITY OF THE CLOUD
- CUSTOMER = RESPONSIBILITY FOR THE SECURITY IN THE CLOUD
- https://aws.amazon.com/compliance/shared-responsibility-model/

## 3.16. AWS Acceptable Use Policy

- https://aws.amazon.com/aup/
- No Illegal, Harmful, or Offensive Use or Content
- No Security Violations
- No Network Abuse
- No E-Mail or Other Message Abuse

# 4. IAM - Identity and Access Management and AWS CLI

[IAM - Identity and Access Management and AWS CLI](/Security,%20Identity,%20&%20Compliance/AWS%20IAM.md)

## 4.1. IAM - Summary

- **Users:** Mapped to a physical user, has a password for AWS Console.
- **Groups:** Contains users only.
- **Policies:** JSON document that outlines permissions for users or groups.
- **Roles:** For EC2 instances or AWS services.
- **Security:** MFA + Password Policy.
- **AWS CLI:** Manage your AWS services using the command-line.
- **AWS SDK:** Manage your AWS services using a programming language.
- **Access Keys:** Access AWS using the CLI or SDK.
- **Audit:** IAM Credential Reports & IAM Access Advisor.

# 5. Compute

[Compute](Compute/README.md)

# 6. Storage

[Storage](Storage/README.md)

# 7. Databases

[Databases](Database/README.md)

# 8. Other Compute Services: ECS, Batch, Lightsail

## 8.1. ECS - Elastic Container Service, Fargate and ECR - Elastic Container Registry

[AWS ECS, Fargate and ECR](/Containers/Amazon%20ECS.md)

## 8.2. Amazon Lightsail

- **Amazon Lightsail is designed to be the easiest way to launch and manage a virtual private server with AWS. Lightsail plans include everything you need to jumpstart your project - a virtual machine, SSD- based storage, data transfer, DNS management, and a static IP address - for a low, predictable price. It can be used to create a simple web application, a website or a dev/test environment.**
- Virtual servers, storage, databases, and networking.
- Low & predictable pricing.
- Simpler alternative to using EC2, RDS, ELB, EBS, Route 53...
- Great for people with little cloud experience!
- Can setup notifications and monitoring of your Lightsail resources.
- **Use cases**
  - Simple web applications (has templates for LAMP, Nginx, MEAN, Node.js...).
  - Websites (templates for WordPress, Magento, Plesk, Joomla).
  - Dev / Test environment.
- Has high availability but no auto-scaling, limited AWS integrations.

## 8.3. Other Compute - Summary

- Docker: container technology to run applications.
- ECS: run Docker containers on EC2 instances.
- Fargate:
  - Run Docker containers without provisioning the infrastructure.
  - Serverless offering (no EC2 instances).
- ECR: Private Docker Images Repository.
- Lightsail: predictable & low pricing for simple application & DB stacks.

# 9. Global Infrastructure

## 9.1. Why make a global application?

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

## 9.2. Global AWS Infrastructure

- **Regions:** For deploying applications and infrastructure
- **Availability Zones:** Made of multiple data centers
- **Edge Locations (Points of Presence):** for content delivery as close as possible to users

## 9.3. Global Applications in AWS

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

## 9.4. AWS Outposts

[AWS Outposts](/Compute/AWS%20Outposts.md)

## 9.5. AWS WaveLength

- **AWS Wavelength is an AWS Infrastructure offering optimized for mobile edge computing applications. Wavelength combines the high bandwidth and ultra-low latency of 5G networks with AWS compute and storage services to enable developers to innovate and build a whole new class of applications.**
- WaveLength Zones are infrastructure deployments embedded within the telecommunications providers datacenters at the edge of the 5G networks.
- Brings AWS services to the edge of the 5G networks.
- Example: EC2, EBS, VPC...
- Ultra-low latency applications through 5G networks.
- Traffic doesn't leave the Communication Service Provider's (CSP) network.
- High-bandwidth and secure connection to the parent AWS Region.
- No additional charges or service agreements.
- **Use cases:** Smart Cities, ML-assisted diagnostics, Connected Vehicles, Interactive Live Video Streams, AR/VR, Real-time Gaming, ...

## 9.6. AWS Local Zones

- Places AWS compute, storage, database, and other selected AWS services closer to end users to run latency-sensitive applications
- Extend your VPC to more locations - "Extension of an AWS Region"
- Compatible with EC2, RDS, ECS, EBS, ElastiCache, Direct Connect ...
- Example:
  - AWS Region: N. Virginia (us-east-1)
  - AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami, ...

## 9.7. Global Applications Architecture

- Single Region, Single AZ.
- Single Region, Multi AZ.
- Multi Region, Active-Passive.
- Multi Region, Active-Active.

## 9.8. Global Applications in AWS - Summary

- AWS Outposts
  - Deploy Outposts Racks in your own Data Centers to extend AWS services
- AWS WaveLength
  - Brings AWS services to the edge of the 5G networks
  - Ultra-low latency applications
- AWS Local Zones
  - Bring AWS resources (compute, database, storage, ...) closer to your users
  - Good for latency-sensitive applications

# 10. Application Integration

[Application Integration](/Application%20Integration/README.md)

## 10.1. Integration - Summary

# 11. Cloud Monitoring

## 11.1. Amazon CloudWatch Logs

[Amazon CloudWatch](Management%20&%20Governance/Amazon%20CloudWatch.md)

## 11.2. Amazon EventBridge

[Amazon EventBridge](Application%20Integration/Amazon%20EventBridge.md)

## 11.3. AWS CloudTrail

- **Is a web service that records activity made on your account and delivers log files to your Amazon S3 bucket. AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account.**
- **Can record the history of events/API calls made within you AWS account, which will help determine who or what deleted the resource. You should investigate it first.** [AWS CloudTrail](Management%20&%20Governance/AWS%20CloudTrail.md)

## 11.4. AWS X-Ray

- **AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture.** [AWS X-Ray](Developer%20Tools/AWS%20X-Ray.md)

## 11.5. Amazon CodeGuru

[Amazon CodeGuru](Developer%20Tools/AWS%20CICD.md)

## 11.6. AWS Health Dashboard

[AWS Health Dashboard](Management%20&%20Governance/AWS%20Health%20Dashboard.md)

## 11.7. Monitoring Summary

- X-Ray: trace requests made through your distributed applications.
- Amazon CodeGuru: automated code reviews and application performance recommendation.

# 12. VPC

![Amazon VPC](Networking%20&%20Content%20Delivery/Amazon%20VPC.md)

# 13. Security & Compliance

## 13.1. AWS Shared Responsibility Model

- AWS responsibility - Security of the Cloud
  - Protecting infrastructure (hardware, software, facilities, and networking) that runs all the AWS services.
  - Managed services like S3, DynamoDB, RDS, etc.
- Customer responsibility - Security in the Cloud
  - For EC2 instance, customer is responsible for management of the guest OS (including security patches and updates), firewall & network configuration, IAM.
  - Encrypting application data.
- Shared controls:
  - **AWS is responsible for patching and fixing flaws within the infrastructure, but customers are responsible for patching their guest OS and applications. Shared Controls also includes Configuration Management, and Awareness and Training.**
  - Patch Management, Configuration Management, Awareness & Training.

### 13.1.1. Example, for RDS

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

### 13.1.2. Example, for S3

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

## 13.2. DDOS Protection on AWS

- [AWS WAF & Shield](/Security,%20Identity,%20&%20Compliance/AWS%20WAF.md)

## 13.3. Penetration Testing on AWS Cloud

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

## 13.4. Data at rest vs. Data in transit

- **At rest:** data stored or archived on a device.
  - On a hard disk, on a RDS instance, in S3 Glacier Deep Archive, etc.
- **In transit (in motion):** data being moved from one location to another.
  - Transfer from on-premises to AWS, EC2 to DynamoDB, etc.
  - Means data transferred on the network.
- We want to encrypt data in both states to protect it!
- For this we leverage **encryption keys**.

## 13.5. AWS Artifact (not really a service)

- **AWS Artifact is your go-to, central resource for compliance-related information that matters to you.**
- Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements.
- **Artifact Reports** - Allows you to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports.
- **Artifact Agreements** - Allows you to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in your organization.
- Can be used to support internal audit or compliance.

## 13.6. AWS Config

- **AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time.** [AWS Config](Management%20&%20Governance/AWS%20Config.md)

## 13.7. Amazon Macie

[Amazon Macie](/Security,%20Identity,%20&%20Compliance/Amazon%20Macie.md)

## 13.8. AWS Security Hub

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

## 13.9. Amazon Detective

- GuardDuty, Macie, and Security Hub are used to identify potential
  security issues, or findings.
- Sometimes security findings require deeper analysis to isolate the root cause and take action - it's a complex process.
- Amazon Detective **analyzes, investigates, and quickly identifies the root cause of security issues or suspicious activities using ML and graphs**.
- **Automatically collects and processes events** from VPC Flow Logs, CloudTrail, and GuardDuty to create a unified view.
- Produces visualizations with details and context to get to the root cause.

## 13.10. AWS Abuse

- **Report suspected AWS resources used for abusive or illegal purposes**.
- Abusive & prohibited behaviors are:
  - **Spam** - receving undesired emails from AWS-owned IP address, websites & forums spammed by AWS resources.
  - **Port scanning**- sending packets to your ports to discover the unsecured ones.
  - **DoS or DDoS attacks** - AWS-owned IP addresses attempting to overwhlem or crash your servers/softwares.
  - **Intrusion attempts** - logging in on your resources.
  - **Hosting objectionable** or copyrighted content - distributing illegal or copyrighted content without consent.
  - **Distributing malware** - AWS resources distributing softwares to harm computers or machines.
- Contact the AWS Abuse team: AWS abuse form, or abuse@amazonaws.com.

## 13.11. Root user privileges

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

## 13.12. Summary: Security & Compliance

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

# 14. Machine Learning

[Machine Learning](Machine%20Learning/README.md)

# 15. Account Management, Billing & Support

## 15.1. AWS Organizations

[AWS Organizations](AWS%20Organizations.md)

## 15.2. AWS Control Tower

- **AWS Control Tower offers the easiest way to set up and govern a new, secure, multi-account AWS environment. It establishes a landing zone that is based on best-practices blueprints, and enables governance using guardrails you can choose from a pre-packaged list.** [AWS Control Tower](AWS%20Control%20Tower.md)

## 15.3. Pricing Models in AWS

- AWS has 4 pricing models:
  - **Pay as you go:** pay for what you use, remain agile, responsive, meet scale demands.
  - **Save when you reserve:** minimize risks, predictably manage budgets, comply with long-terms requirements.
    - Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElastiCache Reserved Nodes, RDS Reserved Instance, Redshift Reserved Nodes.
  - **Pay less by using more:** volume-based discounts
  - **Pay less as AWS grows**

### 15.3.1. Free services and free tier in AWS

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

### 15.3.2. Compute Pricing - EC2

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

### 15.3.3. Compute Pricing - Lambda & ECS

- Lambda:
  - Pay per call.
  - Pay per duration.
- ECS:
  - EC2 Launch Type Model: No additional fees, you pay for AWS resources stored and created in your application.
- Fargate:
  - Fargate Launch Type Model: Pay for vCPU and memory resources allocated to your applications in your containers.

### 15.3.4. Storage Pricing - S3

- Storage class: S3 Standard, S3 Infrequent Access, S3 One-Zone IA, S3 Intelligent Tiering, S3 Glacier and S3 Glacier Deep Archive.
- Number and size of objects: Price can be tiered (based on volume).
- Number and type of requests.
- Data transfer OUT of the S3 region.
- S3 Transfer Acceleration.
- Lifecycle transitions.
- Similar service: EFS (pay per use, has infrequent access & lifecycle rules).

### 15.3.5. Storage Pricing - EBS

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

### 15.3.6. Database Pricing - RDS

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

### 15.3.7. Content Delivery - CloudFront

- Pricing is different across different geographic regions.
- Aggregated for each edge location, then applied to your bill.
- Data Transfer Out (volume discount).
- Number of HTTP/HTTPS requests.

#### 15.3.7.1. Networking Costs in AWS per GB - Simplified

- Use Private IP instead of Public IP for good savings and better network performance.
- Use same AZ for maximum savings (at the cost of high availability).

### 15.3.8. Savings Plan

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

## 15.4. AWS Compute Optimizer

- Reduce costs and improve performance by recommending optimal AWS resources for your workloads.
- Helps you choose optimal configurations and right - size your workloads (over/under provisioned).
- Uses Machine Learning to analyze your resources, configurations and their utilization CloudWatch metrics.
- **Supported resources**
  - EC2 instances.
  - EC2 Auto Scaling Groups.
  - EBS volumes.
  - Lambda functions.
- Lower your costs by up to 25%.
- Recommendations can be exported to S3.

## 15.5. Billing and Costing Tools

[Billing and Costing Tools](Cloud%20Financial%20Management/README.md)

# 16. AWS Directory Services

[AWS Directory Services](/Security,%20Identity,%20&%20Compliance/AWS%20Directory%20Services.md)

# 17. Security, Identity, & Compliance

[Security, Identity, & Compliance](/Security,%20Identity,%20&%20Compliance/README.md)

# 18. Other AWS Services

## 18.1. Amazon WorkSpaces

- **Amazon WorkSpaces is a fully managed, secure cloud desktop service. You can use Amazon WorkSpaces to provision either Windows or Linux desktops in just a few minutes and quickly scale to provide thousands of desktops to workers across the globe.**
- Managed Desktop as a Service (DaaS) solution to easily provision Windows or Linux desktops.
- Great to eliminate management of on-premise VDI (Virtual Desktop Infrastructure).
- Fast and quickly scalable to thousands of users.
- Secured data - integrates with KMS.
- Pay-as-you-go service with monthly or hourly rates.

## 18.2. Amazon AppStream 2.0

- **Amazon AppStream 2.0 is a fully managed non-persistent application and desktop streaming service that provides users instant access to their desktop applications from anywhere.**
- Desktop Application Streaming Service.
- Deliver to any computer, without acquiring, provisioning infrastructure.
- The application is delivered from within a web browser.

### 18.2.1. Amazon AppStream 2.0 vs WorkSpaces

- **Workspaces:**
  - Fully managed VDI and desktop available.
  - The users connect to the VDI and open native or WAM applications.
  - Workspaces are on-demand or always on.
- **AppStream 2.0:**
  - Stream a desktop application to web browsers (no need to connect to a VDI).
  - Works with any device (that has a web browser).
  - Allow to configure an instance type per application type (CPU, RAM, GPU).

## 18.3. Amazon Sumerian

- **Amazon Sumerian is a managed service that lets you create and run 3D, Augmented Reality (AR) and Virtual Reality (VR) applications. You can build immersive and interactive scenes that run on AR and VR, mobile devices, and your web browser.**
- Create and run virtual reality (VR), augmented reality (AR), and 3D applications.
- Can be used to quickly create 3D models with animations.
- Ready-to-use templates and assets - no programming or 3D expertise required.
- Accessible via a web-browser URLs or on popular hardware for AR/VR.

## 18.4. AWS IoT Core

- **AWS IoT Core, is serverless and lets you connect billions of devices to the AWS Cloud, lets you securely connect IoT devices to the AWS Cloud and other devices without the need to provision or manage servers.**
- IoT stands for "Internet of Things" - the network of internet-connected devices that are able to collect and transfer data.
- AWS IoT Core allows you to easily connect IoT devices to the AWS Cloud.
- Serverless, secure & scalable to billions of devices and trillions of messages.
- Your applications can communicate with your devices even when they aren't connected.
- Integrates with a lot of AWS services (Lambda, S3, SageMaker, etc.).
- Build IoT applications that gather, process, analyze, and act on data.

## 18.5. Amazon Elastic Transcoder

- **Amazon Elastic Transcoder is media transcoding in the cloud. It is used to convert media files from their source format into versions that will play back on devices like smartphones, tablets, and PCs.**
- Elastic Transcoder is used to convert media files stored in S3 into media files in the formats required by consumer playback devices (phones etc..)
- Benefits:
  - Easy to use.
  - Highly scalable - can handle large volumes of media files and large file sizes.
  - Cost effective - duration-based pricing model.
  - Fully managed & secure, pay for what you use.

## 18.6. AWS Device Farm

- **AWS Device Farm is an application testing service that lets you improve the quality of your web and mobile apps by testing them across an extensive range of desktop browsers and real mobile devices; without having to provision and manage any testing infrastructure.**
- Fully-managed service that tests your web and mobile apps against desktop browsers, real mobile devices, and tablets.
- Run tests concurrently on multiple devices (speed up execution).
- Ability to configure device settings (GPS, language, Wi-Fi, Bluetooth, ...).

## 18.7. AWS Backup

[AWS Backup](/Storage/AWS%20Backup.md)

### 18.7.1. Disaster Recovery Strategies

[Disaster Recovery Strategies](Disaster%20Recovery.md)

## 18.8. AWS Elastic Disaster Recovery (DRS)

- Used to be named "CloudEndure Disaster Recovery".
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS.
- Example: protect your most critical databases (including Oracle, MySQL, and SQL Server), enterprise apps (SAP), protect your data from ransomware attacks, ...
- Continuous block-level replication for your servers.

## 18.9. AWS DataSync

[text](/Migration%20&%20Transfer/AWS%20DataSync.md)

## 18.10. AWS Fault Injection Simulator (FIS)

- A fully managed service for running fault injection experiments on AWS workloads.
- Based on **Chaos Engineering** - stressing an application by creating disruptive events (e.g., sudden increase in CPU or memory), observing how the system responds, and implementing improvements.
- Helps you uncover hidden bugs and performance bottlenecks.
- Supports the following AWS services: EC2, ECS, EKS, RDS...
- Use pre-built templates that generate the desired disruptions.

## 18.11. AWS CloudSearch

- Amazon CloudSearch is a managed service in the AWS Cloud that makes it simple and cost-effective to set up, manage, and scale a search solution for your website or application.
- Amazon CloudSearch supports 34 languages and popular search features such as highlighting, autocomplete, and geospatial search.

## 18.12. Amazon SES - Simple Email Service

- **Fully managed service to send emails securely, globally and at scale.** [AWS SES](/Business%20Applications/Amazon%20SES.md)

# 19. AWS Architecting & Ecosystem

## 19.1. Well Architected Framework General - Guiding Principles

[White Papers & Architectures](White%20Papers%20&%20Architectures.md)

## 19.2. AWS Cloud Best Practices - Design Principles

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

### 19.2.1. Operational Excellence

- Includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures.
- Design Principles:
  - **Perform operations as code** - Infrastructure as code.
  - **Annotate documentation** - Automate the creation of annotated documentation after every build.
  - **Make frequent, small, reversible changes** - So that in case of any failure, you can reverse it.
  - **Refine operations procedures frequently** - And ensure that team members are familiar with it.
  - **Anticipate failure**.
  - **Learn from all operational failures**.

#### 19.2.1.1. Operational Excellence AWS Services

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

### 19.2.2. Security

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

#### 19.2.2.1. Security AWS Services

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

### 19.2.3. Reliability

- Ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.
- Design Principles:
  - **Test recovery procedures** - Use automation to simulate different failures or to recreate scenarios that led to failures before.
  - **Automatically recover from failure** - Anticipate and remediate failures before they occur.
  - **Scale horizontally to increase aggregate system availability** - Distribute requests across multiple, smaller resources to ensure that they don't share a common point of failure.
  - **Stop guessing capacity** - Maintain the optimal level to satisfy demand without over or under provisioning - Use Auto Scaling.
  - **Manage change in automation** - Use automation to make changes to infrastructure.

#### 19.2.3.1. Reliability AWS Services

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

### 19.2.4. Performance Efficiency

- Includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve.
- Design Principles:
  - **Democratize advanced technologies** - Advance technologies become services and hence you can focus more on product development.
  - **Go global in minutes** - Easy deployment in multiple regions.
  - **Use serverless architectures** - Avoid burden of managing servers.
  - **Experiment more often** - Easy to carry out comparative testing.
  - **Mechanical sympathy** - Be aware of all AWS services.

#### 19.2.4.1. Performance Efficiency AWS Services

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

### 19.2.5. Cost Optimization

- Includes the ability to run systems to deliver business value at the lowest price point.
- Design Principles:
  - **Adopt a consumption mode** - Pay only for what you use.
  - **Measure overall efficiency** - Use CloudWatch.
  - **Stop spending money on data center operations** - AWS does the infrastructure part and enables customer to focus on organization projects.
  - **Analyze and attribute expenditure** - Accurate identification of system usage and costs, helps measure return on investment (ROI)- Make sure to use tags.
  - **Use managed and application level services to reduce cost of ownership** - As managed services operate at cloud scale, they can offer a lower cost per transaction or service.

#### 19.2.5.1. Cost Optimization AWS Services

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

### 19.2.6. Sustainability

- The sustainability pillar focuses on minimizing the environmental impacts of running cloud workloads.
- Design Principles:
  - **Understand your impact** - establish performance indicators, evaluate improvements.
  - **Establish sustainability goals** - Set long-term goals for each workload, model return on investment (ROI).
  - **Maximize utilization** - Right size each workload to maximize the energy efficiency of the underlying hardware and minimize idle resources.
  - **Anticipate and adopt new, more efficient hardware and software offerings** - and design for flexibility to adopt new technologies over time.
  - **Use managed services** - Shared services reduce the amount of infrastructure; Managed services help automate sustainability best practices as moving infrequent accessed data to cold storage and adjusting compute capacity.
  - **Reduce the downstream impact of your cloud workloads** - Reduce the amount of energy or resources required to use your services and reduce the need for your customers to upgrade their devices.

#### 19.2.6.1. Sustainability AWS Services

- EC2 Auto Scaling, Serverless Offering (Lambda, Fargate).
- Cost Explorer, AWS Graviton 2, EC2 T instances, @Spot Instances.
- EFS-IA, Amazon S3 Glacier, EBS Cold HDD volumes.
- S3 Lifecycle Configurations, S3 Intelligent Tiering.
- Amazon Data Lifecycle Manager.
- Read Local, Write Global: RDS Read Replicas, Aurora Global DB, DynamoDB Global Table, CloudFront.

## 19.3. AWS Right Sizing

- EC2 has many instance types, but choosing the most powerful instance type isn't the best choice, because the cloud is **elastic**.
- Right sizing is the process of matching instance types and sizes to your workload performance and capacity requirements **at the lowest possible cost**.
- **Scaling up is easy so always start small**.
- It's also the process of looking at deployed instances and identifying opportunities to eliminate or downsize without compromising capacity or other requirements, which results in lower costs.
- It's important to Right Size...
  - **Before a Cloud Migration**.
  - **Continuously after the cloud onboarding process (requirements change over time)**.
- CloudWatch, Cost Explorer, Trusted Advisor, 3rd party tools can help.

## 19.4. AWS Ecosystem - Free resources

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

## 19.5. AWS Marketplace

- Digital catalog with thousands of software listings from **independent software vendors** (3rd party).
- Example:
  - Custom AMI (custom OS, firewalls, technical solutions...).
  - CloudFormation templates.
  - Software as a Service.
  - Containers.
- If you buy through the AWS Marketplace, it goes into your AWS bill.
- You can **sell your own solutions** on the AWS Marketplace.

## 19.6. AWS Training

- AWS Digital (online) and Classroom Training (in-person or virtual).
- AWS Private Training (for your organization).
- Training and Certification for the U.S Government.
- Training and Certification for the Enterprise.
- AWS Academy: helps universities teach AWS.

## 19.7. AWS Professional Services & Partner Network

- The AWS Professional Services organization is a global team of experts
- They work alongside your team and a chosen member of the APN
- APN = AWS Partner Network
- APN Technology Partners: providing hardware, connectivity, and software
- APN Consulting Partners: professional services firm to help build on AWS
- APN Training Partners: find who can help you learn AWS
- AWS Competency Program: AWS Competencies are granted to APN Partners who have demonstrated technical proficiency and proven
  customer success in specialized solution areas.
- AWS Navigate Program: help Partners become better Partners

## 19.8. AWS Knowledge Center

- Contains the most frequent & common questions and requests
  - https://aws.amazon.com/premiumsupport/knowledge-center/

# 20. AWS Cloud Map

- A fully managed resource discovery service.
- Creates a map of the backend services/resources that your applications depend on
- You register your application components, their locations, attributes, and health status with AWS Cloud Map.
- Integrated health checking (stop sending traffic to unhealthy endpoints).
- Your applications can query AWS Cloud Map using AWS SDK, API, or DNS.

# 21. AWS Limits (Quotas)

[AWS Limits](AWS%20Limits.md)

# 22. Private vs Public IP (IPv4)

- Networking has two sorts of IPs. IPv4 and IPv6:
  - IPv4: 1.160.10.240
  - IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
- IPv4 is still the most common format used online.
- IPv6 is newer and solves problems for the Internet of Things (IoT).
- IPv4 allows for **3.7 billion** different addresses in the public space.
- IPv4: [0-255].[0-255].[0-255].[0-255].

## 22.1. Private vs Public IP (IPv4) Fundamental Differences

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

# 23. AWS charges for IPv4 addresses

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

# 24. Commands AWS CLI

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

## 24.1. DynamoDB

- List all itens of table (Projection expression)
  - aws dynamodb scan --table-name `<table_name>`
  - aws dynamodb scan --table-name `<table_name>` --page-size 1
  - aws dynamodb scan --table-name `<table_name>` --max-items 1
  - aws dynamodb scan --table-name `<table_name>` --projection-expression "`<attribute_fields_or_columns>`" # Filter attributes
- List all content of table (F ilter expression)
  - aws dynamodb scan --table-name DemoTTL --filter-expression "`<attribute_fields_or_columns>` = :u" --expression-attribute-values '{":u": {"S":"`<content>`"}}'

## 24.2. S3

- List all itens of bucket with pagination
  - aws s3api list-objects --bucket `<bucket_name>` --page-size 100 --max-items 5

## 24.3. Lambda

- List all funcions
  - aws lambda list-functions
- Invoke a synchronous or asynchronous lambda function.
  - aws lambda invoke --function-name `<lambda_name>` --invocation-type `<invocation_type>` response.json # `invocation_type` like: `Event` or `RequestResponse`

## 24.4. ECS

- There is no option to delete a task definition on the AWS console. But, you can deregister (delete) a task definition by executing the following command.
  - aws ecs deregister-task-definition --task-definition `<name_task_definition:"revision>`

## 24.5. Systems Manager

- Create parameter
  - aws ssm put-parameter --name myEC2TypeDev --type String --value "t2.small"
