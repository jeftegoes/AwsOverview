# Storage <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. AWS Storage Cloud Native Options](#1-aws-storage-cloud-native-options)
- [2. Amazon S3](#2-amazon-s3)
- [3. AWS Snow Family](#3-aws-snow-family)
- [4. AWS Storage Gateway](#4-aws-storage-gateway)
- [5. AWS Backup](#5-aws-backup)
- [6. Summary](#6-summary)

# 1. AWS Storage Cloud Native Options

![AWS Storage Cloud Native Options](/Images/Storage/AWSStorageCloudNativeOptions.png)

# 2. Amazon S3

[Amazon S3](Amazon%20S3.md)

# 3. AWS Snow Family

[AWS Snow Family](AWS%20Snow%20Family.md)

# 4. AWS Storage Gateway

- **AWS Storage Gateway is a hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage. Storage Gateway provides a standard set of storage protocols such as iSCSI, SMB, and NFS, which allow you to use AWS storage without rewriting your existing applications. It provides low-latency performance by caching frequently accessed data on premises, while storing data securely and durably in Amazon cloud storage services.** [AWS Storage Gateway](AWS%20Storage%20Gateway.md)

# 5. AWS Backup

- **AWS Backup is a centralized backup service that makes it easy and cost-effective for you to backup your application data across AWS services in the AWS Cloud**
- **CloudEndure Disaster Recovery minimizes downtime and data loss by providing fast, reliable recovery into AWS of your physical, virtual, and cloud-based servers.** [AWS Backup](AWS%20Backup.md)

# 6. Summary

- **S3:** Object Storage.
- **S3 Glacier:** Object Archival.
- **EBS volumes:** Network storage for one EC2 instance at a time.
- **Instance Storage:** Physical storage for your EC2 instance (high IOPS).
- **EFS:** Network File System for Linux instances, POSIX filesystem.
- **FSx for Windows:** Network File System for Windows servers.
- **FSx for Lustre:** High Performance Computing Linux file system.
- **FSx for NetApp ONTAP:** High OS Compatibility.
- **FSx for OpenZFS:** Managed ZFS file system.
- **Storage Gateway:** S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway.
- **Snowcone / Snowball / Snowmobile:** to move large amount of data to the cloud, physically.
- **Database:** for specific workloads, usually with indexing and querying.
