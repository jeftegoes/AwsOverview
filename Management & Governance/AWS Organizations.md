# AWS Organizations <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Advantages](#11-advantages)
- [2. OrganizationAccountAccessRole](#2-organizationaccountaccessrole)
- [3. Multi account strategies](#3-multi-account-strategies)
- [4. Feature Modes](#4-feature-modes)
- [5. Reserved Instances](#5-reserved-instances)
- [6. Moving Accounts](#6-moving-accounts)
- [7. Service Control Policies (SCP)](#7-service-control-policies-scp)
- [8. Resource Policies \& aws:PrincipalOrgID](#8-resource-policies--awsprincipalorgid)

# 1. Introduction

- Global service.
- Allows to manage **multiple AWS accounts**.
- The main account is the master account.
- Member accounts can only be part of one organization.
- **Shared reserved instances and Savings Plans discounts across accounts.**
- **Cost Benefits**
  - **Consolidated Billing** across all accounts - single payment method.
  - Pricing benefits from **aggregated usage** (volume discount for EC2, S3...).
  - **Pooling of Reserved EC2 instances** for optimal savings.
- API is available to **automate AWS account creation**.
- **Restrict account privileges using Service Control Policies (SCP)**.

## 1.1. Advantages

- Multi Account vs One Account Multi VPC.
- Use tagging standards for billing purposes.
- Enable CloudTrail on all accounts, send logs to central S3 account.
- Send CloudWatch Logs to central logging account.
- Establish Cross Account Roles for Admin purposes.
- **Security: Service Control Policies (SCP)**
  - IAM policies applied to OU or Accounts to restrict Users and Roles.
  - They do not apply to the management account (full admin power).
  - Must have an explicit allow from the root through each OU in the direct path to the target account (does not allow anything by default - like IAM).

# 2. OrganizationAccountAccessRole

- IAM role which grants full administrator permissions in the Member account to the Management account.
- Used to perform admin tasks in the Member accounts (e.g., creating IAM users).
- Could be assumed by IAM users in the Management account.
- Automatically added to all new Member accounts created with AWS Organizations.
- Must be created manually if you invite an existing Member account.

# 3. Multi account strategies

- Create accounts per:
  - **Department**
  - **Cost center**
  - **Dev / test / prod**
  - Based on **regulatory restrictions** (using SCP), for **better resource isolation** (ex: VPC), to have **separate per-account service limits**, isolated account for **logging**.
- Multi Account vs One Account Multi VPC.
- Use tagging standards for billing purposes.
- Enable CloudTrail on all accounts, send logs to central S3 account.
- Send CloudWatch Logs to central logging account.
- Strategy to create an account for security.

# 4. Feature Modes

- Consolidated billing features:
  - Consolidated Billing across all accounts - single payment method.
  - Pricing benefits from aggregated usage (volume discount for EC2, S3...).
- All Features (Default):
  - Includes consolidated billing features, SCP.
  - Invited accounts must approve enabling all features.
  - Ability to apply an SCP to prevent member accounts from leaving the org.
  - Can't switch back to Consolidated Billing Features only.

# 5. Reserved Instances

- For billing purposes, the consolidated billing feature of AWS Organizations treats all the accounts in the organization as one account.
- This means that **all accounts** in the organization can receive the hourly cost benefit of Reserved Instances that are purchased by any other account.
- **The payer account (Management account) of an organization** can turn off Reserved Instance (RI) discount and Savings Plans discount sharing for any accounts in that organization, including the payer account.
- This means that RIs and Savings Plans discounts aren't shared between any accounts that have sharing turned off.
- To share an RI or Savings Plans discount with an account, both accounts must have sharing turned on.

# 6. Moving Accounts

1. Remove the member account from the old AWS Organization.
2. Send an invite to the member account from the new AWS Organization.
3. Accept the invite to the new Organization from the member account.

# 7. Service Control Policies (SCP)

- **Service control policies (SCPs) are a type of organization policy that you can use to manage permissions in your organization. An SCP spans all IAM users, groups, and roles, including the AWS account root user.**
- Define allowlist or blocklist IAM actions.
- Applied at the **OU - Organizational Unit** or **Account** level.
- Does not apply to the Master Account.
- SCP is applied to all the **Users and Roles** of the Account, including Root user.
- The SCP does not affect service-linked roles.
  - Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs.
- SCP must have an explicit Allow (does not allow anything by default).
- **Use cases**
  - Restrict access to certain services (for example: can't use EMR).
  - Enforce PCI compliance by explicitly disabling services.

# 8. Resource Policies & aws:PrincipalOrgID

- `aws:PrincipalOrgID` can be used in any resource policies to restrict access to accounts that are member of an AWS Organization.
