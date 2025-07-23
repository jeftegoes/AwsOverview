# AWS Billing and Cost Management <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Billing and Costing Tools](#1-billing-and-costing-tools)
- [2. AWS Pricing Calculator](#2-aws-pricing-calculator)
- [3. AWS Billing Dashboard](#3-aws-billing-dashboard)
  - [3.1. Cost Allocation Tags](#31-cost-allocation-tags)
  - [3.2. AWS Resource Groups and Tagging](#32-aws-resource-groups-and-tagging)
  - [3.3. Cost and Usage Reports](#33-cost-and-usage-reports)
  - [3.4. Cost Explorer](#34-cost-explorer)
  - [3.5. AWS Cost Anomaly Detection](#35-aws-cost-anomaly-detection)
- [4. Billing Alarms in CloudWatch](#4-billing-alarms-in-cloudwatch)
- [5. AWS Budgets](#5-aws-budgets)
- [6. AWS Support Plans Pricing](#6-aws-support-plans-pricing)
  - [6.1. AWS Basic Support Plan](#61-aws-basic-support-plan)
  - [6.2. AWS Developer Support Plan](#62-aws-developer-support-plan)
  - [6.3. AWS Business Support Plan (24/7)](#63-aws-business-support-plan-247)
  - [6.4. AWS Enterprise On-Ramp Support Plan (24/7)](#64-aws-enterprise-on-ramp-support-plan-247)
  - [6.5. AWS Enterprise Support Plan (24/7)](#65-aws-enterprise-support-plan-247)
- [7. Account Best Practices - Summary](#7-account-best-practices---summary)
- [8. Billing and Costing Tools - Summary](#8-billing-and-costing-tools---summary)
- [9. Cost Optimization Hub](#9-cost-optimization-hub)

# 1. Billing and Costing Tools

- **Estimating costs in the cloud:**
  - Pricing Calculator.
- **Tracking costs in the cloud:**
  - Billing Dashboard.
  - Cost Allocation Tags.
  - Cost and Usage Reports.
  - Cost Explorer.
- **Monitoring against costs plans:**
  - Billing Alarms.
  - Budgets.

# 2. AWS Pricing Calculator

- **The AWS Simple Monthly Calculator is an easy-to-use online tool that enables you to estimate their architecture solution monthly cost of AWS services for your use case based on your expected usage. It is being replaced by AWS Pricing Calculator.**
- **The TCO calculators allow you to estimate the cost savings when using AWS and provide a detailed set of reports that can be used in executive presentations.**
- Available at https://calculator.aws/.
- Estimate the cost for your solution architecture.

# 3. AWS Billing Dashboard

## 3.1. Cost Allocation Tags

- Use **cost allocation tags** to track your AWS costs on a detailed level.
- **AWS generated tags:**
  - Automatically applied to the resource you create.
  - Starts with Prefix aws: (e.g. aws: createdBy).
- **User-defined tags:**
  - Defined by the user.

## 3.2. AWS Resource Groups and Tagging

- [AWS Resource Groups and Tagging](/AWS%20Resource%20Groups.md)

## 3.3. Cost and Usage Reports

- Dive deeper into your AWS costs and usage.
- The AWS Cost & Usage Report contains the most comprehensive set of AWS cost and usage data available, including additional metadata about AWS services, pricing, and reservations (e.g., Amazon EC2 Reserved Instances (RIs)).
- The AWS Cost & Usage Report lists AWS usage for each service category used by an account and its IAM users in hourly or daily line items, as well as any tags that you have activated for cost allocation purposes.
- Can be integrated with Athena, Redshift or QuickSight.

## 3.4. Cost Explorer

- **Cost Explorer can be used to forecast usage up to 12 months based on the previous usage. It can also be used to choose an optimal Savings Plan. Cost Explorer has an easy-to-use interface that lets you visualize, understand, and manage your AWS costs and usage over time.**
- Visualize, understand, and manage your AWS costs and usage over time.
- Create custom reports that analyze cost and usage data.
- Analyze your data at a high level: total costs and usage across all accounts.
- Or Monthly, hourly, resource level granularity.
- Choose an optimal **Savings Plan** (to lower prices on your bill).
- **Forecast usage up to 12 months based on previous usage.**

## 3.5. AWS Cost Anomaly Detection

- **Continuously monitor your cost and usage using ML to detect unusual spends.**
- It learns your unique, historic spend patterns to detect one-time cost spike and/or continuous cost increases (you don't need to define thresholds).
- Monitor AWS services, member accounts, cost allocation tags, or cost categories.
- Sends you the anomaly detection report with root-cause analysis.
- Get notified with individual alerts or daily/weekly summary (using SNS).

# 4. Billing Alarms in CloudWatch

- Billing data metric is stored in CloudWatch `sa-east-1`.
- Billing data are for overall worldwide AWS costs.
- It's for actual cost, not for projected costs.
- Intended a simple alarm (not as powerful as AWS Budgets).

# 5. AWS Budgets

- **AWS Budgets gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount.**
- **Difference with CloudWatch Billing Alarms: CloudWatch Billing Alarms only send alerts when your costs and usage are exceeding your budget, not when it is forecasted to exceed your budget, while AWS Budgets does both.**
- Create budget and **send alarms when costs exceeds the budget**.
- 3 types of budgets: Usage, Cost, Reservation.
- **For Reserved Instances (RI)**
  - Track utilization.
  - Supports EC2, ElastiCache, RDS, Redshift.
- Up to 5 SNS notifications per budget.
- Can filter by: Service, Linked Account, Tag, Purchase Option, Instance Type, Region, Availability Zone, API Operation, etc...
- Same options as AWS Cost Explorer!
- 2 budgets are free, then $0.02/day/budget.

# 6. AWS Support Plans Pricing

## 6.1. AWS Basic Support Plan

- **Customer Service & Communities -** 24x7 access to customer service, documentation, whitepapers, and support forums.
- **AWS Trusted Advisor -** Access to the 7 core Trusted Advisor checks and guidance to provision your resources following best practices to increase performance and improve security.
- **AWS Personal Health Dashboard -** A personalized view of the health of AWS services, and alerts when your resources are impacted.

## 6.2. AWS Developer Support Plan

- All Basic Support Plan +
- **Business hours email access** to Cloud Support Associates.
- Unlimited cases / 1 primary contact.
- Case severity / response times:
  - General guidance: < 24 business hours.
  - System impaired: < 12 business hours.

## 6.3. AWS Business Support Plan (24/7)

- Intended to be used if you have **production workloads**.
- **Trusted Advisor** - Full set of checks + API access.
- **24x7 phone, email, and chat access** to Cloud Support Engineers.
- Unlimited cases / unlimited contacts.
- Access to Infrastructure Event Management **for additional fee**.
- Case severity / response times:
  - General guidance: < 24 business hours.
  - System impaired: < 12 business hours.
  - Production system impaired: < 4 hours.
  - Production system down: < 1 hour.

## 6.4. AWS Enterprise On-Ramp Support Plan (24/7)

- Intended to be used if you have production or business critical workloads.
- All of Business Support Plan +
- Access to a pool of **Technical Account Managers (TAM)**.
- **Concierge Support Team** (for billing and account best practices).
- **Infrastructure Event Management, Well-Architected & Operations Reviews**.
- Case severity / response times:
  - Production system impaired: < 4 hours.
  - Production system down: < 1 hour.
  - **Business-critical system down: < 30 minutes**.

## 6.5. AWS Enterprise Support Plan (24/7)

- Intended to be used if you have **mission critical workloads**.
- All of Business Support Plan +
- Access to a **designated** Technical Account Manager (TAM).
- **Concierge Support Team** (for billing and account best practices).
- **Infrastructure Event Management, Well-Architected & Operations Reviews**.
- Case severity / response times:
  - Production system impaired: < 4 hours.
  - Production system down: < 1 hour.
  - **Business-critical system down: < 15 minutes**.

# 7. Account Best Practices - Summary

- Operate multiple accounts using **Organizations**.
- Use **SCP** (service control policies) to restrict account power.
- Easily setup multiple accounts with best-practices with **AWS Control Tower**.
- **Use Tags & Cost Allocation Tags** for easy management & billing.
- **IAM guidelines**: MFA, least-privilege, password policy, password rotation.
- **Config** to record all resources configurations & compliance over time.
- **CloudFormation** to deploy stacks across accounts and regions.
- **Trusted Advisor** to get insights, Support Plan adapted to your needs.
- Send Service Logs and Access Logs to **S3 or CloudWatch Logs**.
- **CloudTrail** to record API calls made within your account.
- **If your Account is compromised:** change the root password, delete and rotate all passwords / keys, contact the AWS support.

# 8. Billing and Costing Tools - Summary

- **Compute Optimizer:** Recommends resources, configurations to reduce cost.
- **Pricing Calculator:** Cost of services on AWS.
- **Billing Dashboard:** High level overview + free tier dashboard.
- **Cost Allocation Tags:** Tag resources to create detailed reports.
- **Cost and Usage Reports:** Most comprehensive billing dataset.
- **Cost Explorer:** View current usage (detailed) and forecast usage.
- **Billing Alarms:** In `sa-east-1` - track overall and per-service billing.
- **Budgets:** More advanced - track usage, costs, RI, and get alerts.
- **Savings Plans:** Easy way to save based on long-term usage of AWS.

# 9. Cost Optimization Hub

- A feature in **AWS Billing and Cost Management**.
- Consolidates cost-saving **recommendations across accounts and Regions**.
- **Helps identify and act on**
  - Resource rightsizing.
  - Idle resource deletion.
  - Savings Plans.
  - Reserved Instances.
- Offers a **centralized dashboard**, reducing the need to check multiple services for cost opportunities.
