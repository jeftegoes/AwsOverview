# AWS CloudTrail<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Possible integrations](#1-possible-integrations)
- [2. Impossible integrations](#2-impossible-integrations)

# 1. Possible integrations

- Amazon GuardDuty > Amazon EventBridge > SQS/SNS/Lambda
- Amazon Inspector > Security Hub/Amazon EventBridge
- Amazon CloudWatch > Subscription Filter > Amazon Kinesis/ AWS Lambda (Monitoring real-time)
- Amazon CloudWatch > CloudWatch Metric Filter > SNS
- Amazon CloudWatch > Amazon S3
- AWS SAM > AWS CloudFormation
- AWS WAF > CloudWatch/Kiensis/Amazon S3 Bucket (Monitoring)
- AWS WAF > CloudFront > AWS WAF > ALB > EC2
- Amazon Aurora > Lambda (Helth Check)
- Amazon Kinesis Firehose > Amazon S3
- EC2 Instance > Amazon CloudWatch Logs > Metric Filters > Amazon CloudWatch Alarm > SNS (Monitoring)
- ALB/NLB/CLB > Amazon S3
- CloudTrail Logs > Amazon S3/Amazon CloudWatch Logs/Amazon EventBridge
- VPC Flow Logs > Amazon S3/Amazon CloudWatch Logs
- Route 53 > Amazon S3/Amazon CloudWatch Logs
- CloudFront Access Logs > Amazon CloudWatch/Amazon S3
- AWS Config > Amazon EventBridge > Lambda/SNS/SQS
- Amazon Inspector > Amazon EventBridge/AWS Security Hub
- SDK/CLI/Console/IAM Users & IAM Roles > AWS CloudTrail > Amazon S3/Amazon CloudWatch Logs

# 2. Impossible integrations

- Amazon EventBridge > Amazon S3
- Amazon CloudWatch Agent > Amazon S3
- Amazon Athena > Amazon CloudWatch Logs
- AWS CloudTrail > AWS Kinesis Data Streams
- Amazon CloudWatch Agent > AWS Kinesis Data Streams
