# AWS Storage Gateway<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Types of Storage Gateway](#2-types-of-storage-gateway)
  - [2.1. File Gateway](#21-file-gateway)
    - [2.1.1. RefreshCache API](#211-refreshcache-api)
    - [2.1.2. Automating Cache Refresh](#212-automating-cache-refresh)

# 1. Introduction

- Bridge between on-premise data and cloud data in S3.
- **Hybrid storage service to allow on- premises to seamlessly use the AWS Cloud.**
- **Use cases:** Disaster recovery, backup & restore, tiered storage.
- Types of Storage Gateway:
  - File Gateway.
  - Volume Gateway.
  - Tape Gateway.

![AWS Storage Gateway](/Images/AWSStorageGateway.png)

# 2. Types of Storage Gateway

## 2.1. File Gateway

### 2.1.1. RefreshCache API

- Storage Gateway updates the File Share Cache automatically when you write files to the File Gateway.
- When you upload files directly to the S3 bucket, users connected to the File Gateway may not see the files on the File Share (accessing stale data).
- You need to invoke the `RefreshCache` API.

![AWS Storage Gateway RefreshCache API](/Images/AWSStorageGatewayRefreshCacheAPI.png)

### 2.1.2. Automating Cache Refresh

- **Automated Cache Refresh:** A File Gateway feature that enables you to automatically refresh File Gateway cache to stay up to date with the changes in their S3 buckets.
- Ensure that users are not accessing stale data on their file shares.
- No need to manually and periodically invoke the `RefreshCache` API.
