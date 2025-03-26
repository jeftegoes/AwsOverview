# Serverless Architectures <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. WhatIsTheTime.com](#1-whatisthetimecom)
  - [1.1. Requirements](#11-requirements)
  - [1.2. Solutions](#12-solutions)
  - [1.3. Diagram](#13-diagram)
- [2. MyClothes.com](#2-myclothescom)
  - [2.1. Requirements](#21-requirements)
  - [2.2. Solutions](#22-solutions)
  - [2.3. Diagram](#23-diagram)
- [3. MyWordPress.com](#3-mywordpresscom)
  - [3.1. Requirements](#31-requirements)
  - [3.2. Solutions](#32-solutions)
  - [3.3. Diagram](#33-diagram)

# 1. WhatIsTheTime.com

- Stateless.

## 1.1. Requirements

- WhatIsTheTime.com allows people to know what time it is.
- We don't need a database.
- We want to start small and can accept downtime.
- We want to fully scale vertically and horizontally, no downtime.
- Let's go through the Solutions Architect journey for this app.

## 1.2. Solutions

- Public vs Private IP and EC2 instances.
- Elastic IP vs Route 53 vs Load Balancers.
- Route 53 TTL, A records and Alias Records.
- Maintaining EC2 instances manually vs Auto Scaling Groups.
- Multi AZ to survive disasters.
- ELB Health Checks.
- Security Group Rules.
- Reservation of capacity for costing savings when possible.

## 1.3. Diagram

TODO: DIAGRAM

# 2. MyClothes.com

- Stateful.

## 2.1. Requirements

- MyClothes.com allows people to buy clothes online.
- There's a shopping cart.
- Our website is having hundreds of users at the same time.
- We need to scale, maintain horizontal scalability and keep our web application as stateless as possible.
- Users should not lose their shopping cart.
- Users should have their details (address, etc) in a database.

## 2.2. Solutions

- 3-tier architectures for web applications.
- ELB sticky sessions.
- Web clients for storing cookies and making our web app stateless.
- **ElastiCache**
  - For storing sessions (alternative: DynamoDB).
  - For caching data from RDS.
  - Multi AZ.
- **RDS**
  - For storing user data.
  - Read replicas for scaling reads.
  - Multi AZ for disaster recovery.
- Tight Security with security groups referencing each other.

## 2.3. Diagram

TODO: DIAGRAM

# 3. MyWordPress.com

- Stateful.

## 3.1. Requirements

- We are trying to create a fully scalable WordPress website.
- We want that website to access and correctly display picture uploads.
- Our user data, and the blog content should be stored in a MySQL database.

## 3.2. Solutions

- Aurora Database to have easy Multi-AZ and Read-Replicas.
- Storing data in EBS (single instance application).
- Vs Storing data in EFS (distributed application).

## 3.3. Diagram

TODO: DIAGRAM
