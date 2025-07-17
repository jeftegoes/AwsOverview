# Amazon ECR - Elastic Container Registry <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Lifecycle Policies](#2-lifecycle-policies)
- [3. Amazon ECR public](#3-amazon-ecr-public)

# 1. Introduction

- ECR = Elastic Container Registry.
- Store and manage Docker images on AWS.
- **Private** and **Public** repository (Amazon ECR Public Gallery https://gallery.ecr.aws).
- Fully integrated with ECS, backed by [Amazon S3](/Storage/Amazon%20S3.md).
- Access is controlled through IAM (permission errors => policy).
- Supports image vulnerability scanning, versioning, image tags, image lifecycle, ...

# 2. Lifecycle Policies

- Automatically remove old or unused images based on age or count.
- Each Lifecycle Policy contains one or more rules.
- All rules are evaluated at the same time, then applied based on priority.
- Images are expired within 24 hours after they meet the criteria.
- Helps we reduce unnecessary storage costs.

# 3. Amazon ECR public

- Developers building container-based applications can now discover and download Docker Official Images directly from Amazon Elastic Container Registry (Amazon ECR) Public.
