# Amazon EKS - Elastic Kubernetes Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Node Types](#2-node-types)
- [3. Data Volumes](#3-data-volumes)
- [4. Control Plane Logging](#4-control-plane-logging)
- [5. Nodes \& Containers Logging](#5-nodes--containers-logging)
- [6. IAM Roles for Service Accounts (IRSA)](#6-iam-roles-for-service-accounts-irsa)
- [7. Encrypting EKS Secrets with KMS](#7-encrypting-eks-secrets-with-kms)
- [8. Kubernetes Cluster Autoscaler on EKS](#8-kubernetes-cluster-autoscaler-on-eks)

# 1. Introduction

- Amazon EKS = Amazon Elastic **Kubernetes** Service.
- It is a way to launch **managed Kubernetes clusters on AWS**.
- Kubernetes is an **open-source system** for automatic deployment, scaling and management of containerized (usually Docker) application.
- It's an alternative to ECS, similar goal but different API.
- EKS supports **EC2** if we want to deploy worker nodes or **Fargate** to deploy serverless containers.
- **Use case:** If our company is already using Kubernetes on-premises or in another cloud, and wants to migrate to AWS using Kubernetes.
- **Kubernetes is cloud-agnostic** (can be used in any cloud - Azure, GCP...).
- For multiple regions, deploy one EKS cluster per region.
- Collect logs and metrics using **CloudWatch Container Insights**.

# 2. Node Types

- **Managed Node Groups**
  - Creates and manages Nodes (EC2 instances) for we.
  - **Nodes** are part of an ASG managed by EKS.
  - Supports On-Demand or Spot Instances.
- **Self-Managed Nodes**
  - **Nodes** created by we and registered to the EKS cluster and managed by an ASG.
  - We can use prebuilt AMI - Amazon EKS Optimized AMI.
  - Supports On-Demand or Spot Instances.
- **AWS Fargate**
  - **No maintenance required:** No nodes managed.

# 3. Data Volumes

- Need to specify **StorageClass** manifest on our EKS cluster.
- Leverages a **Container Storage Interface (CSI)** compliant driver.
- Support for...
  - Amazon EBS.
  - Amazon EFS (works with Fargate).
  - Amazon FSx for Lustre.
  - Amazon FSx for NetApp ONTAP.

# 4. Control Plane Logging

- Send EKS Control Plane audit and diagnostic logs to CloudWatch Logs.
- **EKS Control Plane Log Types**
  - API Server (api).
  - Audit (audit).
  - Authenticator (authenticator).
  - Controller Manager (controllerManager).
  - Scheduler (scheduler).
- Ability to select the exact log types to send to CloudWatch Logs.

# 5. Nodes & Containers Logging

- We can capture node, pod, and containers logs and send them to CloudWatch Logs.
- Use **CloudWatch Agent** to send metrics to CloudWatch.
- Use the Fluent Bit, or Fluentd log drivers to send logs to CloudWatch Logs.
- Container logs are stored on a Node directory /var/log/containers.
- Use CloudWatch Container Insights to get a dashboarding monitoring solution for nodes, pods, tasks, and services.

# 6. IAM Roles for Service Accounts (IRSA)

- IAM Roles for Service Accounts (IRSA) enables assigning fine-grained IAM roles to Kubernetes service accounts in Amazon EKS.
- This provides Pod-level access to AWS resources without relying on node-level instance profiles.
- IRSA leverages an **OpenID Connect (OIDC)** provider integrated with the EKS cluster.
- Pods assume the associated IAM role via the service account securely, without storing credentials in the container.
- **Benefits**
  - Enforces **least privilege** by scoping permissions to each service's needs.
  - Ensures **Pod-level isolation** for access control.
  - Eliminates the need for **EC2 instance-wide roles**, reducing risk of over-permissioning.
  - Aligns with **AWS security best practices** for running workloads in EKS.

# 7. Encrypting EKS Secrets with KMS

- **Problem:** By default, EKS stores secrets in `etcd` unencrypted (security risk).
- **Solution:** Enable **secret encryption** with a new **AWS KMS key**.
- **How it works**
  - Create a KMS key for the EKS cluster.
  - Configure EKS to encrypt secrets before saving them in `etcd`.
- **Benefits**
  - Secrets encrypted **at rest**.
  - Stronger **data confidentiality and integrity**.
  - Meets **industry compliance** and security standards.

# 8. Kubernetes Cluster Autoscaler on EKS

- **Purpose:** Automatically adjusts the size of an EKS cluster's compute resources.
- **How it works**
  - Monitors for **unschedulable pods** (pods without enough CPU/memory).
  - If needed, scales out by increasing the **Auto Scaling group (ASG)** capacity to add EC2 nodes.
  - Scales in by draining and removing **underutilized nodes** when workloads can be shifted.
- **Benefits**
  - Ensures elasticity and high availability.
  - Reduces costs by removing idle nodes.
  - Minimizes admin overhead while supporting real-time, variable workloads.
