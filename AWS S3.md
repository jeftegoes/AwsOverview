# AWS S3 - Simples Storage Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. S3 Use cases](#2-s3-use-cases)
- [3. Buckets](#3-buckets)
- [4. Objects](#4-objects)
- [5. Security](#5-security)
  - [5.1. Bucket Policies](#51-bucket-policies)
  - [5.2. Bucket settings for Block Public Access](#52-bucket-settings-for-block-public-access)
- [6. Static Website Hosting](#6-static-website-hosting)
- [7. Versioning](#7-versioning)
- [8. Access Logs](#8-access-logs)
- [9. Replication (CRR \& SRR)](#9-replication-crr--srr)
  - [9.1. Replication (Notes)](#91-replication-notes)
- [10. Storage Classes](#10-storage-classes)
  - [10.1. Durability and Availability](#101-durability-and-availability)
  - [10.2. Standard - General Purposes](#102-standard---general-purposes)
  - [10.3. S3 Storage Classes - Infrequent Access](#103-s3-storage-classes---infrequent-access)
  - [10.4. Amazon S3 Glacier Storage Classes](#104-amazon-s3-glacier-storage-classes)
  - [10.5. S3 Intelligent-Tiering](#105-s3-intelligent-tiering)
  - [10.6. S3 Storage Classes Comparison](#106-s3-storage-classes-comparison)
- [11. Princing](#11-princing)
- [12. Moving between Storage Classes](#12-moving-between-storage-classes)
- [13. Amazon S3 - Lifecycle Rules](#13-amazon-s3---lifecycle-rules)
  - [13.1. Scenario 1](#131-scenario-1)
  - [13.2. Scenario 2](#132-scenario-2)
- [14. Amazon S3 Analytics - Storage Class Analysis](#14-amazon-s3-analytics---storage-class-analysis)
- [15. Event Notifications](#15-event-notifications)
  - [15.1. S3 Event Notifications](#151-s3-event-notifications)
- [16. Baseline Performance](#16-baseline-performance)
  - [16.1. Multi-Part upload:](#161-multi-part-upload)
  - [16.2. S3 Transfer Acceleration](#162-s3-transfer-acceleration)
- [17. Byte-Range Fetches](#17-byte-range-fetches)
- [18. S3 Select \& Glacier Select](#18-s3-select--glacier-select)
- [19. S3 User-Defined Object Metadata \& S3 Object Tags](#19-s3-user-defined-object-metadata--s3-object-tags)
- [20. Object Encryption](#20-object-encryption)
  - [20.1. SSE-S3](#201-sse-s3)
  - [20.2. SSE-KMS](#202-sse-kms)
    - [20.2.1. SSE-KMS Limitation](#2021-sse-kms-limitation)
  - [20.3. SSE-C](#203-sse-c)
  - [20.5. Client-Side Encryption](#205-client-side-encryption)
  - [20.6. Encryption in transit (SSL/TLS)](#206-encryption-in-transit-ssltls)
  - [20.7. Default Encryption vs Bucket Policies](#207-default-encryption-vs-bucket-policies)
- [21. What is CORS?](#21-what-is-cors)
  - [21.1. Amazon S3 - CORS](#211-amazon-s3---cors)
  - [21.2. CloudFront to respect CORS settings](#212-cloudfront-to-respect-cors-settings)
- [22. MFA Delete](#22-mfa-delete)
- [23. Access Logs](#23-access-logs)
  - [23.1. Access Logs WARNING](#231-access-logs-warning)
- [24. Pre-Signed URLs](#24-pre-signed-urls)
- [25. S3 - Access Points](#25-s3---access-points)
- [26. S3 Object Lambda](#26-s3-object-lambda)
- [27. Shared Responsibility Model for S3](#27-shared-responsibility-model-for-s3)

# 1. Introduction

- Amazon S3 is one of the main building blocks of AWS.
- It's advertised as "infinitely scaling" storage.
- Many websites use Amazon S3 as a backbone.
- Many AWS services use Amazon S3 as an integration as well.

# 2. S3 Use cases

- Backup and storage.
- Disaster Recovery.
- Archive.
- Hybrid Cloud storage.
- Application hosting.
- Media hosting.
- Data lakes & big data analytics.
- Software delivery.
- Static website.

# 3. Buckets

- Amazon S3 allows people to store objects (files) in "buckets" (directories).
- Buckets must have a **globally unique name (across all regions all accounts)**.
- Buckets are defined at the region level.
- S3 looks like a global service but buckets are created in a region.
- Naming convention:
  - No uppercase, no underscore.
  - 3-63 characters long.
  - Not an IP.
  - Must start with lowercase letter or number.
  - Must NOT start with the prefix **xn--**.
  - Must NOT end with the suffix **-s3alias**.

# 4. Objects

- Objects (files) have a Key.
- The **key** is the **FULL** path:
  - s3://my-bucket/**my_file.txt**
  - s3://my-bucket/**my_folder1/another_folder/my_file.txt**
- The key is composed of **prefix** + _object name_
  - s3://my-bucket/**my_folder1/another_folder/**_my_file.txt_
- There's no concept of "directories" within buckets (although the UI will trick you to think otherwise).
- Just keys with very long names that contain slashes ("/").
- Object values are the content of the body:
  - Max Object Size is 5TB (5000GB).
  - If uploading more than 5GB, must use "multi-part upload".
- **Metadata** (List of text key / value pairs - system or user metadata).
- **Tags** (Unicode key / value pair - up to 10) - useful for security / lifecycle.
- Version ID (if versioning is enabled).

# 5. Security

- **User based:**
  - **IAM policies:** Which API calls should be allowed for a specific user from IAM console.

![S3 IAM Permissions](Images/S3IamPermissions.png)
![EC2 instance access - IAM Roles](Images/S3IamRoles.png)
![Scenario IAM Role and EC2 Instance Profile](Images/AWSCrendentialScenario.png)

- **Resource Based:**
  - **Bucket Policies:** bucket wide rules from the S3 console - allows cross account.
  - **Object Access Control List (ACL):** finer grain (Can be disabled).
  - **Bucket Access Control List (ACL):** less common (Can be disabled).

![S3 Use bucket policy](Images/S3UseBucketPolicy.png)

- **Note:** An IAM principal can access an S3 object if:
  - The user IAM permissions **ALLOW** it **OR** the resource policy **ALLOWS** it.
  - **AND** there's no explicit **DENY**.
- **Encryption:** encrypt objects in Amazon S3 using encryption keys.

## 5.1. Bucket Policies

- **JSON based policies:**
  - Resources: buckets and objects
  - Actions: Set of API to Allow or Deny
  - Effect: Allow / Deny
  - Principal: The account or user to apply the policy to
- **Use S3 bucket for policy to:**
  - Grant public access to the bucket
  - Force objects to be encrypted at upload
  - Grant access to another account (Cross Account)

```
{
    "Version": "2012-10-17",
    "Id": "Policy1648843736704",
    "Statement": [
        {
            "Sid": "Stmt1648843733338",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::test-bucket-jefte-goes/*"
        }
    ]
}
```

## 5.2. Bucket settings for Block Public Access

![Settings for Block Public Access](/Images/EditBlockPublicAccess.PNG)

- **These settings were created to prevent company data leaks.**
- If you know your bucket should never be public, leave these on.
- Can be set at the account level.

# 6. Static Website Hosting

- S3 can host static websites and have them accessible on the Internet.
- The website URL will be (depending on the region):
  - https://**bucket-name**.s3-website-**aws-region**.amazonaws.com
- If you get a **403 Forbidden** error, make sure the bucket policy allows public reads.

# 7. Versioning

- You can version your files in Amazon S3.
- It is enabled at the **bucket level**.
- Same key overwrite will increment the "version": 1, 2, 3...
- It is best practice to version your buckets:
  - Protect against unintended deletes (ability to restore a version).
  - Easy roll back to previous version.
- Notes:
  - Any file that is not versioned prior to enabling versioning will have version **"null"**.
  - Suspending versioning does not delete the previous versions.

# 8. Access Logs

- For audit purpose, you may want to log all access to S3 buckets.
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket.
- That data can be analyzed using data analysis tools...
- Very helpful to come down to the root cause of an issue, or audit usage, view suspicious patterns, etc...

# 9. Replication (CRR & SRR)

- **Must enable versioning** in source and destination.
- **Cross Region Replication (CRR).**
- **Same Region Replication (SRR).**
- Buckets can be in different accounts.
- Copying is asynchronous.
- Must give proper IAM permissions to S3.
- Use cases:
  - **CRR:** compliance, lower latency access, replication across accounts.
  - **SRR:** log aggregation, live replication between production and test accounts.

## 9.1. Replication (Notes)

- After you enable Replication, only new objects are replicated.
- Optionally, you can replicate existing objects using **S3 Batch Replication**.
  - Replicates existing objects and objects that failed replication.
- For DELETE operations:
  - Can replicate delete markers from source to target (optional setting).
  - Deletions with a version ID are not replicated (to avoid malicious deletes).
- **There is no "chaining" of replication**
  - If bucket 1 has replication into bucket 2, which has replication into bucket 3.
  - Then objects created in bucket 1 are not replicated to bucket 3.

# 10. Storage Classes

- **Standart:**
  - Amazon S3 Standard - General Purpose.
  - Amazon S3 Standard-Infrequent Access (IA).
  - Amazon S3 One Zone-Infrequent Access.
- **Glacier:**
  - Amazon S3 Glacier Instant Retrieval.
  - Amazon S3 Glacier Flexible Retrieval.
  - Amazon S3 Glacier Deep Archive.
- **Intelligent Tiering:**
  - Amazon S3 Intelligent Tiering.
- **Can move between classes manually or using S3 Lifecycle configurations**.

## 10.1. Durability and Availability

- **Durability:**
  - High durability (99.999999999%, 11 9's) of objects across multiple AZ.
  - If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years.
  - Same for all storage classes.
- **Availability:**
  - Measures how readily available a service is.
  - Varies depending on storage class.
  - Example: S3 standard has 99.99% availability, which means it will not be available 53 minutes a year.

## 10.2. Standard - General Purposes

- 99.99% Availability.
- Used for frequently accessed data.
- Low latency and high throughput.
- Sustain 2 concurrent facility failures.
- Use Cases: Big Data analytics, mobile & gaming applications, content distribution...

## 10.3. S3 Storage Classes - Infrequent Access

- For data that is less frequently accessed, but requires rapid access when needed.
- Lower cost than S3 Standard.
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA):**
  - 99.9% Availability.
  - Use cases: Disaster Recovery, backups.
- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA):**
  - High durability (99.999999999%) in a single AZ; data lost when AZ is destroyed.
  - 99.5% Availability.
  - Use Cases: Storing secondary backup copies of on-premises data, or data you can recreate.

## 10.4. Amazon S3 Glacier Storage Classes

- Low-cost object storage meant for archiving / backup.
- Pricing: price for storage + object retrieval cost.
- **Amazon S3 Glacier Instant Retrieval:**
  - Millisecond retrieval, great for data accessed once a quarter.
  - Minimum storage duration of 90 days.
- **Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier):**
  - Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) - free.
  - Minimum storage duration of 90 days.
- **Amazon S3 Glacier Deep Archive - for long term storage:**
  - Standard (12 hours), Bulk (48 hours).
  - Minimum storage duration of 180 days.

## 10.5. S3 Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee.
- Moves objects automatically between Access Tiers based on usage.
- There are no retrieval charges in S3 Intelligent-Tiering.
- **Frequent Access tier (automatic):** default tier.
- **Infrequent Access tier (automatic):** objects not accessed for 30 days.
- **Archive Instant Access tier (automatic):** objects not accessed for 90 days.
- **Archive Access tier (optional):** configurable from 90 days to 700+ days.
- **Deep Archive Access tier (optional):** config. from 180 days to 700+ days.

## 10.6. S3 Storage Classes Comparison

[Storage Classes Comparison](https://aws.amazon.com/s3/storage-classes/)

# 11. Princing

[S3 Pricing](https://aws.amazon.com/s3/pricing/)

# 12. Moving between Storage Classes

- You can transition objects between storage classes.
- For infrequently accessed object, move them to **Standard IA**.
- For archive objects that you don't need fast access to, move them to **Glacier or Glacier Deep Archive**.
- Moving objects can be automated using a **Lifecycle Rules**.

# 13. Amazon S3 - Lifecycle Rules

- **Transition Actions:** Configure objects to transition to another storage class.
  - Move objects to Standard IA class 60 days after creation.
  - Move to Glacier for archiving after 6 months.
- **Expiration actions:** Configure objects to expire (delete) after some time.
  - Access log files can be set to delete after a 365 days.
  - **Can be used to delete old versions of files (if versioning is enabled)**.
  - Can be used to delete incomplete Multi-Part uploads.
- Rules can be created for a certain prefix (example: s3://mybucket/mp3/\*).
- Rules can be created for certain objects Tags (example: Department: Finance).

## 13.1. Scenario 1

- **Your application on EC2 creates images thumbnails after profile photos are uploaded to Amazon S3. These thumbnails can be easily recreated, and only need to be kept for 60 days. The source images should be able to be immediately retrieved for these 60 days, and afterwards, the user can wait up to 6 hours. How would you design this?**
  - S3 source images can be on **Standard**, with a lifecycle configuration to transition them to **Glacier** after 60 days.
  - S3 thumbnails can be on **One-Zone IA**, with a lifecycle configuration to expire them (delete them) after 60 days.

## 13.2. Scenario 2

- **A rule in your company states that you should be able to recover your deleted S3 objects immediately for 30 days, although this may happen rarely. After this time, and for up to 365 days, deleted objects should be recoverable within 48 hours.**
  - **Enable S3 Versioning** in order to have object versions, so that "deleted objects" are in fact hidden by a "delete marker" and can be recovered.
  - Transition the "noncurrent versions" of the object to **Standard IA**.
  - Transition afterwards the "noncurrent versions" to **Glacier Deep Archive**.

# 14. Amazon S3 Analytics - Storage Class Analysis

- Help you decide when to transition objects to the right storage class.
- Recommendations for **Standard** and **Standard IA**:
  - Does NOT work for One-Zone IA or Glacier.
- Report is updated daily.
- 24 to 48 hours to start seeing data analysis.
- Good first step to put together Lifecycle Rules (or improve them)!

# 15. Event Notifications

- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication...
- Object name filtering possible (\*.jpg).
- Use case: generate thumbnails of images uploaded to S3.
- Can create as many "S3 events" as desired.
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer.

## 15.1. S3 Event Notifications

- **Advanced filtering** options with JSON rules (metadata, object size, name...).
- **Multiple Destinations:** Ex Step Functions, Kinesis Streams / Firehose...
- **EventBridge Capabilities:** Archive, Replay Events, Reliable delivery.

# 16. Baseline Performance

- Amazon S3 automatically scales to high request rates, latency 100-200 ms.
- Your application can achieve at least:
  - **3,500 PUT/COPY/POST/DELETE.**
  - **5,500 GET/HEAD requests per second per prefix in a bucket.**
- There are no limits to the number of prefixes in a bucket.
- Example (object path => prefix):
  - bucket/folder1/sub1/file => /folder1/sub1/
  - bucket/folder1/sub2/file => /folder1/sub2/
  - bucket/1/file => /1/
  - bucket/2/file => /2/
- If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD.

## 16.1. Multi-Part upload:

- **Recommended for files > 100MB, must use for files > 5GB**.
- Can help parallelize uploads (speed up transfers).

## 16.2. S3 Transfer Acceleration

- **Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket. Transfer Acceleration takes advantage of Amazon CloudFront's globally distributed edge locations. As the data arrives at an edge location, data is routed to Amazon S3 over an optimized network path.**
- Compatible with multi-part upload.

# 17. Byte-Range Fetches

- Parallelize GETs by requesting specific byte ranges.
- Better resilience in case of failures.
- Can be used to speed up downloads.
- Can be used to retrieve only partial data (for example the head of a file).

# 18. S3 Select & Glacier Select

- Retrieve less data using SQL by performing server-side filtering.
- Can filter by rows & columns (simple SQL statements).
- Less network transfer, less CPU cost client-side.

# 19. S3 User-Defined Object Metadata & S3 Object Tags

- **S3 User-Defined Object Metadata**
  - When uploading an object, you can also assign metadata.
  - Name-value (key-value) pairs.
  - User-defined metadata names must begin with "x-amz-meta-".
  - Amazon S3 stores user-defined metadata keys in lowercase.
  - Metadata can be retrieved while retrieving the object.
- **S3 Object Tags**
  - Key-value pairs for objects in Amazon S3.
  - Useful for fine-grained permissions (only access specific objects with specific tags).
  - Useful for analytics purposes (using S3 Analytics to group by tags).
- **You cannot search the object metadata or object tags.**
- Instead, you must use an external DB as a search index such as DynamoDB.

# 20. Object Encryption

- You can encrypt objects in S3 buckets using one of 4 methods:
  - **Server-Side Encryption (SSE):**
    - **Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3):**
      - Encrypts S3 objects using keys handled, managed, and owned by AWS.
    - **Server-Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS):**
      - Leverage AWS Key Management Service (AWS KMS) to manage encryption keys.
    - **Server-Side Encryption with Customer-Provided Keys (SSE-C):**
      - When you want to manage your own encryption keys.
- **Client-Side Encryption.**

## 20.1. SSE-S3

- Encryption using keys handled, managed, and owned by AWS.
  - **You never have access to this key.**
- Object is encrypted server-side.
- Encryption type is **AES-256**.
- Must set header `"x-amz-server-side-encryption": "AES256"`.
- Enabled by default for new buckets and new objects.

![Encryption SSE-S3](Images/S3EncryptionSSES3.png)

## 20.2. SSE-KMS

- Encryption using keys handled and managed by AWS KMS (Key Management Service).
  - **Manage your own keys.**
- KMS advantages: user control + audit key usage using CloudTrail.
- Object is encrypted server side.
- Must set header `"x-amz-server-side-encryption": "aws:kms"`.

![Encryption SSE-KMS](Images/S3EncryptionSSEKMS.png)

### 20.2.1. SSE-KMS Limitation

- If you use SSE-KMS, you may be impacted by the KMS limits.
  - **Upload and download files from Amazon S3, you need to leverage a KMS Key.**

![Encryption SSE-KMS Limitaition](Images/S3EncryptionSSEKMSLimitation.png)

- When you upload, it calls the `GenerateDataKey` KMS API.
- When you download, it calls the `Decrypt` KMS API.
- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region).
- You can request a quota increase using the Service Quotas Console.

## 20.3. SSE-C

- Server-Side Encryption using keys fully managed by the customer outside of AWS.
- Amazon S3 does **NOT** store the encryption key you provide.
- **HTTPS must be used.**
- Encryption key must provided in HTTP headers, for every HTTP request made.
- Must set header `"x-amz-server-side-encryption-customer-algorithm": "AES256"`.

![Encryption SSE-C](Images/S3EncryptionSSEC.png)

| Name                                            | Description                                                                                                                                                                                                                             |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-amz-server-side-encryption-customer-algorithm | Use this header to specify the encryption algorithm. The header value must be AES256.                                                                                                                                                   |
| x-amz-server-side-encryption-customer-key       | Use this header to provide the 256-bit, base64-encoded encryption key for Amazon S3 to use to encrypt or decrypt your data.                                                                                                             |
| x-amz-server-side-encryption-customer-key-MD5   | Use this header to provide the base64-encoded 128-bit MD5 digest of the encryption key according to RFC 1321. Amazon S3 uses this header for a message integrity check to ensure that the encryption key was transmitted without error. |

## 20.5. Client-Side Encryption

- Use client libraries such as **Amazon S3 Client-Side Encryption Library**.
- Clients must encrypt data themselves before sending to Amazon S3.
- Clients must decrypt data themselves when retrieving from Amazon S3.
- Customer fully manages the keys and encryption cycle.

## 20.6. Encryption in transit (SSL/TLS)

- Encryption in flight is also called SSL/TLS.
- Amazon S3 exposes two endpoints:
  - HTTP Endpoint - non encrypted.
  - HTTPS Endpoint - encryption in flight.
- **HTTPS is recommended.**
- **HTTPS is mandatory for SSE-C.**
- Most clients would use the HTTPS endpoint by default.

## 20.7. Default Encryption vs Bucket Policies

- **SSE-S3 encryption is automatically applied to new objects stored in S3 bucket.**
- Optionally, you can "force encryption" using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C).
- **Note: Bucket Policies are evaluated before "default encryption".**

# 21. What is CORS?

- **Cross-Origin Resource Sharing (CORS).**
- **Origin = scheme (protocol) + host (domain) + port.**
- example: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP).
- **Web Browser** based mechanism to allow requests to other origins while visiting the main origin.
- Same origin: http://example.com/app1 & http://example.com/app2.
- Different origins: http://www.example.com & http://other.example.com.
- The requests won't be fulfilled unless the other origin allows for the requests, using **CORS Headers** (example: `Access-Control-Allow-Origin`).

![CORS Diagram](Images/APIGatewayCORS.png)

## 21.1. Amazon S3 - CORS

- If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers.
- You can allow for a specific origin or for \* (all origins).

## 21.2. CloudFront to respect CORS settings

- If you want `OPTIONS` responses to be cached, do the following:
  - Choose the options for default cache behavior settings that enable caching for `OPTIONS` responses.
  - Configure CloudFront to forward the following headers:
    - `Origin`
    - `Access-Control-Request-Headers`
    - `Access-Control-Request-Method`

# 22. MFA Delete

- **MFA (Multi-Factor Authentication)** - force users to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3.
- MFA will be required to:
  - Permanently delete an object version.
  - Suspend Versioning on the bucket.
- MFA won't be required to:
  - Enable Versioning.
  - List deleted versions.
- To use MFA Delete, **Versioning must be enabled** on the bucket.
- **Only the bucket owner (root account) can enable/disable MFA Delete.**

# 23. Access Logs

- For audit purpose, you may want to log all access to S3 buckets.
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket.
- That data can be analyzed using data analysis tools...
- The target logging bucket must be in the same AWS region.
- The log format is at: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html

## 23.1. Access Logs WARNING

- Do not set your logging bucket to be the monitored bucket.
- It will create a logging loop, and **your bucket will grow exponentially**.

# 24. Pre-Signed URLs

- Generate pre-signed URLs using the **S3 Console, AWS CLI or SDK**.
- **URL Expiration:**
  - **S3 Console:** 1 min up to 720 mins (12 hours)
  - **AWS CLI:** configure expiration with --expires-in parameter in seconds (default 3600 secs, max. 604800 secs ~ 168 hours).
- Users given a pre-signed URL inherit the permissions of the user that generated the URL for GET / PUT.
- Examples:
  - Allow only logged-in users to download a premium video from your S3 bucket.
  - Allow an ever-changing list of users to download files by generating URLs dynamically.
  - Allow temporarily a user to upload a file to a precise location in your S3 bucket.

# 25. S3 - Access Points

- Each Access Point gets its own DNS and policy to limit who can access it:
  - A specific IAM user / group.
  - One policy per Access Point => **Easier to manage than complex bucket policies**.

# 26. S3 Object Lambda

- Use AWS Lambda Functions to change the object before it is retrieved by the caller application.
- Only one S3 bucket is needed, on top of which we create **S3 Access Point and S3 Object Lambda Access Points**.
- **Use Cases:**
  - Redacting personally identifiable information for analytics or non- production environments.
  - Converting across data formats, such as converting XML to JSON.
  - Resizing and watermarking images on the fly using caller-specific details, such as the user who requested the object.

# 27. Shared Responsibility Model for S3

- Aws:
  - Infrastructure (global security, durability, availability, sustain concurrent loss of data in two facilities)
  - Configuration and vulnerability analysis
  - Compliance validation
- You:
  - S3 Versioning
  - S3 Bucket Policies
  - S3 Replication Setup
  - Logging and Monitoring
  - S3 Storage Classes
  - Data encryption at rest and in transit
