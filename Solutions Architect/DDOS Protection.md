# AWS Best Practices for DDoS Resiliency <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Overview](#1-overview)
- [2. Edge Location Mitigation (BP1, BP3)](#2-edge-location-mitigation-bp1-bp3)
- [3. Best pratices for DDoS mitigation](#3-best-pratices-for-ddos-mitigation)
- [4. Application Layer Defense](#4-application-layer-defense)
- [5. Attack surface reduction](#5-attack-surface-reduction)

# 1. Overview

- To protect your system from DDoS attack, you can do the following:
  - Use an Amazon CloudFront service for distributing both static and dynamic content.
  - Use an Application Load Balancer with Auto Scaling groups for your EC2 instances. Prevent direct Internet traffic to your Amazon RDS database by deploying it to a new private subnet.
  - Set up alerts in Amazon CloudWatch to look for high Network In and CPU utilization metrics.

# 2. Edge Location Mitigation (BP1, BP3)

- **BP1 - CloudFront**
  - Web Application delivery at the edge.
  - Protect from DDoS Common Attacks (SYN floods, UDP reflection...).
- **BP1 - Global Accelerator**
  - Access your application from the edge.
  - Integration with Shield for DDoS protection.
  - Helpful if your backend is not compatible with CloudFront.
- **BP3 - Route 53**
  - Domain Name Resolution at the edge.
  - DDoS Protection mechanism.

# 3. Best pratices for DDoS mitigation

- **Infrastructure layer defense (BP1, BP3, BP6)**
  - Protect Amazon EC2 against high traffic.
  - That includes using Global Accelerator, Route 53, CloudFront, Elastic Load Balancing.
- **Amazon EC2 with Auto Scaling (BP7)**
  - Helps scale in case of sudden traffic surges including a flash crowd or a DDoS attack.
- **Elastic Load Balancing (BP6)**
  - Elastic Load Balancing scales with the traffic increases and will distribute the traffic to many EC2 instances.

# 4. Application Layer Defense

- **Detect and filter malicious web requests (BP1, BP2)**
  - CloudFront cache static content and serve it from edge locations, protecting your backend.
  - AWS WAF is used on top of CloudFront and Application Load Balancer to filter and block requests based on request signatures.
  - WAF rate-based rules can automatically block the IPs of bad actors.
  - Use managed rules on WAF to block attacks based on IP reputation, or block anonymous Ips.
  - CloudFront can block specific geographies.
- **Shield Advanced (BP1, BP2, BP6)**
  - Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks.

# 5. Attack surface reduction

- **Obfuscating AWS resources (BP1, BP4, BP6)**
  - Using CloudFront, API Gateway, Elastic Load Balancing to hide your backend resources (Lambda functions, EC2 instances).
- **Security groups and Network ACLs (BP5)**
  - Use security groups and NACLs to filter traffic based on specific IP at the subnet or ENI-level.
  - Elastic IP are protected by AWS Shield Advanced.
- **Protecting API endpoints (BP4)**
  - Hide EC2, Lambda, elsewhere.
  - Edge-optimized mode, or CloudFront + regional mode (more control for DDoS).
  - WAF + API Gateway: burst limits, headers filtering, use API keys.
