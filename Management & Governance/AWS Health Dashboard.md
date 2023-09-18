# AWS Health Dashboard <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Your account](#11-your-account)
- [2. Health Event Notifications](#2-health-event-notifications)
- [3. Status Checks](#3-status-checks)
- [4. CW Metrics \& Recovery](#4-cw-metrics--recovery)

# 1. Introduction

- Shows all regions, all services health.
- Shows historical information for each day.
- Has an RSS feed you can subscribe to.

## 1.1. Your account

- Previously called AWS Personal Health Dashboard (PHD).
- AWS Health Dashboard provides **alerts and remediation guidance** when AWS is experiencing **events that may impact you**.
- While the Service Health Dashboard displays the general status of AWS services, AWS Health Dashboard gives you a **personalized view into the performance and availability of the AWS services underlying your AWS resources**.
- The dashboard displays **relevant and timely information** to help you manage events in progress and **provides proactive** notification to help you plan for **scheduled activities**.
- **Can aggregate data from an entire AWS Organization.**
- Global service.
- Shows how AWS outages directly impact you & your AWS resources.
- Alert, remediation, proactive, scheduled activities.

# 2. Health Event Notifications

- Use [Amazon EventBridge](/Application%20Integration/Amazon%20EventBridge.md) to react to changes for **AWS Health events** in your AWS account.
- Example: receive email notifications when EC2 instances in your AWS account are scheduled for updates.
- This is possible for Account events (resources that are affected in your account) and Public Events (Regional availability of a service).
- **Use cases**
  - Send notifications.
  - capture event information.
  - take corrective action.

![Health Event Notifications](/Images/AWSHealthDashboardNotifications.png)

# 3. Status Checks

- Automated checks to identify hardware and software issues.
- **System Status Checks**
  - Monitors problems with AWS systems (software/hardware issues on the physical host, loss of system power...).
  - Check Personal Health Dashboard for any scheduled critical maintenance by AWS to your instance's host.
  - Resolution: stop and start the instance (instance migrated to a new host).
- **Instance Status Checks**
  - Monitors software/network configuration of your instance (invalid network configuration, exhausted memory...).
  - Resolution: reboot the instance or change instance configuration.

# 4. CW Metrics & Recovery

- CloudWatch Metrics (1 minute interval)
  - `StatusCheckFailed_System`
  - `StatusCheckFailed_Instance`
  - `StatusCheckFailed` (for both)
- Option 1: **CloudWatch Alarm**
  - Recover EC2 instance with the same private/public IP, EIP, metadata, and Placement Group.
  - Send notifications using SNS
- Option 2: **Auto Scaling Group**
  - Set min/max/desired 1 to recover an instance but won't keep the same private and elastic IP.
