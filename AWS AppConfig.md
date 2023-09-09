# AWS AppConfig<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)

# 1. Introduction

- Configure, validate, and deploy dynamic configurations.
- Deploy dynamic configuration changes to your applications independently of any code deployments.
  - You don't need to restart the application.
- Feature flags, application tuning, allow/block listing...
- Use with apps on EC2 instances, Lambda, ECS, EKS...
- Gradually deploy the configuration changes and rollback if issues occur.
- Validate configuration changes before deployment using:
  - **JSON Schema** (syntactic check) or
  - **Lambda Function** - run code to perform validation (semantic check).
