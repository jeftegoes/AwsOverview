# Amazon Route 53<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is DNS?](#1-what-is-dns)
  - [1.1. DNS Terminologies](#11-dns-terminologies)
  - [1.2. How DNS Works](#12-how-dns-works)
- [2. Amazon Route 53](#2-amazon-route-53)
- [3. Records](#3-records)
  - [3.1. Record Types](#31-record-types)
  - [3.2. Hosted Zones](#32-hosted-zones)
    - [3.2.1. VPC Configuration Required:](#321-vpc-configuration-required)
  - [3.3. Public vs. Private Hosted Zones](#33-public-vs-private-hosted-zones)
    - [3.3.1. DNS hostnames and DNS resolution](#331-dns-hostnames-and-dns-resolution)
  - [3.4. Records TTL (Time To Live)](#34-records-ttl-time-to-live)
  - [3.5. CNAME vs Alias](#35-cname-vs-alias)
    - [3.5.1. Alias Records](#351-alias-records)
      - [3.5.1.1. Targets](#3511-targets)
    - [3.5.2. Core Difference (Simplified)](#352-core-difference-simplified)
- [4. Health Checks](#4-health-checks)
  - [4.1. Monitor an Endpoint](#41-monitor-an-endpoint)
  - [4.2. Calculated Health Checks](#42-calculated-health-checks)
  - [4.3. Private Hosted Zones](#43-private-hosted-zones)
- [5. Routing Policies](#5-routing-policies)
  - [5.1. Simple](#51-simple)
  - [5.2. Weighted](#52-weighted)
  - [5.3. Latency-based](#53-latency-based)
  - [5.4. Failover (Active-Passive)](#54-failover-active-passive)
  - [5.5. Geolocation](#55-geolocation)
  - [5.6. Geoproximity](#56-geoproximity)
  - [5.7. IP-based Routing](#57-ip-based-routing)
  - [5.8. Multi-Value](#58-multi-value)
  - [5.9. Traffic flow](#59-traffic-flow)
- [6. Domain Registar vs DNS Service](#6-domain-registar-vs-dns-service)
  - [6.1. 3rd Party Registrar with Amazon Route 53](#61-3rd-party-registrar-with-amazon-route-53)
- [7. Amazon Route 53 - Resolver](#7-amazon-route-53---resolver)
  - [7.1. Outbound](#71-outbound)
  - [7.2. Inbound](#72-inbound)
- [8. Routing Traffic to an S3 Bucket with Amazon Route 53](#8-routing-traffic-to-an-s3-bucket-with-amazon-route-53)

# 1. What is DNS?

- Domain Name System which translates the human friendly hostnames into the machine IP addresses:
  - www.google.com => 172.217.18.36
  - DNS is the backbone of the Internet
  - DNS uses hierarchical naming structure:
    - .com
    - example.com
    - www.example.com
    - api.example.com

## 1.1. DNS Terminologies

- **Domain Registrar:** Amazon Route 53, GoDaddy, ...
- **DNS Records:** A, AAAA, CNAME, NS, ...
- **Zone File:** Contains DNS records.
- **Name Server:** Resolves DNS queries (Authoritative or Non-Authoritative).
- **Top Level Domain (TLD):** .com, .us, .in, .gov, .org, ...
- **Second Level Domain (SLD):** amazon.com, google.com, ...

![Structure of URL](/Images/StructureOfUrl.png)

## 1.2. How DNS Works

![DNS Route Traffic](/Images/DNSRouteTraffic.png)

# 2. Amazon Route 53

- A highly available, scalable, fully managed and Authoritative DNS.
  - Authoritative = The customer (we) can update the DNS records.
- **Route 53 is also a Domain Registrar**.
- Ability to check the health of your resources.
- The only AWS service which provides 100% availability SLA.
- Why Route 53? 53 is a reference to the traditional DNS port.

# 3. Records

- How we want to route traffic for a domain.
- **Each record contains**
  - **Domain/subdomain Name:** E.g., example.com
  - **Record Type:** E.g., A or AAAA
  - **Value:** E.g., 12.34.56.78
  - **Routing Policy:** How Route 53 responds to queries.
  - **TTL:** Amount of time the record cached at DNS Resolvers.
- Route 53 supports the following DNS record types:
  - (must know) A / AAAA / CNAME / NS
  - (advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV

## 3.1. Record Types

- `A` - Maps a hostname to IPv4.
- `AAAA` - Maps a hostname to IPv6.
- `CNAME` - Maps a hostname to another hostname:
  - The target is a domain name which must have an `A` or `AAAA` record.
  - Can't create a `CNAME` record for the top node of a DNS namespace (Zone Apex).
  - **Example:** We can't create for example.com, but we can create for www.example.com
- `NS` - Name Servers for the Hosted Zone.
  - Control how traffic is routed for a domain.

## 3.2. Hosted Zones

- A container for records that define how to route traffic to a domain and its subdomains.
- **Public Hosted Zones**
  - Contains records that specify how to route traffic on the Internet (public domain names) `application1.mypublicdomain.com`.
- **Private Hosted Zones**
  - Contain records that specify how we route traffic within one or more VPCs (private domain names) `application1.company.internal`.
  - You create records (e.g., A, CNAME) to define how traffic should be routed **privately**.
- **We pay $0.50 per month per hosted zone.**

### 3.2.1. VPC Configuration Required:

- To associate a VPC with a private hosted zone, ensure these settings are enabled:
  - `enableDnsHostnames = true`
  - `enableDnsSupport = true`

## 3.3. Public vs. Private Hosted Zones

- Public Hosted Zone
  ![Public Hosted Zone](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53PublicHostedZone.png)
- Private Hosted Zone
  ![Private Hosted Zone](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53PrivateHostedZone.png)

### 3.3.1. DNS hostnames and DNS resolution

- DNS hostnames and DNS resolution are required settings for private hosted zones.
- DNS queries for private hosted zones can be resolved by the Amazon-provided VPC DNS server only.
- As a result, these options must be enabled for your private hosted zone to work.
  ![Amazon Route 53 DNS Hostname and DNS Resolution](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53DNSHostnameResolution.png)

## 3.4. Records TTL (Time To Live)

- **High TTL - e.g., 24 hr**
  - Less traffic on Route 53.
  - Possibly outdated records.
- **Low TTL - e.g., 60 sec**
  - More traffic on Route 53 ($$).
  - Records are outdated for less time.
  - Easy to change records.
- **Except for Alias records, TTL is mandatory for each DNS record.**

## 3.5. CNAME vs Alias

- AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname:
  - **lb1-1234.us-east-2.elb.amazonaws.com** and we **want myapp.mydomain.com**
- **CNAME**
  - Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com).
  - ONLY FOR NON ROOT DOMAIN (aka. something.mydomain.com).
- **Alias**
  - Points a hostname to an AWS Resource (app.mydomain.com => blabla.**amazonaws**.com).
  - **Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com).**
  - Free of charge.
  - Native health check.

### 3.5.1. Alias Records

- Maps a hostname to an AWS resource.
- An extension to DNS functionality.
- Automatically recognizes changes in the resource's IP addresses.
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g.: example.com
- Alias Record is always of type A/AAAA for AWS resources (IPv4 / IPv6).
- **We can't set the TTL.**

#### 3.5.1.1. Targets

- Elastic Load Balancers.
- CloudFront Distributions.
- [API Gateway](/Networking%20&%20Content%20Delivery/Amazon%20API%20Gateway.md).
- Elastic Beanstalk environments.
- S3 Websites.
- VPC Interface Endpoints.
- Global Accelerator.
- Route 53 record in the same hosted zone.
- **We cannot set an ALIAS record for an EC2 DNS name.**

### 3.5.2. Core Difference (Simplified)

| Feature          | Alias Record                             | CNAME Record                                |
| ---------------- | ---------------------------------------- | ------------------------------------------- |
| **Target**       | AWS resources (like ELB, CloudFront, S3) | Any domain name                             |
| **Root Domain?** | Yes (e.g., `example.com`)                | No (only subdomains like `www.example.com`) |
| **DNS Type**     | Acts like **A or AAAA**                  | Always a **CNAME**                          |
| **Cost**         | Free DNS queries to AWS targets          | Standard DNS pricing                        |

# 4. Health Checks

- HTTP Health Checks are only for **public resources**.
- Health Check => Automated DNS Failover
  1. Health checks that monitor an endpoint (application, server, other AWS resource).
  2. Health checks that monitor other health checks (Calculated Health Checks).
  3. Health checks that monitor CloudWatch Alarms (full control !!) - e.g., throttles of DynamoDB, alarms on RDS, custom metrics, ... (helpful for private resources).
- Health Checks are integrated with [Amazon CloudWatch](/Management%20&%20Governance/Amazon%20CloudWatch.md) metrics.
  ![Amazon Route 53 - Health Checks](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53HealthChecks.png)

## 4.1. Monitor an Endpoint

- **About 15 global health checkers will check the endpoint health**
  - Healthy/Unhealthy Threshold - 3 (default).
  - Interval - 30 sec (can set to 10 sec - higher cost).
  - **Supported protocol:** HTTP, HTTPS and TCP.
  - If > 18% of health checkers report the endpoint is healthy, Route 53 considers it.**Healthy**.
    - Otherwise, it's **Unhealthy**.
  - Ability to choose which locations we want Route 53 to use.
- Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes.
- Health Checks can be setup to pass / fail based on the text in the first 5120 bytes of the response.
- Configure we router/firewall to allow incoming requests from Route 53 Health Checkers.

## 4.2. Calculated Health Checks

- Combine the results of multiple Health Checks into a single Health Check.
- We can use **OR, AND, or NOT**.
- Can monitor up to 256 Child Health Checks.
- Specify how many of the health checks need to pass to make the parent pass.
- **Usage:** Perform maintenance to your website without causing all health checks to fail.

## 4.3. Private Hosted Zones

- Route 53 health checkers are outside the VPC.
- They can't access **private** endpoints (private VPC or on-premises resource).
- We can create a **CloudWatch Metric** and associate a **CloudWatch Alarm**, then create a Health Check that checks the alarm itself.

# 5. Routing Policies

- Define how Route 53 responds to DNS queries.
- **Don't get confused by the word "Routing"**
  - It's not the same as Load balancer routing which routes the traffic.
  - DNS does not route any traffic, it only responds to the DNS queries.
- **Route 53 Supports the following Routing Policies**
  - Simple.
  - Weighted.
  - Failover.
  - Latency based.
  - Geolocation.
  - Multi-Value Answer.
  - Geoproximity (using Route 53 Traffic Flow feature).

## 5.1. Simple

- Typically, route traffic to a single resource.
- Can specify multiple values in the same record.
- **If multiple values are returned, a random one is chosen by the client.**
- When Alias enabled, specify only one AWS resource.
- Can't be associated with Health Checks.

## 5.2. Weighted

- Control the % of the requests that go to each specific resource.
- Assign each record a relative weight.
  - Weights don't need to sum up to 100.
- DNS records must have the same name and type.
- Can be associated with Health Checks.
- **Use cases:** Load balancing between regions, testing new application versions...
- **Assign a weight of 0 to a record to stop sending traffic to a resource.**
- **If all records have weight of 0, then all records will be returned equally.**

## 5.3. Latency-based

- Redirect to the resource that has the least latency close to us.
- Super helpful when latency for users is a priority.
- **Latency is based on traffic between users and AWS Regions.**
- Germany users may be directed to the US (if that's the lowest latency).
- Can be associated with Health Checks (has a failover capability).

## 5.4. Failover (Active-Passive)

- Use **active-passive failover** when a primary resource should handle traffic most of the time, with a secondary resource on standby.
- **Route 53** responds to DNS queries with only **healthy primary resources**.
- If all primary resources become **unhealthy**, Route 53 automatically switches to respond with the **healthy secondary resources**.
- This ensures high availability and automatic DNS-based failover.
  ![Amazon Route 53 Failover Policie](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53FailoverPolicie.png)

## 5.5. Geolocation

- Different from Latency-based!
- **This routing is based on user location.**
- Specify location by Continent, Country or by US State (if there's overlapping, most precise location selected).
- Should create a **"Default"** record (in case there's no match on location).
- **Use cases:** Website localization, restrict content distribution, load balancing, ...
- Can be associated with Health Checks.

## 5.6. Geoproximity

- Route traffic to your resources based on the geographic location of users and resources.
- Ability **to shift more traffic to resources based** on the defined bias.
- To change the size of the geographic region, specify bias values:
  - To expand (1 to 99) - more traffic to the resource.
  - To shrink (-1 to -99) - less traffic to the resource.
- **Resources can be**
  - AWS resources (specify AWS region).
  - Non-AWS resources (specify Latitude and Longitude).
- We must use Route 53 **Traffic Flow** (advanced) to use this feature.

## 5.7. IP-based Routing

- Routing is based on clients' IP addresses.
- **We provide a list of CIDRs for your clients** and the corresponding endpoints/locations (user-IP-to-endpoint mappings).
- **Use cases:** Optimize performance, reduce network costs...
- **Example:** Route end users from a particular ISP to a specific endpoint.

## 5.8. Multi-Value

- Use when routing traffic to multiple resources.
- Route 53 return multiple values/resources.
- Can be associated with Health Checks (return only values for healthy resources).
- Up to 8 healthy records are returned for each Multi-Value query.
- **Multi-Value is not a substitute for having an ELB.**

## 5.9. Traffic flow

- Simplify the process of creating and maintaining records in large and complex configurations.
- Visual editor to manage complex routing decision trees.
- Configurations can be saved as Traffic Flow Policy:
  - Can be applied to different Route 53 Hosted Zones (different domain names).
  - Supports versioning.

# 6. Domain Registar vs DNS Service

- We buy or register your domain name with a Domain Registrar typically by paying annual charges (e.g., GoDaddy, Amazon Registrar Inc., ...).
- The Domain Registrar usually provides we with a DNS service to manage your DNS records.
- But we can use another DNS service to manage your DNS records.
- **Example:** Purchase the domain from GoDaddy and use Route 53 to manage your DNS records.

## 6.1. 3rd Party Registrar with Amazon Route 53

- If we buy your domain on a 3rd party registrar, we can still use Route 53 as the DNS Service provider.

1. Create a Hosted Zone in Route 53.
2. Update NS Records on 3rd party website to use Route 53 Name Servers.

- **Domain Registrar != DNS Service**
- But every Domain Registrar usually comes with some DNS features.

# 7. Amazon Route 53 - Resolver

## 7.1. Outbound

![Amazon Route 53 - Resolver](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53ResolverOutboundEndpoint.png)

- **The diagram illustrates the following steps**
  1. An Amazon EC2 instance needs to resolve a DNS query to the domain internal.example.com. The authoritative DNS server is in the on-premises data center. This DNS query is sent to the VPC+2 in the VPC that connects to Route 53 Resolver.
  2. A Route 53 Resolver forwarding rule is configured to forward queries to internal.example.com in the on-premises data center.
  3. The query is forwarded to an outbound endpoint.
  4. The outbound endpoint forwards the query to the on-premises DNS resolver through a private connection between AWS and the data center. The connection can be either AWS Direct Connect or AWS Site-to-Site VPN, depicted as a virtual private gateway.
  5. The on-premises DNS resolver resolves the DNS query for internal.example.com and returns the answer to the Amazon EC2 instance via the same path in reverse.

## 7.2. Inbound

![Amazon Route 53 - Resolver](/Images/Networking%20&%20Content%20Delivery/AmazonRoute53ResolverInboundEndpoint.png)

- **The diagram illustrates the following steps**
  1. A client in the on-premises data center needs to resolve a DNS query to an AWS resource for the domain **dev.example.com**.
     - It sends the query to the on-premises DNS resolver.
  2. The on-premises DNS resolver has a forwarding rule that points queries to dev.example.com to an inbound endpoint.
  3. The query arrives at the inbound endpoint through a private connection, such as AWS Direct Connect or AWS Site-to-Site VPN, depicted as a virtual gateway.
  4. The inbound endpoint sends the query to Route 53 Resolver, and Route 53 Resolver resolves the DNS query for **dev.example.com** and returns the answer to the client via the same path in reverse.

# 8. Routing Traffic to an S3 Bucket with Amazon Route 53

- Amazon S3 allows you to host a static website (webpages and client-side scripts).
- You have an **S3 bucket configured for static website hosting**.
- The bucket name **exactly matches** the intended domain or subdomain (e.g., `acme.example.com` for `acme.example.com`).
