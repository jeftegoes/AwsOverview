# AWS Resource Groups and Tagging<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. AWS Tag Editor](#2-aws-tag-editor)

# 1. Introduction

- **Tags** are used for organizing resources:
  - EC2: instances, images, load balancers, security groups...
  - RDS, VPC resources, Route 53, IAM users, etc...
  - Resources created by CloudFormation are all tagged the same way.
- Free naming, common tags are: Name, Environment, Team ...
- Tags can be used to create Resource Groups:
  - Create, maintain, and view a collection of resources that share common tags.
  - Manage these tags using the Tag Editor.

# 2. AWS Tag Editor

- Allows you to manage tags of multiple resources at once.
- You can add/update/delete tags.
- Search tagged/untagged resources in all AWS Regions.
