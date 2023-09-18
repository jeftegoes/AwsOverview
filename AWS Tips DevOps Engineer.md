# AWS Tips to AWS Certified DevOps Engineer - Professional<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. IAM](#1-iam)
- [2. AZs](#2-azs)

# 1. IAM

- AWS proactively monitors popular code repository sites for IAM access keys that have been publicly exposed.
- Upon detection of an exposed IAM access key, AWS Health generates an `AWS_RISK_CREDENTIALS_EXPOSED` event in the AWS account related to the exposed key.

# 2. AZs

- With their own power infrastructure, the AZs are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles of each other).
