# Amazon Route 53<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is DNS?](#1-what-is-dns)
  - [1.1. DNS Terminologies](#11-dns-terminologies)
  - [1.2. How DNS Works](#12-how-dns-works)
- [2. Amazon Route 53](#2-amazon-route-53)
  - [2.1. Records](#21-records)
  - [2.2. Record Types](#22-record-types)
  - [2.3. Hosted Zones](#23-hosted-zones)
  - [2.4. Records TTL (Time To Live)](#24-records-ttl-time-to-live)
  - [2.5. CNAME vs Alias](#25-cname-vs-alias)
    - [2.5.1. Alias Records](#251-alias-records)
      - [2.5.1.1. Targets](#2511-targets)
  - [2.6. Health Checks](#26-health-checks)
    - [2.6.1. Monitor an Endpoint](#261-monitor-an-endpoint)
    - [2.6.2. Calculated Health Checks](#262-calculated-health-checks)
    - [2.6.3. Private Hosted Zones](#263-private-hosted-zones)
  - [2.7. Routing Policies](#27-routing-policies)
    - [2.7.1. Simple](#271-simple)
    - [2.7.2. Weighted](#272-weighted)
    - [2.7.3. Latency-based](#273-latency-based)
    - [2.7.4. Failover (Active-Passive)](#274-failover-active-passive)
    - [2.7.5. Geolocation](#275-geolocation)
    - [2.7.6. Geoproximity](#276-geoproximity)
    - [2.7.7. IP-based Routing](#277-ip-based-routing)
    - [2.7.8. Multi-Value](#278-multi-value)
  - [2.8. Traffic flow](#28-traffic-flow)
  - [2.9. Domain Registar vs DNS Service](#29-domain-registar-vs-dns-service)
    - [2.9.1. 3rd Party Registrar with Amazon Route 53](#291-3rd-party-registrar-with-amazon-route-53)

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
  - Authoritative = The customer (you) can update the DNS records.
- **Route 53 is also a Domain Registrar**.
- Ability to check the health of your resources.
- The only AWS service which provides 100% availability SLA.
- Why Route 53? 53 is a reference to the traditional DNS port.

## 2.1. Records

- How you want to route traffic for a domain.
- **Each record contains**
  - **Domain/subdomain Name:** E.g., example.com
  - **Record Type:** E.g., A or AAAA
  - **Value:** E.g., 12.34.56.78
  - **Routing Policy:** How Route 53 responds to queries.
  - **TTL:** Amount of time the record cached at DNS Resolvers.
- Route 53 supports the following DNS record types:
  - (must know) A / AAAA / CNAME / NS
  - (advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV

## 2.2. Record Types

- `A` - Maps a hostname to IPv4.
- `AAAA` - Maps a hostname to IPv6.
- `CNAME` - Maps a hostname to another hostname:
  - The target is a domain name which must have an `A` or `AAAA` record.
  - Can't create a `CNAME` record for the top node of a DNS namespace (Zone Apex).
  - **Example:** You can't create for example.com, but you can create for www.example.com
- `NS` - Name Servers for the Hosted Zone.
  - Control how traffic is routed for a domain.

## 2.3. Hosted Zones

- A container for records that define how to route traffic to a domain and its subdomains.
- **Public Hosted Zones**
  - Contains records that specify how to route traffic on the Internet (public domain names) **application1.mypublicdomain.com**.
- **Private Hosted Zones**
  - Contain records that specify how you route traffic within one or more VPCs (private domain names) **application1.company.internal**.
- **You pay $0.50 per month per hosted zone.**

## 2.4. Records TTL (Time To Live)

- **High TTL - e.g., 24 hr**
  - Less traffic on Route 53.
  - Possibly outdated records.
- **Low TTL - e.g., 60 sec**
  - More traffic on Route 53 ($$).
  - Records are outdated for less time.
  - Easy to change records.
- **Except for Alias records, TTL is mandatory for each DNS record.**

## 2.5. CNAME vs Alias

- AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname:
  - **lb1-1234.us-east-2.elb.amazonaws.com** and you **want myapp.mydomain.com**
- **CNAME**
  - Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com).
  - ONLY FOR NON ROOT DOMAIN (aka. something.mydomain.com).
- **Alias**
  - Points a hostname to an AWS Resource (app.mydomain.com => blabla.**amazonaws**.com).
  - **Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com).**
  - Free of charge.
  - Native health check.

### 2.5.1. Alias Records

- Maps a hostname to an AWS resource.
- An extension to DNS functionality.
- Automatically recognizes changes in the resource's IP addresses.
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g.: example.com
- Alias Record is always of type A/AAAA for AWS resources (IPv4 / IPv6).
- **You can't set the TTL.**

#### 2.5.1.1. Targets

- Elastic Load Balancers.
- CloudFront Distributions.
- [API Gateway](/Networking%20&%20Content%20Delivery/Amazon%20API%20Gateway.md).
- Elastic Beanstalk environments.
- S3 Websites.
- VPC Interface Endpoints.
- Global Accelerator.
- Route 53 record in the same hosted zone.
- **You cannot set an ALIAS record for an EC2 DNS name.**

## 2.6. Health Checks

- HTTP Health Checks are only for **public resources**.
- Health Check => Automated DNS Failover
  1. Health checks that monitor an endpoint (application, server, other AWS resource).
  2. Health checks that monitor other health checks (Calculated Health Checks).
  3. Health checks that monitor CloudWatch Alarms (full control !!) - e.g., throttles of DynamoDB, alarms on RDS, custom metrics, ... (helpful for private resources).
- Health Checks are integrated with [Amazon CloudWatch](/Management%20&%20Governance/Amazon%20CloudWatch.md) metrics.

### 2.6.1. Monitor an Endpoint

- **About 15 global health checkers will check the endpoint health**
  - Healthy/Unhealthy Threshold - 3 (default).
  - Interval - 30 sec (can set to 10 sec - higher cost).
  - Supported protocol: HTTP, HTTPS and TCP.
  - If > 18% of health checkers report the endpoint is healthy, Route 53 considers it.**Healthy**.
    - Otherwise, it's **Unhealthy**.
  - Ability to choose which locations you want Route 53 to use.
- Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes.
- Health Checks can be setup to pass / fail based on the text in the first 5120 bytes of the response.
- Configure you router/firewall to allow incoming requests from Route 53 Health Checkers.

### 2.6.2. Calculated Health Checks

- Combine the results of multiple Health Checks into a single Health Check.
- We can use **OR, AND, or NOT**.
- Can monitor up to 256 Child Health Checks.
- Specify how many of the health checks need to pass to make the parent pass.
- **Usage:** Perform maintenance to your website without causing all health checks to fail.

### 2.6.3. Private Hosted Zones

- Route 53 health checkers are outside the VPC.
- They can't access **private** endpoints (private VPC or on-premises resource).
- We can create a **CloudWatch Metric** and associate a **CloudWatch Alarm**, then create a Health Check that checks the alarm itself.

## 2.7. Routing Policies

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

### 2.7.1. Simple

- Typically, route traffic to a single resource.
- Can specify multiple values in the same record.
- **If multiple values are returned, a random one is chosen by the client.**
- When Alias enabled, specify only one AWS resource.
- Can't be associated with Health Checks.

### 2.7.2. Weighted

- Control the % of the requests that go to each specific resource.
- Assign each record a relative weight.
  - Weights don't need to sum up to 100.
- DNS records must have the same name and type.
- Can be associated with Health Checks.
- **Use cases:** Load balancing between regions, testing new application versions...
- **Assign a weight of 0 to a record to stop sending traffic to a resource.**
- **If all records have weight of 0, then all records will be returned equally.**

### 2.7.3. Latency-based

- Redirect to the resource that has the least latency close to us.
- Super helpful when latency for users is a priority.
- **Latency is based on traffic between users and AWS Regions.**
- Germany users may be directed to the US (if that's the lowest latency).
- Can be associated with Health Checks (has a failover capability).

### 2.7.4. Failover (Active-Passive)

![Amazon Route53 Failover Policie](/Images/AmazonRoute53FailoverPolicie.png)

### 2.7.5. Geolocation

- Different from Latency-based!
- **This routing is based on user location.**
- Specify location by Continent, Country or by US State (if there's overlapping, most precise location selected).
- Should create a **"Default"** record (in case there's no match on location).
- **Use cases:** website localization, restrict content distribution, load balancing, ...
- Can be associated with Health Checks.

### 2.7.6. Geoproximity

- Route traffic to your resources based on the geographic location of users and resources.
- Ability **to shift more traffic to resources based** on the defined bias.
- To change the size of the geographic region, specify bias values:
  - To expand (1 to 99) - more traffic to the resource.
  - To shrink (-1 to -99) - less traffic to the resource.
- **Resources can be**
  - AWS resources (specify AWS region).
  - Non-AWS resources (specify Latitude and Longitude).
- You must use Route 53 **Traffic Flow** (advanced) to use this feature.

### 2.7.7. IP-based Routing

- Routing is based on clients' IP addresses.
- **You provide a list of CIDRs for your clients** and the corresponding endpoints/locations (user-IP-to-endpoint mappings).
- **Use cases:** Optimize performance, reduce network costs...
- **Example:** Route end users from a particular ISP to a specific endpoint.

### 2.7.8. Multi-Value

- Use when routing traffic to multiple resources.
- Route 53 return multiple values/resources.
- Can be associated with Health Checks (return only values for healthy resources).
- Up to 8 healthy records are returned for each Multi-Value query.
- **Multi-Value is not a substitute for having an ELB.**

## 2.8. Traffic flow

- Simplify the process of creating and maintaining records in large and complex configurations.
- Visual editor to manage complex routing decision trees.
- Configurations can be saved as Traffic Flow Policy:
  - Can be applied to different Route 53 Hosted Zones (different domain names).
  - Supports versioning.

## 2.9. Domain Registar vs DNS Service

- You buy or register your domain name with a Domain Registrar typically by paying annual charges (e.g., GoDaddy, Amazon Registrar Inc., ...).
- The Domain Registrar usually provides you with a DNS service to manage your DNS records.
- But you can use another DNS service to manage your DNS records.
- **Example:** Purchase the domain from GoDaddy and use Route 53 to manage your DNS records.

### 2.9.1. 3rd Party Registrar with Amazon Route 53

- If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS Service provider.

1. Create a Hosted Zone in Route 53.
2. Update NS Records on 3rd party website to use Route 53 Name Servers.

- **Domain Registrar != DNS Service**
- But every Domain Registrar usually comes with some DNS features.
