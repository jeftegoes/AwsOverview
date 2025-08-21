# AWS Multi Region Architectures <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Multi Region Services](#1-multi-region-services)
- [2. Multi Region with Route 53](#2-multi-region-with-route-53)

# 1. Multi Region Services

- DynamoDB Global Tables (multi-way replication, enabled by Streams).
- AWS Config Aggregators (multi region & multi account).
- RDS Cross-Region Read Replicas (used for Read & DR).
- Aurora Global Database (one region is master, other is for Read & DR).
- EBS volumes snapshots, AMI, RDS snapshots can be copied to other regions.
- VPC peering to allow private traffic between regions.
- Route 53 uses a global network of DNS servers.
- S3 Cross-Region Replication.
- CloudFront for Global CDN at the Edge Locations.
- Lambda@Edge for Global Lambda function at Edge Locations (A/B testing).

# 2. Multi Region with Route 53

- Health Check => automated DNS failovers:

1. Health checks that monitor an endpoint (application, server, other AWS resource) Helpful to create a `/health` route.
2. Health checks that monitor other health checks (calculated health checks).
3. Health checks that monitor CloudWatch alarms (full control !!) - e.g. throttles of DynamoDB, custom metrics, etc.

- Health Checks are integrated with CW metrics.
