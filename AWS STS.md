# AWS STS - Security Token Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. STS – Security Token Service](#1-sts--security-token-service)
- [2. Using STS to Assume a Role](#2-using-sts-to-assume-a-role)
- [3. Cross account access with STS](#3-cross-account-access-with-sts)
- [4. STS with MFA](#4-sts-with-mfa)

# 1. STS – Security Token Service

- Allows to grant limited and temporary access to AWS resources (up to 1 hour).
- **AssumeRole:** Assume roles within your account or cross account.
- **AssumeRoleWithSAML:** return credentials for users logged with SAML.
- **AssumeRoleWithWebIdentity:**
  - Return creds for users logged with an IdP (Facebook Login, Google Login, OIDC compatible...).
  - **AWS recommends against using this, and using Cognito Identity Pools instead.**
- **GetSessionToken:** for MFA, from a user or AWS account root user.
- **GetFederationToken:** obtain temporary creds for a federated user.
- **GetCallerIdentity:** return details about the IAM user or role used in the API call.
- **DecodeAuthorizationMessage:** decode error message when an AWS API is denied.

# 2. Using STS to Assume a Role

- Define an IAM Role within your account or cross-account.
- Define which principals can access this IAM Role.
- Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (AssumeRole API).
- Temporary credentials can be valid between 15 minutes to 1 hour.

![AWS STS Assume a Role](Images/AWSSTSAssumeRole.png)

# 3. Cross account access with STS

[Example scenario using separate development and production accounts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html)

# 4. STS with MFA

- Use GetSessionToken from STS.
- Appropriate IAM policy using IAM Conditions.
- **aws:MultiFactorAuthPresent:true**
- Reminder, GetSessionToken returns:
  - Access ID.
  - Secret Key.
  - Session Token.
  - Expiration date.
