# AWS Application Discovery Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Application Discovery Service APIs](#2-application-discovery-service-apis)
- [3. Discovery and collecting](#3-discovery-and-collecting)
  - [3.1. Agentless](#31-agentless)
  - [3.2. Agent-based](#32-agent-based)
- [4. AWS Application Migration Service (MGN)](#4-aws-application-migration-service-mgn)

# 1. Introduction

- **AWS Application Discovery Service** helps you plan your migration to the AWS cloud by collecting usage and configuration data about your on-premises servers.
- Application Discovery Service is integrated with **AWS Migration Hub**, which simplifies your migration tracking.
- After performing discovery, you can view the discovered servers, group them into applications, and then track the migration status of each application from the Migration Hub console.
- The discovered data can be exported for analysis in Microsoft Excel or AWS analysis tools such as [Amazon Athena](/Analytics/Amazon%20Athena.md) and Amazon QuickSight.

# 2. Application Discovery Service APIs

- Using Application Discovery Service APIs, you can export the system performance and utilization data for your discovered servers.
- You can input this data into your cost model to compute the cost of running those servers in AWS.
- Additionally, you can export the network connections and process data to understand the network connections that exist between servers.
- This will help you determine the network dependencies between servers and group them into applications for migration planning.

# 3. Discovery and collecting

- Application Discovery Service offers two ways of performing discovery and collecting data about your on-premises servers:

## 3.1. Agentless

- **Agentless discovery** can be performed by deploying the **AWS Agentless Discovery Connector (OVA file)** through your VMware vCenter.
- After the Discovery Connector is configured, it identifies virtual machines (VMs) and hosts associated with vCenter.
- The Discovery Connector collects the following static configuration data: Server hostnames, IP addresses, MAC addresses, and disk resource allocations.
  - Additionally, it collects the utilization data for each VM and computes average and peak utilization for metrics such as CPU, RAM, and Disk I/O. You can export a summary of the system performance information for all the VMs associated with a given VM host and perform a cost analysis of running them in AWS.

## 3.2. Agent-based

- Agent-based discovery can be performed by deploying the **AWS Application Discovery Agent** on each of your VMs and physical servers.
- The agent installer is available for both Windows and Linux operating systems.
- It collects static configuration data, detailed time-series system-performance information, inbound and outbound network connections, and processes that are running.
- You can export this data to perform a detailed cost analysis and to identify network connections between servers for grouping servers as applications.

# 4. AWS Application Migration Service (MGN)

- The "AWS evolution" of CloudEndure Migration, replacing AWS Server Migration Service (SMS).
- Lift-and-shift (rehost) solution which simplify migrating applications to AWS.
- Converts your physical, virtual, and cloud-based servers to run natively on AWS.
- Supports wide range of platforms, Operating Systems, and databases.
- Minimal downtime, reduced costs.
