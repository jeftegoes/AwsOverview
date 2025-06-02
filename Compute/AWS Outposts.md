# AWS Outposts <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Benefits](#2-benefits)
- [3. Some services that work on Outposts](#3-some-services-that-work-on-outposts)

# 1. Introduction

- **Hybrid Cloud:** Businesses that keep an on-premises infrastructure alongside a cloud infrastructure.
- **Therefore, two ways of dealing with IT systems**
  - One for the AWS cloud (using the AWS console, CLI, and AWS APIs).
  - One for their on-premises infrastructure.
- **AWS Outposts are "server racks"** that offers the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud.
- **AWS will setup and manage "Outposts Racks"** within your on-premises infrastructure and you can start leveraging AWS services on-premises.
- _You are responsible for the Outposts Rack physical security._
  ![AWS Outposts](/Images/Compute/AWSOutposts.png)

# 2. Benefits

- Low-latency access to on-premises systems.
- Local data processing.
- Data residency.
- Easier migration from on-premises to the cloud.
- Fully managed service.

# 3. Some services that work on Outposts

- Amazon EC2
- Amazon EBS
- Amazon S3
- Amazon EKS
- Amazon ECS
- Amazon RDS
- Amazon EMR
