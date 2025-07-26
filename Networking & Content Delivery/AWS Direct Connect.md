# AWS Direct Connect <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Direct Connect (DX)](#1-direct-connect-dx)
- [2. Direct Connect Gateway](#2-direct-connect-gateway)
  - [2.1. Connection Types](#21-connection-types)
- [3. Encryption](#3-encryption)
- [4. Resiliency](#4-resiliency)
- [5. Virtual Interfaces (VIF)](#5-virtual-interfaces-vif)
- [6. Data Transfer Out (DTO) over AWS Direct Connect](#6-data-transfer-out-dto-over-aws-direct-connect)

# 1. Direct Connect (DX)

- Provides a dedicated **private** connection from a remote network to your VPC.
- Dedicated connection must be setup between your DC (Datacenter) and AWS Direct Connect locations
- We need to setup a Virtual Private Gateway on your VPC.
- Access public resources (S3) and private (EC2) on same connection.
- **Use Cases**
  - Increase bandwidth throughput - working with large data sets - lower cost.
  - More consistent network experience - applications using real-time data feeds.
  - Hybrid Environments (on prem + cloud).
- Supports both IPv4 and IPv6
  ![AWS Direct Connect Diagram](/Images/Networking%20&%20Content%20Delivery/AWSDirectConnectDiagram.png)

# 2. Direct Connect Gateway

- If we want to setup a Direct Connect to one or more VPC in many different regions (same account), we must use a Direct Connect Gateway.
  ![Direct Connect Gateway](/Images/Networking%20&%20Content%20Delivery/AWSDirectConnectGateway.png)

## 2.1. Connection Types

- **Dedicated Connections:** 1 Gbps, 10 Gbps and 100 Gbps capacity.
  - Physical ethernet port dedicated to a customer.
  - Request made to AWS first, then completed by AWS Direct Connect Partners.
- **Hosted Connections:** 50 Mbps, 500 Mbps, to 10 Gbps.
  - Connection requests are made via AWS Direct Connect Partners.
  - Capacity can be **added or removed on demand**.
  - 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners.
- Lead times are often longer than 1 month to establish a new connection.

# 3. Encryption

- Data in transit is **not encrypted** but is private.
- AWS Direct Connect plus (+) virtual private network (VPN) provides an **IPsec-encrypted** private connection.
- Good for an extra level of security, but slightly more complex to put in place.

# 4. Resiliency

- High Resiliency for Critical Workloads.
  - One connection at multiple locations.
- Maximum Resiliency for Critical Workloads.
  - Maximum resilience is achieved by separate connections terminating on separate devices in more than one location.

# 5. Virtual Interfaces (VIF)

- We must create one of the following virtual interfaces (VIFs) to begin using your AWS Direct Connect connection.
  - **Private virtual interface:** A private virtual interface should be used to access an Amazon VPC using private IP addresses.
  - **Public virtual interface:** A public virtual interface can access all AWS public services using public IP addresses.
- **Transit virtual interface (VIF):** A transit virtual interface should be used to access one or more Amazon VPC Transit Gateways associated with Direct Connect gateways.
- We can use **transit virtual interfaces** with any **AWS Direct Connect** dedicated or hosted connection of any speed.

# 6. Data Transfer Out (DTO) over AWS Direct Connect

- **DTO** refers to the total amount of data sent **out of AWS** via an **AWS Direct Connect** location.
- It is **charged per gigabyte (GB)**, based on **data volume**, not speed.
- **Pricing varies** depending on the AWS Region, Local Zone, and Direct Connect location.
- **Important:** Direct Connect transfers data to your on-premises location, **not over the internet**.
- For internet-based DTO charges, see the [EC2 pricing page](https://aws.amazon.com/directconnect/pricing/).
