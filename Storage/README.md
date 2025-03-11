# Storage <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon S3](#1-amazon-s3)
- [2. AWS Snow Family](#2-aws-snow-family)
- [3. AWS Storage Gateway](#3-aws-storage-gateway)
- [4. AWS Backup](#4-aws-backup)
- [5. Amazon S3 - Summary](#5-amazon-s3---summary)

# 1. Amazon S3

[Amazon S3](Amazon%20S3.md)

# 2. AWS Snow Family

[AWS Snow Family](AWS%20Snow%20Family.md)

# 3. AWS Storage Gateway

- **AWS Storage Gateway is a hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage. Storage Gateway provides a standard set of storage protocols such as iSCSI, SMB, and NFS, which allow you to use AWS storage without rewriting your existing applications. It provides low-latency performance by caching frequently accessed data on premises, while storing data securely and durably in Amazon cloud storage services.** [AWS Storage Gateway](AWS%20Storage%20Gateway.md)

# 4. AWS Backup

- **AWS Backup is a centralized backup service that makes it easy and cost-effective for you to backup your application data across AWS services in the AWS Cloud**
- **CloudEndure Disaster Recovery minimizes downtime and data loss by providing fast, reliable recovery into AWS of your physical, virtual, and cloud-based servers.** [AWS Backup](AWS%20Backup.md)

# 5. Amazon S3 - Summary

- Buckets vs Objects: Global unique name, tied to a region.
- S3 security: IAM policy, S3 Bucket Policy (public access), S3 Encryption.
- S3 Websites: Host a static website on Amazon S3.
- S3 Versioning: Multiple versions for files, prevent accidental deletes.
- S3 Access Logs: Log requests made within your S3 bucket.
- S3 Replication: Same-region or cross-region, must enable versioning.
- S3 Storage Classes: Standard, IA, 1Z-IA, Intelligent, Glacier, Glacier Deep Archive.
- S3 Lifecycle Rules: Transition objects between classes.
- S3 Glacier Vault Lock / S3 Object Lock: WORM (Write Once Read Many).
- Snow Family: Import data onto S3 through a physical device, edge computing.
- OpsHub: desktop application to manage Snow Family devices.
- Storage Gateway: Hybrid solution to extend on-premises storage to S3.
