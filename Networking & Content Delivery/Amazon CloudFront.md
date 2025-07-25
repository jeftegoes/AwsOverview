# Amazon CloudFront <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Origins](#11-origins)
  - [1.2. CloudFront at a high level](#12-cloudfront-at-a-high-level)
  - [1.3. Restricting access to an Amazon S3 origin](#13-restricting-access-to-an-amazon-s3-origin)
- [2. CloudFront vs S3 Cross-Region Replication](#2-cloudfront-vs-s3-cross-region-replication)
- [3. Caching](#3-caching)
  - [3.1. Cache Key](#31-cache-key)
  - [3.2. Cache Policy](#32-cache-policy)
    - [3.2.1. Managing cache expiration](#321-managing-cache-expiration)
    - [3.2.2. HTTP Headers](#322-http-headers)
    - [3.2.3. Cache Policy Query Strings](#323-cache-policy-query-strings)
  - [3.3. Origin Request Policy](#33-origin-request-policy)
  - [3.4. Cache invalidations](#34-cache-invalidations)
  - [3.5. Cache Behaviors](#35-cache-behaviors)
- [4. Geo Restriction](#4-geo-restriction)
- [5. Signed URL / Signed Cookies](#5-signed-url--signed-cookies)
  - [5.1. Signed URL vs S3 Pre-Signed URL](#51-signed-url-vs-s3-pre-signed-url)
    - [5.1.1. CloudFront Signed URL](#511-cloudfront-signed-url)
    - [5.1.2. S3 Pre-Signed URL](#512-s3-pre-signed-url)
  - [5.2. Signed URL Process](#52-signed-url-process)
- [6. Pricing](#6-pricing)
  - [6.1. Price Classes](#61-price-classes)
- [7. Multiple Origin](#7-multiple-origin)
  - [7.1. Origin Groups](#71-origin-groups)
- [8. Field Level Encryption](#8-field-level-encryption)
- [9. Real Time Logs](#9-real-time-logs)
- [10. HTTPS for communication between viewers and CloudFront](#10-https-for-communication-between-viewers-and-cloudfront)
- [11. CloudFront and ACM Certificate Region Requirement](#11-cloudfront-and-acm-certificate-region-requirement)
- [12. Origin Access Control (OAC)](#12-origin-access-control-oac)

# 1. Introduction

- Content Delivery Network (CDN).
- **Improves read performance, content is cached at the edge.**
- Improves users experience.
- 216 Point of Presence globally (edge locations).
- **DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall.**
- **IMPORTANT!** For **objects smaller than 1 GB** or total data sets under 1 GB, use **Amazon CloudFront's PUT/POST** commands for better performance.
  - **Otherwise**, use **S3 Transfer Acceleration (S3TA)** to achieve faster and more efficient transfers.

## 1.1. Origins

- **S3 bucket**
  - For distributing files and caching them at the edge.
  - Enhanced security with CloudFront **Origin Access Control (OAC)**.
    - OAC is replacing Origin Access Identity (OAI).
    - We want to enforce users to access the website only through CloudFront.
  - CloudFront can be used as an ingress (to upload files to S3).
    ![Amazon CloudFront S3 as an Origin](/Images/Networking%20&%20Content%20Delivery/AmazonCloudFrontS3Origin.png)
- **Custom Origin (HTTP)**
  - Application Load Balancer.
    ![Amazon CloudFront ALB as an Origin](/Images/Networking%20&%20Content%20Delivery/AmazonCloudFrontALBOrigin.png)
  - EC2 instance.
    ![Amazon CloudFront EC2 as an Origin](/Images/Networking%20&%20Content%20Delivery/AmazonCloudFrontEC2Origin.png)
  - S3 website (must first enable the bucket as a static S3 website).
  - Any HTTP backend we want.
- **IMPORTANT!** CloudFront doesn't require the origin to be hosted on AWS-it can point to any publicly accessible HTTP/HTTPS server, including one running in your on-premise data center.

## 1.2. CloudFront at a high level

![Amazon CloudFront High Level](/Images/Networking%20&%20Content%20Delivery/AmazonCloudFrontHighLevel.png)

## 1.3. Restricting access to an Amazon S3 origin

- All Amazon S3 buckets in all AWS Regions, including opt-in Regions launched after December 2022.
- Amazon S3 server-side encryption with AWS KMS (SSE-KMS)
- Dynamic requests (PUT and DELETE) to Amazon S3

# 2. CloudFront vs S3 Cross-Region Replication

- **CloudFront**
  - Global Edge network.
  - Files are cached for a TTL (maybe a day).
  - **Great for static content that must be available everywhere.**
- **S3 Cross-Region Replication**
  - Must be setup for each region we want replication to happen.
  - Files are updated in near real-time.
  - Read only.
  - **Great for dynamic content that needs to be available at low-latency in few regions.**

# 3. Caching

- The cache lives at each CloudFront **Edge Location**.
- CloudFront identifies each object in the cache using the **Cache Key** (See next section).
- We want to maximize the cache hit rate to minimize requests on the origin.
- We can invalidate part of the cache using the **CreateInvalidation** API.

## 3.1. Cache Key

- A unique identifier for every object in the cache.
- By default, consists of **hostname + resource portion of the URL**.
- If we have an application that serves up content that varies based on user, device, language, location...
- We can add other elements (HTTP headers, cookies, query strings) to the Cache Key using **CloudFront Cache Policies**.

## 3.2. Cache Policy

- Cache based on:
  - **HTTP Headers:** None - Whitelist.
  - **Cookies:** None - Whitelist - Include All-Except - All.
  - **Query Strings:** None - Whitelist - Include All-Except - All.
- Control the TTL (0 seconds to 1 year), can be set by the origin using the `Cache-Control` header, `Expires` header...
- Create your own policy or use Predefined Managed Policies.
- _All HTTP headers, cookies, and query strings that we include in the Cache Key are automatically included in origin requests._

### 3.2.1. Managing cache expiration

- **By default, each file automatically expires after 24 hours**, but we can change the default behavior in two ways:
  - To change the cache duration for all files that match the same path pattern, we can change the CloudFront settings for:
    - **Minimum TTL**
    - **Maximum TTL**
    - **Default TTL**
  - To change the cache duration for an individual file, we can configure your origin to add a `Cache-Control` header with the `max-age` or `s-maxage` directive, or an `Expires` header to the file.

### 3.2.2. HTTP Headers

- None:
  - Don't include any headers in the Cache Key (except default).
  - Headers are not forwarded (except default).
  - Best caching performance.
- Whitelist:
  - _Only specified headers_ included in the Cache Key.
  - Specified headers are also forwarded to Origin.
- Example:

  ```
  GET /blogs/myblog.html HTTP/1.1
  Host: mywebsite.com
  User-Agent: Mozilla/5.0 (Mac OS X 10_15_2....)
  Date: Tue, 28 Jan 2021 17:01:57 GMT
  Authorization: SAPISIDHASH fdd00ecee39fe....
  Keep-Alive: 300
  Language: fr-fr
  ```

### 3.2.3. Cache Policy Query Strings

- **None**
  - Don't include any query strings in the Cache Key.
  - Query strings are not forwarded.
- **Whitelist**
  - Only specified query strings included in the Cache Key.
  - Only specified query strings are forwarded.
- **Include All-Except**
  - Include all query strings in the Cache Key except the specified list.
  - All query strings are forwarded except the specified list.
- **All**
  - Include all query strings in the Cache Key.
  - All query strings are forwarded.
  - Worst caching performance.
- Example: `GET /image/cat.jpg?border=red&size=large HTTP/1.1`

## 3.3. Origin Request Policy

- Specify values that we want to include in origin requests **without including them in the Cache Key (no duplicated cached content)**.
- **We can include**
  - **HTTP headers:** None - Whitelist - All viewer headers options.
  - **Cookies:** None - Whitelist - All.
  - **Query Strings:** None - Whitelist - All.
- Ability to add CloudFront HTTP headers and Custom Headers to an origin request that were not included in the viewer request.
- Create your own policy or use Predefined Managed Policies.

## 3.4. Cache invalidations

- In case we update the back-end origin, CloudFront doesn't know about it and will only get the refreshed content after the TTL has expired.
- However, we can force an entire or partial cache refresh (thus bypassing the TTL) by performing a **CloudFront Invalidation**.
- We can invalidate all files (\*) or a special path (/Images/\*).
  ![Cache Invalidations](/Images/Networking%20&%20Content%20Delivery/AmazonCloudFrontCacheInvalidations.png)

## 3.5. Cache Behaviors

- Configure different settings for a given URL path pattern.
- Example: one specific cache behavior to `Images/*.jpg` files on your origin web server.
- Route to different kind of origins/origin groups based on the content type or path pattern:
  - /Images/\*
  - /api/\*
  - /\* (default cache behavior)
- When adding additional Cache Behaviors, the Default Cache Behavior is always the last to be processed **and is always /\***

# 4. Geo Restriction

- **We can restrict who can access your distribution**
  - **Whitelist:** Allow your users to access your content only if they're in one of the countries on a list of approved countries.
  - **Blacklist:** Prevent your users from accessing your content if they're in one of the countries on a blacklist of banned countries.
- The "country" is determined using a 3rd party Geo-IP database.
- **Use case:** Copyright Laws to control access to content.

# 5. Signed URL / Signed Cookies

- We want to distribute paid shared content to premium users over the world.
- To **Restrict Viewer Access**, we can create a CloudFront Signed URL / Cookie.
- How long should the URL be valid for?
  - Shared content (movie, music): Make it short (a few minutes).
  - Private content (private to the user): We can make it last for years.
- Signed URL = access to individual files (one signed URL per file).
- Signed Cookies = access to multiple files (one signed cookie for many files).

## 5.1. Signed URL vs S3 Pre-Signed URL

### 5.1.1. CloudFront Signed URL

- Allow access to a path, no matter the origin.
- **Account wide key-pair, only the root can manage it.**
- Can filter by IP, path, date, expiration.
- Can leverage caching features.

### 5.1.2. S3 Pre-Signed URL

- Issue a request as the person who pre-signed the URL.
- Uses the IAM key of the signing IAM principal.
- Limited lifetime.

## 5.2. Signed URL Process

- Two types of signers:
  - Either a trusted key group (recommended):
    - Can leverage APIs to create and rotate keys (and IAM for API security).
  - An AWS Account that contains a CloudFront Key Pair:
    - Need to manage keys using **the root account and the AWS console**.
    - Not recommended because we shouldn't use the root account for this.
- In your CloudFront distribution, create one or more trusted key groups.
- **We generate your own public / private key**
  - The private key is used by your applications (e.g. EC2) to sign URLs.
  - The public key (uploaded) is used by CloudFront to verify URLs.

# 6. Pricing

- CloudFront Edge locations are all around the world.
- The cost of data out per edge location varies.

[CloudFront Pricing](https://aws.amazon.com/cloudfront/pricing/)

## 6.1. Price Classes

- We can reduce the number of edge locations for **cost reduction**.
- Three price classes:
  1. Price Class All: all regions - best performance.
  2. Price Class 200: most regions, but excludes the most expensive regions.
  3. Price Class 100: only the least expensive regions.

# 7. Multiple Origin

- To route to different kind of origins based on the content type
- Based on path pattern:
  - /Images/\*
  - /api/\*
  - /\*

## 7.1. Origin Groups

- To increase high-availability and do failover.
- Origin Group: one primary and one secondary origin.
- If the primary origin fails, the second one is used.
  ![CloudFront Origin Groups](/Images/Networking%20&%20Content%20Delivery/AmazonCloudFrontOriginGroups.png)

# 8. Field Level Encryption

- Protect user sensitive information through application stack.
- Adds an additional layer of security along with HTTPS.
- Sensitive information encrypted at the edge close to user.
- Uses asymmetric encryption.
- **Usage**
  - Specify set of fields in POST requests that we want to be encrypted (up to 10 fields).
  - Specify the public key to encrypt them.

# 9. Real Time Logs

- Get real-time requests received by CloudFront sent to Kinesis Data Streams.
- Monitor, analyze, and take actions based on content delivery performance.
- **Allows we to choose**
  - Sampling Rate - percentage of requests for which we want to receive.
  - Specific fields and specific Cache Behaviors (path patterns).

# 10. HTTPS for communication between viewers and CloudFront

- Change the **Origin Protocol Policy** and **Viewer Protocol Policy**.

# 11. CloudFront and ACM Certificate Region Requirement

- **CloudFront only supports ACM certificates from the `us-east-1` (N. Virginia) Region**.
- This applies even if your content is hosted in a different Region (e.g., `eu-west-2`).
- To serve content over **HTTPS with a custom domain**, the **ACM public certificate must be created in `us-east-1`**.
- The certificate secures HTTPS access through CloudFront.

# 12. Origin Access Control (OAC)

- **Origin Access Control (OAC)** is the **recommended** way to grant CloudFront secure access to S3.
- It **replaces** the older Origin Access Identity (OAI) method.
- OAC supports **both read and write** operations.
- It enables you to **restrict direct access** to the S3 bucket, ensuring only CloudFront can interact with it securely.
