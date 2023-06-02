# IAM - Identity and Access Management and AWS CLI<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. IAM: Users \& Groups](#1-iam-users--groups)
- [2. IAM: Permissions](#2-iam-permissions)
- [3. IAM: Policies Structure](#3-iam-policies-structure)
- [4. IAM - Password Policy](#4-iam---password-policy)
  - [4.1. Multi Factor Authentication - MFA](#41-multi-factor-authentication---mfa)
  - [4.2. MFA devices options in AWS](#42-mfa-devices-options-in-aws)
- [5. How can users access AWS?](#5-how-can-users-access-aws)
- [6. What's the AWS CLI?](#6-whats-the-aws-cli)
- [7. What's the AWS SDK?](#7-whats-the-aws-sdk)
- [8. Roles for Services](#8-roles-for-services)
- [9. Security Tools](#9-security-tools)
- [10. Guidelines \& Best Practices](#10-guidelines--best-practices)
- [11. Certificate Store](#11-certificate-store)
- [12. Providing access to externally authenticated users (identity federation)](#12-providing-access-to-externally-authenticated-users-identity-federation)
  - [12.1. Custom identity broker application](#121-custom-identity-broker-application)
- [13. Shared Responsibility Model for IAM](#13-shared-responsibility-model-for-iam)

# 1. IAM: Users & Groups

- IAM = Identity and Access Management, **Global service**.
- **Root account** created by default, shouldn't be used or shared.
- **Users** are people within your organization, and can be grouped.
- **Groups** only contain users, not other groups.
- Users don't have to belong to a group, and user can belong to multiple groups.

![IAM - Users and groups](/Images/IAMUsersAndGroups.png)

# 2. IAM: Permissions

- **Users or Groups** can be assigned JSON documents called policies.
- These policies define the **permissions** of the users.
- In AWS you apply the **least privilege** principle: don't give more permissions than a user needs.

# 3. IAM: Policies Structure

- Consists of:
  - **Version**: policy language version, always include "YYYY-MM-DD".
  - **Id**: an identifier for the policy (optional).
  - **Statement**: one or more individual statements (required).
  - Statements consists of:
    - **Sid**: an identifier for the statement (optional).
    - **Effect**: whether the statement allows or denies access (Allow, Deny).
    - **Principal**: account/user/role to which this policy applied to.
    - **Action**: list of actions this policy allows or denies.
    - **Resource**: list of resources to which the actions applied to.
    - **Condition**: conditions for when this policy is in effect (optional).

![IAM - Policies](/Images/IAMPoliciesInheritance.png)
![IAM - Policies](/Images/IAMPoliciesStructure.PNG)

# 4. IAM - Password Policy

- Strong passwords = higher security for your account.
- In AWS, you can setup a password policy:
  - Set a minimum password length.
  - Require specific character types:
    - Including uppercase letters.
    - Lowercase letters.
    - Numbers.
    - Non-alphanumeric characters.
- Allow all IAM users to change their own passwords.
- Require users to change their password after some time (password expiration).
- Prevent password re-use.


## 4.1. Multi Factor Authentication - MFA

- Users have access to your account and can possibly change configurations or delete resources in your AWS account.
- **You want to protect your Root Accounts and IAM users.**
- MFA = password you know + security device you own.
- Main benefit of MFA:
  - If a password is stolen or hacked, the account is not compromised.

## 4.2. MFA devices options in AWS

- Virtual MFA device:
  - Google AuthenLcator (phone only).
  - Authy (multi-device).
  - Support for multiple tokens on a single device.
- Universal 2nd Factor (U2F) Security Key:
  - YubiKey by Yubico (3rd party):
    - Support for multiple tokens on a single device. Support for multiple root and IAM users using a single security key.
- Hardware Key Fob MFA Device:
  - Provided by Gemalto (3rd party).
- Hardware Key Fob MFA Device for AWS GovCloud (US):
  - Provided by SurePassID (3rd party).

# 5. How can users access AWS?

- To access AWS, you have three options:
  - **AWS Management Console** (protected by password + MFA).
  - **AWS Command Line Interface (CLI):** protected by access keys.
  - **AWS Software Developer Kit (SDK)** - for code: protected by access keys.
- Access Keys are generated through the AWS Console.
- Users manage their own access keys.
- **Access Keys are secret, just like a password. Don't share them.**
  - Access Key ID ~= username.
  - Secret Access Key ~= password.
- Example (Fake) Access Keys.
  - Access key ID: AKIASK4E37PV4983d6C.
  - Secret Access Key: AZPN3zojWozWCndIjhB0Unh8239a1bzbzO5fqqkZq.
  - **Remember: don't share your access keys.**

# 6. What's the AWS CLI?

- A tool that enables you to interact with AWS services using commands in your command-line shell.
- Direct access to the public APIs of AWS services.
- You can develop scripts to manage your resources.
- It's open-source https://github.com/aws/aws-cli.
- Alternative to using AWS Management Console.

# 7. What's the AWS SDK?

- AWS Software Development Kit (AWS SDK).
- Language-specific APIs (set of libraries).
- Enables you to access and manage AWS services programmatically.
- Embedded within your application.
- Supports:
  - SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++).
  - Mobile SDKs (Android, iOS, ...).
  - IoT Device SDKs (Embedded C, Arduino, ...).
- Example: AWS CLI is built on AWS SDK for Python.

# 8. Roles for Services

- Some AWS service will need to perform actions on your behalf.
- To do so, we will assign permissions to AWS services with IAM Roles.
- Common roles:
  - EC2 Instance Roles.
  - Lambda Function Roles.
  - Roles for CloudFormation.

# 9. Security Tools

- **IAM Credentials Report (account-level):**
  - A report that lists all your account's users and the status of their various credentials.
- **IAM Access Advisor (user-level):**
  - Access advisor shows the service permissions granted to a user and when those services were last accessed.
  - You can use this information to revise your policies.

# 10. Guidelines & Best Practices

- Don't use the root account except for AWS account setup.
- One physical user = One AWS user.
- **Assign users to groups** and assign permissions to groups.
- Create a **strong password policy**.
- Use and enforce the use of **Multi Factor Authentication (MFA)**.
- Create and use **Roles** for giving permissions to AWS services.
- Use Access Keys for Programmatic Access (CLI / SDK).
- Audit permissions of your account with the IAM Credentials Report.
- **Never share IAM users & Access Keys.**

# 11. Certificate Store

- Use IAM as a certificate manager only when you must support HTTPS connections in a Region that is not supported by ACM.
- IAM securely encrypts your private keys and stores the encrypted version in IAM SSL certificate storage.
- IAM supports deploying server certificates in all Regions, but you must obtain your certificate from an external provider for use with AWS.
- You cannot upload an ACM certificate to IAM.
- Additionally, you cannot manage your certificates from the IAM Console.

# 12. Providing access to externally authenticated users (identity federation)

## 12.1. Custom identity broker application

![Custom Identity Federation Diagram](Images/AWSIAMCustomIdentityFederation.png)

# 13. Shared Responsibility Model for IAM

- AWS:
  - Infrastructure (global network security).
  - Configuration and vulnerability analysis.
  - Compliance validation.
- You (Customer):
  - Users, Groups, Roles, Policies management and monitoring.
  - Enable MFA on all accounts.
  - Rotate all your keys often.
  - Use IAM tools to apply appropriate permissions.
  - Analyze access patterns & review permissions.