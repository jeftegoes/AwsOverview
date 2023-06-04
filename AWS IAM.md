# IAM - Identity and Access Management and AWS CLI<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. IAM: Users \& Groups](#1-iam-users--groups)
- [2. IAM: Permissions](#2-iam-permissions)
- [3. IAM: Policies Structure](#3-iam-policies-structure)
- [4. Policy variations](#4-policy-variations)
  - [4.1. Policy Variables](#41-policy-variables)
  - [4.2. Policy Condition](#42-policy-condition)
  - [4.3. Policy Resource](#43-policy-resource)
  - [4.4. Policy Principal](#44-policy-principal)
- [5. IAM - Password Policy](#5-iam---password-policy)
  - [5.1. Multi Factor Authentication - MFA](#51-multi-factor-authentication---mfa)
  - [5.2. MFA devices options in AWS](#52-mfa-devices-options-in-aws)
- [6. How can users access AWS?](#6-how-can-users-access-aws)
- [7. What's the AWS CLI?](#7-whats-the-aws-cli)
- [8. What's the AWS SDK?](#8-whats-the-aws-sdk)
- [9. Roles for Services](#9-roles-for-services)
- [10. Security Tools](#10-security-tools)
- [11. Guidelines \& Best Practices](#11-guidelines--best-practices)
- [12. Certificate Store](#12-certificate-store)
- [13. Providing access to externally authenticated users (identity federation)](#13-providing-access-to-externally-authenticated-users-identity-federation)
  - [13.1. Custom identity broker application](#131-custom-identity-broker-application)
- [14. Shared Responsibility Model for IAM](#14-shared-responsibility-model-for-iam)
- [15. Identity-based policies and resource-based policies](#15-identity-based-policies-and-resource-based-policies)
- [16. Permissions boundary](#16-permissions-boundary)

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

# 4. Policy variations

## 4.1. Policy Variables

- Use AWS Identity and Access Management (IAM) policy variables as placeholders when you don't know the exact value of a resource or condition key when you write the policy.
  - Example:
    ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Action": ["s3:ListBucket"],
            "Effect": "Allow",
            "Resource": ["arn:aws:s3:::mybucket"],
            "Condition": {"StringLike": {"s3:prefix": ["David/*"]}}
          },
          {
            "Action": [
              "s3:GetObject",
              "s3:PutObject"
            ],
            "Effect": "Allow",
            "Resource": ["arn:aws:s3:::mybucket/David/*"]
          }
        ]
      }
    ```

## 4.2. Policy Condition

- The `Condition` element (or Condition block) lets you specify conditions for when a policy is in effect, like so - `"Condition" : { "StringEquals" : { "aws:username" : "johndoe" }}`.
  - This can not be used to address the requirements of the given use-case.

## 4.3. Policy Resource

- The Resource element specifies the object or objects that the statement covers. You specify a resource using an ARN.
- This cannot be used to address the requirements of the given use-case.

## 4.4. Policy Principal

- You can use the `Principal` element in a policy to specify the principal that is allowed or denied access to a resource (In IAM, a principal is a person or application that can make a request for an action or operation on an AWS resource.
- The principal is authenticated as the AWS account root user or an IAM entity to make requests to AWS).
- You cannot use the `Principal` element in an IAM identity-based policy.
- You can use it in the trust policies for IAM roles and in resource-based policies.

# 5. IAM - Password Policy

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

## 5.1. Multi Factor Authentication - MFA

- Users have access to your account and can possibly change configurations or delete resources in your AWS account.
- **You want to protect your Root Accounts and IAM users.**
- MFA = password you know + security device you own.
- Main benefit of MFA:
  - If a password is stolen or hacked, the account is not compromised.

## 5.2. MFA devices options in AWS

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

# 6. How can users access AWS?

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

# 7. What's the AWS CLI?

- A tool that enables you to interact with AWS services using commands in your command-line shell.
- Direct access to the public APIs of AWS services.
- You can develop scripts to manage your resources.
- It's open-source https://github.com/aws/aws-cli.
- Alternative to using AWS Management Console.

# 8. What's the AWS SDK?

- AWS Software Development Kit (AWS SDK).
- Language-specific APIs (set of libraries).
- Enables you to access and manage AWS services programmatically.
- Embedded within your application.
- Supports:
  - SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++).
  - Mobile SDKs (Android, iOS, ...).
  - IoT Device SDKs (Embedded C, Arduino, ...).
- Example: AWS CLI is built on AWS SDK for Python.

# 9. Roles for Services

- Some AWS service will need to perform actions on your behalf.
- To do so, we will assign permissions to AWS services with IAM Roles.
- Common roles:
  - EC2 Instance Roles.
  - Lambda Function Roles.
  - Roles for CloudFormation.

# 10. Security Tools

- **IAM Credentials Report (account-level):**
  - A report that lists all your account's users and the status of their various credentials.
- **IAM Access Advisor (user-level):**
  - Access advisor shows the service permissions granted to a user and when those services were last accessed.
  - You can use this information to revise your policies.

# 11. Guidelines & Best Practices

- Don't use the root account except for AWS account setup.
- One physical user = One AWS user.
- **Assign users to groups** and assign permissions to groups.
- Create a **strong password policy**.
- Use and enforce the use of **Multi Factor Authentication (MFA)**.
- Create and use **Roles** for giving permissions to AWS services.
- Use Access Keys for Programmatic Access (CLI / SDK).
- Audit permissions of your account with the IAM Credentials Report.
- **Never share IAM users & Access Keys.**

# 12. Certificate Store

- Use IAM as a certificate manager only when you must support HTTPS connections in a Region that is not supported by ACM.
- IAM securely encrypts your private keys and stores the encrypted version in IAM SSL certificate storage.
- IAM supports deploying server certificates in all Regions, but you must obtain your certificate from an external provider for use with AWS.
- You cannot upload an ACM certificate to IAM.
- Additionally, you cannot manage your certificates from the IAM Console.

# 13. Providing access to externally authenticated users (identity federation)

## 13.1. Custom identity broker application

![Custom Identity Federation Diagram](Images/AWSIAMCustomIdentityFederation.png)

# 14. Shared Responsibility Model for IAM

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

# 15. Identity-based policies and resource-based policies

![Difference between both](Images/AWSIAMIdentityBasedVsResourceBasedPolicies.png)

# 16. Permissions boundary

- Permissions boundary is a managed policy that is used for an IAM entity (user or role).
- The policy defines the maximum permissions that the identity-based policies can grant to an entity, but does not grant permissions.
