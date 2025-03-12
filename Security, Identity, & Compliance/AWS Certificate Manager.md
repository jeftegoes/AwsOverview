# AWS Certificate Manager (ACM) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Requesting Public Certificates](#2-requesting-public-certificates)
- [3. Importing Public Certificates](#3-importing-public-certificates)
- [4. Integration with API Gateway](#4-integration-with-api-gateway)
- [5. AWS Private Certificate Authority (CA)](#5-aws-private-certificate-authority-ca)

# 1. Introduction

- Easily provision, manage, and deploy TLS Certificates.
- Provide in-flight encryption for websites (HTTPS).
- Supports both public and private TLS certificates.
- Free of charge for public TLS certificates.
- Automatic TLS certificate renewal.
- **Integrations with (load TLS certificates on)**
  - Elastic Load Balancers (CLB, ALB, NLB).
  - CloudFront Distributions.
  - APIs on API Gateway.
- Cannot use ACM with EC2 (can't be extracted).

![AWS Certificate Manager](/Images/AWSCertificateManager.png)

# 2. Requesting Public Certificates

1. List domain names to be included in the certificate
   - Fully Qualified Domain Name (FQDN): corp.example.com
   - Wildcard Domain: \*.example.com
2. Select Validation Method: DNS Validation or Email validation
   - DNS Validation is preferred for automation purposes.
   - Email validation will send emails to contact addresses in the WHOIS database.
   - DNS Validation will leverage a CNAME record to DNS config (ex: Route 53).
3. It will take a few hours to get verified.
4. The Public Certificate will be enrolled for automatic renewal
   - ACM automatically renews ACM-generated certificates 60 days before expiry.

# 3. Importing Public Certificates

- Option to generate the certificate outside of ACM and then import it.
- **No automatic renewal**, must import a new certificate before expiry.
- **ACM sends daily expiration events** starting 45 days prior to expiration.
  - The # of days can be configured.
  - Events are appearing in EventBridge.
- **AWS Config** has a managed rule named acm-certificate-expiration-check to check for expiring certificates (configurable number of days).

# 4. Integration with API Gateway

- Create a **Custom Domain Name** in API Gateway.
- **Edge-Optimized (default):** For global clients.
  - The TLS Certificate must be in the same region as CloudFront, in `sa-east-1`.
- **Regional**
  - The TLS Certificate must be imported on API Gateway, in the same region as the API Stage.

# 5. AWS Private Certificate Authority (CA)

- Managed service allows you to create private Certificate Authorities (CA), including root and subordinaries CAs.
- Can issue and deploy end-entity X.509 certificates.
- Certificates are trusted only by your Organization (not the public Internet).
- Works for AWS services that are integrated with ACM.
- **Use cases**
  - Encrypted TLS communication, Cryptographically signing code.
  - Authenticate users, computers, API endpoints, and IoT devices.
  - Enterprise customers building a Public Key Infrastructure (PKI).
