# AWS STS - Security Token Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. STS - Security Token Service](#1-sts---security-token-service)
- [2. Using STS to Assume a Role](#2-using-sts-to-assume-a-role)
- [3. Cross account access with STS](#3-cross-account-access-with-sts)
- [4. STS with MFA](#4-sts-with-mfa)
- [5. IAM Best Practices](#5-iam-best-practices)
  - [5.1. General](#51-general)
  - [5.2. IAM Roles](#52-iam-roles)
  - [5.3. Cross Account Access](#53-cross-account-access)
- [6. Advanced IAM - Authorization Model Evaluation of Policies, simplified](#6-advanced-iam---authorization-model-evaluation-of-policies-simplified)
- [7. IAM Policies \& S3 Bucket Policies](#7-iam-policies--s3-bucket-policies)
  - [7.1. Example 1](#71-example-1)
  - [7.2. Example 2](#72-example-2)
  - [7.3. Example 3](#73-example-3)
  - [7.4. Example 4](#74-example-4)
- [8. Dynamic Policies with IAM](#8-dynamic-policies-with-iam)
- [9. Inline vs Managed Policies](#9-inline-vs-managed-policies)
- [10. Granting a User Permissions to Pass a Role to an AWS Service](#10-granting-a-user-permissions-to-pass-a-role-to-an-aws-service)

# 1. STS - Security Token Service

- Allows to grant limited and temporary access to AWS resources (up to 1 hour).
- `AssumeRole` - Is useful for allowing existing IAM users to access AWS resources that they don't already have access to.
  - For example, the user might need access to resources **in another AWS account (cross account)**.
  - It is also useful as a means to temporarily gain privileged access—for example, to provide multi-factor authentication (MFA).
  - We must call this API using existing IAM user credentials.
- `AssumeRoleWithSAML` - Return credentials for users logged with SAML.
- `AssumeRoleWithWebIdentity`
  - Return creds for users logged with an IdP (Facebook Login, Google Login, OIDC compatible...).
  - **AWS recommends against using this, and using Cognito Identity Pools instead.**
- `GetSessionToken` - Gives you a set of temporary credentials **based on your own IAM User**.
  - Returns a set of temporary security credentials to an existing IAM user.
  - This is useful for providing enhanced security, such as allowing AWS requests only when MFA is enabled for the IAM user.
  - Because the credentials are temporary, they provide enhanced security when you have an IAM user who accesses your resources through a less secure environment.
- `GetFederationToken` - Obtain temporary creds for a federated user.
- `GetCallerIdentity` - Return details about the IAM user or role used in the API call.
- `DecodeAuthorizationMessage` - Decode error message when an AWS API is denied.

# 2. Using STS to Assume a Role

- Define an IAM Role within your account or cross-account.
- Define which principals can access this IAM Role.
- Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (`AssumeRole` API).
- Temporary credentials can be valid between 15 minutes to 1 hour.

![AWS STS Assume a Role](/Images/AWSSTSAssumeRole.png)
![AWS STS Assume a Role](/Images/AWSSTSAssumeRole2.png)

# 3. Cross account access with STS

![STS Diagram](/Images/AWSSTSCrossAccount.png)

[Example scenario using separate development and production accounts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html)

# 4. STS with MFA

- Use `GetSessionToken` from STS.
- Appropriate IAM policy using IAM Conditions.
- `aws:MultiFactorAuthPresent:true`
- Reminder, `GetSessionToken` returns:
  - Access ID.
  - Secret Key.
  - Session Token.
  - Expiration date.

# 5. IAM Best Practices

## 5.1. General

- Never use Root Credentials, enable MFA for Root Account.
- **Grant Least Privilege**
  - Each Group / User / Role should only have the minimum level of permission it needs.
  - Never grant a policy with "\*" access to a service.
  - Monitor API calls made by a user in CloudTrail (especially Denied ones).
- Never ever ever store IAM key credentials on any machine but a personal computer or on-premise server.
- On premise server best practice is to call STS to obtain temporary security credentials.

## 5.2. IAM Roles

- EC2 machines should have their own roles.
- Lambda functions should have their own roles.
- ECS Tasks should have their own roles (ECS_ENABLE_TASK_IAM_ROLE=true).
- CodeBuild should have its own service role.
- Create a least-privileged role for any service that requires it.
- Create a role per application / lambda function (do not reuse roles).

## 5.3. Cross Account Access

- Define an IAM Role for another account to access.
- Define which accounts can access this IAM Role.
- Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (`AssumeRole` API).
- Temporary credentials can be valid between 15 minutes to 1 hour.

# 6. Advanced IAM - Authorization Model Evaluation of Policies, simplified

1. If there's an explicit DENY, end decision and DENY
2. If there's an ALLOW, end decision with ALLOW
3. Else DENY

# 7. IAM Policies & S3 Bucket Policies

- IAM Policies are attached to users, roles, groups.
- S3 Bucket Policies are attached to buckets.
- When evaluating if an IAM Principal can perform an operation X on a bucket, the union of its assigned IAM Policies and S3 Bucket Policies will be evaluated.

## 7.1. Example 1

- IAM Role attached to EC2 instance, authorizes RW to "my_bucket"
- No S3 Bucket Policy attached
- => EC2 instance **can** read and write to "my_bucket"

## 7.2. Example 2

- IAM Role attached to EC2 instance, authorizes RW to "my_bucket"
- S3 Bucket Policy attached, explicit deny to the IAM Role
- => EC2 instance **cannot** read and write to "my_bucket"

## 7.3. Example 3

- IAM Role attached to EC2 instance, no S3 bucket permissions
- S3 Bucket Policy attached, explicit RW allow to the IAM Role
- => EC2 instance **can** read and write to "my_bucket"

## 7.4. Example 4

- IAM Role attached to EC2 instance, explicit deny S3 bucket permissions
- S3 Bucket Policy attached, explicit RW allow to the IAM Role
- => EC2 instance **cannot** read and write to "my_bucket"

# 8. Dynamic Policies with IAM

- How do you assign each user a /home/<user> folder in an S3 bucket?
- Option 1:
  - Create an IAM policy allowing georges to have access to /home/georges.
  - Create an IAM policy allowing sarah to have access to /home/sarah.
  - Create an IAM policy allowing matt to have access to /home/matt.
  - ... One policy per user!
  - This doesn't scale.
- Option 2:
  - Create one dynamic policy with IAM.
  - Leverage the special policy variable ${aws:username}.

# 9. Inline vs Managed Policies

- AWS Managed Policy
  - Maintained by AWS
  - Good for power users and administrators
  - Updated in case of new services / new APIs
- Customer Managed Policy
  - Best Practice, re-usable, can be applied to many principals
  - Version Controlled + rollback, central change management
- Inline
  - Strict one-to-one relationship between policy and principal
  - Policy is deleted if you delete the IAM principal

# 10. Granting a User Permissions to Pass a Role to an AWS Service

- To configure many AWS services, you must pass an IAM role to the service (this happens only once during setup).
- The service will later assume the role and perform actions.
- Example of passing a role:
  - To an EC2 instance
  - To a Lambda function
  - To an ECS task
  - To CodePipeline to allow it to invoke other services
- For this, you need the IAM permission `iam:PassRole`.
- It often comes with `iam:GetRole` to view the role being passed.
- Example:

  ```
    {
      "Version": "2012-10-17",
      "Statement":[
        {
          "Effect": "Allow",
          "Action": [
            "ec2:*"
          ],
          "Resource": "*"
        },
        {
          "Effect": "Allow",
          "Action": "iam:PassRole",
          "Resource": "arn:aws:iam::123456789012:role/S3Access"
        }
      ]
    }
  ```

- Can a role be passed to any service?
  - **No: Roles can only be passed to what their trust allows.**
  - A trust policy for the role that allows the service to assume the role.
  - Example:
  ```
    {
      "Version": "2012-10-17",
        "Statement": {
        "Sid": "TrustPolicyStatementThatAllowsEC2ServiceToAssumeTheAttachedRole",
        "Effect": "Allow",
        "Principal": {
          "Service": "ec2.amazonaws.com"
        },
        "Action": "sts: AssumeRole"
      }
    }
  ```
