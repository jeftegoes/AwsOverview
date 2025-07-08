# AWS Global Accelerator <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Global users for our application](#1-global-users-for-our-application)
- [2. Unicast IP vs Anycast IP](#2-unicast-ip-vs-anycast-ip)
- [3. Introduction](#3-introduction)
- [4. Single-region blue/green deployment with AWS Global Accelerator](#4-single-region-bluegreen-deployment-with-aws-global-accelerator)
- [5. AWS Global Accelerator vs CloudFront](#5-aws-global-accelerator-vs-cloudfront)

# 1. Global users for our application

- We have deployed an application and have global users who want to access it directly.
- They go over the public internet, which can add a lot of latency due to many hops.
- We wish to go as fast as possible through AWS network to minimize latency.
  ![AWS Global Accelerator](/Images/Networking%20&%20Content%20Delivery/AWSGlobalAccelerator.png)

# 2. Unicast IP vs Anycast IP

- **Unicast IP:** One server holds one IP address.
- **Anycast IP:** All servers hold the same IP address and the client is routed to the nearest one.

# 3. Introduction

- Improve global application availability and performance using the AWS global network.
- Leverage the AWS internal network to optimize the route to your application (60% improvement).
- **2 Anycast IP** are created for your application and traffic is sent through Edge Locations.
- The Edge locations send the traffic to your application.
- Works with **Elastic IP, EC2 instances, ALB, NLB, public or private**.
- **Consistent Performance**
  - Intelligent routing to lowest latency and fast regional failover.
  - **IMPORTANT!** No issue with client cache (because the IP doesn't change).
  - Internal AWS network.
- **Health Checks**
  - Global Accelerator performs a health check of your applications.
  - Helps make your application global (failover less than 1 minute for unhealthy).
  - Great for disaster recovery (thanks to the health checks).
- **Security**
  - Only 2 external IP need to be whitelisted.
  - DDoS protection thanks to AWS Shield.
    ![AWS Global Accelerator Traffic Multi Region](/Images/Networking%20&%20Content%20Delivery/AWSGlobalAcceleratorTrafficMultiRegion.png)

# 4. Single-region blue/green deployment with AWS Global Accelerator

![AWS Global Accelerator Single Region Blue/Green](/Images/Networking%20&%20Content%20Delivery/AWSGlobalAcceleratorSingleRegionBlueGreen.png)

# 5. AWS Global Accelerator vs CloudFront

- They both use the AWS global network and its edge locations around the world.
- Both services integrate with AWS Shield for DDoS protection.
- **CloudFront**
  - Improves performance for your cacheable content (such as images and videos).
  - Dynamic content (such as API acceleration and dynamic site delivery).
  - Content is served at the edge.
- **Global Accelerator**
  - Improves performance for a wide range of applications over TCP or UDP.
  - Proxying packets at the edge to applications running in one or more AWS Regions.
  - Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP.
  - Good for HTTP use cases that require static IP addresses.
  - Good for HTTP use cases that required deterministic, fast regional failover.
