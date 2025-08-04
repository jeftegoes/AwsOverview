# Network Costs <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Networking Costs in AWS per GB - Simplified](#1-networking-costs-in-aws-per-gb---simplified)
- [2. Minimizing egress traffic network cost](#2-minimizing-egress-traffic-network-cost)
- [3. S3 Data Transfer Pricing - Analysis for USA](#3-s3-data-transfer-pricing---analysis-for-usa)
- [4. NAT Gateway vs Gateway VPC Endpoint](#4-nat-gateway-vs-gateway-vpc-endpoint)

# 1. Networking Costs in AWS per GB - Simplified

- Use Private IP instead of Public IP for good savings and better network performance.
- Use same AZ for maximum savings (at the cost of high availability).
  ![Networking Costs in AWS per GB - Simplified](/Images/Solution%20Architect/NetworkingCostsAWSPerGB%20.png)

# 2. Minimizing egress traffic network cost

- **Egress traffic:** Outbound traffic (from AWS to outside).
- **Ingress traffic:** Inbound traffic - from outside to AWS (typically free).
- Try to keep as much internet traffic within AWS to minimize costs.
- Direct Connect location that are co-located in the same AWS Region result in lower cost for egress network.

TODO: DIAGRAM

# 3. S3 Data Transfer Pricing - Analysis for USA

- **S3 ingress:** Free.
- **S3 to Internet:** $0.09 per GB.
- **S3 Transfer Acceleration**
  - Faster transfer times (50 to 500% better).
  - Additional cost on top of Data Transfer Pricing: +$0.04 to $0.08 per GB.
- **S3 to CloudFront:** $0.00 per GB.
- **CloudFront to Internet:** $0.085 per GB (slightly cheaper than S3).
  - Caching capability (lower latency).
  - Reduce costs associated with S3 Requests Pricing (7x cheaper with CloudFront).
- **S3 Cross-Region Replication:** $0.02 per GB.

TODO: DIAGRAM

# 4. NAT Gateway vs Gateway VPC Endpoint

TODO: DIAGRAM
