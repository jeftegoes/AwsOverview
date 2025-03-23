# Amazon VPC - Virtual Private Cloud <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Understanding CIDR](#1-understanding-cidr)
  - [1.1. Subnet Mask](#11-subnet-mask)
  - [1.2. Public vs. Private IP (IPv4)](#12-public-vs-private-ip-ipv4)
  - [1.3. Default VPC Walkthrough](#13-default-vpc-walkthrough)
- [2. VPC in AWS](#2-vpc-in-aws)
- [3. VPC and Subnets Primer](#3-vpc-and-subnets-primer)
- [4. NAT Gateways](#4-nat-gateways)
  - [4.1. NAT Gateway with High Availability](#41-nat-gateway-with-high-availability)
  - [4.2. NAT Gateway vs. NAT Instance](#42-nat-gateway-vs-nat-instance)
- [5. Internet Gateway](#5-internet-gateway)
  - [5.1. Subnet](#51-subnet)
- [6. Network ACL and Security Groups](#6-network-acl-and-security-groups)
  - [6.1. Network ACLs vs Security Groups](#61-network-acls-vs-security-groups)
- [7. VPC Flow Logs](#7-vpc-flow-logs)
- [8. VPC Peering](#8-vpc-peering)
- [9. VPC Endpoints](#9-vpc-endpoints)
- [10. Site to Site VPN \& Direct Connect](#10-site-to-site-vpn--direct-connect)
- [11. Site-to-Site VPN](#11-site-to-site-vpn)
- [12. Transit Gateway](#12-transit-gateway)
- [13. VPC Closing Comments](#13-vpc-closing-comments)

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

## 1.3. Default VPC Walkthrough

- All new AWS accounts have a default VPC.
- New EC2 instances are launched into the default VPC if no subnet is specified.
- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses.
- We also get a public and a private IPv4 DNS names.

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

# 3. VPC and Subnets Primer

- **VPC - Virtual Private Cloud:** Private network to deploy your resources (regional resource).
- **Subnets** allow you to partition your network inside your VPC (Availability Zone resource).
- A **public subnet** is a subnet that is accessible from the internet.
- A **private subnet** is a subnet that is not accessible from the internet.
- To define access to the internet and between subnets, we use **Route Tables**.
  ![Amazon VPC ](/Images/Networking%20&%20Content%20Delivery/AwsVPCDiagram.png)

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

- **Internet Gateways** helps our VPC instances connect with the internet.

## 5.1. Subnet

- Public Subnets have a route to the internet gateway.
- A subnet can only be associated with one route table at a time (1..1).
  - A subnet is **implicitly associated** with the **main route table** if it is not explicitly associated with a particular route table.
  - So, a subnet is always associated with some route table.
  - **But you can associate multiple subnets with the same subnet route table (1..N).**
- A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. The route table in the instance's subnet should have a route defined to the Internet Gateway.

# 6. Network ACL and Security Groups

- **A network access control list (NACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.**
- **They have both ALLOW and DENY rules.**
- **NACL (Network ACL):**
  - A firewall which controls traffic from and to subnet.
  - Can have ALLOW and DENY rules.
  - Are attached at the **Subnet** level.
  - Rules only include IP addresses.
- **Security Groups:**
  - A firewall that controls traffic to and from **an ENI / an EC2 Instance**.
  - Can have only ALLOW rules.
  - Rules include IP addresses and other security groups.

## 6.1. Network ACLs vs Security Groups

| Security group                                                                                                                                               | Network ACL                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operates at the instance level                                                                                                                               | Operates at the subnet level                                                                                                                                                           |
| Supports allow rules only                                                                                                                                    | Supports allow rules and deny rules                                                                                                                                                    |
| Is stateful: Return traffic is automatically allowed, regardless of any rules                                                                                | Is stateless: Return traffic must be explicitly allowed by rules                                                                                                                       |
| We evaluate all rules before deciding whether to allow traffic                                                                                               | We process rules in order, starting with the lowest numbered rule, when deciding whether to allow traffic                                                                              |
| Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on | Automatically applies to all instances in the subnets that it's associated with (therefore, it provides an additional layer of defense if the security group rules are too permissive) |

- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html

# 7. VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
  - **VPC Flow** Logs.
  - **Subnet** Flow Logs.
  - **Elastic Network Interface** Flow Logs.
- Helps to monitor & troubleshoot connectivity issues. Example:
  - Subnets to internet.
  - Subnets to subnets.
  - Internet to subnets.
- Captures network information from AWS managed interfaces too: Elastic Load Balancers, ElastiCache, RDS, Aurora, etc...
- VPC Flow logs data can go to S3 / CloudWatch Logs.

# 8. VPC Peering

- Connect two VPC, privately using AWS' network.
- Make them behave as if they were in the same network.
- Must not have overlapping CIDR (IP address range).
- VPC Peering connection is not transitive (must be established for each VPC that need to communicate with one another).

# 9. VPC Endpoints

- Endpoints allow you to connect to AWS Services **using a private network** instead of the public www network.
- This gives you enhanced security and lower latency to access AWS services.
- **VPC Endpoint Gateway**: S3 and DynamoDB.
- **VPC Endpoint Interface**: the rest.

![VPC Endpoints Diagram](/Images/Networking%20&%20Content%20Delivery/VPCEndpoints.png)

![VPC Endpoints Console](/Images/Networking%20&%20Content%20Delivery/VPCEndpointsConsole.png)

# 10. Site to Site VPN & Direct Connect

- **Site to Site VPN:**
  - Connect an on-premises VPN to AWS.
  - The connection is automatically encrypted.
  - Goes over the public internet.
- **AWS Direct Connect is a cloud service solution that makes it easy to establish a dedicated private network connection from your premises to AWS.**
  - **Direct Connect (DX):**
    - Establish a physical connection between on-premises and AWS.
    - The connection is private, secure and fast.
    - Goes over a private network.
    - Takes at least a month to establish.
- Note: **Site-to-site VPN** and **Direct Connect** cannot access **VPC endpoints**.

# 11. Site-to-Site VPN

- On-premises: must use a **Customer Gateway** (CGW)
- AWS: must use a **Virtual Private Gateway** (VGW)

# 12. Transit Gateway

- **Transit Gateway connects thousands of VPC and on-premises networks together in a single gateway.**
- For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection.
- One single Gateway to provide this functionality.
- Works with Direct Connect Gateway, VPN connections.

# 13. VPC Closing Comments

- **VPC:** Virtual Private Cloud.
- **Subnets:** Tied to an AZ, network partition of the VPC.
- **Internet Gateway:** At the VPC level, provide Internet Access.
- **NAT Gateway / Instances:** Give internet access to private subnets.
- **NACL:** Stateless, subnet rules for inbound and outbound.
- **Security Groups:** Stateful, operate at the EC2 instance level or ENI.
- **VPC Peering:** Connect two VPC with non overlapping IP ranges, nontransitive.
- **VPC Endpoints:** Provide private access to AWS Services within VPC.
- **VPC Flow Logs:** Network traffic logs.
- **Site to Site VPN:** VPN over public internet between on-premises DC and AWS.
- **Direct Connect:** Direct private connection to AWS.
- **Transit Gateway:** Connect thousands of VPC and on-premises networks together.
