# AWS Service Catalog<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Service Catalog diagram](#2-service-catalog-diagram)
- [3. Stack Set Constraints](#3-stack-set-constraints)
- [4. Launch Constraints](#4-launch-constraints)

# 1. Introduction

- Users that are new to AWS have too many options, and may create stacks that are not compliant / in line with the rest of the organization.
- Some users just want a quick **self-service portal** to launch a set of **authorized products** pre-defined **by admins**.
- Includes: virtual machines, databases, storage options, etc...

# 2. Service Catalog diagram

- Create and manage catalogs of IT services that are approved on AWS.
- The "products" are CloudFormation templates.
- Ex: Virtual machine images, Servers, Software, Databases, Regions, IP address ranges.
- **CloudFormation helps ensure consistency, and standardization by Admins.**
- They are assigned to Portfolios (teams).
- Teams are presented a self-service portal where they can launch the products.
- All the deployed products are centrally managed deployed services.
- **Helps with governance, compliance, and consistency.**
- Can give user access to launching products without requiring deep AWS knowledge.
- Integrations with "self-service portals" such as ServiceNow.

# 3. Stack Set Constraints

- Allows you to configure Product deployment options using CloudFormation StackSets.
- **Accounts:** Identify AWS accounts where you want to create Products.
- **Regions:** Identify AWS regions where you want to deploy to (with Order).
- **Permissions:** IAM StackSet Administrator Role to manage target AWS accounts.

# 4. Launch Constraints

- IAM Role assigned to a Product which allows a user to launch, update, or terminate a product with minimal IAM permissions.
- Example: end user has access only to Service Catalog, all other permissions required are attached to the Launch Constraint IAM Role.
- IAM Role must have the following permissions:
  - CloudFormation (Full Access)
  - AWS Services in the CloudFormation template
  - S3 Bucket which contains the CloudFormation template (Read Access)
