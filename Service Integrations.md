# 1. Possible integrations

- Amazon GuardDuty > Amazon EventBridge > SQS/SNS/Lambda
- Amazon Inspector > Security Hub/Amazon EventBridge
- Amazon CloudWatch > Subscription Filter > Amazon Kinesis/ AWS Lambda (Monitoring real-time)
- Amazon CloudWatch > CloudWatch Metric Filter > SNS
- Amazon CloudWatch > Amazon S3
- AWS SAM > AWS CloudFormation
- AWS WAF > CloudWatch/Kiensis/S3 Bucket (Monitoring)
- AWS WAF > CloudFront > AWS WAF > ALB > EC2
- Amazon Aurora > Lambda (Helth Check)
- Amazon Kinesis Firehose > Amazon S3
- EC2 Instance > Amazon CloudWatch Logs > Metric Filters > Amazon CloudWatch Alarm > SNS (Monitoring)
