# Amazon S3 - Simple Storage Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Use cases](#2-use-cases)
- [3. Buckets](#3-buckets)
- [4. Objects](#4-objects)
- [5. Security](#5-security)
  - [5.1. Bucket Policies](#51-bucket-policies)
  - [5.2. Examples](#52-examples)
    - [5.2.1. Public Access - Use Bucket Policy](#521-public-access---use-bucket-policy)
    - [5.2.2. User Access to S3 - IAM permissions](#522-user-access-to-s3---iam-permissions)
    - [5.2.3. EC2 instance access - Use IAM Roles](#523-ec2-instance-access---use-iam-roles)
    - [5.2.4. Cross-Account Access - Use Bucket Policy](#524-cross-account-access---use-bucket-policy)
  - [5.3. Bucket settings for Block Public Access](#53-bucket-settings-for-block-public-access)
- [6. Static Website Hosting](#6-static-website-hosting)
- [7. Versioning](#7-versioning)
- [8. Replication (CRR \& SRR)](#8-replication-crr--srr)
  - [8.1. Notes](#81-notes)
- [9. Storage Classes](#9-storage-classes)
  - [9.1. Durability and Availability](#91-durability-and-availability)
  - [9.2. S3 Standard - General Purposes](#92-s3-standard---general-purposes)
  - [9.3. S3 Storage Classes - Infrequent Access](#93-s3-storage-classes---infrequent-access)
  - [9.4. Amazon S3 Glacier Storage Classes](#94-amazon-s3-glacier-storage-classes)
  - [9.5. S3 Intelligent-Tiering](#95-s3-intelligent-tiering)
  - [9.6. S3 Storage Classes Comparison](#96-s3-storage-classes-comparison)
- [10. Princing](#10-princing)
- [11. Moving between Storage Classes](#11-moving-between-storage-classes)
- [12. Lifecycle Rules](#12-lifecycle-rules)
  - [12.1. Scenario 1](#121-scenario-1)
  - [12.2. Scenario 2](#122-scenario-2)
  - [12.3. Minimum Storage Duration](#123-minimum-storage-duration)
  - [12.4. Lifecycle Rule Restriction](#124-lifecycle-rule-restriction)
  - [12.5. Reason for the Restriction](#125-reason-for-the-restriction)
- [13. Amazon S3 Analytics - Storage Class Analysis](#13-amazon-s3-analytics---storage-class-analysis)
- [14. Requester Pays](#14-requester-pays)
- [15. S3 Event Notifications](#15-s3-event-notifications)
  - [15.1. S3 Event Notifications with Amazon EventBridge](#151-s3-event-notifications-with-amazon-eventbridge)
- [16. Object Integrity](#16-object-integrity)
- [17. Baseline Performance](#17-baseline-performance)
- [18. S3 Performance](#18-s3-performance)
  - [18.1. Multi-Part upload](#181-multi-part-upload)
    - [18.1.1. Sample multipart upload calls](#1811-sample-multipart-upload-calls)
  - [18.2. S3 Transfer Acceleration (S3TA)](#182-s3-transfer-acceleration-s3ta)
- [19. Byte-Range Fetches](#19-byte-range-fetches)
- [20. Batch Operations](#20-batch-operations)
- [21. Amazon S3 - Storage Lens](#21-amazon-s3---storage-lens)
  - [21.1. Default Dashboard](#211-default-dashboard)
  - [21.2. Metrics](#212-metrics)
  - [21.3. Free vs. Paid](#213-free-vs-paid)
- [22. S3 Select \& Glacier Select](#22-s3-select--glacier-select)
- [23. S3 User-Defined Object Metadata \& S3 Object Tags](#23-s3-user-defined-object-metadata--s3-object-tags)
- [24. Object Encryption](#24-object-encryption)
  - [24.1. SSE-S3](#241-sse-s3)
  - [24.2. SSE-KMS](#242-sse-kms)
    - [24.2.1. SSE-KMS Limitation](#2421-sse-kms-limitation)
  - [24.3. SSE-C](#243-sse-c)
  - [24.4. Client-Side Encryption](#244-client-side-encryption)
  - [24.5. Encryption in transit (SSL/TLS)](#245-encryption-in-transit-ssltls)
  - [24.6. Default Encryption vs Bucket Policies](#246-default-encryption-vs-bucket-policies)
- [25. What is CORS?](#25-what-is-cors)
  - [25.1. Amazon S3 - CORS](#251-amazon-s3---cors)
  - [25.2. CloudFront to respect CORS settings](#252-cloudfront-to-respect-cors-settings)
  - [25.3. CORS configuration](#253-cors-configuration)
- [26. MFA Delete](#26-mfa-delete)
- [27. Access Logs](#27-access-logs)
  - [27.1. Access Logs WARNING](#271-access-logs-warning)
- [28. Pre-Signed URLs](#28-pre-signed-urls)
- [29. S3 Glacier Vault Lock](#29-s3-glacier-vault-lock)
- [30. S3 Object Lock](#30-s3-object-lock)
- [31. S3 - Access Points](#31-s3---access-points)
  - [31.1. VPC Origin](#311-vpc-origin)
  - [31.2. S3 Object Lambda](#312-s3-object-lambda)
- [32. Shared Responsibility Model for S3](#32-shared-responsibility-model-for-s3)
- [33. S3 Sync Command](#33-s3-sync-command)
- [34. Read-After-Write Consistency](#34-read-after-write-consistency)
- [35. Amazon S3 Bucket Keys with SSE-KMS](#35-amazon-s3-bucket-keys-with-sse-kms)
- [36. Summary](#36-summary)

# 1. Introduction

- Amazon S3 is one of the main building blocks of AWS.
- It's advertised as "infinitely scaling" storage.
- Many websites use Amazon S3 as a backbone.
- Many AWS services use Amazon S3 as an integration as well.

# 2. Use cases

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
- **Naming convention**
  - No uppercase, no underscore.
  - 3-63 characters long.
  - Not an IP.
  - Must start with lowercase letter or number.
  - Must NOT start with the prefix **xn--**.
  - Must NOT end with the suffix **-s3alias**.

# 4. Objects

- Objects (files) have a Key.
- The `key` is the **FULL** path:
  - s3://my-bucket/**my_file.txt**
  - s3://my-bucket/**my_folder1/another_folder/my_file.txt**
- The key is composed of **prefix** + _object name_
  - s3://my-bucket/**my_folder1/another_folder/**_my_file.txt_
- There's no concept of "directories" within buckets (although the UI will trick you to think otherwise).
- Just keys with very long names that contain slashes ("/").
- **Object values are the content of the body**
  - Max Object Size is 5TB (5000GB).
  - If uploading more than 5GB, must use "multi-part upload".
- **Metadata** (List of text key / value pairs - system or user metadata).
- **Tags** (Unicode key / value pair - up to 10) - useful for security / lifecycle.
- Version ID (if versioning is enabled).

# 5. Security

- **User based**
  - **IAM policies:** Which API calls should be allowed for a specific user from IAM console.
- **Resource Based**
  - **Bucket Policies:** Bucket wide rules from the S3 console - allows cross account.
  - **Object Access Control List (ACL):** Finer grain (Can be disabled).
  - **Bucket Access Control List (ACL):** Less common (Can be disabled).
- **Note:** An IAM principal can access an S3 object if:
  - The user IAM permissions **ALLOW** it **OR** the resource policy **ALLOWS** it.
  - **AND** there's no explicit **DENY**.
- **Encryption:** Encrypt objects in Amazon S3 using encryption keys.

## 5.1. Bucket Policies

- S3 bucket policy allows for securing access to objects in buckets so that only users with the necessary permissions can access them.
  - Even authenticated users can be restricted from accessing Amazon S3 resources if they lack the appropriate permissions.
- **JSON based policies**
  - An S3 bucket policy statement is composed of several elements, and the following are required to create a valid policy:
    - `Effect` - The effect can be Allow or Deny.
    - `Action` - The specific API action for which you are granting or denying permission.
    - `Principal` - The user, account, service, or other entity that is allowed or denied access to the bucket or objects within the bucket.
    - `Resource` - The resource that's affected by the action.
      - You specify a resource using an Amazon Resource Name (ARN).
      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Principal": {
              "AWS": ["arn:aws:iam::1234567890:role/Developer"]
            },
            "Action": ["s3:GetObject"],
            "Resource": "arn:aws:s3:::jeftegoesdev/*"
          },
          {
            "Effect": "Allow",
            "Principal": {
              "AWS": ["arn:aws:iam::1234567890:role/QA"]
            },
            "Action": ["s3:GetObject"],
            "Resource": "arn:aws:s3:::jeftegoesdev/qa/*"
          }
        ]
      }
      ```
      ![Example ](/Images/Storage/AmazonS3BucketPoliciesExample.png)
- **Use S3 bucket for policy to**
  - Grant public access to the bucket.
  - Force objects to be encrypted at upload.
  - Grant access to another account (Cross Account).

## 5.2. Examples

### 5.2.1. Public Access - Use Bucket Policy

![S3 Use bucket policy](/Images/Storage/AmazonS3UseBucketPolicy.png)

### 5.2.2. User Access to S3 - IAM permissions

![S3 IAM Permissions](/Images/Storage/AmazonS3IamPermissions.png)

### 5.2.3. EC2 instance access - Use IAM Roles

![EC2 instance access - IAM Roles](/Images/Storage/AmazonS3IamRoles.png)

### 5.2.4. Cross-Account Access - Use Bucket Policy

![Cross-Account Access - Use Bucket Policy](/Images/Storage/AmazonS3CrossAccountPermissions.png)

## 5.3. Bucket settings for Block Public Access

![Settings for Block Public Access](/Images/Storage/EditBlockPublicAccess.png)

- **These settings were created to prevent company data leaks.**
- If you know your bucket should never be public, leave these on.
- Can be set at the account level.

# 6. Static Website Hosting

- S3 can host static websites and have them accessible on the Internet.
- The website URL will be (depending on the region):
  - https://**bucket-name**.s3-website-**aws-region**.amazonaws.com OR https://**bucket-name**.s3-website.**aws-region**.amazonaws.com
- If you get a **403 Forbidden** error, make sure the bucket policy allows public reads.

# 7. Versioning

- You can version your files in Amazon S3.
- It is enabled at the **bucket level**.
- Same key overwrite will increment the "version": 1, 2, 3...
- **It is best practice to version your buckets**
  - Protect against unintended deletes (ability to restore a version).
  - Easy roll back to previous version.
- **Notes**
  - Any file that is not versioned prior to enabling versioning will have version `null`.
  - Suspending versioning does not delete the previous versions.
  - Once you version-enable a bucket, it can never return to an unversioned state.
    - Versioning can only be suspended once it has been enabled.

# 8. Replication (CRR & SRR)

- **Must enable versioning** in source and destination.
- **Cross-Region Replication (CRR).**
- **Same-Region Replication (SRR).**
- Buckets can be in different accounts.
- Copying is asynchronous.
- Must give proper IAM permissions to S3.
- **Use cases**
  - **CRR:** Compliance, lower latency access, replication across accounts.
  - **SRR:** Log aggregation, live replication between production and test accounts.
    ![Cross-Region Replication](/Images/Storage/AmazonS3Replication.png)

## 8.1. Notes

- After you **enable Replication**, only new objects are replicated.
- Optionally, you can replicate existing objects using **S3 Batch Replication**.
  - Replicates existing objects and objects that failed replication.
- **For DELETE operations**
  - **Can replicate delete markers** from source to target (optional setting).
  - Deletions with a version ID are not replicated (to avoid malicious deletes).
- **There is no "chaining" of replication**
  - If bucket 1 has replication into bucket 2, which has replication into bucket 3.
  - Then objects created in bucket 1 are not replicated to bucket 3.

# 9. Storage Classes

- **Standart**
  - Amazon S3 Standard - General Purpose.
  - Amazon S3 Standard-Infrequent Access (IA).
  - Amazon S3 One Zone-Infrequent Access.
- **Glacier**
  - Amazon S3 Glacier Instant Retrieval.
  - Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier).
  - Amazon S3 Glacier Deep Archive.
- **Intelligent Tiering**
  - Amazon S3 Intelligent Tiering.
- **Can move between classes manually or using S3 Lifecycle configurations**.

## 9.1. Durability and Availability

- **Durability**
  - High durability (99.999999999%, 11 9's) of objects across multiple AZ.
  - If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years.
  - Same for all storage classes.
- **Availability**
  - Measures how readily available a service is.
  - Varies depending on storage class.
  - **Example:** S3 standard has 99.99% availability, which means it will not be available 53 minutes a year.

## 9.2. S3 Standard - General Purposes

- 99.99% Availability.
- Used for frequently accessed data.
- Low latency and high throughput.
- Sustain 2 concurrent facility failures.
- **Use cases:** Big Data analytics, mobile & gaming applications, content distribution...

## 9.3. S3 Storage Classes - Infrequent Access

- For data that is less frequently accessed, but requires rapid access when needed.
- Lower cost than S3 Standard.
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA)**
  - 99.9% Availability.
  - **Use cases:** Disaster Recovery, backups.
- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)**
  - High durability (99.999999999%) in a single AZ; data lost when AZ is destroyed.
  - 99.5% Availability.
  - **Use cases:** Storing secondary backup copies of on-premises data, or data you can re-create.
- **S3 One Zone-IA** stores data in a **single Availability Zone (AZ)**, offering 20% lower storage costs, while **S3 Standard-IA** stores data across at least **three AZs**, providing higher durability and availability.

## 9.4. Amazon S3 Glacier Storage Classes

- Low-cost object storage meant for archiving / backup.
- **Pricing:** Price for storage + object retrieval cost.
- **Amazon S3 Glacier Instant Retrieval**
  - Millisecond retrieval, great for data accessed once a quarter.
  - Minimum storage duration of 90 days.
- **Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier)**
  - Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) - free.
  - Minimum storage duration of 90 days.
- **Amazon S3 Glacier Deep Archive - for long term storage**
  - Standard (12 hours), Bulk (48 hours).
  - Minimum storage duration of 180 days.

## 9.5. S3 Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee.
- Moves objects automatically between Access Tiers based on usage.
- There are no retrieval charges in S3 Intelligent-Tiering.
- **Frequent Access tier (automatic):** Default tier.
- **Infrequent Access tier (automatic):** Objects not accessed for 30 days.
- **Archive Instant Access tier (automatic):** Objects not accessed for 90 days.
- **Archive Access tier (optional):** Configurable from 90 days to 700+ days.
- **Deep Archive Access tier (optional):** Config. from 180 days to 700+ days.

## 9.6. S3 Storage Classes Comparison

[Storage Classes Comparison](https://aws.amazon.com/s3/storage-classes/)

# 10. Princing

[S3 Pricing](https://aws.amazon.com/s3/pricing/)

# 11. Moving between Storage Classes

- You can transition objects between storage classes.
- For infrequently accessed object, move them to **Standard IA**.
- For archive objects that you don't need fast access to, move them to **Glacier or Glacier Deep Archive**.
- Moving objects can be automated using a **Lifecycle Rules**.
  ![Amazon S3 Lifecycle Transitions.png](/Images/Storage/AmazonS3LifecycleTransitions.png)

# 12. Lifecycle Rules

- **Transition Actions:** Configure objects to transition to another storage class.
  - Move objects to Standard IA class 60 days after creation.
  - Move to Glacier for archiving after 6 months.
- **Expiration actions:** Configure objects to expire (delete) after some time.
  - Access log files can be set to delete after a 365 days.
  - **Can be used to delete old versions of files (if versioning is enabled)**.
  - Can be used to delete incomplete Multi-Part uploads.
- Rules can be created for a certain prefix (example: s3://mybucket/mp3/\*).
- Rules can be created for certain objects Tags (example: Department: Finance).

## 12.1. Scenario 1

- **Your application on EC2 creates images thumbnails after profile photos are uploaded to Amazon S3. These thumbnails can be easily recreated, and only need to be kept for 60 days. The source images should be able to be immediately retrieved for these 60 days, and afterwards, the user can wait up to 6 hours. How would you design this?**
  - S3 source images can be on **Standard**, with a lifecycle configuration to transition them to **Glacier** after 60 days.
  - S3 thumbnails can be on **One-Zone IA**, with a lifecycle configuration to expire them (delete them) after 60 days.

## 12.2. Scenario 2

- **A rule in your company states that you should be able to recover your deleted S3 objects immediately for 30 days, although this may happen rarely. After this time, and for up to 365 days, deleted objects should be recoverable within 48 hours.**
  - **Enable S3 Versioning** in order to have object versions, so that "deleted objects" are in fact hidden by a "delete marker" and can be recovered.
  - Transition the "noncurrent versions" of the object to **Standard IA**.
  - Transition afterwards the "noncurrent versions" to **Glacier Deep Archive**.

## 12.3. Minimum Storage Duration

- Objects must be stored in Amazon S3 for **at least 30 days** before transitioning to:
  - **S3 Standard-IA**
  - **S3 One Zone-IA**

## 12.4. Lifecycle Rule Restriction

- You **cannot** configure a Lifecycle rule to transition objects to these storage classes **within the first 30 days** of object creation.

## 12.5. Reason for the Restriction

- **Newer objects are typically**
  - Accessed more frequently.
  - More likely to be deleted sooner.
- These patterns are **not suitable** for the cost and retrieval models of S3 Standard-IA or S3 One Zone-IA.

# 13. Amazon S3 Analytics - Storage Class Analysis

- Help you decide when to transition objects to the right storage class.
- Recommendations for **Standard** and **Standard IA**.
  - Does NOT work for One-Zone IA or Glacier.
- Report is updated daily.
- 24 to 48 hours to start seeing data analysis.
- Good first step to put together Lifecycle Rules (or improve them)!
  ![Amazon S3 Analytics - Storage Class Analysis](/Images/Storage/AmazonS3AnalyticsStorageClassAnalysis.png)

# 14. Requester Pays

- In general, bucket owners pay for all Amazon S3 storage and data transfer costs associated with their bucket.
- **With Requester Pays buckets**, the requester instead of the bucket owner pays the cost of the request and the data download from the bucket.
- Helpful when you want to share large datasets with other accounts.
- The requester must be authenticated in AWS (cannot be anonymous).

# 15. S3 Event Notifications

- `S3:ObjectCreated`, `S3:ObjectRemoved`, `S3:ObjectRestore`, `S3:Replication`...
- Object name filtering possible `*.jpg`.
- **Use case:** Generate thumbnails of images uploaded to S3.
- **Can create as many "S3 events" as desired.**
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer.

## 15.1. S3 Event Notifications with Amazon EventBridge

- **Advanced filtering** options with JSON rules (metadata, object size, name...).
- **Multiple Destinations:** Ex: Step Functions, Kinesis Streams / Firehose...
- **EventBridge Capabilities:** Archive, Replay Events, Reliable delivery.

# 16. Object Integrity

- S3 uses checksum to validate the integrity of uploaded objects.
  - Using MD5.
  - Using MD5 and Etag.
    - ETag - represents a specific version of the object, ETag = MD5 (if SSE-S3).
- **Other supported checksums:** SHA-1, SHA-256, CRC32, CRC32C.

# 17. Baseline Performance

- Amazon S3 automatically scales to high request rates, latency 100-200 ms.
- **Your application can achieve at least**
  - **3,500 PUT/COPY/POST/DELETE.**
  - **5,500 GET/HEAD requests per second per prefix in a bucket.**
- There are no limits to the number of prefixes in a bucket.
- Example (object path => prefix):
  - bucket/folder1/sub1/file => /folder1/sub1/
  - bucket/folder1/sub2/file => /folder1/sub2/
  - bucket/1/file => /1/
  - bucket/2/file => /2/
- If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD.

# 18. S3 Performance

## 18.1. Multi-Part upload

- **Recommended for files > 100MB, must use for files > 5GB**.
- Can help parallelize uploads (speed up transfers).

### 18.1.1. Sample multipart upload calls

- For this example, assume that you are generating a multipart upload for a 100 GB file.
- In this case, you would have the following API calls for the entire process.
- There would be a total of 1002 API calls.
  - A `CreateMultipartUpload` call to start the process.
  - 1000 individual `UploadPart` calls, each uploading a part of 100 MB, for a total size of 100 GB
  - A `CompleteMultipartUpload` call to finish the process.

## 18.2. S3 Transfer Acceleration (S3TA)

- **Amazon S3 Transfer Acceleration (S3TA) enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket. Transfer Acceleration takes advantage of Amazon CloudFront's globally distributed edge locations. As the data arrives at an edge location, data is routed to Amazon S3 over an optimized network path.**
- Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region.
- Compatible with multi-part upload.
- **Use Cases**
  - **Web or mobile applications** with **widespread users**.
  - Applications **hosted far away** from their S3 bucket.
- **Activation and Testing**
  - **Enable** S3TA easily via the **S3 console**.
  - **Test** the benefits with a **speed comparison tool**.
- **Pricing Model**
  - **Pay only** for the **accelerated** transfers.

# 19. Byte-Range Fetches

- Parallelize GETs by requesting specific byte ranges.
- Better resilience in case of failures.
- Can be used to speed up downloads.
  ![Byte-Range Fetches to speed up downloads](/Images/Storage/AmazonS3ByteRangeFetches1.png)
- Can be used to retrieve only partial data (for example the head of a file).
  ![Byte-Range Fetches to retrieve only partial data](/Images/Storage/AmazonS3ByteRangeFetches2.png)

# 20. Batch Operations

- Perform bulk operations on existing S3 objects with a single request, example:
  - Modify object metadata & properties.
  - Copy objects between S3 buckets.
  - **Encrypt un-encrypted objects.**
  - Modify ACLs, tags.
  - Restore objects from S3 Glacier.
  - Invoke Lambda function to perform custom action on each object.
- A job consists of a list of objects, the action to perform, and optional parameters
- S3 Batch Operations manages retries, tracks progress, sends completion notifications, generate reports...
- **You can use S3 Inventory to get object list and use Athena to query and filter your objects.**

# 21. Amazon S3 - Storage Lens

- Understand, analyze, and optimize storage across entire **AWS Organization**.
- Discover anomalies, identify cost efficiencies, and apply data protection best practices across entire AWS Organization (30 days usage & activity metrics).
- Aggregate data for Organization, specific accounts, regions, buckets, or prefixes.
- Default dashboard or create your own dashboards.
- Can be configured to export metrics daily to an S3 bucket (CSV, Parquet).
  ![Amazon S3 - Storage Lens](/Images/Storage/AmazonS3StorageLens.png)

## 21.1. Default Dashboard

- Visualize summarized insights and trends for both free and advanced metrics.
- Default dashboard shows Multi-Region and Multi-Account data.
- Preconfigured by Amazon S3.
- Can't be deleted, but can be disabled.

## 21.2. Metrics

- **Summary Metrics**
  - General insights about your S3 storage.
  - `StorageBytes`, `ObjectCount`...
  - **Use cases:** Identify the fastest-growing (or not used) buckets and prefixes.
- **Cost-Optimization Metrics**
  - Provide insights to manage and optimize your storage costs.
  - `NonCurrentVersionStorageBytes`, `IncompleteMultipartUploadStorageBytes`...
  - **Use cases:** Identify buckets with incomplete multipart uploaded older than 7 days, Identify which objects could be transitioned to lower-cost storage class.
- **Data-Protection Metrics**
  - Provide insights for data protection features.
  - `VersioningEnabledBucketCount`, `MFADeleteEnabledBucketCount`, `SSEKMSEnabledBucketCount`, `CrossRegionReplicationRuleCount`...
  - **Use cases:** Identify buckets that aren't following data-protection best practices.
- **Access-management Metrics**
  - Provide insights for S3 Object Ownership.
  - `ObjectOwnershipBucketOwnerEnforcedBucketCount`...
  - **Use cases:** Identify which Object Ownership settings your buckets use.
- **Event Metrics**
  - Provide insights for S3 Event Notifications.
  - `EventNotificationEnabledBucketCount` (identify which buckets have S3 Event Notifications configured).
- **Performance Metrics**
  - Provide insights for S3 Transfer Acceleration.
  - `TransferAccelerationEnabledBucketCount` (identify which buckets have S3 Transfer Acceleration enabled).
- **Activity Metrics**
  - Provide insights about how your storage is requested.
  - `AllRequests`, `GetRequests`, `PutRequests`, `ListRequests`, `BytesDownloaded`...
- **Detailed Status Code Metrics**
  - Provide insights for HTTP status codes.
  - `200OKStatusCount`, `403ForbiddenErrorCount`, `404NotFoundErrorCount`...

## 21.3. Free vs. Paid

- **Free Metrics**
  - Automatically available for all customers.
  - Contains around 28 usage metrics.
  - Data is available for queries for 14 days.
- **Advanced Metrics and Recommendations**
  - Additional paid metrics and features.
  - Advanced Metrics - Activity, Advanced Cost Optimization, Advanced Data Protection, Status Code.
  - **CloudWatch Publishing:** Access metrics in CloudWatch without additional charges.
  - **Prefix Aggregation:** Collect metrics at the prefix level.
  - Data is available for queries for 15 months.

# 22. S3 Select & Glacier Select

- Retrieve less data using SQL by performing server-side filtering.
- Can filter by rows & columns (simple SQL statements).
- Less network transfer, less CPU cost client-side.

# 23. S3 User-Defined Object Metadata & S3 Object Tags

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

# 24. Object Encryption

- **We can encrypt objects in S3 buckets using one of 4 methods**
  - **Server-Side Encryption (SSE)**
    - **Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)**
      - Encrypts S3 objects using keys handled, managed, and owned by AWS.
    - **Server-Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)**
      - Leverage AWS Key Management Service (AWS KMS) to manage encryption keys.
    - **Server-Side Encryption with Customer-Provided Keys (SSE-C)**
      - When you want to manage your own encryption keys.
- **Client-Side Encryption.**

## 24.1. SSE-S3

- Encryption using keys handled, managed, and owned by AWS.
  - **You never have access to this key.**
- Object is encrypted server-side.
- Encryption type is **AES-256**.
- Must set header `"x-amz-server-side-encryption": "AES256"`.
- Enabled by default for new buckets and new objects.
  ![Encryption SSE-S3](/Images/Storage/AmazonS3EncryptionSSES3.png)
- **Server-side encryption (SSE)** encrypts data at rest on the storage service (e.g., Amazon S3).
- **SSE-S3** uses Amazon S3 managed keys to encrypt each object with a unique key.
- The object key is further encrypted with a **root key** that is regularly rotated for added security.
- As of **January 5, 2023 (Default Behavior)**, all new object uploads to S3 are automatically encrypted using SSE-S3:
  - **No additional cost**
  - **No impact on performance**
  - **Applies by default to every S3 bucket**

## 24.2. SSE-KMS

- Encryption using keys handled and managed by AWS KMS (Key Management Service).
  - **Manage your own keys.**
- **KMS advantages:** User control + audit key usage using [AWS CloudTrail](/Management%20&%20Governance/AWS%20CloudTrail.md).
- Object is encrypted server side.
- Must set header `"x-amz-server-side-encryption": "aws:kms"`.
  ![Encryption SSE-KMS](/Images/Storage/AmazonS3EncryptionSSEKMS.png)

### 24.2.1. SSE-KMS Limitation

- If you use SSE-KMS, you may be impacted by the KMS limits.
  - **Upload and download files from Amazon S3, you need to leverage a KMS Key.**
    ![Encryption SSE-KMS Limitaition](/Images/Storage/AmazonS3EncryptionSSEKMSLimitation.png)
- When you upload, it calls the `GenerateDataKey` KMS API.
- When you download, it calls the `Decrypt` KMS API.
- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region).
- You can request a quota increase using the Service Quotas Console.

## 24.3. SSE-C

- Server-Side Encryption using keys fully managed by the customer outside of AWS.
- Amazon S3 does **NOT** store the encryption key you provide.
- **HTTPS must be used.**
- Encryption key must provided in HTTP headers, for every HTTP request made.
- Must set header `"x-amz-server-side-encryption-customer-algorithm": "AES256"`.
  ![Encryption SSE-C](/Images/Storage/AmazonS3EncryptionSSEC.png)

| Name                                            | Description                                                                                                                                                                                                                             |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-amz-server-side-encryption-customer-algorithm | Use this header to specify the encryption algorithm. The header value must be AES256.                                                                                                                                                   |
| x-amz-server-side-encryption-customer-key       | Use this header to provide the 256-bit, base64-encoded encryption key for Amazon S3 to use to encrypt or decrypt your data.                                                                                                             |
| x-amz-server-side-encryption-customer-key-MD5   | Use this header to provide the base64-encoded 128-bit MD5 digest of the encryption key according to RFC 1321. Amazon S3 uses this header for a message integrity check to ensure that the encryption key was transmitted without error. |

## 24.4. Client-Side Encryption

- Use client libraries such as **Amazon S3 Client-Side Encryption Library**.
- Clients must encrypt data themselves before sending to Amazon S3.
- Clients must decrypt data themselves when retrieving from Amazon S3.
- Customer fully manages the keys and encryption cycle.

## 24.5. Encryption in transit (SSL/TLS)

- Encryption in flight is also called SSL/TLS.
- **Amazon S3 exposes two endpoints**
  - **HTTP Endpoint:** Non encrypted.
  - **HTTPS Endpoint:** Encryption in flight.
- **HTTPS is recommended.**
- **HTTPS is mandatory for SSE-C.**
- Most clients would use the HTTPS endpoint by default.

## 24.6. Default Encryption vs Bucket Policies

- **SSE-S3 encryption is automatically applied to new objects stored in S3 bucket.**
- Optionally, we can "force encryption" using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C).
- **Note: Bucket Policies are evaluated before "default encryption".**

# 25. What is CORS?

- **Cross-Origin Resource Sharing (CORS).**
- **Origin = scheme (protocol) + host (domain) + port.**
- **Example:** https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP).
- **Web Browser** based mechanism to allow requests to other origins while visiting the main origin.
- **Same origin:** http://example.com/app1 & http://example.com/app2.
- **Different origins:** http://www.example.com & http://other.example.com.
- The requests won't be fulfilled unless the other origin allows for the requests, using **CORS Headers** (example: `Access-Control-Allow-Origin`).
  ![CORS Diagram](/Images/Networking%20&%20Content%20Delivery/AmazonAPIGatewayCORS.png)

## 25.1. Amazon S3 - CORS

- If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers.
- You can allow for a specific origin or for \* (all origins).

## 25.2. CloudFront to respect CORS settings

- If you want `OPTIONS` responses to be cached, do the following:
  - Choose the options for default cache behavior settings that enable caching for `OPTIONS` responses.
  - Configure CloudFront to forward the following headers:
    - `Origin`
    - `Access-Control-Request-Headers`
    - `Access-Control-Request-Method`

## 25.3. CORS configuration

```
  <?xml version="1.0" encoding="UTF-8"?>
  <CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
      <AllowedOrigin>https://example.org</AllowedOrigin>
      <AllowedMethod>HEAD</AllowedMethod>
      <AllowedMethod>GET</AllowedMethod>
      <AllowedMethod>PUT</AllowedMethod>
      <AllowedMethod>POST</AllowedMethod>
      <AllowedMethod>DELETE</AllowedMethod>
      <AllowedHeader>*</AllowedHeader>
      <ExposeHeader>ETag</ExposeHeader>
      <ExposeHeader>x-amz-meta-custom-header</ExposeHeader>
    </CORSRule>
  </CORSConfiguration>
```

- `AllowedOrigin` - Specifies domain origins that you allow to make cross-domain requests.
- `AllowedMethod` - Specifies a type of request you allow (GET, PUT, POST, DELETE, HEAD) in cross-domain requests.
- `AllowedHeader` - Specifies the headers allowed in a preflight request.
- Below are some of the CORSRule elements:
  - `MaxAgeSeconds` - Specifies the amount of time in seconds (in this example, 3000) that the browser caches an Amazon S3 response to a preflight OPTIONS request for the specified resource.
    - By caching the response, the browser does not have to send preflight requests to Amazon S3 if the original request will be repeated.
  - `ExposeHeader` - Identifies the response headers (in this example, `x-amz-server-side-encryption`, `x-amz-request-id`, and `x-amz-id-2`) that customers are able to access from their applications (for example, from a JavaScript XMLHttpRequest object).

# 26. MFA Delete

- **MFA (Multi-Factor Authentication):** Force users to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3.
- **MFA will be required to**
  - Permanently delete an object version.
  - Suspend Versioning on the bucket.
- **MFA won't be required to**
  - Enable Versioning.
  - List deleted versions.
- To use MFA Delete, **Versioning must be enabled** on the bucket.
- **Only the bucket owner (root account) can enable/disable MFA Delete.**

# 27. Access Logs

- For audit purpose, you may want to log all access to S3 buckets.
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket.
- That data can be analyzed using data analysis tools...
- The target logging bucket must be in the same AWS region.
- Very helpful to come down to the root cause of an issue, or audit usage, view suspicious patterns, etc...
- **The log format is at:** https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html

## 27.1. Access Logs WARNING

- Do not set your logging bucket to be the monitored bucket.
- It will create a logging loop, and **your bucket will grow exponentially**.

# 28. Pre-Signed URLs

- Generate pre-signed URLs using the **S3 Console, AWS CLI or SDK**.
- **URL Expiration:**
  - **S3 Console:** 1 min up to 720 mins (12 hours)
  - **AWS CLI:** configure expiration with --expires-in parameter in seconds (default 3600 secs, max. 604800 secs ~ 168 hours).
- Users given a pre-signed URL inherit the permissions of the user that generated the URL for GET / PUT.
- **Examples**
  - Allow only logged-in users to download a premium video from your S3 bucket.
  - Allow an ever-changing list of users to download files by generating URLs dynamically.
  - Allow temporarily a user to upload a file to a precise location in your S3 bucket.
    ![Amazon S3 Pre-Signed URLs](/Images/Storage/AmazonS3PreSignedURLs.png)

# 29. S3 Glacier Vault Lock

- Adopt a WORM (Write Once Read Many) model.
- Create a Vault Lock Policy.
- Lock the policy for future edits (can no longer be changed or deleted).
- Helpful for compliance and data retention.

# 30. S3 Object Lock

- Adopt a WORM (Write Once Read Many) model.
- Block an object version deletion for a specified amount of time.
- **Retention mode - Compliance**
  - Object versions can't be overwritten or deleted by any user, including the root user.
  - Objects retention modes can't be changed, and retention periods can't be shortened.
- **Retention mode - Governance**
  - Most users can't overwrite or delete an object version or alter its lock settings.
  - Some users have special permissions to change the retention or delete the object.
- **Retention Period**
  - Protect the object for a fixed period, it can be extended.
  - When we apply a retention period to an object version explicitly, we specify a `Retain Until Date` for the object version.
  - Different versions of a single object can have different retention modes and periods.
- **Legal Hold**
  - Protect the object indefinitely, independent from retention period.
  - Can be freely placed and removed using the `s3:PutObjectLegalHold` IAM permission.

# 31. S3 - Access Points

- Access Points simplify security management for S3 Buckets.
- **Each Access Point has**
  - Its own DNS name (Internet Origin or VPC Origin).
  - An access point policy (similar to bucket policy) - manage security at scale.
    ![S3 - Access Points](/Images/Storage/AmazonS3AccessPoints.png)

## 31.1. VPC Origin

- We can define the access point to be accessible only from within the VPC.
- You must create a VPC Endpoint to access the Access Point (Gateway or Interface Endpoint).
- The VPC Endpoint Policy must allow access to the target bucket and Access Point.
  ![Access Points - VPC Origin](/Images/Storage/AmazonS3AccessPointsVPCOrigin.png)

## 31.2. S3 Object Lambda

- Use AWS Lambda Functions to change the object before it is retrieved by the caller application.
- Only one S3 bucket is needed, on top of which we create **S3 Access Point and S3 Object Lambda Access Points**.
- **Use cases**
  - Redacting personally identifiable information for analytics or non- production environments.
  - Converting across data formats, such as converting XML to JSON.
  - Resizing and watermarking images on the fly using caller-specific details, such as the user who requested the object.
    ![Access Points - S3 Object Lambda](/Images/Storage/AmazonS3AccessPointsObjectLambda.png)

# 32. Shared Responsibility Model for S3

- **Aws**
  - Infrastructure (global security, durability, availability, sustain concurrent loss of data in two facilities)
  - Configuration and vulnerability analysis
  - Compliance validation
- **You**
  - S3 Versioning
  - S3 Bucket Policies
  - S3 Replication Setup
  - Logging and Monitoring
  - S3 Storage Classes
  - Data encryption at rest and in transit

# 33. S3 Sync Command

- **Purpose**: Copies objects between Amazon S3 buckets using the **CopyObject** API.
- **How it works**:
  - Compares source and target buckets.
  - **Copies objects that**
    - Are **missing** in the target bucket.
    - Have **different LastModified** timestamps.
- **Example usage**
  ```bash
    aws s3 sync s3://DOC-EXAMPLE-BUCKET-SOURCE s3://DOC-EXAMPLE-BUCKET-TARGET
  ```

# 34. Read-After-Write Consistency

- **Amazon S3 provides strong read-after-write consistency** automatically and at no extra cost.
- After a **successful write or overwrite**, any **immediate read** returns the **latest version** of the object.
- **List operations** are also strongly consistent-newly written or deleted objects are immediately reflected.
- Applies to **GET, PUT, LIST**, and changes to **tags, ACLs, and metadata**.
- Ensures that **"what you write is what you read"**, supporting use cases that require immediate access to updated objects.

# 35. Amazon S3 Bucket Keys with SSE-KMS

- **S3 Bucket Keys** optimize server-side encryption with AWS KMS (SSE-KMS) by using a **bucket-level key**.
- This reduces the number of **direct KMS API calls** by allowing S3 to generate data keys locally.
- Can lower **AWS KMS request costs by up to 99%**.
- Maintains the **same security and compliance** as standard SSE-KMS.
- Especially beneficial for **high-volume workloads** with frequent reads or writes.
  ![Amazon S3 Bucket Keys with SSE-KMS](/Images/Storage/AmazonS3BucketKeys.png)

# 36. Summary

- S3 is a... key / value store for objects.
- Great for bigger objects, not so great for many small objects.
- Serverless, scales infinitely, max object size is 5 TB, versioning capability.
- **Tiers:** S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier + lifecycle policy.
- **Features:** Versioning, Encryption, Replication, MFA-Delete, Access Logs...
- **Security:** IAM, Bucket Policies, ACL, Access Points, Object Lambda, CORS, Object/Vault Lock.
- **Encryption:** SSE-S3, SSE-KMS, SSE-C, client-side, TLS in transit, default encryption.
- **Batch operations** on objects using S3 Batch, listing files using S3 Inventory.
- **Performance:** Multi-part upload, S3 Transfer Acceleration, S3 Select.
- **Automation:** S3 Event Notifications (SNS, SQS, Lambda, EventBridge).
- **Use cases:** static files, key value store for big files, website hosting.
