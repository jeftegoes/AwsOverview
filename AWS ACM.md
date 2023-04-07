# Amazon Certificate Manager (ACM)<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. AWS Private Certificate Authority (CA)](#2-aws-private-certificate-authority-ca)

# 1. Introduction

- Let's you easily provision, manage,and deploy SSL/TLS Certificates.
- Used to provide in-flight encryption for websites (HTTPS).
- Supports both public and private TLS certificates.
- Free of charge for public TLS certificates.
- Automatic TLS certificate renewal.
- Integrations with (load TLS certificates on):
  - Elastic Load Balancers.
  - CloudFront Distributions.
  - APIs on API Gateway Auto Scaling group.

![AWS Certificate Manager](Images/AWSCertificateManager.png)

# 2. AWS Private Certificate Authority (CA)

- Managed service allows you to create private Certificate Authorities (CA), including root and subordinaries CAs.
- Can issue and deploy end-entity X.509 certificates.
- Certificates are trusted only by your Organization (not the public Internet).
- Works for AWS services that are integrated with ACM.
- Use cases:
  - Encrypted TLS communication, Cryptographically signing code.
  - Authenticate users, computers, API endpoints, and IoT devices.
  - Enterprise customers building a Public Key Infrastructure (PKI).
