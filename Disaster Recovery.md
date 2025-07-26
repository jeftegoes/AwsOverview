# Disaster Recovery <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. RPO \& RTO](#2-rpo--rto)
- [3. Disaster Recovery Strategies (Faster RTO)](#3-disaster-recovery-strategies-faster-rto)
  - [3.1. Backup and Restore (High RPO)](#31-backup-and-restore-high-rpo)
  - [3.2. Pilot Light](#32-pilot-light)
  - [3.3. Warm Standby](#33-warm-standby)
  - [3.4. Multi Site / Hot Site Approach (Low RTO)](#34-multi-site--hot-site-approach-low-rto)
    - [3.4.1. All AWS Multi Region](#341-all-aws-multi-region)
- [4. Disaster Recovery Tips](#4-disaster-recovery-tips)

# 1. Introduction

- Any event that has a negative impact on a company's business continuity or finances is a disaster.
- Disaster recovery (DR) is about preparing for and recovering from a disaster.
- What kind of disaster recovery?
  - **On-premise => On-premise:** Traditional DR, and very expensive.
  - **On-premise => AWS Cloud:** Hybrid recovery.
  - AWS Cloud Region A => AWS Cloud Region B.

# 2. RPO & RTO

- **RPO:** Recovery Point Objective.
  - How much of a data loss are we willing to accept in case of a disaster happens?
- **RTO:** Recovery Time Objective.
  - Is when we recover from your disaster.
    ![RPO & RTO](/Images/DisasterRecoveryRpoRto.png)

# 3. Disaster Recovery Strategies (Faster RTO)

- Backup and Restore.
- Pilot Light.
- Warm Standby.
- Hot Site / Multi Site Approach.
  - ![Faster RTO](/Images/DisasterRecoveryFasterRTO.png)

## 3.1. Backup and Restore (High RPO)

![Backup and Restore](/Images/DisasterRecoveryBackupAndRestore.png)

## 3.2. Pilot Light

- A small version of the app is always running in the cloud.
- Useful for the critical core (pilot light).
- Very similar to Backup and Restore.
- Faster than Backup and Restore as critical systems are already up.
  ![Pilot Light](/Images/DisasterRecoveryPilotLight.png)

## 3.3. Warm Standby

- Full system is up and running, but at minimum size.
- Upon disaster, we can scale to production load.
  ![Warm Standby](/Images/DisasterRecoveryWarmStandby.png)

## 3.4. Multi Site / Hot Site Approach (Low RTO)

- Very low RTO (minutes or seconds).
  - **Very expensive.**
- Full Production Scale is running AWS and On Premise.

![Multi Site / Hot Site Approach](/Images/DisasterRecoveryMultiSiteHotSite.png)

### 3.4.1. All AWS Multi Region

![Multi Site / Hot Site Approach all AWS](/Images/DisasterRecoveryMultiSiteHotSiteAllAWS.png)

# 4. Disaster Recovery Tips

- **Backup**
  - EBS Snapshots, RDS automated backups / Snapshots, etc...
  - Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross-Region Replication.
  - From On-Premise: Snowball or Storage Gateway.
- **High Availability**
  - Use Route53 to migrate DNS over from Region to Region.
  - RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3.
  - Site to Site VPN as a recovery from Direct Connect.
- **Replication**
  - RDS Replication (Cross-Region), AWS Aurora + Global Databases.
  - Database replication from on-premise to RDS.
  - Storage Gateway.
- **Automation**
  - CloudFormation / Elastic Beanstalk to re-create a whole new environment.
  - Recover / Reboot EC2 instances with CloudWatch if alarms fail.
  - AWS Lambda functions for customized automations.
- **Chaos**
  - Netflix has a "simian-army" randomly terminating EC2.
