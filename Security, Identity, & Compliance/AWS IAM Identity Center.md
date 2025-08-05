# AWS IAM Identity Center (successor to AWS Single Sign-On) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Login flow](#2-login-flow)
- [3. AWS IAM Identity Center](#3-aws-iam-identity-center)
- [4. IAM Identity Center](#4-iam-identity-center)
- [5. Fine-grained Permissions and Assignments](#5-fine-grained-permissions-and-assignments)
- [6. External IdPs](#6-external-idps)
- [7. Attribute-Based Access Control (ABAC)](#7-attribute-based-access-control-abac)
- [8. Multi-Factor Authentication (MFA)](#8-multi-factor-authentication-mfa)

# 1. Introduction

- Successor to AWS Single Sign-On.
- One login (single sign-on) for all your.
  - **AWS accounts in AWS Organizations.**
  - Business cloud applications (e.g., Salesforce, Box, Microsoft 365...).
  - SAML2.0-enabled applications.
  - EC2 Windows Instances.
- **Identity providers**
  - Built-in identity store in IAM Identity Center.
  - 3rd party: Active Directory (AD), OneLogin, Okta...

# 2. Login flow

![Single Sign-ON](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMIdentitySSO.png)

# 3. AWS IAM Identity Center

![alt](/Images/Security,%20Identity,%20&%20Compliance/AWSIAMIdentityCenterDiagram.png)

# 4. IAM Identity Center

TODO: DIAGRAM

# 5. Fine-grained Permissions and Assignments

- **Multi-Account Permissions**
  - Manage access across AWS accounts in your AWS Organization.
  - Permission Sets - a collection of one or more IAM Policies assigned to users and groups to define AWS access.
- **Application Assignments**
  - SSO access to many SAML 2.0 business applications (Salesforce, Box, Microsoft 365...).
  - Provide required URLs, certificates, and metadata.
- **Attribute-Based Access Control (ABAC)**
  - Fine-grained permissions based on users' attributes stored in IAM Identity Center Identity Store.
  - **Example:** Cost center, title, locale...
  - **Use case:** Define permissions once, then modify AWS access by changing the attributes.

TODO: DIAGRAM

# 6. External IdPs

- **SAML 2.0:** Authenticate identities from external IdP.
  - Users sign into AWS access portal using their corporate identities.
  - Supports Okta, Azure AD, OneLogin...
  - SAML 2.0 does NOT provide a way to query the IdP to learn about users and groups.
  - You must create users and groups in the IAM Identity Center that are identical to the users and groups in the External IdP.
- **SCIM:** Automatic provisoning (synchronization) of user identities from an external IdP into IAM Identity Center.
  - SCIM = System for Cross-domain Identity Management.
  - Must be supported by the external IdP.
  - Perfect complement to using SAML 2.0.

# 7. Attribute-Based Access Control (ABAC)

- Fine-grained permissions based on users' attributes stored in IAM Identity Center Identity Store.
- User attributes can be used in **IAM Identity Center Permission Sets** and **Resource-based Policies**.
- Example: cost center, title, locale...
- Use case: Define permissions once, then modify AWS access by changing the attributes.
- **User attributes are mapped from the IdP as key-value pairs.**

# 8. Multi-Factor Authentication (MFA)

- Supports Multi-Factor Authentication (MFA) with authentication modes:
  - **Every Time They Sign-in (Always-on).**
  - **Only When Their Sign-in Context Changes (Context-aware):** Analyzes user behavior (e.g., device, browser, location...).
