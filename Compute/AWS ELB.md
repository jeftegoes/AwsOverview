# AWS ELB - Elastic Load Balancing <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Why use a load balancer?](#11-why-use-a-load-balancer)
- [2. Why use an Elastic Load Balancer (ELB)?](#2-why-use-an-elastic-load-balancer-elb)
  - [2.1. DualStack Networking](#21-dualstack-networking)
  - [2.2. PrivateLink Integration](#22-privatelink-integration)
- [3. Health Checks](#3-health-checks)
  - [3.1. Settings](#31-settings)
- [4. Types of load balancer on AWS](#4-types-of-load-balancer-on-aws)
  - [4.1. Load Balancer Security Groups](#41-load-balancer-security-groups)
  - [4.2. Classic Load Balancers](#42-classic-load-balancers)
  - [4.3. Application Load Balancer](#43-application-load-balancer)
    - [4.3.1. HTTP Based Traffic](#431-http-based-traffic)
    - [4.3.2. Target Groups](#432-target-groups)
      - [4.3.2.1. Query Strings/Parameters Routing](#4321-query-stringsparameters-routing)
      - [4.3.2.2. Weighting](#4322-weighting)
    - [4.3.3. Good to Know](#433-good-to-know)
    - [4.3.4. Access Logs](#434-access-logs)
    - [4.3.5. Request tracing](#435-request-tracing)
    - [4.3.6. Listener Rules](#436-listener-rules)
  - [4.4. Network Load Balancer (v2)](#44-network-load-balancer-v2)
    - [4.4.1. Based Traffic](#441-based-traffic)
    - [4.4.2. Target Groups](#442-target-groups)
  - [4.5. Gateway Load Balancer](#45-gateway-load-balancer)
    - [4.5.1. Target Groups](#451-target-groups)
- [5. Sticky Sessions (Session Affinity)](#5-sticky-sessions-session-affinity)
  - [5.1. Sticky Sessions - Cookie Names](#51-sticky-sessions---cookie-names)
- [6. Cross-Zone Load Balancing](#6-cross-zone-load-balancing)
- [7. SSL/TLS - Basics](#7-ssltls---basics)
  - [7.1. Load Balancer - SSL Certificates](#71-load-balancer---ssl-certificates)
  - [7.2. SSL - Server Name Indication (SNI)](#72-ssl---server-name-indication-sni)
  - [7.3. Elastic Load Balancers - SSL Certificates](#73-elastic-load-balancers---ssl-certificates)
- [8. Connection Draining](#8-connection-draining)

# 1. Introduction

- Load balancers are servers that forward internet traffic to multiple servers (e.g., EC2 Instances) downstream.

![Load Balance](/Images/LoadBalanceDiagram.png)

## 1.1. Why use a load balancer?

- Spread load across multiple downstream instances.
- Expose a single point of access (DNS) to your application.
- Seamlessly handle failures of downstream instances.
- Do regular health checks to your instances.
- Provide SSL termination (HTTPS) for your websites.
- Enforce stickiness with cookies.
- High availability across zones.
- Separate public traffic from private traffic.

# 2. Why use an Elastic Load Balancer (ELB)?

- An ELB (Elastic Load Balancer) is a **managed load balancer**:
  - AWS guarantees that it will be working.
  - AWS takes care of upgrades, maintenance, high availability.
  - AWS provides only a few configuration knobs.
- It costs less to setup your own load balancer but it will be a lot more effort on your end (maintenance, integrations).
- It is integrated with many AWS offerings / services:
  - EC2, EC2 Auto Scaling Groups, Amazon ECS.
  - AWS Certificate Manager (ACM), CloudWatch.
  - Route 53, AWS WAF, AWS Global Accelerator.

## 2.1. DualStack Networking

- Allows clients communicate with the ELB using both IPv4 and IPv6.
- Supports both ALB and NLB.
- ALB and NLB can have mixed IPv4 and IPv6 targets in separate target groups.
- ELB DualStack ensures compatibility between client and target IP versions.
- IPv4 clients communicate with IPv4 targets, and IPv6 clients communicate with IPv6 targets.
- If you only have IPv4 targets, the ELB automatically converts requests from IPv6 to IPv4.
- Note: AZ must be added/enabled for instances to receive traffic.

## 2.2. PrivateLink Integration

- Exposing Service in VPCs with Overlapping IP addresses (instead of VPC Peering).

# 3. Health Checks

- Health Checks are crucial for Load Balancers.
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests.
- The health check is done on a port and a route (/health is common).
- If the response is not 200 (OK), then the instance is unhealthy.

## 3.1. Settings

| Setting                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| HealthCheckProtocol        | The protocol the load balancer uses when performing health checks on targets. The possible protocols are HTTP and HTTPS. The default is the HTTP protocol. These protocols use the HTTP GET method to send health check requests.                                                                                                                                                                                                                                                                                                                |
| HealthCheckPort            | The port the load balancer uses when performing health checks on targets. The default is to use the port on which each target receives traffic from the load balancer.                                                                                                                                                                                                                                                                                                                                                                           |
| HealthCheckPath            | The destination for health checks on the targets. If the protocol version is HTTP/1.1 or HTTP/2, specify a valid URI (/path?query). The default is /. If the protocol version is gRPC, specify the path of a custom health check method with the format /package.service/method. The default is /AWS.ALB/healthcheck.                                                                                                                                                                                                                            |
| HealthCheckTimeoutSeconds  | The amount of time, in seconds, during which no response from a target means a failed health check. The range is 2-120 seconds. The default is 5 seconds if the target type is instance or ip and 30 seconds if the target type is lambda.                                                                                                                                                                                                                                                                                                       |
| HealthCheckIntervalSeconds | The approximate amount of time, in seconds, between health checks of an individual target. The range is 5-300 seconds. The default is 30 seconds if the target type is instance or ip and 35 seconds if the target type is lambda.                                                                                                                                                                                                                                                                                                               |
| HealthyThresholdCount      | The number of consecutive successful health checks required before considering an unhealthy target healthy. The range is 2-10. The default is 5.                                                                                                                                                                                                                                                                                                                                                                                                 |
| UnhealthyThresholdCount    | The number of consecutive failed health checks required before considering a target unhealthy. The range is 2-10. The default is 2.                                                                                                                                                                                                                                                                                                                                                                                                              |
| Matcher                    | The codes to use when checking for a successful response from a target. These are called Success codes in the console. If the protocol version is HTTP/1.1 or HTTP/2, the possible values are from 200 to 499. You can specify multiple values (for example, "200,202") or a range of values (for example, "200-299"). The default value is 200. If the protocol version is gRPC, the possible values are from 0 to 99. You can specify multiple values (for example, "0,1") or a range of values (for example, "0-5"). The default value is 12. |

# 4. Types of load balancer on AWS

- AWS has **4 kinds of managed Load Balancers**.
- **Classic Load Balancer** (v1 - old generation) - 2009 - CLB.
  - HTTP, HTTPS, TCP, SSL (secure TCP).
  - Layer 4 & 7
- **Application Load Balancer** (v2 - new generation) - 2016 - ALB.
  - HTTP, HTTPS, WebSocket.
  - Layer 7
- **Network Load Balancer** (v2 - new generation) - 2017 - NLB.
  - TCP, TLS (secure TCP), UDP.
  - Layer 4
- **Gateway Load Balancer** - 2020 - GWLB.
  - Operates at layer 3 (Network layer) - IP Protocol.
- Overall, it is recommended to use the newer generation load balancers as they provide more features.
- Some load balancers can be setup as **internal** (private) or **external** (public) ELBs.

## 4.1. Load Balancer Security Groups

![Load Balancer Security Groups](/Images/Compute/LoadBalancerSecurityGroups.png)

- Load Balancer Security Group: Allowing HTTP (80) / HTTPS (443).
- Application Security Group: Allow traffic **ONLY** from Load Balancer.

## 4.2. Classic Load Balancers

- Supports TCP (Layer 4), HTTP & HTTPS (Layer 7).
- Health checks are TCP or HTTP based.
- Fixed hostname XXX.region.elb.amazonaws.com.
  ![Classic Load Balancers ](/Images/Compute/ClassicLoadBalancers.png)

## 4.3. Application Load Balancer

- **Application Load Balancer provides a static DNS name but it does NOT provide a static IP.**
- **The reason being that AWS wants your Elastic Load Balancer to be accessible using a static endpoint, even if the underlying infrastructure that AWS manages changes.**
- Application load balancers is Layer 7 (HTTP).
- Load balancing to multiple HTTP applications across machines (target groups).
- Load balancing to multiple applications on the same machine (ex: containers).
- Support for HTTP/2 and WebSocket.
- Support redirects (from HTTP to HTTPS for example).
- **Routing tables to different target groups**
  - Routing based on path (Path-based Routing) in URL (`example.com/users` and `example.com/posts`).
  - Routing based on hostname in URL (`one.example.com` and `other.example.com`).
  - Routing based on URL Path, Query String, Headers (Content-Based Routing) (`example.com/users?id=123&order=false`).
- ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS).
- Has a port mapping feature to redirect to a dynamic port in ECS.
- In comparison, we'd need multiple Classic Load Balancer per application.

### 4.3.1. HTTP Based Traffic

![Application Load Balancer HTTP Based Traffic](/Images/Compute/ApplicationLoadBalancerHTTPBasedTraffic.png)

### 4.3.2. Target Groups

- EC2 instances (can be managed by an Auto Scaling Group) - HTTP.
- ECS tasks (managed by ECS itself) - HTTP.
- Lambda functions - HTTP request is translated into a JSON event.
- IP Addresses - must be private IPs.
- ALB can route to multiple target groups.
- Health checks are at the target group level.

#### 4.3.2.1. Query Strings/Parameters Routing

![Query Strings/Parameters Routing](/Images/Compute/ApplicationLoadBalancerQueryStringsParametersRouting.png)

#### 4.3.2.2. Weighting

- Specify weight for each Target Group on a single Rule.
- Example: multiple versions of your app, blue/green deployment.
- Allows you to control the distribution of the traffic to your applications.

### 4.3.3. Good to Know

- Fixed hostname (XXX.region.elb.amazonaws.com).
- The application servers don't see the IP of the client directly.
  - The true IP of the client is inserted in the header `X-Forwarded-For`.
  - We can also get Port (`X-Forwarded-Port`) and proto (`X-Forwarded-Proto`).

### 4.3.4. Access Logs

- Elastic Load Balancing provides **access logs** that capture detailed information about requests sent to your load balancer.
- Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses.
- You can use these access logs to analyze traffic patterns and troubleshoot issues.
- Access logging is an optional feature of Elastic Load Balancing that is **disabled by default**.

### 4.3.5. Request tracing

- You can use request tracing to track HTTP requests.
- The load balancer adds a header with a trace identifier to each request it receives.
- Request tracing will not help you to analyze latency specific data.

### 4.3.6. Listener Rules

- Processed in order (with Default Rule).
- Supported Actions (forward, redirect, fixed-response).
- Rule Conditions:
  - `host-header`
  - `http-request-method`
  - `path-pattern`
  - `source-ip`
  - `http-header`
  - `query-string`

## 4.4. Network Load Balancer (v2)

- Network load balancers (Layer 4) allow to:
  - **Forward TCP & UDP traffic to your instances.**
  - Handle millions of request per seconds.
  - Less latency ~100 ms (vs 400 ms for ALB).
- **NLB has one static IP per AZ, and supports assigning Elastic IP** (helpful for whitelisting specific IP).
- NLB are used for extreme performance, TCP or UDP traffic.
- Not included in the AWS free tier.

### 4.4.1. Based Traffic

![Network Load Balancer TCP Based Traffic](/Images/Compute/ApplicationLoadBalancerHTTPBasedTraffic.png)

### 4.4.2. Target Groups

- EC2 instances.
- IP Addresses - must be private IPs.
- Application Load Balancer.
- Health Checks support the **TCP, HTTP and HTTPS Protocols**.

## 4.5. Gateway Load Balancer

- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS.
- Example: Firewalls, Intrusion Detection and Prevention Systems, Deep Packet Inspection Systems, payload manipulation, ...
- Operates at Layer 3 (Network Layer) - IP Packets.
- **Combines the following functions**
  - **Transparent Network Gateway:** Single entry/exit for all traffic.
  - **Load Balancer:** Distributes traffic to your virtual appliances.
- Uses the **GENEVE** protocol on port **6081**.

### 4.5.1. Target Groups

- EC2 instances.
- IP Addresses - must be private IPs.

# 5. Sticky Sessions (Session Affinity)

- It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer.
- **This works for**
  - Classic Load Balancer.
  - Application Load Balancer.
  - Network Load Balancer.
- For both CLB & ALB, the "cookie" used for stickiness has an expiration date you control.
- Use case: make sure the user doesn't lose his session data.
- Enabling stickiness may bring imbalance to the load over the backend EC2 instances.

## 5.1. Sticky Sessions - Cookie Names

- **Application-based Cookies**
  - **Custom cookie**
    - Generated by the target.
    - Can include any custom attributes required by the application.
    - Cookie name must be specified individually for each target group.
    - Don't use `AWSALB`, `AWSALBAPP`, or `AWSALBTG` (reserved for use by the ELB).
  - **Application cookie**
    - Generated by the load balancer.
    - Cookie name is `AWSALBAPP`.
- **Duration-based Cookies**
  - Cookie generated by the load balancer.
  - Cookie name is `AWSALB` for ALB, `AWSELB` for CLB.

# 6. Cross-Zone Load Balancing

- **Without Cross Zone Load Balancing:** Requests are distributed in the instances of the node of the Elastic Load Balancer.
  ![Without Cross Zone Load Balancing](/Images/Compute/AWSELBWithoutCrossZone.png)
- **With Cross Zone Load Balancing:** Each load balancer instance distributes evenly across all registered instances in all AZ.
  ![With Cross Zone Load Balancing](/Images/Compute/AWSELBWithCrossZone.png)
- **Application Load Balancer**
  - Always on (can't be disabled).
  - No charges for inter AZ data.
- **Network Load Balancer**
  - Disabled by default.
  - You pay charges ($) for inter AZ data if enabled.
- **Classic Load Balancer**
  - Disabled by default.
  - No charges for inter AZ data if enabled.

# 7. SSL/TLS - Basics

- An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption).
- **SSL** refers to Secure Sockets Layer, used to encrypt connections.
- **TLS** refers to Transport Layer Security, which is a newer version.
- Nowadays, **TLS certificates are mainly used**, but people still refer as SSL.
- Public SSL certificates are issued by Certificate Authorities (CA).
- Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc...
- SSL certificates have an expiration date (you set) and must be renewed.

## 7.1. Load Balancer - SSL Certificates

- The load balancer uses an X.509 certificate (SSL/TLS server certificate).
- You can manage certificates using ACM (AWS Certificate Manager).
- You can create upload your own certificates alternatively.
- HTTPS listener:
  - You must specify a default certificate.
  - You can add an optional list of certs to support multiple domains.
  - **Clients can use SNI (Server Name Indication) to specify the hostname they reach.**
  - Ability to specify a security policy to support older versions of SSL / TLS (legacy clients).

## 7.2. SSL - Server Name Indication (SNI)

- **Server Name Indication (SNI) allows you to expose multiple HTTPS applications each with its own SSL certificate on the same listener. Read more here: https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/**
- SNI solves the problem of loading **multiple SSL certificates onto one web server** (to serve multiple websites).
- It's a "newer" protocol, and requires the client to **indicate** the hostname of the target server in the initial SSL handshake.
- The server will then find the correct certificate, or return the default one.
- **Note**
  - Only works for ALB & NLB (newer generation), CloudFront.
  - Does not work for CLB (older gen).

## 7.3. Elastic Load Balancers - SSL Certificates

- **Classic Load Balancer (v1)**
  - Support only one SSL certificate.
  - Must use multiple CLB for multiple hostname with multiple SSL certificates.
- **Application Load Balancer (v2)**
  - Supports multiple listeners with multiple SSL certificates.
  - Uses Server Name Indication (SNI) to make it work.
- **Network Load Balancer (v2)**
  - Supports multiple listeners with multiple SSL certificates.
  - Uses Server Name Indication (SNI) to make it work.

# 8. Connection Draining

- **Feature naming**
  - Connection Draining - for CLB.
  - Deregistration Delay - for ALB & NLB.
- Time to complete "in-flight requests" while the instance is de-registering or unhealthy.
- Stops sending new requests to the EC2 instance which is de-registering.
- Between 1 to 3600 seconds (default: 300 seconds).
- Can be disabled (set value to 0).
- Set to a low value if your requests are short.
  TODO: DIAGRAM
