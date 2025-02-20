# Compute<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. EC2 - Elastic Compute Cloud](#1-ec2---elastic-compute-cloud)
  - [1.1. EC2 - Summary](#11-ec2---summary)
- [2. AWS EC2 EBS Elastic Block Storage](#2-aws-ec2-ebs-elastic-block-storage)
  - [2.1. EC2 EBS Summary](#21-ec2-ebs-summary)
- [3. Scalability and High Availability](#3-scalability-and-high-availability)
  - [3.1. Vertical Scalability](#31-vertical-scalability)
  - [3.2. Horizontal Scalability](#32-horizontal-scalability)
  - [3.3. High Availability](#33-high-availability)
  - [3.4. High Availability and Scalability for EC2](#34-high-availability-and-scalability-for-ec2)
  - [3.5. Scalability vs Elasticity (vs Agility)](#35-scalability-vs-elasticity-vs-agility)
  - [3.6. AWS ELB - Elastic Load Balancing](#36-aws-elb---elastic-load-balancing)
  - [3.7. AWS ASG - Auto Scaling Groups](#37-aws-asg---auto-scaling-groups)
  - [3.8. Multi AZ in AWS](#38-multi-az-in-aws)
    - [3.8.1. ELB and ASG - Summary](#381-elb-and-asg---summary)
- [4. AWS Elastic Beanstalk](#4-aws-elastic-beanstalk)
  - [4.1. Elastic Beanstalk Summary:](#41-elastic-beanstalk-summary)
- [5. Lambda](#5-lambda)
  - [5.1. Lambda Summary](#51-lambda-summary)

# 1. EC2 - Elastic Compute Cloud

[AWS EC2](Amazon%20EC2.md)

## 1.1. EC2 - Summary

- EC2 Instance: AMI (OS) + Instance Size (CPU + RAM) + Storage + security groups + EC2 User Data.
- Security Groups: Firewall attached to the EC2 instance.
- EC2 User Data: Script launched at the first start of an instance.
- SSH: start a terminal into our EC2 Instances (port 22).
- EC2 Instance Role: link to IAM roles.
- Purchasing Options: On-Demand, Spot, Reserved (Standard + Convertible + Scheduled), Dedicated Host, Dedicated Instance.

# 2. AWS EC2 EBS Elastic Block Storage

[AWS EBS](Amazon%20EC2%20-%20EBS.md)

## 2.1. EC2 EBS Summary

- EBS volumes:
  - Network drives attached to one EC2 instance at a time.
  - Mapped to an Availability Zones.
  - Can use EBS Snapshots for backups / transferring EBS volumes across AZ.
- AMI: create ready-to-use EC2 instances with our customizations.
- EC2 Image Builder: automatically build, test and distribute AMIs.
- EC2 Instance Store:
  - High performance hardware disk attached to our EC2 instance.
  - Lost if our instance is stopped / terminated.
- EFS: network file system, can be attached to 100s of instances in a region.
- EFS-IA: cost-optimized storage class for infrequent accessed files.
- FSx for Windows: Network File System for Windows servers.
- FSx for Lustre: High Performance Computing Linux file system.

# 3. Scalability and High Availability

- Scalability means that an application / system can handle greater loads by adapting.
- There are two kinds of scalability:
  - Vertical Scalability.
  - Horizontal Scalability (= elasticity).
- **Scalability is linked but different to High Availability.**

## 3.1. Vertical Scalability

- Vertical Scalability means increasing the size of the instance.
- For example, your application runs on a t2.micro.
  - Scaling that application vertically means running it on a t2.large.
- Vertical scalability is very common for **NON** distributed systems, such as a database.
- RDS, ElastiCache are services that can scale vertically.
- There's usually a limit to how much you can vertically scale (hardware limit).

## 3.2. Horizontal Scalability

- Horizontal Scalability means increasing the number of instances / systems for your application.
- Horizontal scaling implies distributed systems.
- This is very common for web applications / modern applications.
- It's easy to horizontally scale thanks the cloud offerings such as Amazon EC2.

## 3.3. High Availability

- High Availability usually goes **hand in hand with Horizontal Scaling**.
- High availability means running your application / system in at least 2 data centers (Availability Zones).
- The goal of high availability is to survive a data center loss (disaster).
- The high availability can be passive (for RDS Multi AZ for example).
- The high availability can be active (for horizontal scaling).

## 3.4. High Availability and Scalability for EC2

- **Vertical Scaling:** Increase instance size (= scale up / down).
  - From: t2.nano - 0.5G of RAM, 1 vCPU.
  - To: u-12tb1.metal - 12.3 TB of RAM, 448 vCPUs.
- **Horizontal Scaling:** Increase number of instances (= scale out / in).
  - Auto Scaling Group.
  - Load Balancer.
- **High Availability:** Run instances for the same application across multi AZ.
  - Auto Scaling Group multi AZ.
  - Load Balancer multi AZ.

## 3.5. Scalability vs Elasticity (vs Agility)

- **Scalability:** Ability to accommodate a larger load by making the hardware stronger (scale up), or by adding nodes (scale out).
- **Elasticity:** Once a system is scalable, elasticity means that there will be some "auto-scaling" so that the system can scale based on the load.
  - This is "cloud-friendly": pay-per-use, match demand, optimize costs.
- **Agility:** (not related to scalability - distractor) new IT resources are only a click away, which means that you reduce the time to make those resources available to your developers from weeks to just minutes.

## 3.6. AWS ELB - Elastic Load Balancing

[AWS ELB](AWS%20ELB.md)

## 3.7. AWS ASG - Auto Scaling Groups

- **Auto Scaling in EC2 allows you to have the right number of instances to handle the application load. Auto Scaling in DynamoDB automatically adjusts read and write throughput capacity, in response to dynamically changing request volumes, with zero downtime. These are both examples of horizontal scaling.**
- **For each Auto Scaling Group, there's a Cooldown Period after each scaling activity. In this period, the ASG doesn't launch or terminate EC2 instances. This gives time to metrics to stabilize. The default value for the Cooldown Period is 300 seconds (5 minutes).** [AWS ASG](Amazon%20ASG.md)

## 3.8. Multi AZ in AWS

- Services where Multi-AZ must be enabled manually:
  - EFS, ELB, ASG, Beanstalk: assign AZ.
  - RDS, ElastiCache: multi-AZ (synchronous standby DB for failovers).
  - Aurora:
    - Data is stored automatically across multi-AZ.
    - Can have multi-AZ for the DB itself (same as RDS).
  - OpenSearch (managed): multi master.
  - Jenkins (self deployed): multi master.
- Service where Multi-AZ is implicitly there:
  - S3 (except OneZone-Infrequent Access).
  - DynamoDB.
  - All of AWS proprietary, managed services.

### 3.8.1. ELB and ASG - Summary

- High Availability vs Scalability (vertical and horizontal) vs Elasticity vs Agility in the Cloud.
- Elastic Load Balancers (ELB):
  - Distribute traffic across backend EC2 instances, can be Multi-AZ
  - Supports health checks
  - 3 types: Application LB (HTTP - L7), Network LB (TCP - L4), Classic LB (old)
- Auto Scaling Groups (ASG):
  - Implement Elasticity for your application, across multiple AZ
  - Scale EC2 instances based on the demand on your system, replace unhealthy
  - Integrated with the ELB

# 4. AWS Elastic Beanstalk

- **Elastic Beanstalk is a Platform as a Service (PaaS).**
- **You only manage data and applications.**
- **AWS Elastic Beanstalk makes it even easier for developers to quickly deploy and manage applications in the AWS Cloud.**
- [AWS Elastic Beanstalk](Amazon%20Elastic%20Beanstalk.md)

## 4.1. Elastic Beanstalk Summary:

- Platform as a Service (PaaS), limited to certain programming languages or Docker.
- Deploy code consistently with a known architecture: ex, ALB + EC2 + RDS.

# 5. Lambda

[Lambda](AWS%20Lambda.md)

## 5.1. Lambda Summary

- Lambda is Serverless, Function as a Service, seamless scaling, reactive.
- Lambda Billing:
  - By the time run x by the RAM provisioned.
  - By the number of invocations.
- Language Support: many programming languages except (arbitrary) Docker.
- Invocation time: up to 15 minutes.
- Use cases:
  - Create Thumbnails for images uploaded onto S3.
  - Run a Serverless cron job.
- API Gateway: expose Lambda functions as HTTP API.
