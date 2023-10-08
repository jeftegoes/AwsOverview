# Amazon ASG - Auto Scaling Groups<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Auto Scaling Group Attributes](#2-auto-scaling-group-attributes)
- [3. CloudWatch Alarms \& Scaling](#3-cloudwatch-alarms--scaling)
- [4. Dynamic Scaling Policies](#4-dynamic-scaling-policies)
- [5. Predictive Scaling](#5-predictive-scaling)
- [6. Good metrics to scale on](#6-good-metrics-to-scale-on)
- [7. Scaling Cooldowns](#7-scaling-cooldowns)
- [8. Lifecycle Hooks](#8-lifecycle-hooks)
  - [8.1. EventBridge Rule Integration](#81-eventbridge-rule-integration)
- [9. ASG -\> SNS Notifications](#9-asg---sns-notifications)
- [10. EventBridge Events](#10-eventbridge-events)
- [11. Auto Scaling - Instance Refresh](#11-auto-scaling---instance-refresh)
- [12. Scaling Strategies (Resume)](#12-scaling-strategies-resume)
- [13. Termination Policies](#13-termination-policies)
  - [13.1. Different Termination Policies](#131-different-termination-policies)
- [14. Scale-out Latency Problem](#14-scale-out-latency-problem)
- [15. ASG Warm Pools](#15-asg-warm-pools)
  - [15.1. Instance States](#151-instance-states)
  - [15.2. Pricing: m5.large](#152-pricing-m5large)
  - [15.3. Instance Reuse Policy](#153-instance-reuse-policy)
- [16. AWS Application Auto Scaling](#16-aws-application-auto-scaling)
  - [16.1. Integrated AWS Services](#161-integrated-aws-services)

# 1. Introduction

- In real-life, the load on your websites and application can change.
- In the cloud, you can create and get rid of servers very quickly.
- The goal of an Auto Scaling Group (ASG) is to:
  - Scale out (add EC2 instances) to match an increased load.
  - Scale in (remove EC2 instances) to match a decreased load.
  - Ensure we have a minimum and a maximum number of machines running.
  - Automatically register new instances to a load balancer.
  - Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy) (Replace unhealthy instances).
- Cost Savings: only run at an optimal capacity (principle of the cloud).
- ASG are free (you only pay for the underlying EC2 instances).

# 2. Auto Scaling Group Attributes

- A **Launch Template** (older "Launch Configurations" are deprecated):
  - AMI + Instance Type.
  - EC2 User Data.
  - EBS Volumes.
  - Security Groups.
  - SSH Key Pair.
  - IAM Roles for your EC2 Instances.
  - Network + Subnets Information.
  - Load Balancer Information.
- Min Size / Max Size / Initial Capacity.
- Scaling Policies.

# 3. CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms.
- An alarm monitors a metric (such as **Average CPU**, or a **custom metric**).
- Metrics such as Average CPU are computed for the overall ASG instances.
- Based on the alarm:
  - We can create scale-out policies (increase the number of instances).
  - We can create scale-in policies (decrease the number of instances).

# 4. Dynamic Scaling Policies

- **Target Tracking Scaling**
  - Most simple and easy to set-up.
  - Example: I want the average ASG CPU to stay at around 40%.
- **Simple / Step Scaling**
  - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units.
  - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1.
- **Scheduled Actions**
  - Anticipate a scaling based on known usage patterns.
  - Example: increase the min capacity to 10 at 5 pm on Fridays.

# 5. Predictive Scaling

- **Predictive scaling:** continuously forecast load and schedule scaling ahead.

# 6. Good metrics to scale on

- `CPUUtilization` - Average CPU utilization across your instances.
- `RequestCountPerTarget` - To make sure the number of requests per EC2 instances is stable.
- **Average Network In / Out** (if you're application is network bound).
- **Any custom metric** (that you push using CloudWatch).

# 7. Scaling Cooldowns

- After a scaling activity happens, you are in the **cooldown period (default 300 seconds)**.
- During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize).
- Advice: Use a ready-to-use AMI to reduce configuration time in order to be serving request fasters and reduce the cooldown period.

# 8. Lifecycle Hooks

- By default, as soon as an instance is launched in an ASG it's in service.
- You can perform extra steps before the instance goes in service (Pending state).
  - Define a script to run on the instances as they start.
- You can perform some actions before the instance is terminated (Terminating state).
  - Pause the instances before they're terminated for troubleshooting.
- Use cases: cleanup, log extraction, special health checks.
- Integration with EventBridge, SNS, and SQS.

![Amazon ASG - Lifecycle Hooks](/Images/AmazonAsgLifecycleHooks.png)

## 8.1. EventBridge Rule Integration

![EventBridge Rule Integration](/Images/AmazonAsgEc2RuleLifecycleAction.png)

# 9. ASG -> SNS Notifications

- ASG supports sending SNS notifications for the following events:
  - `autoscaling:EC2_INSTANCE_LAUNCH`
  - `autoscaling:EC2_INSTANCE_LAUNCH_ERROR`
  - `autoscaling:EC2_INSTANCE_TERMINATE`
  - `autoscaling:EC2_INSTANCE_TERMINATE_ERROR`

# 10. EventBridge Events

- You can create Rules that match the following ASG events:
  - EC2 Instance Launching, EC2 Instance Launch Successful/Unsuccessful.
  - EC2 Instance Terminating, EC2 Instance Terminate Successful/Unsuccessful.
  - EC2 Auto Scaling Instance Refresh Checkpoint Reached.
  - EC2 Auto Scaling Instance Refresh Started, Succeeded, Failed, Cancelled.

# 11. Auto Scaling - Instance Refresh

- Goal: update launch template and then re-creating all EC2 instances.
- For this we can use the native feature of Instance Refresh.
- Setting of minimum healthy percentage.
- Specify warm-up time (how long until the instance is ready to use).

# 12. Scaling Strategies (Resume)

- Manual Scaling: Update the size of an ASG manually
- Dynamic Scaling: Respond to changing demand
  - Simple / Step Scaling
    - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
    - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
- Target Tracking Scaling
  - Example: I want the average ASG CPU to stay at around 40%
- Scheduled Scaling
  - Anticipate a scaling based on known usage patterns
  - Example: increase the min. capacity to 10 at 5 pm on Fridays
- Predictive Scaling
  - Uses Machine Learning to predict future traffic ahead of time.
  - Automatically provisions the right number of EC2 instances in advance.
  - Useful when your load has predictable time-based patterns.

# 13. Termination Policies

- Determine which instances to terminates first during scale-in events, Instance Refresh, and AZ Rebalancing.
- **Default Termination Policy**
  - Select AZ with more instances.
  - Terminate instance with oldest Launch Template or Launch Configuration.
  - If instances were launched using the same Launch Template, terminate the instance that is closest to the next billing hour.

## 13.1. Different Termination Policies

- `Default` - Terminates instances according to Default Termination Policy
- `AllocationStrategy` - Terminates instances to align the remaining instances to the Allocation Strategy (e.g., lowest-price for Spot Instances, or lower priority On-Demand Instances)
- `OldestLaunchTemplate` - Terminates instances that have the oldest Launch Template
- `OldestLaunchConfiguration` - Terminates instances that have the oldest Launch Configuration
- `ClosestToNextInstanceHour` - Terminates instances that are closest to the next billing hour
- `NewestInstance` - Terminates the newest instance (testing new launch template)
- `OldestInstance` - Terminates the oldest instance (upgrading instance size, not launch template)
- **Note: you can use one or more policies and specify the evaluation order.**
- **Note: can define Custom Termination Policy backed by a Lambda function.**

# 14. Scale-out Latency Problem

- When an ASG scales out, it tries to launch instances as fast as possible.
- Some applications contain a lengthy unavoidable latency that exists at the application initialization/bootstrap layer (several minutes or more).
- Processes that can only happen at initial boot: applying updates, data or state hydration, running configuration scripts...
- Solution was to over-provision compute resources to absorb unexpected demand increases (increased cost) or use Golden Images to try to reduce boot time.
- **New solution: ASG Warm Pools**

# 15. ASG Warm Pools

- Reduces scale-out latency by maintaining a pool of pre-initialized instances.
- In a scale-out event, ASG uses the pre-initialized instances from the Warm Pool instead of launching new instances.
- **Warm Pool Size Settings**
  - **Minimum warm pool size** (always in the warm pool).
  - **Max prepared capacity** = Max capacity of ASG (default).
  - **OR Max prepared capacity** = Set number of instances.
- **Warm Pool Instance State** - What state to keep your Warm Pool instances in after initialization **(Running, Stopped, Hibernated)**.
- Warm Pools instances don't contribute to ASG metrics that affect Scaling Policies.

## 15.1. Instance States

|                 | Running                                                                    | Stopped                                                                                                                                                 | Hibernated                                                                                                                                                                                   |
| --------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scale Out Delay | Faster - immediately available to accept traffic                           | Slower - need to be started before traffic can be served which adds some delay                                                                          | Medium - pre-initialized EC2 instances (disk, memory...)                                                                                                                                     |
| Start Up Delay  | Lower - EC2 instances are already running                                  | Slower - needs to go through the application startup process which may need some time especially if there are many application components to initialize | Medium - faster startup process than EC2 instances in the Stopped state because components and applications are already initialized in memory                                                |
| Costs           | Higher - incur costs for their usage even when they're not serving traffic | Lower Cost - do not incur running costs, only pay for the attached resources, more cost effective                                                       | Lower Cost - do not incur running costs, only pay for the attached resources, more cost effective The hibernation state must be able to fit onto the EBS volume (might need a bigger volume) |

## 15.2. Pricing: m5.large

- If we "over provision an EC2 instance" in an ASG.
- Running Cost = $0.096/hour _ 24 hours/day _ 30 days = $69.12 \* EBS charges.
- If the EC2 instance is stopped, we only pay for the attached EBS volume.
- EBS Volume Cost = $0.10/GB-month \* 10GB = $1.00

## 15.3. Instance Reuse Policy

- By default, ASG terminates instances when ASG scales in, then it launches new instances into the Warm Pool.
- **Instance Reuse Policy allows you to return instances to the Warm Pool when a scale-in event happens.**

# 16. AWS Application Auto Scaling

- Monitors your apps and automatically adjusts capacity to maintain steady, predictable performance at lowest cost.
- Setup scaling for multiple resouces across multiple services from a single place (no need to navigate across different services).
- Point to your app and select the services and resources you want to scale (no need to setup alarms and scaling actions for each service).
- Search for resources/services using CloudFormation Stack, Tags, or EC2 ASG.
- Build Scaling Plans to automatically add/remove capacity from your resources in real-time as demand changes.
- **Supports Target Tracking, Step, and Scheduled Scaling Policies.**

## 16.1. Integrated AWS Services

- AppStream 2.0 - Fleets
- Aurora - Replicas
- Comprehend - Document classification and Entity recognizer endpoints
- DynamoDB - Tables & GSI
- ECS - Services
- ElastiCache for Redis - Replication Groups
- EMR - Clusters
- KeySpaces - Tables
- Lambda - Provisioned Concurrency
- MSK - Broker Storage
- Neptune - Clusters
- SageMaker - Endpoint Variants
- Spot Fleet - Requests
- Custom Resources
