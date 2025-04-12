# Amazon VPC - Virtual Private Cloud <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Understanding CIDR](#1-understanding-cidr)
  - [1.1. Subnet Mask](#11-subnet-mask)
  - [1.2. Public vs. Private IP (IPv4)](#12-public-vs-private-ip-ipv4)
  - [1.3. Default VPC](#13-default-vpc)
- [2. VPC in AWS](#2-vpc-in-aws)
- [3. Subnet](#3-subnet)
- [4. NAT Gateways](#4-nat-gateways)
  - [4.1. NAT Gateway with High Availability](#41-nat-gateway-with-high-availability)
  - [4.2. NAT Gateway vs. NAT Instance](#42-nat-gateway-vs-nat-instance)
- [5. Internet Gateway](#5-internet-gateway)
  - [5.1. Subnet](#51-subnet)
- [6. Bastion Hosts](#6-bastion-hosts)
- [7. NAT Instance (outdated)](#7-nat-instance-outdated)
  - [7.1. Comments](#71-comments)
- [8. Network Access Control List (NACL)](#8-network-access-control-list-nacl)
  - [8.1. Default NACL](#81-default-nacl)
  - [8.2. Network ACLs vs Security Groups](#82-network-acls-vs-security-groups)
- [9. Ephemeral Ports](#9-ephemeral-ports)
- [10. VPC Flow Logs](#10-vpc-flow-logs)
  - [10.1. Logs Syntax](#101-logs-syntax)
- [11. VPC Peering](#11-vpc-peering)
- [12. VPC Endpoints (AWS PrivateLink)](#12-vpc-endpoints-aws-privatelink)
  - [12.1. Types of Endpoints](#121-types-of-endpoints)
  - [12.2. Gateway or Interface Endpoint for S3?](#122-gateway-or-interface-endpoint-for-s3)
- [13. Site-to-Site VPN](#13-site-to-site-vpn)
  - [13.1. Site-to-Site VPN Connections](#131-site-to-site-vpn-connections)
- [14. AWS VPN CloudHub](#14-aws-vpn-cloudhub)
- [15. Direct Connect (DX)](#15-direct-connect-dx)
  - [15.1. Direct Connect Gateway](#151-direct-connect-gateway)
    - [15.1.1. Connection Types](#1511-connection-types)
  - [15.2. Encryption](#152-encryption)
  - [15.3. Resiliency](#153-resiliency)
- [16. Site-to-Site VPN connection as a backup](#16-site-to-site-vpn-connection-as-a-backup)
- [17. Transit Gateway](#17-transit-gateway)
  - [17.1. ransit Gateway: Site-to-Site VPN ECMP](#171-ransit-gateway-site-to-site-vpn-ecmp)
- [18. VPC - Traffic Mirroring](#18-vpc---traffic-mirroring)

# 1. Understanding CIDR

- **Classless Inter-Domain Routing** - a method for allocating IP addresses.
- Used in **Security Groups** rules and AWS networking in general.
- **They help to define an IP address range**
  - We've seen WW.XX.YY.ZZ/32 => one IP.
  - We've seen 0.0.0.0/0 => all IPs.
  - But we can define:192.168.0.0/26 =>192.168.0.0 - 192.168.0.63 (64 IP addresses).
- **A CIDR consists of two components**
  - **Base IP**
    - Represents an IP contained in the range (XX.XX.XX.XX).
    - **Example:** 10.0.0.0, 192.168.0.0, ...
  - **Subnet Mask**
    - Defines how many bits can change in the IP.
    - **Example:** /0, /24, /32.
    - **Can take two forms**
      - /8 -> 255.0.0.0
      - /16 -> 255.255.0.0
      - /24 -> 255.255.255.0
      - /32 -> 255.255.255.255

## 1.1. Subnet Mask

- The Subnet Mask basically allows part of the underlying IP to get additional next values from the base IP.
  192.168.0.0 /32 => allows for 1 IP (2^0) 192.168.0.0
  192.168.0.0 /31 => allows for 2 IP (2^1) 192.168.0.0 -> 192.168.0.1
  192.168.0.0 /30 => allows for 4 IP (2^2) 192.168.0.0 -> 192.168.0.3
  192.168.0.0 /29 => allows for 8 IP (2^3) 192.168.0.0 -> 192.168.0.7
  192.168.0.0 /28 => allows for 16 IP (2^4) 192.168.0.0 -> 192.168.0.15
  192.168.0.0 /27 => allows for 32 IP (2^5) 192.168.0.0 -> 192.168.0.31
  192.168.0.0 /26 => allows for 64 IP (2^6) 192.168.0.0 -> 192.168.0.63
  192.168.0.0 /25 => allows for 128 IP (2^7) 192.168.0.0 -> 192.168.0.127
  192.168.0.0 /24 => allows for 256 IP (2^8) 192.168.0.0 -> 192.168.0.255
  ... 192.168.0.0 /16 => allows for 65,536 IP (2^16) 192.168.0.0 -> 192.168.255.255
  ... 192.168.0.0 /0 => allows for All IPs 0.0.0.0 -> 255.255.255.255
- [CIDR calculator](https://www.ipaddressguide.com/cidr)

## 1.2. Public vs. Private IP (IPv4)

- The Internet Assigned Numbers Authority (IANA) established certain blocks of IPv4 addresses for the use of private (LAN) and public (Internet) addresses.
- **Private IP can only allow certain values**
  - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) = in big networks.
  - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12) = AWS default VPC in that range.
  - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) = e.g., home networks.
- All the rest of the IP addresses on the Internet are Public.

## 1.3. Default VPC

- All new AWS accounts have a default VPC.
- New EC2 instances are launched into the default VPC if no subnet is specified.
- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses.
- We also get a public and a private IPv4 DNS names.
  ![Default VPC](/Images/Networking%20&%20Content%20Delivery/AmazonVPCDefaultVPC.png)

# 2. VPC in AWS

- **A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. It is logically isolated from other virtual networks in the AWS Cloud. You can launch your AWS resources, such as Amazon EC2 instances, into your VPC.**
- **VPC = Virtual Private Cloud**
- You can have multiple VPCs in an AWS region (max. 5 per region - soft limit).
- **Max. CIDR per VPC is 5, for each CIDR**
  - Min. size is /28 (16 IP addresses).
  - Max. size is /16 (65536 IP addresses).
- **Because VPC is private, only the Private IPv4 ranges are allowed**
  - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
  - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
  - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
- **Your VPC CIDR should NOT overlap with your other networks (e.g., corporate).**

# 3. Subnet

- **Subnets** allow you to partition your network inside your VPC (Availability Zone resource).
  ![Amazon VPC Subnets](/Images/Networking%20&%20Content%20Delivery/AmazonVPCDiagram.png)
- AWS reserves 5 IP addresses (first 4 & last 1) in each subnet.
- These 5 IP addresses are not available for use and can't be assigned to an EC2 instance.
- **Example:** If CIDR block 10.0.0.0/24, then reserved IP addresses are:
  - 10.0.0.0 - Network Address.
  - 10.0.0.1 - reserved by AWS for the VPC router.
  - 10.0.0.2 - reserved by AWS for mapping to Amazon-provided DNS.
  - 10.0.0.3 - reserved by AWS for future use.
  - 10.0.0.255 - Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved.
- **ATENTION:** If you need 29 IP addresses for EC2 instances:
  - You can't choose a subnet of size /27 (32 IP addresses, 32 - 5 = 27 < 29).
  - You need to choose a subnet of size /26 (64 IP addresses, 64 - 5 = 59 > 29).
- **Example**
  ![Amazon VPC Subnets](/Images/Networking%20&%20Content%20Delivery/AmazonVPCSubnets.png)

# 4. NAT Gateways

- **NAT Gateways allow your instances in your private subnets to access the Internet while remaining private, and are managed by AWS.**
- **NAT Gateways** (AWS-managed) and **NAT Instances** (self-managed) allow your instances in your **Private Subnets** to access the internet while remaining private.
- AWS-managed NAT, higher bandwidth, high availability, no administration.
- Pay per hour for usage and bandwidth.
- NATGW is created in a specific Availability Zone, uses an Elastic IP.
- Can't be used by EC2 instance in the same subnet (only from other subnets).
- Requires an IGW (Private Subnet => NATGW => IGW).
- 5 Gbps of bandwidth with automatic scaling up to 100 Gbps.
- No Security Groups to manage / required.

## 4.1. NAT Gateway with High Availability

- **NAT Gateway is resilient within a single Availability Zone.**
- Must create **multiple NAT** Gateways in **multiple AZs** for fault-tolerance.
- There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT.

## 4.2. NAT Gateway vs. NAT Instance

[Compare NAT gateways and NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)

# 5. Internet Gateway

- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet.
- It scales horizontally and is highly available and redundant.
- Must be created separately from a VPC.
- One VPC can only be attached to one IGW and vice versa.
- **Internet Gateways on their own do not allow Internet access!**
  - Route tables must also be edited!
    ![Internet Gateway](/Images/Networking%20&%20Content%20Delivery/AmazonVPCInternetGateway.png)

## 5.1. Subnet

- Public Subnets have a route to the internet gateway.
- A subnet can only be associated with one route table at a time (1..1).
  - A subnet is **implicitly associated** with the **main route table** if it is not explicitly associated with a particular route table.
  - So, a subnet is always associated with some route table.
  - **But you can associate multiple subnets with the same subnet route table (1..N).**
- A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. The route table in the instance's subnet should have a route defined to the Internet Gateway.

# 6. Bastion Hosts

- We can use a Bastion Host to SSH into our private EC2 instances.
- The bastion is in the public subnet which is then connected to all other private subnets.
- **Bastion Host security group must allow** inbound from the internet on port 22 from restricted CIDR, for example the public CIDR of your corporation.
- Security Group of the EC2 Instances must allow the Security Group of the Bastion Host, or the private IP of the Bastion host.

# 7. NAT Instance (outdated)

- NAT = Network Address Translation.
- Allows EC2 instances in private subnets to connect to the Internet.
- Must be launched in a public subnet.
- **Must disable EC2 setting:** Source / destination Check.
- Must have Elastic IP attached to it.
- Route Tables must be configured to route traffic from private subnets to the NAT Instance.

## 7.1. Comments

- Pre-configured Amazon Linux AMI is available.
  - Reached the end of standard support on December 31, 2020.
- Not highly available / resilient setup out of the box.
  - We need to create an ASG in multi-AZ + resilient user-data script.
- Internet traffic bandwidth depends on EC2 instance type.
- **We must manage Security Groups & rules:**
  - **Inbound**
    - Allow HTTP / HTTPS traffic coming from Private Subnets.
    - Allow SSH from your home network (access is provided through Internet Gateway).
  - **Outbound**
    - Allow HTTP / HTTPS traffic to the Internet.

# 8. Network Access Control List (NACL)

- **A network access control list (NACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.**
- NACL are like a firewall which control traffic from and to **subnets**.
- One NACL per subnet, new subnets are assigned the Default NACL.
- **We define NACL Rules**
  - Rules have a number (1-32766), higher precedence with a lower number.
  - First rule match will drive the decision.
  - **Example:** If we define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP address will be allowed because 100 has a higher precedence over 200.
  - The last rule is an asterisk (\*) and denies a request in case of no rule match.
  - AWS recommends adding rules by increment of 100.
- Newly created NACLs will deny everything.
- NACL are a great way of blocking a specific IP address at the subnet level.

## 8.1. Default NACL

- Accepts everything inbound/outbound with the subnets it's associated with.
- Do NOT modify the Default NACL, instead create custom NACLs.

## 8.2. Network ACLs vs Security Groups

| Security group                                                                                                                                               | Network ACL                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operates at the instance level                                                                                                                               | Operates at the subnet level                                                                                                                                                           |
| Supports allow rules only                                                                                                                                    | Supports allow rules and deny rules                                                                                                                                                    |
| Is stateful: Return traffic is automatically allowed, regardless of any rules                                                                                | Is stateless: Return traffic must be explicitly allowed by rules                                                                                                                       |
| We evaluate all rules before deciding whether to allow traffic                                                                                               | We process rules in order, starting with the lowest numbered rule, when deciding whether to allow traffic                                                                              |
| Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on | Automatically applies to all instances in the subnets that it's associated with (therefore, it provides an additional layer of defense if the security group rules are too permissive) |

- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html

# 9. Ephemeral Ports

- For any two endpoints to establish a connection, they must use ports.
- Clients connect to a defined port, and expect a response on an ephemeral port.
- Different Operating Systems use different port ranges, examples:
  - IANA & MS Windows 10 -> 49152 - 65535.
  - Many Linux Kernels -> 32768 - 60999.

# 10. VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
  - VPC Flow Logs.
  - Subnet Flow Logs.
  - Elastic Network Interface (ENI) Flow Logs.
- Helps to monitor & troubleshoot connectivity issues.
- Flow logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose.
- Captures network information from AWS managed interfaces too: ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway...

## 10.1. Logs Syntax

- `srcaddr` & `dstaddr` - Help identify problematic IP.
- `srcport` & `dstport` - Help identity problematic ports.
- **Action** - Success or failure of the request due to Security Group / NACL.
- Can be used for analytics on usage patterns, or malicious behavior.
- **Query VPC flow logs using Athena on S3 or CloudWatch Logs Insights.**
- **Flow Logs examples:** https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs- records-examples.htm

# 11. VPC Peering

- Connect two VPC, privately using AWS' network.
- Make them behave as if they were in the same network.
- Must not have overlapping CIDR (IP address range).
- VPC Peering connection is **NOT transitive** (must be established for each VPC that need to communicate with one another).
- **We must update route tables in each VPC's subnets to ensure EC2 instances can communicate with each othe.**
- We can create VPC Peering connection between VPCs in different **AWS accounts/regions**.
- We can reference a security group in a peered VPC (works cross accounts - same region).

# 12. VPC Endpoints (AWS PrivateLink)

- Every AWS service is publicly exposed (public URL).
- VPC Endpoints (powered by AWS PrivateLink) allows you to connect to AWS services using a **private network** instead of using the public Internet.
- They're redundant and scale horizontally.
- They remove the need of IGW, NATGW, ... to access AWS Services.
- **In case of issues**
  - Check DNS Setting Resolution in your VPC.
  - Check Route Tables.

![VPC Endpoints Diagram](/Images/Networking%20&%20Content%20Delivery/AmazonVPCEndpoints.png)

![VPC Endpoints Console](/Images/Networking%20&%20Content%20Delivery/AmazonVPCEndpointsConsole.png)

## 12.1. Types of Endpoints

- **Interface Endpoints (powered by PrivateLink)**
  - Provisions an ENI (private IP address) as an entry point (must attach a Security Group).
  - Supports most AWS services.
  - $ per hour + $ per GB of data processed.
- **Gateway Endpoints**
  - Provisions a gateway and must be used as a target in a route table (does not use security groups).
  - Supports both S3 and DynamoDB.
  - Free.

## 12.2. Gateway or Interface Endpoint for S3?

- Gateway is most likely going to be preferred all.
- **Cost:** Free for Gateway, $ for interface endpoint.
- Interface Endpoint is preferred access is required from on-premises (Site to Site VPN or Direct Connect), a different VPC or a different region.

# 13. Site-to-Site VPN

- **Virtual Private Gateway (VGW)**
  - VPN concentrator on the AWS side of the VPN connection.
  - VGW is created and attached to the VPC from which we want to create the Site-to-Site VPN connection.
  - Possibility to customize the ASN (Autonomous System Number).
- **Customer Gateway (CGW)**
  - Software application or physical device on customer side of the VPN connection.
  - https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#DevicesTested

## 13.1. Site-to-Site VPN Connections

- **Customer Gateway Device (On-premises)**
  - **What IP address to use?**
    - Public Internet-routable IP address for your Customer Gateway device.
    - If it's behind a NAT device that's enabled for NAT traversal (NAT-T), use the public IP address of the NAT device.
- **Important step:** Enable **Route Propagation** for the Virtual Private Gateway in the route table that is associated with your subnets.
- If we need to ping your EC2 instances from on-premises, make sure we add the ICMP protocol on the inbound of your security groups.

# 14. AWS VPN CloudHub

- Provide secure communication between multiple sites, if we have multiple VPN connections.
- Low-cost hub-and-spoke model for primary or secondary network connectivity between different locations (VPN only).
- It's a VPN connection so it goes over the public Internet.
- To set it up, connect multiple VPN connections on the same VGW, setup dynamic routing and configure route tables.

# 15. Direct Connect (DX)

- Provides a dedicated **private** connection from a remote network to your VPC.
- Dedicated connection must be setup between your DC and AWS Direct Connect locations
- We need to setup a Virtual Private Gateway on your VPC.
- Access public resources (S3) and private (EC2) on same connection.
- **Use Cases**
  - Increase bandwidth throughput - working with large data sets - lower cost.
  - More consistent network experience - applications using real-time data feeds.
  - Hybrid Environments (on prem + cloud).
- Supports both IPv4 and IPv6

## 15.1. Direct Connect Gateway

- If we want to setup a Direct Connect to one or more VPC in many different regions (same account), we must use a Direct Connect Gateway.

### 15.1.1. Connection Types

- **Dedicated Connections:** 1 Gbps, 10 Gbps and 100 Gbps capacity.
  - Physical ethernet port dedicated to a customer.
  - Request made to AWS first, then completed by AWS Direct Connect Partners.
- **Hosted Connections:** 50 Mbps, 500 Mbps, to 10 Gbps.
  - Connection requests are made via AWS Direct Connect Partners.
  - Capacity can be **added or removed on demand**.
  - 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners.
- Lead times are often longer than 1 month to establish a new connection.

## 15.2. Encryption

- Data in transit is **not encrypted** but is private.
- AWS Direct Connect + VPN provides an IPsec-encrypted private connection.
- Good for an extra level of security, but slightly more complex to put in place.

## 15.3. Resiliency

- High Resiliency for Critical Workloads.
  - One connection at multiple locations.
- Maximum Resiliency for Critical Workloads.
  - Maximum resilience is achieved by separate connections terminating on separate devices in more than one location.

# 16. Site-to-Site VPN connection as a backup

- In case Direct Connect fails, we can set up a backup Direct Connect connection (expensive), or a Site-to-Site VPN connection.

# 17. Transit Gateway

- **For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection.**
- Regional resource, can work cross-region.
- Share cross-account using Resource Access Manager (RAM).
- We can peer Transit Gateways across regions.
- Route Tables: limit which VPC can talk with other VPC.
- Works with Direct Connect Gateway, VPN connections.
- Supports **IP Multicast** (not supported by any other AWS service).

## 17.1. ransit Gateway: Site-to-Site VPN ECMP

- ECMP = Equal-cost multi-path routing.
- Routing strategy to allow to forward a packet over multiple best path.
- **Use case:** Create multiple Site-to-Site VPN connections to increase the bandwidth of your connection to AWS.

# 18. VPC - Traffic Mirroring

- Allows you to capture and inspect network traffic in your VPC.
- Route the traffic to security appliances that you manage.
- **Capture the traffic**
  - **From (Source)** - ENIs.
  - **To (Targets)** - an ENI or a Network Load Balancer.
- Capture all packets or capture the packets of your interest (optionally, truncate packets).
- Source and Target can be in the same VPC or different VPCs (VPC Peering).
- **Use cases:** Content inspection, threat monitoring, troubleshooting, ...
