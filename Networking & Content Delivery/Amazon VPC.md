# Amazon VPC - Virtual Private Cloud <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Understanding CIDR](#1-understanding-cidr)
  - [1.1. Subnet Mask](#11-subnet-mask)
  - [1.2. Public vs. Private IP (IPv4)](#12-public-vs-private-ip-ipv4)
  - [1.3. Default VPC](#13-default-vpc)
- [2. VPC in AWS](#2-vpc-in-aws)
- [3. Subnet](#3-subnet)
- [4. NAT Instance (outdated)](#4-nat-instance-outdated)
  - [4.1. Comments](#41-comments)
- [5. NAT Gateways](#5-nat-gateways)
  - [5.1. NAT Gateway with High Availability](#51-nat-gateway-with-high-availability)
  - [5.2. NAT Gateway vs. NAT Instance](#52-nat-gateway-vs-nat-instance)
- [6. Internet Gateway](#6-internet-gateway)
  - [6.1. Subnet](#61-subnet)
- [7. Bastion Hosts](#7-bastion-hosts)
- [8. Network Access Control List (NACL)](#8-network-access-control-list-nacl)
  - [8.1. Default NACL](#81-default-nacl)
  - [8.2. Network ACLs vs Security Groups](#82-network-acls-vs-security-groups)
    - [8.2.1. Security Groups (Stateful)](#821-security-groups-stateful)
    - [8.2.2. Network ACLs (Stateless)](#822-network-acls-stateless)
    - [8.2.3. Summary](#823-summary)
- [9. Ephemeral Ports](#9-ephemeral-ports)
- [10. VPC Flow Logs](#10-vpc-flow-logs)
  - [10.1. Logs Syntax](#101-logs-syntax)
- [11. VPC Peering](#11-vpc-peering)
- [12. VPC Endpoints (AWS PrivateLink)](#12-vpc-endpoints-aws-privatelink)
  - [12.1. Types of Endpoints](#121-types-of-endpoints)
  - [12.2. Gateway or Interface Endpoint for S3?](#122-gateway-or-interface-endpoint-for-s3)
  - [12.3. Use cases](#123-use-cases)
- [13. Site-to-Site VPN (aka VPN Connection)](#13-site-to-site-vpn-aka-vpn-connection)
  - [13.1. Connections](#131-connections)
  - [13.2. IPsec VPN connection](#132-ipsec-vpn-connection)
- [14. AWS VPN CloudHub](#14-aws-vpn-cloudhub)
- [15. Site-to-Site VPN connection as a backup](#15-site-to-site-vpn-connection-as-a-backup)
- [16. Transit Gateway](#16-transit-gateway)
  - [16.1. With AWS Transit Gateway](#161-with-aws-transit-gateway)
  - [16.2. Without AWS Transit Gateway](#162-without-aws-transit-gateway)
  - [16.3. Transit Gateway: Site-to-Site VPN ECMP](#163-transit-gateway-site-to-site-vpn-ecmp)
  - [16.4. Share Direct Connect between multiple accounts](#164-share-direct-connect-between-multiple-accounts)
- [17. VPC Sharing](#17-vpc-sharing)
- [18. VPC - Traffic Mirroring](#18-vpc---traffic-mirroring)
- [19. What is IPv6?](#19-what-is-ipv6)
  - [19.1. IPv6 in VPC](#191-ipv6-in-vpc)
  - [19.2. IPv6 Troubleshooting](#192-ipv6-troubleshooting)
  - [19.3. Egress-only Internet Gateway](#193-egress-only-internet-gateway)
- [20. Network Protection on AWS](#20-network-protection-on-aws)
  - [20.1. AWS Network Firewall](#201-aws-network-firewall)
    - [20.1.1. Fine Grained Controls](#2011-fine-grained-controls)
- [21. Transit VPC](#21-transit-vpc)
- [22. VPC Section Summary](#22-vpc-section-summary)

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
- **AWS reserves 5 IP addresses (first 4 & last 1) in each subnet.**
- These 5 IP addresses are not available for use and can't be assigned to an EC2 instance.
- **Example**
  - If CIDR block 10.0.0.0/24, then reserved IP addresses are:
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

# 4. NAT Instance (outdated)

- NAT = Network Address Translation.
- Allows EC2 instances in private subnets to connect to the Internet.
- Must be launched in a public subnet.
- **Must disable EC2 setting:** Source / destination Check.
- Must have Elastic IP attached to it.
- Route Tables must be configured to route traffic from private subnets to the NAT Instance.
  ![NAT Instance](/Images/Networking%20&%20Content%20Delivery/NATInstance.png)
- **IMPORTANT!**
  - NAT instance can be used as a bastion server.
  - NAT instance supports port forwarding.

## 4.1. Comments

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

# 5. NAT Gateways

- **NAT Gateways** (AWS-managed) and **NAT Instances** (self-managed) allow your instances in your **Private Subnets** to access the internet while remaining private.
- AWS-managed NAT, higher bandwidth, high availability, no administration.
- Pay per hour for usage and bandwidth.
- NATGW is created in a specific Availability Zone, uses an Elastic IP.
- Can't be used by EC2 instance in the same subnet (only from other subnets).
- Requires an IGW (Private Subnet => NATGW => IGW).
- 5 Gbps of bandwidth with automatic scaling up to 100 Gbps.
- No Security Groups to manage / required.

## 5.1. NAT Gateway with High Availability

- **NAT Gateway is resilient within a single Availability Zone.**
- Must create **multiple NAT** Gateways in **multiple AZs** for fault-tolerance.
- There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT.

## 5.2. NAT Gateway vs. NAT Instance

[Compare NAT gateways and NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)

# 6. Internet Gateway

- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet.
- It scales horizontally and is highly available and redundant.
- Must be created separately from a VPC.
- One VPC can only be attached to one IGW and vice versa.
- **Internet Gateways on their own do not allow Internet access!**
  - Route tables must also be edited!
    ![Internet Gateway](/Images/Networking%20&%20Content%20Delivery/AmazonVPCInternetGateway.png)

## 6.1. Subnet

- Public Subnets have a route to the internet gateway.
- A subnet can only be associated with one route table at a time (1..1).
  - A subnet is **implicitly associated** with the **main route table** if it is not explicitly associated with a particular route table.
  - So, a subnet is always associated with some route table.
  - **But you can associate multiple subnets with the same subnet route table (1..N).**
- A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. The route table in the instance's subnet should have a route defined to the Internet Gateway.

# 7. Bastion Hosts

- We can use a Bastion Host to SSH into our private EC2 instances.
- The bastion is in the public subnet which is then connected to all other private subnets.
- **Bastion Host security group must allow** inbound from the internet on port 22 from restricted CIDR, for example the public CIDR of your corporation.
- Security Group of the EC2 Instances must allow the Security Group of the Bastion Host, or the private IP of the Bastion host.

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

### 8.2.1. Security Groups (Stateful)

- **Stateful** means AWS **remembers the connection**.
- If you allow **inbound traffic**, the **response (outbound traffic)** is automatically allowed — **you don't need to explicitly allow it**.
- **Example:** If port 22 (SSH) is allowed **inbound**, the return SSH traffic **outbound is automatically allowed**.

### 8.2.2. Network ACLs (Stateless)

- **Stateless** means AWS does **not remember the connection**.
- You must explicitly allow **both inbound and outbound traffic** for the connection to work.
- **Example:** If you allow inbound on port 80 (HTTP), you also need to allow **outbound traffic** for responses on the appropriate ephemeral port range (usually 1024-65535).

### 8.2.3. Summary

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
  ![Amazon VPC Peering](/Images/Networking%20&%20Content%20Delivery/AmazonVPCPeering.png)

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
  - **Supports both S3 and DynamoDB.**
  - Free.

## 12.2. Gateway or Interface Endpoint for S3?

- Gateway is most likely going to be preferred all.
- **Cost:** Free for Gateway, $ for interface endpoint.
- Interface Endpoint is preferred access is required from on-premises (Site to Site VPN or Direct Connect), a different VPC or a different region.

## 12.3. Use cases

- In the following diagram, the VPC on the left has several [Amazon EC2](/Compute/Amazon%20EC2.md) instances in a private subnet and five **VPC endpoints** - three **interface VPC endpoints**, a **resource VPC endpoint** and a **service-network VPC endpoint**.
  - The first interface VPC endpoint connects to an AWS service.
  - The second interface VPC endpoint connects to a service hosted by another AWS account (a VPC endpoint service).
  - The third interface VPC endpoint connects to an AWS Marketplace partner service.
  - The resource VPC endpoint connects to a database.
  - The service network VPC endpoint connects to a service network.
    ![Amazon VPC Link Use Cases](/Images/Networking%20&%20Content%20Delivery/AmazonVPCLinkUseCases.png)

# 13. Site-to-Site VPN (aka VPN Connection)

- **Virtual Private Gateway (VGW)**
  - VPN concentrator on the AWS side of the VPN connection.
  - VGW is created and attached to the VPC from which we want to create the Site-to-Site VPN connection.
  - Possibility to customize the ASN (Autonomous System Number).
- **Customer Gateway (CGW)**
  - Software application or physical device on customer side of the VPN connection.
  - https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#DevicesTested

## 13.1. Connections

- **Customer Gateway Device (On-premises)**
  - **What IP address to use?**
    - Public Internet-routable IP address for your Customer Gateway device.
    - If it's behind a NAT device that's enabled for NAT traversal (NAT-T), use the public IP address of the NAT device.
- **Important step:** Enable **Route Propagation** for the Virtual Private Gateway in the route table that is associated with your subnets.
- If we need to ping your EC2 instances from on-premises, make sure we add the ICMP protocol on the inbound of your security groups.
  ![AWS Site-To-Site VPN Connections](/Images/Networking%20&%20Content%20Delivery/AWSSiteToSiteVPNConnections.png)

## 13.2. IPsec VPN connection

- Amazon VPC provides the facility to create an IPsec VPN connection (also known as AWS site-to-site VPN) between remote customer networks and their Amazon VPC over the internet.
- The following are the key concepts for a site-to-site VPN:
  - **Virtual private gateway:** A **virtual private gateway (VGW)**, also known as a **VPN Gateway** is the endpoint on the AWS VPC side of your VPN connection.
  - **Site-to-Site VPN (aka VPN Connection):** A secure connection between your on-premises equipment and your VPCs.
  - **VPN tunnel:** An encrypted link where data can pass from the customer network to or from AWS.
  - **Customer Gateway:** An AWS resource that provides information to AWS about your Customer Gateway device.
  - **Customer Gateway device:** A physical device or software application on the customer side of the Site-to-Site VPN connection.
    ![IPsec VPN connection](/Images/Networking%20&%20Content%20Delivery/AWSSiteToSiteVPNIpSec.png)

# 14. AWS VPN CloudHub

- Provide secure communication between multiple sites, if we have multiple VPN connections.
- Low-cost hub-and-spoke model for primary or secondary network connectivity between different locations (VPN only).
- It's a VPN connection so it goes over the public Internet.
- To set it up, connect multiple VPN connections on the same VGW, setup dynamic routing and configure route tables.
  ![AWS VPN CloudHub](/Images/Networking%20&%20Content%20Delivery/AmazonVPCVPNCloudHub.png)

# 15. Site-to-Site VPN connection as a backup

- In case Direct Connect fails, we can set up a backup Direct Connect connection (expensive), or a Site-to-Site VPN connection.

# 16. Transit Gateway

- AWS Transit Gateway is a service that enables customers to connect their Amazon Virtual Private Clouds (VPCs) and their on-premises networks to a single gateway.
  - With AWS Transit Gateway, we only have to create and manage a single connection from the central gateway into each Amazon VPC, on-premises data center, or remote office across your network.
  - Transit Gateway acts as a hub that controls how traffic is routed among all the connected networks which act like spokes.
  - So, this is a perfect use-case for the Transit Gateway.
- **For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection.**
- Regional resource, can work cross-region.
- Share cross-account using [Resource Access Manager (RAM)](/Security,%20Identity,%20&%20Compliance/AWS%20Resource%20Access%20Manager.md).
- We can peer Transit Gateways across regions.
- **Route Tables:** Limit which VPC can talk with other VPC.
- Works with Direct Connect Gateway, VPN connections.
- Supports **IP Multicast** (not supported by any other AWS service).

## 16.1. With AWS Transit Gateway

![With AWS Transit Gateway](/Images/Networking%20&%20Content%2r0Delivery/AmazonVPCTransitGateway.png)

## 16.2. Without AWS Transit Gateway

![Without AWS Transit Gateway](/Images/Networking%20&%20Content%20Delivery/AmazonVPCWithoutTransitGateway.png)

## 16.3. Transit Gateway: Site-to-Site VPN ECMP

- ECMP = Equal-cost multi-path routing.
- Routing strategy to allow to forward a packet over multiple best path.
- **Use case:** Create multiple Site-to-Site VPN connections to increase the bandwidth of your connection to AWS.

## 16.4. Share Direct Connect between multiple accounts

![Amazon VPC Transit Gateway Share Direct Connect](/Images/Networking%20&%20Content%20Delivery/AmazonVPCTransitGatewayShareDirectConnect.png)

# 17. VPC Sharing

- **VPC Sharing**, part of AWS Resource Access Manager (RAM), allows multiple AWS accounts to deploy resources (e.g., EC2, RDS, Redshift, Lambda) into a **shared and _centrally-managed_ VPC** controlled by a central **owner account**.
- The **owner** shares subnets with **participant accounts** within the same AWS Organization.
- **Participants** can manage their own resources in shared subnets but **cannot access** resources owned by others or the VPC owner.
- Ideal for applications needing **high interconnectivity** within **trusted boundaries**.
- **Benefits**
  - Simplifies network architecture.
  - Reduces VPC sprawl.
  - Maintains **separate billing and access control** per account.

# 18. VPC - Traffic Mirroring

- Allows you to capture and inspect network traffic in your VPC.
- Route the traffic to security appliances that we manage.
- **Capture the traffic**
  - **From (Source)** - ENIs.
  - **To (Targets)** - an ENI or a Network Load Balancer.
- Capture all packets or capture the packets of your interest (optionally, truncate packets).
- Source and Target can be in the same VPC or different VPCs (VPC Peering).
- **Use cases:** Content inspection, threat monitoring, troubleshooting, ...

# 19. What is IPv6?

- IPv4 designed to provide 4.3 Billion addresses (they'll be exhausted soon).
- IPv6 is the successor of IPv4.
- IPv6 is designed to provide 3.4 x 10^38 unique IP addresses.
- **Every IPv6 address in AWS is public** and Internet-routable (no private range).
- Format -> x.x.x.x.x.x.x.x (x is hexadecimal, range can be from 0000 to ffff).
- **Examples**
  - 2001:db8:3333:4444:5555:6666:7777:8888
  - 2001:db8:3333:4444:cccc:dddd:eeee:ffff
  - :: -> all 8 segments are zero
  - 2001:db8:: -> the last 6 segments are zero
  - ::1234:5678 -> the first 6 segments are zero
  - 2001:db8::1234:5678 -> the middle 4 segments are zero

## 19.1. IPv6 in VPC

- **IPv4 cannot be disabled for your VPC and subnets**
- We can enable IPv6 (they're public IP addresses) to operate in dual-stack mode.
- Your EC2 instances will get at least a private internal IPv4 and a public IPv6.
- They can communicate using either IPv4 or IPv6 to the internet through an Internet Gateway EC2 Instance.

## 19.2. IPv6 Troubleshooting

- So, if we cannot launch an EC2 instance in your subnet.
  - It's not because it cannot acquire an IPv6 (the space is very large).
  - It's because there are no available IPv4 in your subnet.
- **Solution:** Create a new IPv4 CIDR in your subnet.

## 19.3. Egress-only Internet Gateway

- **Used for IPv6 only.**
- (similar to a NAT Gateway but for IPv6).
- Allows instances in your VPC outbound connections over IPv6 while preventing the internet to initiate an IPv6 connection to your instances.
- **You must update the Route Tables.**

# 20. Network Protection on AWS

- **To protect network on AWS, we've seen**
  - Network Access Control Lists (NACLs).
  - Amazon VPC security groups.
  - AWS WAF (protect against malicious requests).
  - AWS Shield & AWS Shield Advanced.
  - AWS Firewall Manager (to manage them across accounts).
- But what if we want to protect in a sophisticated way our entire VPC?

## 20.1. AWS Network Firewall

- Protect your entire Amazon VPC.
- From Layer 3 to Layer 7 protection.
- Any direction, you can inspect.
  - VPC to VPC traffic.
  - Outbound to internet.
  - Inbound from internet.
  - To / from Direct Connect & Site-to-Site VPN.
- Internally, the AWS Network Firewall uses the AWS Gateway Load Balancer.
- Rules can be centrally managed cross- account by AWS Firewall Manager to apply to many VPCs.

### 20.1.1. Fine Grained Controls

- Supports 1000s of rules.
  - IP & port - example: 10,000s of IPs filtering.
  - Protocol - example: block the SMB protocol for outbound communications.
  - Stateful domain list rule groups: only allow outbound traffic to \*.mycorp.com or third-party software repo.
  - General pattern matching using regex.
- **Traffic filtering: Allow, drop, or alert for the traffic that matches the rules.**
- **Active flow inspection** to protect against network threats with intrusion-prevention capabilities (like Gateway Load Balancer, but all managed by AWS).
- Send logs of rule matches to Amazon S3, CloudWatch Logs, Kinesis Data Firehose.

# 21. Transit VPC

- AWS Transit VPC is a networking architecture that allows you to centralize connectivity between multiple Amazon VPCs (in the same or different AWS regions), on-premises networks, or remote offices, using a hub-and-spoke model.
- Think of it like a central router (the Transit VPC) that connects to all your other networks (the spokes) so they can communicate through it.
- Transit VPC uses **customer-managed** Amazon Elastic Compute Cloud (Amazon EC2) VPN instances in a dedicated transit VPC with an Internet gateway.
- This design requires the customer to deploy, configure, and manage EC2-based VPN appliances, which will result in additional EC2, and potentially third-party product and licensing charges.
  - Note that this design will generate additional data transfer charges for traffic traversing the transit VPC: data is charged when it is sent from a spoke VPC to the transit VPC, and again from the transit VPC to the on-premises network or a different AWS Region.
- **IMPORTANT! AWS has introduced AWS Transit Gateway, which is the modern replacement for Transit VPC.**

# 22. VPC Section Summary

- **CIDR:** IP Range.
- **VPC:** Virtual Private Cloud => we define a list of IPv4 & IPv6 CIDR.
- **Subnets:** Tied to an AZ, we define a CIDR.
- **Internet Gateway:** At the VPC level, provide IPv4 & IPv6 Internet Access.
- **Route Tables:** Must be edited to add routes from subnets to the IGW, VPC Peering Connections, VPC Endpoints, ...
- **Bastion Host:** Public EC2 instance to SSH into, that has SSH connectivity to EC2 instances in private subnets.
- **NAT Instances (outdated):** Gives Internet access to EC2 instances in private subnets.
  - Old, must be setup in a public subnet, disable Source / Destination check flag.
- **NAT Gateway:** Managed by AWS, provides scalable Internet access to private EC2 instances, when the target is an IPv4 address.
- **NACL:** Stateless, subnet rules for inbound and outbound, don't forget Ephemeral Ports.
- **Security Groups:** Stateful, operate at the EC2 instance level.
- **VPC Peering:** Connect two VPCs with non overlapping CIDR, non-transitive.
- **VPC Endpoints:** Provide private access to AWS Services (S3, DynamoDB, CloudFormation, SSM) within a VPC.
- **VPC Flow Logs:** Can be setup at the VPC / Subnet / ENI Level, for ACCEPT and REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Logs Insights.
- **Site-to-Site VPN:** Setup a Customer Gateway on DC (Datacenter), a Virtual Private Gateway on VPC, and site-to-site VPN over public Internet.
- **AWS VPN CloudHub:** Hub-and-spoke VPN model to connect your sites.
- **Direct Connect:** Setup a Virtual Private Gateway on VPC, and establish a direct private connection to an AWS Direct Connect Location.
- **Direct Connect Gateway:** Setup a Direct Connect to many VPCs in different AWS regions.
- **AWS PrivateLink / VPC Endpoint Services**
  - Connect services privately from your service VPC to customers VPC.
  - Doesn't need VPC Peering, public Internet, NAT Gateway, Route Tables.
  - Must be used with Network Load Balancer & ENI.
- **ClassicLink:** Connect EC2-Classic EC2 instances privately to your VPC.
- **Transit Gateway:** Transitive peering connections for VPC, VPN & DX.
- **Traffic Mirroring:** Copy network traffic from ENIs for further analysis.
- **Egress-only Internet Gateway:** Like a NAT Gateway, but for IPv6 targets.
