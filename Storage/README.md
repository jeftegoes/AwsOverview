# Storage<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon S3](#1-amazon-s3)
- [2. AWS Snow Family](#2-aws-snow-family)
- [3. AWS Storage Gateway](#3-aws-storage-gateway)
- [4. Amazon S3 - Summary](#4-amazon-s3---summary)

# 1. Amazon S3

[Amazon S3](Amazon%20S3.md)

# 2. AWS Snow Family

[AWS Snow Family](AWS%20Snow%20Family.md)

# 3. AWS Storage Gateway

[AWS Storage Gateway](AWS%20Storage%20Gateway.md)

# 4. Amazon S3 - Summary

- Buckets vs Objects: global unique name, tied to a region.
- S3 security: IAM policy, S3 Bucket Policy (public access), S3 Encryption.
- S3 Websites: host a static website on Amazon S3.
- S3 Versioning: multiple versions for files, prevent accidental deletes.
- S3 Access Logs: log requests made within your S3 bucket.
- S3 Replication: same-region or cross-region, must enable versioning.
- S3 Storage Classes: Standard, IA, 1Z-IA, Intelligent, Glacier, Glacier Deep Archive.
- S3 Lifecycle Rules: transition objects between classes.
- S3 Glacier Vault Lock / S3 Object Lock: WORM (Write Once Read Many).
- Snow Family: import data onto S3 through a physical device, edge computing.
- OpsHub: desktop application to manage Snow Family devices.
- Storage Gateway: hybrid solution to extend on-premises storage to S3.
