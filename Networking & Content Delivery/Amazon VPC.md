# Amazon VPC - Virtual Private Cloud<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. VPC and Subnets Primer](#1-vpc-and-subnets-primer)
- [2. NAT Gateways](#2-nat-gateways)
  - [2.1. NAT Gateway with High Availability](#21-nat-gateway-with-high-availability)
  - [2.2. NAT Gateway vs. NAT Instance](#22-nat-gateway-vs-nat-instance)
- [3. Internet Gateway](#3-internet-gateway)
  - [3.1. Subnet](#31-subnet)
- [4. Network ACL and Security Groups](#4-network-acl-and-security-groups)
  - [4.1. Network ACLs vs Security Groups](#41-network-acls-vs-security-groups)
- [5. VPC Flow Logs](#5-vpc-flow-logs)
- [6. VPC Peering](#6-vpc-peering)
- [7. VPC Endpoints](#7-vpc-endpoints)
- [8. Site to Site VPN \& Direct Connect](#8-site-to-site-vpn--direct-connect)
- [9. Site-to-Site VPN](#9-site-to-site-vpn)
- [10. Transit Gateway](#10-transit-gateway)
- [11. VPC Closing Comments](#11-vpc-closing-comments)

# 1. VPC and Subnets Primer

- **A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. It is logically isolated from other virtual networks in the AWS Cloud. You can launch your AWS resources, such as Amazon EC2 instances, into your VPC.**
- **VPC - Virtual Private Cloud:** private network to deploy your resources (regional resource).
- **Subnets** allow you to partition your network inside your VPC (Availability Zone resource).
- A **public subnet** is a subnet that is accessible from the internet.
- A **private subnet** is a subnet that is not accessible from the internet.
- To define access to the internet and between subnets, we use **Route Tables**.

![Amazon VPC ](Images/AwsVPCDiagram.png)

# 2. NAT Gateways

- **NAT Gateways allow your instances in your private subnets to access the Internet while remaining private, and are managed by AWS.**
- **NAT Gateways** (AWS-managed) and **NAT Instances** (self-managed) allow your instances in your **Private Subnets** to access the internet while remaining private.
- AWS-managed NAT, higher bandwidth, high availability, no administration.
- Pay per hour for usage and bandwidth.
- NATGW is created in a specific Availability Zone, uses an Elastic IP.
- Can't be used by EC2 instance in the same subnet (only from other subnets).
- Requires an IGW (Private Subnet => NATGW => IGW).
- 5 Gbps of bandwidth with automatic scaling up to 100 Gbps.
- No Security Groups to manage / required.

## 2.1. NAT Gateway with High Availability

- **NAT Gateway is resilient within a single Availability Zone.**
- Must create **multiple NAT** Gateways in **multiple AZs** for fault-tolerance.
- There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT.

## 2.2. NAT Gateway vs. NAT Instance

[Compare NAT gateways and NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)

# 3. Internet Gateway

- **Internet Gateways** helps our VPC instances connect with the internet.

## 3.1. Subnet

- Public Subnets have a route to the internet gateway.
- A subnet can only be associated with one route table at a time (1..1).
  - A subnet is **implicitly associated** with the **main route table** if it is not explicitly associated with a particular route table.
  - So, a subnet is always associated with some route table.
  - **But you can associate multiple subnets with the same subnet route table (1..N).**
- A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. The route table in the instance's subnet should have a route defined to the Internet Gateway.

# 4. Network ACL and Security Groups

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

## 4.1. Network ACLs vs Security Groups

| Security group                                                                                                                                               | Network ACL                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operates at the instance level                                                                                                                               | Operates at the subnet level                                                                                                                                                           |
| Supports allow rules only                                                                                                                                    | Supports allow rules and deny rules                                                                                                                                                    |
| Is stateful: Return traffic is automatically allowed, regardless of any rules                                                                                | Is stateless: Return traffic must be explicitly allowed by rules                                                                                                                       |
| We evaluate all rules before deciding whether to allow traffic                                                                                               | We process rules in order, starting with the lowest numbered rule, when deciding whether to allow traffic                                                                              |
| Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on | Automatically applies to all instances in the subnets that it's associated with (therefore, it provides an additional layer of defense if the security group rules are too permissive) |

- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html

# 5. VPC Flow Logs

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

# 6. VPC Peering

- Connect two VPC, privately using AWS' network.
- Make them behave as if they were in the same network.
- Must not have overlapping CIDR (IP address range).
- VPC Peering connection is not transitive (must be established for each VPC that need to communicate with one another).

# 7. VPC Endpoints

- Endpoints allow you to connect to AWS Services **using a private network** instead of the public www network.
- This gives you enhanced security and lower latency to access AWS services.
- **VPC Endpoint Gateway**: S3 and DynamoDB.
- **VPC Endpoint Interface**: the rest.

![VPC Endpoints diagram](Images/VPCEndpoints.png)

# 8. Site to Site VPN & Direct Connect

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

# 9. Site-to-Site VPN

- On-premises: must use a **Customer Gateway** (CGW)
- AWS: must use a **Virtual Private Gateway** (VGW)

# 10. Transit Gateway

- **Transit Gateway connects thousands of VPC and on-premises networks together in a single gateway.**
- For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection.
- One single Gateway to provide this functionality.
- Works with Direct Connect Gateway, VPN connections.

# 11. VPC Closing Comments

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
