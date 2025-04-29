# AWS KMS - Key Management Service <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Why encryption?](#1-why-encryption)
  - [1.1. Encryption in flight (SSL / TLS)](#11-encryption-in-flight-ssl--tls)
  - [1.2. Server side encryption at rest](#12-server-side-encryption-at-rest)
  - [1.3. Client side encryption](#13-client-side-encryption)
- [2. AWS KMS (Key Management Service)](#2-aws-kms-key-management-service)
  - [2.1. KMS Keys Types](#21-kms-keys-types)
    - [2.1.1. Types of KMS Keys](#211-types-of-kms-keys)
    - [2.1.2. Automatic Key rotation](#212-automatic-key-rotation)
  - [2.2. Key Policies](#22-key-policies)
  - [2.3. Copying Snapshots across accounts](#23-copying-snapshots-across-accounts)
  - [2.4. Multi-Region Keys](#24-multi-region-keys)
    - [2.4.1. DynamoDB Global Tables and KMS Multi-Region Keys Client-Side encryption](#241-dynamodb-global-tables-and-kms-multi-region-keys-client-side-encryption)
    - [2.4.2. Global Aurora and KMS Multi-Region Keys](#242-global-aurora-and-kms-multi-region-keys)
  - [2.5. S3 Replication Encryption Considerations](#25-s3-replication-encryption-considerations)
  - [2.6. AMI Sharing Process Encrypted via KMS](#26-ami-sharing-process-encrypted-via-kms)
  - [2.7. How does KMS work? API - Encrypt and Decrypt](#27-how-does-kms-work-api---encrypt-and-decrypt)
  - [2.8. Envelope Encryption](#28-envelope-encryption)
    - [2.8.1. Encrypt](#281-encrypt)
    - [2.8.2. Decrypt](#282-decrypt)
    - [2.8.3. Encryption SDK](#283-encryption-sdk)
    - [2.8.4. Diagram](#284-diagram)
  - [2.9. KMS Symmetric - API Summary](#29-kms-symmetric---api-summary)
- [3. Request Quotas](#3-request-quotas)
- [4. S3 Bucket Key for SSE-KMS encryption](#4-s3-bucket-key-for-sse-kms-encryption)
- [5. CloudHSM](#5-cloudhsm)
  - [5.1. Diagram](#51-diagram)
  - [5.2. High Availability](#52-high-availability)
  - [5.3. Integration with AWS Services](#53-integration-with-aws-services)
- [6. CloudWatch Logs - Encryption](#6-cloudwatch-logs---encryption)
- [7. CodeBuild Security](#7-codebuild-security)
- [8. AWS Nitro Enclaves](#8-aws-nitro-enclaves)

# 1. Why encryption?

## 1.1. Encryption in flight (SSL / TLS)

- Data is encrypted before sending and decrypted after receiving.
- SSL/TLS certificates help with encryption (HTTPS).
- Encryption in flight ensures no MITM (man in the middle attack) can happen.

## 1.2. Server side encryption at rest

- Data is encrypted after being received by the server.
- Data is decrypted before being sent.
- It is stored in an encrypted form thanks to a key (usually a data key).
- The encryption / decryption keys must be managed somewhere and the server must have access to it.

## 1.3. Client side encryption

- Data is encrypted by the client and never decrypted by the server.
- Data will be decrypted by a receiving client.
- The server should not be able to decrypt the data.
- Could leverage Envelope Encryption.

# 2. AWS KMS (Key Management Service)

- Anytime you hear "encryption" for an AWS service, it's most likely KMS.
- AWS manages encryption keys for us.
- Fully integrated with IAM for authorization.
- Easy way to control access to your data.
- **Able to audit KMS Key usage using CloudTrail.**
- Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM...).
- **Never ever store your secrets in plaintext, especially in your code!**
  - KMS Key Encryption also available through API calls (SDK, CLI).
  - Encrypted secrets can be stored in the code / environment variables.

## 2.1. KMS Keys Types

- **KMS Keys is the new name of KMS Customer Master Key (CMK).**
  - AWS KMS is replacing the term customer master key (CMK) with AWS KMS key and KMS key. The concept has not changed. To prevent breaking changes, AWS KMS is keeping some variations of this term.
  - https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html
- **Symmetric (AES-256 keys)**
  - Single encryption key that is used to Encrypt and Decrypt.
  - AWS services that are integrated with KMS use Symmetric CMKs.
  - You never get access to the KMS Key unencrypted (must call KMS API to use).
  - [AES Encryption and Decryption Online](https://www.devglan.com/online-tools/aes-encryption-decryption)
- **Asymmetric (RSA & ECC key pairs)**
  - Public (Encrypt) and Private Key (Decrypt) pair.
  - Used for Encrypt/Decrypt, or Sign/Verify operations.
  - The public key is downloadable, but you can't access the Private Key unencrypted.
  - **Use case:** Encryption outside of AWS by users who can't call the KMS API.
  - [RSA Encryption and Decryption Online](https://www.devglan.com/online-tools/rsa-encryption-decryption)

### 2.1.1. Types of KMS Keys

- AWS Owned Keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key).
- AWS Managed Key: **Free** (aws/service-name, example: aws/rds or aws/ebs).
- Customer managed keys created in KMS: **$1 / month**.
- Customer managed keys imported: **$1 / month**.
- +pay for API call to KMS ($0.03 / 10000 calls).

### 2.1.2. Automatic Key rotation

- AWS-managed KMS Key: Automatic every 1 year.
- Customer-managed KMS Key: (must be enabled) automatic every 1 year.
- Imported KMS Key: Only manual rotation possible using alias.

| Type of KMS key      | Can view KMS key metadata | Can manage KMS key | Used only for my AWS account | Automatic rotation                            | Pricing                                                             |
| -------------------- | ------------------------- | ------------------ | ---------------------------- | --------------------------------------------- | ------------------------------------------------------------------- |
| Customer managed key | Yes                       | Yes                | Yes                          | Optional. Every year (approximately 365 days) | Monthly fee (pro-rated hourly) Per-use fee                          |
| AWS managed key      | Yes                       | No                 | Yes                          | Required. Every year (approximately 365 days) | No monthly fee Per-use fee (some AWS services pay this fee for you) |
| AWS owned key        | No                        | No                 | No                           | Varies                                        | No fees                                                             |

## 2.2. Key Policies

- Control access to KMS keys, "similar" to S3 bucket policies.
- **Difference:** We cannot control access without them.
- **Default KMS Key Policy**
  - Created if we don't provide a specific KMS Key Policy.
  - Complete access to the key to the root user = entire AWS account.
- **Custom KMS Key Policy**
  - Define users, roles that can access the KMS key.
  - Define who can administer the key.
  - Useful for cross-account access of your KMS key.

## 2.3. Copying Snapshots across accounts

1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key).
2. Attach a KMS Key Policy to authorize cross-account access.
3. Share the encrypted snapshot.
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account.
5. Create a volume from the snapshot KMS Key Policy.

## 2.4. Multi-Region Keys

- Identical KMS keys in different AWS Regions that can be used interchangeably.
- Multi-Region keys have the same key ID, key material, automatic rotation...
- Encrypt in one Region and decrypt in other Regions.
- No need to re-encrypt or making cross-Region API calls.
- KMS Multi-Region are NOT global (Primary + Replicas).
- Each Multi-Region key is managed **independently**.
- **Use cases:** Global client-side encryption, encryption on Global DynamoDB, Global Aurora.

### 2.4.1. DynamoDB Global Tables and KMS Multi-Region Keys Client-Side encryption

- We can encrypt specific attributes client-side in our DynamoDB table using the **Amazon DynamoDB Encryption Client**.
- Combined with Global Tables, the client-side encrypted data is replicated to other regions.
- If we use a multi-region key, replicated in the same region as the DynamoDB Global table, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side.
- Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key.

### 2.4.2. Global Aurora and KMS Multi-Region Keys

- We can encrypt specific attributes client-side in our Aurora table using the **AWS Encryption SDK**.
- Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions.
- If we use a multi-region key, replicated in the same region as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side.
- Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key, **we can protect specific fields even from database admins**.

## 2.5. S3 Replication Encryption Considerations

- **Unencrypted objects and objects encrypted with SSE-S3 are replicated by default**.
- Objects encrypted with SSE-C (customer provided key) can be replicated.
- **For objects encrypted with SSE-KMS**, we need to enable the option.
  - Specify which KMS Key to encrypt the objects within the target bucket.
  - Adapt the KMS Key Policy for the target key.
  - An IAM Role with `kms:Decrypt` for the source KMS Key and `kms:Encrypt` for the target KMS Key.
  - We might get KMS throttling errors, in which case we can ask for a Service Quotas increase.
- **We can use multi-region AWS KMS Keys, but they are currently treated as independent keys by Amazon S3 (the object will still be decrypted and then encrypted).**

## 2.6. AMI Sharing Process Encrypted via KMS

1. AMI in Source Account is encrypted with KMS Key from Source Account.
2. Must modify the image attribute to add a Launch Permission which corresponds to the specified target AWS account.
3. Must share the KMS Keys used to encrypted the snapshot the AMI references with the target account / IAM Role.
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt.
5. When launching an EC2 instance from the AMI, optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes.

## 2.7. How does KMS work? API - Encrypt and Decrypt

![Encrypt and Decrypt](/Images/AWSKMSEncryptDecrypt.png)

## 2.8. Envelope Encryption

- KMS Encrypt API call has a **limit of 4 KB**.
- If we want to encrypt >4 KB, we need to use Envelope Encryption.
- The main API that will help us is the GenerateDataKey API.
- **Anything over 4 KB of data that needs to be encrypted must use the Envelope Encryption == `GenerateDataKey` API.**

![Envelope Encryption diagram](/Images/AWSKMSEncryptDecryptEnvelope.png)

### 2.8.1. Encrypt

- It is recommended that we use the following pattern to encrypt data locally in your application:

1. Use the GenerateDataKey operation to get a data encryption key.
2. Use the plaintext data key (returned in the Plaintext field of the response) to encrypt data locally, then erase the plaintext data key from memory.
3. Store the encrypted data key (returned in the CiphertextBlob field of the response) alongside the locally encrypted data.

### 2.8.2. Decrypt

- To decrypt data locally:

1. Use the Decrypt operation to decrypt the encrypted data key. The operation returns a plaintext copy of the data key.
2. Use the plaintext data key to decrypt data locally, then erase the plaintext data key from memory.

### 2.8.3. Encryption SDK

- The AWS Encryption SDK implemented Envelope Encryption for us.
- The Encryption SDK also exists as a CLI tool we can install.
- Implementations for Java, Python, C, JavaScript.
- **Feature - Data Key Caching**
  - Re-use data keys instead of creating new ones for each encryption.
  - Helps with reducing the number of calls to KMS with a security trade-off.
  - Use LocalCryptoMaterialsCache (max age, max bytes, max number of messages).

### 2.8.4. Diagram

- The SDK encrypts the data encryption key and stores it (encrypted) as part of the returned ciphertext.

## 2.9. KMS Symmetric - API Summary

- `Encrypt` - Encrypt **up to 4 KB** of data through KMS.
- `GenerateDataKey`
  - Generates a unique symmetric data key (DEK).
  - Returns a plaintext copy of the data key **AND** a copy that is encrypted under the CMK that you specify.
- `GenerateDataKeyWithoutPlaintext`
  - Generate a DEK to use at some point (not immediately).
  - DEK that is encrypted under the CMK that you specify (must use Decrypt later).
- `Decrypt` - Decrypt **up to 4 KB** of data (including Data Encryption Keys).
- `GenerateRandom` - Returns a random byte string.

# 3. Request Quotas

- When we exceed a request quota, we get a `ThrottlingException`:
  - **We have exceeded the rate at which you may call KMS. Reduce the frequency of your calls. (Service: AWSKMS; Status code: 400; Error Code: ThrottlingException); Request ID: <ID>**.
- To respond, use **exponential backoff** (backoff and retry).
- For cryptographic operations, they share a quota.
- This includes requests made by AWS on your behalf (ex: SSE-KMS).
- For `GenerateDataKey`, consider using DEK caching from the Encryption SDK.
- **We can request a Request Quotas increase through API or AWS support.**

# 4. S3 Bucket Key for SSE-KMS encryption

- New setting to decrease...
  - Number of API calls made to KMS from S3 by 99%.
  - Costs of overall KMS encryption with Amazon S3 by 99%.
- This leverages data keys
  - A "S3 bucket key" is generated.
  - That key is used to encrypt KMS objects with new data keys.
- We will see **less KMS CloudTrail events in CloudTrail**.

# 5. CloudHSM

- KMS => AWS manages the software for encryption.
- CloudHSM => AWS provisions encryption **hardware**.
- Dedicated Hardware (HSM = Hardware Security Module).
- We manage your own encryption keys entirely (not AWS).
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance.
- Supports both symmetric and **asymmetric** encryption (SSL/TLS keys).
- No free tier available.
- Must use the CloudHSM Client Software.
- Redshift supports CloudHSM for database encryption and key management.
- **Good option to use with SSE-C encryption.**

## 5.1. Diagram

- IAM permissions:
  - CRUD an HSM Cluster
- CloudHSM Software:
  - Manage the Keys
  - Manage the Users

## 5.2. High Availability

- CloudHSM clusters are spread across Multi AZ (HA)
- Great for availability and durability

## 5.3. Integration with AWS Services

- Through integration with AWS KMS
- Configure KMS Custom Key Store with CloudHSM
- Example: EBS, S3, RDS ...

# 6. CloudWatch Logs - Encryption

- We can encrypt CloudWatch logs with KMS keys.
- Encryption is enabled at the log group level, by associating a CMK with a log group, either when you create the log group or after it exists.
- We cannot associate a CMK with a log group using the CloudWatch console.
- **We must use the CloudWatch Logs API**
  - `associate-kms-key`: if the log group already exists.
  - `create-log-group`: if the log group doesn't exist yet.

# 7. CodeBuild Security

- To access resources in your VPC, make sure you specify a VPC configuration for your CodeBuild.
- Secrets in CodeBuild:
- Don't store them as plaintext in environment variables.
- Instead...
  - Environment variables can reference parameter store parameters.
  - Environment variables can reference secrets manager secrets.

# 8. AWS Nitro Enclaves

- Process highly sensitive data in an isolated compute environment
  - Personally Identifiable Information (PII), healthcare, financial, ...
- Fully isolated virtual machines, hardened, and highly constrained
  - Not a container, not persistent storage, no interactive access, no external networking.
- Helps reduce the attack surface for sensitive data processing apps
  - Cryptographic Attestation - only authorized code can be running in your Enclave.
  - Only Enclaves can access sensitive data (integration with KMS).
- **Use cases:** securing private keys, processing credit cards, secure multi-party computation...
