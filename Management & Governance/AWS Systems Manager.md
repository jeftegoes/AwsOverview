# AWS Systems Manager <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. How Systems Manager works?](#11-how-systems-manager-works)
  - [1.2. Systems Manager - SSM Session Manager](#12-systems-manager---ssm-session-manager)
- [2. Systems Manager parameters - SSM parameters](#2-systems-manager-parameters---ssm-parameters)
  - [2.1. Policies](#21-policies)
- [3. SSM - Documents](#3-ssm---documents)
- [4. SSM - Run Command](#4-ssm---run-command)
- [5. Systems Manager Automation](#5-systems-manager-automation)
  - [5.1. Automation Runbook](#51-automation-runbook)
- [6. Parameter Store](#6-parameter-store)
  - [6.1. Parameter Store Hierarchy](#61-parameter-store-hierarchy)
  - [6.2. Parameters Policies (for advanced parameters)](#62-parameters-policies-for-advanced-parameters)
- [7. Patch Manager](#7-patch-manager)
  - [7.1. Patch Baseline \& Patch Group](#71-patch-baseline--patch-group)
  - [7.2. Patch Manager Patch Baselines](#72-patch-manager-patch-baselines)
- [8. SSM - Maintenance Windows](#8-ssm---maintenance-windows)
- [9. Session Manager](#9-session-manager)
- [10. SSM - Session Manager](#10-ssm---session-manager)
- [11. Systems Manager - Default Host Management Configuration](#11-systems-manager---default-host-management-configuration)
- [12. Hybrid Environments](#12-hybrid-environments)
- [13. IoT Greengrass Instance Activation](#13-iot-greengrass-instance-activation)
- [14. Systems Manager - Compliance](#14-systems-manager---compliance)
- [15. Systems Manager - OpsCenter](#15-systems-manager---opscenter)
- [16. Quick Setup](#16-quick-setup)
  - [16.1. Supported Configuration Types Include](#161-supported-configuration-types-include)

# 1. Introduction

- Helps we manage your **EC2 and On-Premises** systems at scale.
- Another Hybrid AWS service.
- Get operational insights about the state of your infrastructure.
- Easily detect problems.
- Most important features are:
  - Patching automation for enhanced compliance.
  - Run commands across an entire fleet of servers.
  - Store parameter configuration with the SSM Parameter Store.
- Works for both Windows and Linux OS.
- Integrated with AWS Config.
- Free service.

## 1.1. How Systems Manager works?

- We need to install the **SSM Agent** onto the systems we control.
- Installed by default on Amazon Linux AMI & some Ubuntu AMI.
- If an instance can't be controlled with SSM, it's probably an issue with the **SSM Agent**!
- Thanks to the **SSM Agent**, we can run commands, patch & configure our servers.

![SSM Agent](/Images/AWSSystemsManagerSSMAgent.png)

## 1.2. Systems Manager - SSM Session Manager

- Allows we to start a secure shell on your EC2 and on-premises servers.
- No SSH access, bastion hosts, or SSH keys needed.
- No port 22 needed (better security).
- Supports Linux, macOS, and Windows.
- Send session log data to S3 or CloudWatch Logs.

# 2. Systems Manager parameters - SSM parameters

## 2.1. Policies

- Parameter policies help we manage a growing set of parameters by allowing we to assign specific criteria to a parameter, such as an expiration date or time to live.
- Parameter policies are especially helpful in forcing we to update or delete passwords and configuration data stored in Parameter Store, a capability of AWS Systems Manager.
- Take note that parameter policies are only available for parameters in the **Advanced tier**.
- Parameter Store offers the following types of policies:
  - `Expiration` - deletes the parameter at a specific date
  - `ExpirationNotification` - sends an event to Amazon EventBridge when the specified expiration time is reached.
  - `NoChangeNotification` - sends an event to Amazon EventBridge when a parameter has not been modified for a specified period of time.

# 3. SSM - Documents

- Documents can be in JSON or YAML.
- We define parameters.
- We define actions.
- Many documents already exist in AWS.

# 4. SSM - Run Command

- Execute a document (= script) or just run a command.
- Run command across multiple instances (using resource groups).
- Rate Control / Error Control.
- Integrated with IAM & CloudTrail.
- No need for SSH.
- Command Output can be shown in the Console, sent to S3 bucket or CloudWatch Logs.
- Send notifications to SNS about command statues (In progress, Success, Failed...).
- Can be invoked using EventBridge.

# 5. Systems Manager Automation

- Simplifies common maintenance and deployment tasks of Amazon EC2 instances and other AWS resources.
- **Automation** enables we to do the following:
  - Build Automation workflows to configure and manage instances and AWS resources.
  - Create custom workflows or use pre-defined workflows maintained by AWS.
  - Receive notifications about Automation tasks and workflows by using Amazon CloudWatch Events.
  - Monitor Automation progress and execution details by using the Amazon EC2 or the AWS Systems Manager console.

![SSM - Automation](/Images/AWSSystemsManagerAutomation.png)

## 5.1. Automation Runbook

- SSM Documents of type Automation.
- Defines actions preformed on your EC2 instances or AWS resources.
- Pre-defined runbooks (AWS) or create custom runbooks.

- **Can be triggered**
  - Manually using AWS Console, AWS CLI or SDK.
  - By Amazon EventBridge.
  - On a schedule using Maintenance Windows.
  - By AWS Config for rules remediations.

# 6. Parameter Store

- Secure storage for configuration and secrets.
- Optional Seamless Encryption using KMS.
- Serverless, scalable, durable, easy SDK.
- Version tracking of configurations / secrets.
- Security through IAM.
- Notifications with Amazon EventBridge.
- Integration with CloudFormation.

## 6.1. Parameter Store Hierarchy

- /my-department/
  - my-app/
    - dev/
      - db-url
      - db-password
    - prod/
      - db-url
      - db-password
  - other-app/
- /other-department/
- /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
- /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 (public)

## 6.2. Parameters Policies (for advanced parameters)

- Allow to assign a TTL to a parameter (expiration date) to force updating or deleting sensitive data such as passwords.
- Can assign multiple policies at a time.

# 7. Patch Manager

- Automates the process of patching managed instances.
- OS updates, applications updates, security updates...
- Supports both EC2 instances and on-premises servers.
- Supports Linux, macOS, and Windows.
- Patch on-demand or on a schedule using **Maintenance Windows**.
- Scan instances and generate patch compliance report (missing patches).
- Patch compliance report can be sent to S3.

![Patch Manager](/Images/AWSSystemsManagerPatchManager.png)

## 7.1. Patch Baseline & Patch Group

- **Patch Baseline**
  - Defines which patches should and shouldn't be installed on your instances.
  - Ability to create custom Patch Baselines (specify approved/rejected patches).
  - Patches can be auto-approved within days of their release.
  - By default, install only critical patches and patches related to security.
- **Patch Group**
  - Associate a set of instances with a specific Patch Baseline.
  - Example: create Patch Groups for different environments (dev, test, prod).
  - Instances should be defined with the tag key Patch Group.
  - An instance can only be in one Patch Group.
  - Patch Group can be registered with only one Patch Baseline.

## 7.2. Patch Manager Patch Baselines

- **Pre-Defined Patch Baseline**
  - Managed by AWS for different Operating Systems (can't be modified).
  - **AWS-RunPatchBaseline (SSM Document):** Apply both operating system and application patches (Linux, macOS, Windows Server).
- **Custom Patch Baseline**
  - Create your own Patch Baseline and choose which patches to auto-approve.
  - Operating System, allowed patches, rejected patches...
  - Ability to specify custom and alternative patch repositories.

# 8. SSM - Maintenance Windows

- Defines a schedule for when to perform actions on your instances.
- Example: OS patching, updating drivers, installing software...
- **Maintenance Window contains**
  - Schedule.
  - Duration.
  - Set of registered instances.
  - Set of registered tasks.

# 9. Session Manager

- Allows we to start a secure shell on your EC2 and on-premises servers.
- Access through AWS Console, AWS CLI, or Session Manager SDK.
- **Does not need SSH access, bastion hosts, or SSH keys.**
- No port 22 needed (better security).
- Supports Linux, macOS, and Windows.
- Log connections to your instances and executed commands.
- Session log data can be sent to S3 or CloudWatch Logs.
- CloudTrail can intercept `StartSession` events.

# 10. SSM - Session Manager

- IAM Permissions
  - Control which users/groups can access Session Manager and which instances.
  - Use tags to restrict access to only specific EC2 instances
  - Access SSM + write to S3 + write to CloudWatch
- Optionally, we can restrict commands a user can run in a session

# 11. Systems Manager - Default Host Management Configuration

- When enabled, it automatically configure your EC2 instances as managed instances **without the use of EC2 Instance Profile**.
- **Instance Identity Role:** A type of IAM Role **with no permissions** beyond identifying the EC2 instance to AWS Services (e.g., Systems Manager).
- EC2 instances must have **IMDSv2 enabled** and **SSM Agent installed (doesn't support IMDSv1)**.
- Automatically enables Session Manager, Patch Manager, and Inventory.
- Automatically keeps the SSM Agent up to date.
- Must be enabled per AWS Region.

# 12. Hybrid Environments

- We can setup Systems Manager to manage on-premises servers, IoT devices, edge devices, and virtual machines (e.g., VMs in other cloud providers).
- In Systems Manager Console:
  - EC2 instances use the prefix **"i-"**.
  - Hybrid managed nodes use the prefix **"mi-"**.

# 13. IoT Greengrass Instance Activation

- Manage IoT Greengrass Core devices using SSM.
- Install SSM Agent on Greengrass Core devices (registered as a managed node in SSM).
- SSM Agent can be installed manually or deployed as a Greengrass Component (pre-built software module that you deploy directly to Greengrass Core devices).
- We must add permissions to the Token Exchange Role (IAM Role for the IoT core device) to communicate with Systems Manager.
- Supports all SSM Capabilities (Patch Manager, Session Manager, Run Command...).
- **Use cases:** Easily update and maintain OS and software updates across a fleet of Greengrass Core devices.

# 14. Systems Manager - Compliance

- Scan your fleet of managed nodes for patch compliance and configuration inconsistencies.
- Displays current data about:
  - **Patches in Patch Manager.**
  - **Associations in State Manager.**
- Can sync data to an S3 bucket using **Resource Data Sync**, and we can analyze using Athena and QuickSight.
- Can collect and aggregate data from multiple accounts and regions.
- Can send compliance data to Security Hub.

# 15. Systems Manager - OpsCenter

- Allows we to view, investigate, and remediate issues in one place (no need to navigate across different AWS services).
- Security issues (Security Hub), performance issues (DynamoDB throttle), failures (ASG failed launch instance)...
- Reduce meantime to resolve issues.
- OpsItems
  - Operational issue or interruption that needs investigation and remediation.
  - Event, resource, AWS Config changes, CloudTrail logs, EventBridge...
  - Provides recommended Runbooks to resolve the issue.
- Supports both EC2 instances and on-premises managed nodes.

# 16. Quick Setup

- **Quick Setup** is a tool in AWS Systems Manager that **automates best-practice configurations** for common features across single or multiple AWS accounts and Regions.
- Default Host Management Configuration (part of Quick Setup).
- **Key Benefits**
  - **Simplifies configuration:** Automatically sets up IAM roles, patch scans, inventory collection, CloudWatch Agent, instance scheduling, AWS Config, and more.
  - **Drift remediation:** Periodically checks and corrects configuration drift.
  - **Multi-account & multi-region deployment:** Integrates with AWS Organizations and deploys via CloudFormation StackSets.

## 16.1. Supported Configuration Types Include

- Host management (e.g., SSM & CloudWatch agents, inventory, patch scans)
- Default Host Management for organizations
- AWS Config recorder
- Patch policy enforcement
- Resource Scheduler (auto start/stop EC2)
- Distributor packages deployment
- OpsCenter, DevOps Guru, Resource Explorer, Change Manager, etc.
