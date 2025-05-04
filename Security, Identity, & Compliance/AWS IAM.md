# AWS IAM - Identity and Access Management <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Users \& Groups](#1-users--groups)
- [2. Permissions](#2-permissions)
- [3. Policies inheritance](#3-policies-inheritance)
- [4. Policies Structure](#4-policies-structure)
- [5. Policy variations](#5-policy-variations)
  - [5.1. Policy Variables](#51-policy-variables)
  - [5.2. Policy Condition](#52-policy-condition)
  - [5.3. Policy Resource](#53-policy-resource)
  - [5.4. Policy Principal](#54-policy-principal)
- [6. Password Policy](#6-password-policy)
  - [6.1. Multi Factor Authentication - MFA](#61-multi-factor-authentication---mfa)
  - [6.2. MFA devices options in AWS](#62-mfa-devices-options-in-aws)
- [7. How can users access AWS?](#7-how-can-users-access-aws)
- [8. What's the AWS CLI?](#8-whats-the-aws-cli)
- [9. What's the AWS SDK?](#9-whats-the-aws-sdk)
- [10. Roles for Services](#10-roles-for-services)
- [11. Security Tools](#11-security-tools)
  - [11.1. IAM Access Advisor (user-level)](#111-iam-access-advisor-user-level)
- [12. Guidelines \& Best Practices](#12-guidelines--best-practices)
- [13. Certificate Store](#13-certificate-store)
- [14. Providing access to externally authenticated users (identity federation)](#14-providing-access-to-externally-authenticated-users-identity-federation)
  - [14.1. Custom identity broker application](#141-custom-identity-broker-application)
- [15. Shared Responsibility Model for IAM](#15-shared-responsibility-model-for-iam)
- [16. Identity-based policies and resource-based policies](#16-identity-based-policies-and-resource-based-policies)
- [17. Permissions boundary](#17-permissions-boundary)
  - [17.1. Use cases](#171-use-cases)
- [18. Access Analyzer](#18-access-analyzer)
- [19. IAM Roles vs Resource Based Policies](#19-iam-roles-vs-resource-based-policies)
- [20. Summary](#20-summary)

# 1. Users & Groups

- IAM = Identity and Access Management, **Global service**.
- **Root account** created by default, shouldn't be used or shared.
- **Users** are people within your organization, and can be grouped.
- **Groups** only contain users, not other groups.
- Users don't have to belong to a group, and user can belong to multiple groups.

![IAM - Users and groups](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMUsersAndGroups.png)

# 2. Permissions

- **Users or Groups** can be assigned JSON documents called policies.
- These policies define the **permissions** of the users.
- In AWS we apply the **least privilege** principle: don't give more permissions than a user needs.

# 3. Policies inheritance

![IAM - Policies](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMPoliciesInheritance.png)

# 4. Policies Structure

![IAM - Policies](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMPoliciesStructure.PNG)

- **Consists of**
  - **Version:** Policy language version, always include "YYYY-MM-DD".
  - **Id:** An identifier for the policy (optional).
  - **Statement:** one or more individual statements (required).
- **Statements consists of**
  - **Sid:** an identifier for the statement (optional).
  - **Effect:** whether the statement allows or denies access (Allow, Deny).
  - **Principal**
    - **Definition:** The entity that is allowed or denied access by the policy.
    - **Purpose:** Specifies "who" the policy applies to. This could be an IAM user, a role, an AWS service, or another AWS account.
    - **Example:** In an S3 bucket policy, the Principal field specifies which users or accounts can access the bucket.
  - **Action**: List of actions this policy allows or denies.
  - **Resource**
    - **Definition:** The AWS resource to which the policy applies.
    - **Purpose:** Specifies what resource(s) the policy will affect. For example, an S3 bucket, an IAM user, an EC2 instance, or a Lambda function.
    - **Example:** In a policy granting access to an S3 bucket, the Resource field defines the ARN (Amazon Resource Name) of the bucket or objects in it.
  - **Condition:** Conditions for when this policy is in effect (optional).
- **Remember that we can't modify these AWS-managed policies.**

# 5. Policy variations

## 5.1. Policy Variables

- Use AWS Identity and Access Management (IAM) policy variables as placeholders when we don't know the exact value of a resource or condition key when we write the policy.
  - Example:
    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": ["s3:ListBucket"],
          "Effect": "Allow",
          "Resource": ["arn:aws:s3:::mybucket"],
          "Condition": { "StringLike": { "s3:prefix": ["David/*"] } }
        },
        {
          "Action": ["s3:GetObject", "s3:PutObject"],
          "Effect": "Allow",
          "Resource": ["arn:aws:s3:::mybucket/David/*"]
        }
      ]
    }
    ```

## 5.2. Policy Condition

- The `Condition` element (or Condition block) lets we specify conditions for when a policy is in effect, like so
  ```json
    "Condition" : {
      "StringEquals" : {
        "aws:username" : "johndoe"
      }
    }
  ```
- This can not be used to address the requirements of the given use-case.

## 5.3. Policy Resource

- The Resource element specifies the object or objects that the statement covers. We specify a resource using an ARN.
- This cannot be used to address the requirements of the given use-case.

## 5.4. Policy Principal

- We can use the `Principal` element in a policy to specify the principal that is allowed or denied access to a resource (In IAM, a principal is a person or application that can make a request for an action or operation on an AWS resource.
- The principal is authenticated as the AWS account root user or an IAM entity to make requests to AWS).
- We cannot use the `Principal` element in an IAM identity-based policy.
- We can use it in the trust policies for IAM roles and in resource-based policies.

# 6. Password Policy

- Strong passwords = higher security for your account.
- In AWS, we can setup a password policy:
  - Set a minimum password length.
  - **Require specific character types**
    - Including uppercase letters.
    - Lowercase letters.
    - Numbers.
    - Non-alphanumeric characters.
- Allow all IAM users to change their own passwords.
- Require users to change their password after some time (password expiration).
- Prevent password re-use.

## 6.1. Multi Factor Authentication - MFA

- Users have access to your account and can possibly change configurations or delete resources in your AWS account.
- **We want to protect your Root Accounts and IAM users.**
- MFA = password we know + security device we own.
- **Main benefit of MFA**
  - If a password is stolen or hacked, the account is not compromised.

## 6.2. MFA devices options in AWS

- **Virtual MFA device**
  - Google AuthenLcator (phone only).
  - Authy (multi-device).
  - Support for multiple tokens on a single device.
- **Universal 2nd Factor (U2F) Security Key**
  - YubiKey by Yubico (3rd party):
    - Support for multiple tokens on a single device. Support for multiple root and IAM users using a single security key.
- **Hardware Key Fob MFA Device**
  - Provided by Gemalto (3rd party).
- **Hardware Key Fob MFA Device for AWS GovCloud (US)**
  - Provided by SurePassID (3rd party).

# 7. How can users access AWS?

- **To access AWS, we have three options**
  - **AWS Management Console** (protected by password + MFA).
  - **AWS Command Line Interface (CLI):** Protected by access keys.
  - **AWS Software Developer Kit (SDK)** - For code: protected by access keys.
- Access Keys are generated through the AWS Console.
- Users manage their own access keys.
- **Access Keys are secret, just like a password. Don't share them.**
  - Access Key ID ~= username.
  - Secret Access Key ~= password.
- **Example (Fake) Access Keys**
  - Access key ID: AKIASK4E37PV4983d6C.
  - Secret Access Key: AZPN3zojWozWCndIjhB0Unh8239a1bzbzO5fqqkZq.
  - **Remember: Don't share your access keys.**

# 8. What's the AWS CLI?

- A tool that enables we to interact with AWS services using commands in your command-line shell.
- Direct access to the public APIs of AWS services.
- We can develop scripts to manage your resources.
- It's [open-source](https://github.com/aws/aws-cli).
- Alternative to using AWS Management Console.

# 9. What's the AWS SDK?

- AWS Software Development Kit (AWS SDK).
- Language-specific APIs (set of libraries).
- Enables we to access and manage AWS services programmatically.
- Embedded within your application.
- **Supports**
  - SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++).
  - Mobile SDKs (Android, iOS, ...).
  - IoT Device SDKs (Embedded C, Arduino, ...).
- **Example:** AWS CLI is built on AWS SDK for Python.

# 10. Roles for Services

- Some AWS service will need to perform actions on your behalf.
- To do so, we will assign permissions to AWS services with IAM Roles.
- **Common roles**
  - EC2 Instance Roles.
  - Lambda Function Roles.
  - Roles for CloudFormation.

# 11. Security Tools

- **IAM Credentials Report (account-level)**
  - A report that lists all your account's users and the status of their various credentials.
- **IAM Access Advisor (user-level)**
  - Access advisor shows the service permissions granted to a user and when those services were last accessed.
  - We can use this information to revise your policies.

## 11.1. IAM Access Advisor (user-level)

- To help identify the unused roles, IAM reports the last-used timestamp that represents when a role was last used to make an AWS request.
  - Your security team can use this information to identify, analyze, and then confidently remove unused roles.
  - This helps improve the security posture of your AWS environments.
  - Additionally, by removing unused roles, we can simplify your monitoring and auditing efforts by focusing only on roles that are in use.

# 12. Guidelines & Best Practices

- Don't use the root account except for AWS account setup.
- One physical user = One AWS **user**.
- **Assign users to groups** and assign permissions to groups.
- Create a **strong password policy**.
- Use and enforce the use of **Multi Factor Authentication (MFA)**.
- Create and use **Roles** for giving permissions to AWS services.
- Use Access Keys for Programmatic Access (CLI / SDK).
- Audit permissions of your account with the IAM Credentials Report.
- **Never share IAM users & Access Keys.**

# 13. Certificate Store

- Use IAM as a certificate manager only when we must support HTTPS connections in a Region that is not supported by ACM.
- IAM securely encrypts your private keys and stores the encrypted version in IAM SSL certificate storage.
- IAM supports deploying server certificates in all Regions, but we must obtain your certificate from an external provider for use with AWS.
- We cannot upload an ACM certificate to IAM.
- Additionally, we cannot manage your certificates from the IAM Console.

# 14. Providing access to externally authenticated users (identity federation)

## 14.1. Custom identity broker application

![Custom Identity Federation Diagram](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMCustomIdentityFederation.png)

# 15. Shared Responsibility Model for IAM

- **AWS**
  - Infrastructure (global network security).
  - Configuration and vulnerability analysis.
  - Compliance validation.
- **You (Customer)**
  - Users, Groups, Roles, Policies management and monitoring.
  - Enable MFA on all accounts.
  - Rotate all your keys often.
  - Use IAM tools to apply appropriate permissions.
  - Analyze access patterns & review permissions.

# 16. Identity-based policies and resource-based policies

![Difference between both](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMIdentityBasedVsResourceBasedPolicies.png)

# 17. Permissions boundary

- IAM Permission Boundaries are supported for users and roles (not groups).
- Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get.
- Permissions boundary is a managed policy that is used for an IAM entity (user or role).
- The policy defines the maximum permissions that the identity-based policies can grant to an entity, but does not grant permissions.
- Can be used in combinations of [AWS Organizations SCP](/Management%20&%20Governance/AWS%20Organizations.md).
  ![AWS IAM Permission Boundaries](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMPermissionBoundaries.png)

## 17.1. Use cases

- Delegate responsibilities to non administrators within their permission boundaries, for example create new IAM users.
- Allow developers to self-assign policies and manage their own permissions, while making sure they can't "escalate" their privileges (= make themselves admin).
- Useful to restrict one specific user (instead of a whole account using Organizations & SCP).
- https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html

# 18. Access Analyzer

- AWS IAM Access Analyzer helps we identify the resources in your organization and accounts, such as Amazon S3 buckets or IAM roles, that are shared with an external entity. This lets you identify unintended access to your resources and data, which is a security risk.
- We can set the scope for the analyzer to an organization or an AWS account.
- This is your zone of trust.
- The analyzer scans all of the supported resources within your zone of trust.
- When Access Analyzer finds a policy that allows access to a resource from outside of your zone of trust, it generates an active finding.

# 19. IAM Roles vs Resource Based Policies

- **Cross account**
  - Attaching a resource-based policy to a resource (example: S3 bucket policy).
  - OR using a role as a proxy.
- **When we assume a role (user, application or service), we give up your original permissions and take the permissions assigned to the role.**
- When using a resource-based policy, the principal doesn't have to give up his permissions.
- **Example:** User in account A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B.
- **Supported by:** Amazon S3 buckets, SNS topics, SQS queues, etc...

# 20. Summary

- **Users:** Mapped to a physical user, has a password for AWS Console.
- **Groups:** Contains users only.
- **Policies:** JSON document that outlines permissions for users or groups.
- **Roles:** For EC2 instances or AWS services.
- **Security:** MFA + Password Policy.
- **AWS CLI:** Manage your AWS services using the command-line.
- **AWS SDK:** Manage your AWS services using a programming language.
- **Access Keys:** Access AWS using the CLI or SDK.
- **Audit:** IAM Credential Reports & IAM Access Advisor.
