# AWS ECS - Elastic Container Service, Fargate and ECR - Elastic Container Registry<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is Docker?](#1-what-is-docker)
- [2. Docker Containers Management on AWS](#2-docker-containers-management-on-aws)
- [3. ECS](#3-ecs)
  - [3.1. Launch Types](#31-launch-types)
    - [3.1.1. EC2 Launch Type](#311-ec2-launch-type)
    - [3.1.2. Fargate Launch Type](#312-fargate-launch-type)
  - [3.2. IAM Roles for ECS](#32-iam-roles-for-ecs)
  - [3.3. Load Balancer Integrations](#33-load-balancer-integrations)
  - [3.4. Data Volumes (EFS)](#34-data-volumes-efs)
  - [3.5. Service Auto Scaling](#35-service-auto-scaling)
    - [3.5.1. Auto Scaling EC2 Instances](#351-auto-scaling-ec2-instances)
  - [3.6. Rolling Updates](#36-rolling-updates)
  - [3.7. Task Definitions](#37-task-definitions)
  - [3.8. Load Balancing](#38-load-balancing)
    - [3.8.1. EC2 Launch Type](#381-ec2-launch-type)
    - [3.8.2. Fargate](#382-fargate)
  - [3.9. Environment Variables](#39-environment-variables)
  - [3.10. Data Volumes (Bind Mounts)](#310-data-volumes-bind-mounts)
  - [3.11. Task Placement](#311-task-placement)
    - [3.11.1. Task Placement Process](#3111-task-placement-process)
    - [3.11.2. Task Placement Strategies](#3112-task-placement-strategies)
      - [3.11.2.1. Binpack](#31121-binpack)
      - [3.11.2.2. Random](#31122-random)
      - [3.11.2.3. Spread](#31123-spread)
      - [3.11.2.4. Distinct Instance (distinctInstance)](#31124-distinct-instance-distinctinstance)
      - [3.11.2.5. memberOf](#31125-memberof)
- [4. Amazon ECR](#4-amazon-ecr)
- [5. Amazon EKS Overview](#5-amazon-eks-overview)
  - [5.1. Node Types](#51-node-types)
  - [5.2. Data Volumes](#52-data-volumes)

# 1. What is Docker?

[Docker commands and overview](https://github.com/jeftegoes/DockerOverviewAndCommands)

# 2. Docker Containers Management on AWS

- **Amazon Elastic Container Service (Amazon ECS):**
  - Amazon's own container platform.
- **Amazon Elastic Kubernetes Service (Amazon EKS):**
  - Amazon's managed Kubernetes (open source).
- **AWS Fargate:**
  - Amazon's own Serverless container platform.
  - Works with ECS and with EKS.
- **Amazon Elastic Container Registry (Amazon ECR):**
  - Store container images.

# 3. ECS

## 3.1. Launch Types

### 3.1.1. EC2 Launch Type

- ECS = Elastic Container Service.
- Launch Docker containers on AWS = Launch **ECS Tasks** on **ECS Clusters**.
- **EC2 Launch Type: You must provision and maintain the infrastructure (the EC2 instances).**
- Each EC2 Instance must run the ECS Agent to register in the ECS Cluster.
- AWS takes care of starting / stopping containers.

### 3.1.2. Fargate Launch Type

- Launch Docker containers on AWS.
- **You do not provision the infrastructure (no EC2 instances to manage).**
- **It's all Serverless!**
- You just create task definitions
- AWS just runs ECS Tasks for you based on the CPU / RAM you need.
- To scale, just increase the number of tasks. Simple - no more EC2 instances.

## 3.2. IAM Roles for ECS

- **EC2 Instance Profile (EC2 Launch Type only):**
  - Used by the ECS agent.
  - Makes API calls to ECS service.
  - Send container logs to CloudWatch Logs.
  - Pull Docker image from ECR.
  - Reference sensitive data in Secrets Manager or SSM Parameter Store.
- **ECS Task Role:**
  - Allows each task to have a specific role.
  - Use different roles for the different ECS Services you run.
  - Task Role is defined in the **task definition**.

## 3.3. Load Balancer Integrations

- **Application Load Balancer** supported and works for most use cases.
- **Network Load Balancer** recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link.
- Elastic Load Balancer supported but not recommended (no advanced features - no Fargate).

## 3.4. Data Volumes (EFS)

- Mount EFS file systems onto ECS tasks.
- Works for both **EC2** and **Fargate** launch types.
- Tasks running in any AZ will share the same data in the EFS file system.
- **Fargate + EFS = Serverless.**
- Use cases: Persistent multi-AZ shared storage for your containers.
- Note:
  - Amazon S3 cannot be mounted as a file system.
  -

## 3.5. Service Auto Scaling

- Automatically increase/decrease the desired number of ECS tasks.
- Amazon ECS Auto Scaling uses **AWS Application Auto Scaling**.
  - ECS Service Average CPU Utilization.
  - ECS Service Average Memory Utilization - Scale on RAM.
  - ALB Request Count Per Target - metric coming from the ALB.
- **Target Tracking** - scale based on target value for a specific CloudWatch metric
- **Step Scaling** - scale based on a specified CloudWatch Alarm
- **Scheduled Scaling** - scale based on a specified date/time (predictable changes)
- ECS Service Auto Scaling (task level) **!=** EC2 Auto Scaling (EC2 instance level)
- Fargate Auto Scaling is much easier to setup (because **Serverless**).

### 3.5.1. Auto Scaling EC2 Instances

- Accommodate ECS Service Scaling by adding underlying EC2 Instances.
- **Auto Scaling Group Scaling**
  - Scale your ASG based on CPU Utilization.
  - Add EC2 instances over time.
- **ECS Cluster Capacity Provider**
  - Used to automatically provision and scale the infrastructure for your ECS Tasks.
  - Capacity Provider paired with an Auto Scaling Group.
  - Add EC2 Instances when you're missing capacity (CPU, RAM...).
  -

## 3.6. Rolling Updates

- When updating from v1 to v2, we can control how many tasks can be started and stopped, and in which order

## 3.7. Task Definitions

- Task definitions are metadata in **JSON form** to tell ECS how to run a Docker container.
- It contains crucial information, such as:
  - Image Name.
  - Port Binding for Container and Host.
  - Memory and CPU required.
  - Environment variables.
  - Networking information.
  - IAM Role.
  - Logging configuration (ex CloudWatch).
- Can define up to 10 containers in a Task Definition.

## 3.8. Load Balancing

### 3.8.1. EC2 Launch Type

- We get a **Dynamic Host Port Mapping if you define only the container port in the task definition**.
- The ALB finds the right port on your EC2 Instances.
- **You must allow on the EC2 instance's Security Group any port from the ALB's Security Group.**
- **To enable random host port, set host port = 0 (or empty), which allows multiple containers of the same type to launch on the same EC2 container instance.**

### 3.8.2. Fargate

- Each task has a unique private IP.
- Only define the container port (host port is not applicable).
- Example
  - ECS ENI Security Group
    - Allow port 80 from the ALB
  - ALB Security Group
    - Allow port 80/443 from web

## 3.9. Environment Variables

- Environment Variable:
  - **Hardcoded** - e.g., URLs.
  - **SSM Parameter Store** - sensitive variables (e.g., API keys, shared configs).
  - **Secrets Manager** - sensitive variables (e.g., DB passwords).
- Environment Files (bulk) - Amazon S3.

## 3.10. Data Volumes (Bind Mounts)

- Share data between multiple containers in the same Task Definition.
- Works for both **EC2** and **Fargate** tasks.
- **EC2 Tasks** - using EC2 instance storage:
  - Data are tied to the lifecycle of the EC2 instance
- **Fargate Tasks** - using ephemeral storage
  - Data are tied to the container(s) using them
  - 20 GiB - 200 GiB (default 20 GiB)
- Use cases:
  - Share ephemeral data between multiple containers
  - "Sidecar" container pattern, where the "sidecar" container used to send metrics/logs to other destinations (separation of conerns).

## 3.11. Task Placement

- When an ECS task is started with EC2 Launch Type, ECS must determine where to place it, with the constraints of CPU and memory (RAM).
- Similarly, when a service scales in, ECS needs to determine which task to terminate.
- You can define:
  - **Task Placement Strategy.**
  - **Task Placement Constraints.**
- **Note:** only for ECS Tasks with EC2 Launch Type **(Fargate not supported)**.

### 3.11.1. Task Placement Process

- Task Placement Strategies are a **best effort**.
- When Amazon ECS places a task, it uses the following process to select the appropriate EC2 Container instance:

1. Identify which instances that satisfy the **CPU, memory, and port** requirements.
2. Identify which instances that satisfy the **Task Placement Constraints**.
3. Identify which instances that satisfy the **Task Placement Strategies**.
4. Select the instances.

### 3.11.2. Task Placement Strategies

- You can mix them together.

#### 3.11.2.1. Binpack

- Tasks are placed on the least available amount of CPU and Memory.
- Minimizes the number of EC2 instances in use (cost savings).

#### 3.11.2.2. Random

- Tasks are placed randomly.

#### 3.11.2.3. Spread

- Tasks are placed evenly based on the specified value.
- Example: **instanceId, attribute:ecs.availability-zone, ...**

#### 3.11.2.4. Distinct Instance (distinctInstance)

- Tasks are placed on a different EC2 instance.

#### 3.11.2.5. memberOf

- Tasks are placed on EC2 instances that satisfy a specified expression.
- Uses the Cluster Query Language (advanced).

# 4. Amazon ECR

- ECR = Elastic Container Registry.
- Store and manage Docker images on AWS.
- Private and Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws).
- Fully integrated with ECS, backed by Amazon S3.
- Access is controlled through IAM (permission errors => policy).
- Supports image vulnerability scanning, versioning, image tags, image lifecycle, ...

# 5. Amazon EKS Overview

- Amazon EKS = Amazon Elastic **Kubernetes** Service.
- It is a way to launch **managed Kubernetes clusters on AWS**.
- Kubernetes is an **open-source system** for automatic deployment, scaling and management of containerized (usually Docker) application.
- It's an alternative to ECS, similar goal but different API.
- EKS supports **EC2** if you want to deploy worker nodes or **Fargate** to deploy serverless containers.
- **Use case:** If your company is already using Kubernetes on-premises or in another cloud, and wants to migrate to AWS using Kubernetes.
- **Kubernetes is cloud-agnostic** (can be used in any cloud - Azure, GCP...).
- For multiple regions, deploy one EKS cluster per region.
- Collect logs and metrics using **CloudWatch Container Insights**.

## 5.1. Node Types

- **Managed Node Groups:**
  - Creates and manages Nodes (EC2 instances) for you.
  - Nodes are part of an ASG managed by EKS.
  - Supports On-Demand or Spot Instances.
- **Self-Managed Nodes:**
  - Nodes created by you and registered to the EKS cluster and managed by an ASG.
  - You can use prebuilt AMI - Amazon EKS Optimized AMI.
  - Supports On-Demand or Spot Instances.
- **AWS Fargate:**
  - No maintenance required: no nodes managed.

## 5.2. Data Volumes

- Need to specify **StorageClass** manifest on your EKS cluster.
- Leverages a **Container Storage Interface (CSI)** compliant driver.
- Support for...
  - Amazon EBS.
  - Amazon EFS (works with Fargate).
  - Amazon FSx for Lustre.
  - Amazon FSx for NetApp ONTAP.
