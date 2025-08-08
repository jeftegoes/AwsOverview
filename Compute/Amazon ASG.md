# Amazon ASG - Auto Scaling Groups <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Auto Scaling Group in AWS](#2-auto-scaling-group-in-aws)
- [3. Auto Scaling Group in AWS With Load Balancer](#3-auto-scaling-group-in-aws-with-load-balancer)
- [4. Auto Scaling Group Attributes](#4-auto-scaling-group-attributes)
- [5. CloudWatch Alarms \& Scaling](#5-cloudwatch-alarms--scaling)
- [6. Scaling Strategies (Scaling Policies)](#6-scaling-strategies-scaling-policies)
  - [6.1. Predictive Scaling](#61-predictive-scaling)
- [7. Good metrics to scale on](#7-good-metrics-to-scale-on)
- [8. Scaling Cooldowns](#8-scaling-cooldowns)
- [9. Lifecycle Hooks](#9-lifecycle-hooks)
- [10. ASG -\> SNS Notifications](#10-asg---sns-notifications)
- [11. EventBridge Events](#11-eventbridge-events)
- [12. Auto Scaling - Instance Refresh](#12-auto-scaling---instance-refresh)
- [13. Termination Policies](#13-termination-policies)
  - [13.1. Different Termination Policies](#131-different-termination-policies)
- [14. Scale-out Latency Problem](#14-scale-out-latency-problem)
- [15. ASG Warm Pools](#15-asg-warm-pools)
  - [15.1. Instance States](#151-instance-states)
  - [15.2. Pricing: m5.large](#152-pricing-m5large)
  - [15.3. Instance Reuse Policy](#153-instance-reuse-policy)
- [16. AWS Application Auto Scaling](#16-aws-application-auto-scaling)
  - [16.1. Integrated AWS Services](#161-integrated-aws-services)
- [17. Terraform details](#17-terraform-details)
- [18. ASG and EC2 Impaired status](#18-asg-and-ec2-impaired-status)
- [19. EC2 Auto Scaling and ELB Health Checks](#19-ec2-auto-scaling-and-elb-health-checks)
- [20. Scaling Based on Amazon SQS](#20-scaling-based-on-amazon-sqs)
- [21. Health Check Grace Period](#21-health-check-grace-period)
- [22. Health Check Replacement vs. AZ Rebalancing](#22-health-check-replacement-vs-az-rebalancing)
  - [22.1. Health Check Replacement](#221-health-check-replacement)
  - [22.2. Availability Zone (AZ) Rebalancing](#222-availability-zone-az-rebalancing)
  - [22.3. Summary Table](#223-summary-table)

# 1. Introduction

- In real-life, the load on your websites and application can change.
- In the cloud, you can create and get rid of servers very quickly.
- **The goal of an Auto Scaling Group (ASG) is to**
  - **Scale out** (add EC2 instances) to match an increased load.
  - **Scale in** (remove EC2 instances) to match a decreased load.
  - Ensure we have a minimum and a maximum number of machines running.
  - Automatically register new instances to a load balancer.
  - Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy) (Replace unhealthy instances).
- **Cost Savings:** Only run at an optimal capacity (principle of the cloud).
- ASG are free (you only pay for the underlying EC2 instances).

# 2. Auto Scaling Group in AWS

![Auto Scaling Group in AWS](/Images/Compute/AmazonASGDiagram.png)

# 3. Auto Scaling Group in AWS With Load Balancer

![Auto Scaling Group in AWS With Load Balancer](/Images/Compute/AmazonASGWithLoadBalancer.png)

# 4. Auto Scaling Group Attributes

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
  ![Amazon ASG Launch Template](/Images/Compute/AmazonASGLaunchTemplate.png)

# 5. CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms.
- An alarm monitors a metric (such as **Average CPU**, or a **custom metric**).
- Metrics such as Average CPU are computed for the overall ASG instances.
- **Based on the alarm**
  - We can create scale-out policies (increase the number of instances).
  - We can create scale-in policies (decrease the number of instances).
    ![CloudWatch Alarms & Scaling](/Images/Compute/AmazonASGCloudWatchAlarms.png)

# 6. Scaling Strategies (Scaling Policies)

- **Manual Scaling:** Update the size of an ASG manually.
- **Dynamic Scaling:** Respond to changing demand.
  - **Simple / Step Scaling**
    - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units.
    - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1.
- **Target Tracking Scaling**
  - Most simple and easy to set-up.
  - **Example:** I want the **average** ASG CPU to stay at around 40%.
- **Scheduled Scaling**
  - Anticipate a scaling based on known usage patterns.
  - **Example:** Increase the min. capacity to 10 at 5 pm on Fridays.
- **Predictive Scaling**
  - Uses Machine Learning to predict future traffic ahead of time.
  - Automatically provisions the right number of EC2 instances in advance.
  - Useful when your load has predictable time-based patterns.

## 6.1. Predictive Scaling

- **Predictive scaling:** Continuously forecast load and schedule scaling ahead.
  ![Predictive scaling](/Images/Compute/AmazonEC2ASGPredictiveScaling.png)
- Uses **machine learning** to analyze historical traffic patterns.
- **Forecasts demand** and schedules capacity ahead of time.
- Ideal for **daily and weekly workload trends**.
- Reacts to **real-time traffic changes** (e.g., scale when CPU > 60%).
- Ensures quick response to **unexpected spikes**.

# 7. Good metrics to scale on

- `CPUUtilization` - Average CPU utilization across your instances.
- `RequestCountPerTarget` - To make sure the number of requests per EC2 instances is stable.
- **Average Network In / Out** (if you're application is network bound).
- **Any custom metric** (that you push using CloudWatch).

# 8. Scaling Cooldowns

- After a scaling activity happens, you are in the **cooldown period (default 300 seconds)**.
- During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize).
- **Advice:** Use a ready-to-use AMI to reduce configuration time in order to be serving request fasters and reduce the cooldown period.

# 9. Lifecycle Hooks

- By default, as soon as an instance is launched in an ASG it's in service.
- You can perform extra steps before the instance goes in service (Pending state).
  - Define a script to run on the instances as they start.
- You can perform some actions before the instance is terminated (Terminating state).
  - Pause the instances before they're terminated for troubleshooting.
- **Use cases:** Cleanup, log extraction, special health checks.
- Integration with EventBridge, SNS, and SQS.
  ![Amazon ASG - Lifecycle Hooks](/Images/AmazonAsgLifecycleHooks.png)

# 10. ASG -> SNS Notifications

- ASG supports sending SNS notifications for the following events:
  - `autoscaling:EC2_INSTANCE_LAUNCH`
  - `autoscaling:EC2_INSTANCE_LAUNCH_ERROR`
  - `autoscaling:EC2_INSTANCE_TERMINATE`
  - `autoscaling:EC2_INSTANCE_TERMINATE_ERROR`

# 11. EventBridge Events

- You can create Rules that match the following ASG events:
  - EC2 Instance Launching, EC2 Instance Launch Successful/Unsuccessful.
  - EC2 Instance Terminating, EC2 Instance Terminate Successful/Unsuccessful.
  - EC2 Auto Scaling Instance Refresh Checkpoint Reached.
  - EC2 Auto Scaling Instance Refresh Started, Succeeded, Failed, Cancelled.

![EventBridge Rule Integration](/Images/AmazonAsgEc2RuleLifecycleAction.png)

# 12. Auto Scaling - Instance Refresh

- **Goal:** Update launch template and then re-creating all EC2 instances.
- For this we can use the native feature of Instance Refresh.
- Setting of minimum healthy percentage.
- Specify warm-up time (how long until the instance is ready to use).

# 13. Termination Policies

- Determine which instances to terminates first during scale-in events, Instance Refresh, and AZ Rebalancing.
- **Default Termination Policy**
  1. Determine the Availability Zone with the most instances.
  2. Within that AZ, find instances using the oldest launch configuration.
  3. If there are multiple instances with the same oldest launch configuration, terminate the one that is closest to the next billing hour.
  4. If there's still a tie, terminate randomly among them.
- **Note:** **Launch configuration** and **launch template** are different.
  - AWS treats them separately in termination logic.
  - It prefers launch configurations (legacy method) when deciding termination under this policy.

## 13.1. Different Termination Policies

- `Default` - Terminates instances according to Default Termination Policy.
- `AllocationStrategy` - Terminates instances to align the remaining instances to the Allocation Strategy (e.g., lowest-price for Spot Instances, or lower priority On-Demand Instances).
- `OldestLaunchTemplate` - Terminates instances that have the oldest Launch Template.
- `OldestLaunchConfiguration` - Terminates instances that have the oldest Launch Configuration.
- `ClosestToNextInstanceHour` - Terminates instances that are closest to the next billing hour.
- `NewestInstance` - Terminates the newest instance (testing new launch template).
- `OldestInstance` - Terminates the oldest instance (upgrading instance size, not launch template).
- **Note: We can use one or more policies and specify the evaluation order.**
- **Note: Can define Custom Termination Policy backed by a Lambda function.**

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

# 17. Terraform details

- If you plan to launch an Auto Scaling group of EC2 instances, you can configure the `AWS::AutoScaling::AutoScalingGroup` resource type reference in your CloudFormation template to define an Amazon EC2 Auto Scaling group with the specified name and attributes.
- You can add an `UpdatePolicy` attribute to your Auto Scaling group to perform rolling updates (or replace the group) when a change has been made to the group.
- To specify how AWS CloudFormation handles replacement updates for an Auto Scaling group, use the `AutoScalingReplacingUpdate` policy.
- This policy enables you to specify whether AWS CloudFormation replaces an Auto Scaling group with a new one or replaces only the instances in the Auto Scaling group.
  ```
    UpdatePolicy:
      AutoScalingReplacingUpdate:
        WillReplace: true
  ```
- During replacement, AWS CloudFormation retains the old group until it finishes creating the new one.
  1. If the update fails, AWS CloudFormation can roll back to the old Auto Scaling group and delete the new Auto Scaling group.
  2. While AWS CloudFormation creates the new group, it doesn't detach or attach any instances.
  3. After successfully creating the new Auto Scaling group, AWS CloudFormation deletes the old Auto Scaling group during the cleanup process.
- When you set the `WillReplace` parameter, remember to specify a matching CreationPolicy.
  - If the minimum number of instances (specified by the `MinSuccessfulInstancesPercent` property) doesn't signal success within the Timeout period (specified in the CreationPolicy policy), the replacement update fails, and AWS CloudFormation rolls back to the old Auto Scaling group.

# 18. ASG and EC2 Impaired status

- Amazon EC2 Auto Scaling does not immediately terminate instances with an **Impaired status**.
- Instead, Amazon EC2 Auto Scaling waits a few minutes for the instance to recover.
- Amazon EC2 Auto Scaling might also delay or not terminate instances that fail to report data for status checks.
- This usually happens when there is insufficient data for the status check metrics in Amazon CloudWatch.

# 19. EC2 Auto Scaling and ELB Health Checks

- By default, **Amazon EC2 Auto Scaling uses EC2 health checks**, not ELB health checks.
- This means it **won't terminate instances** that fail **ELB health checks** unless configured otherwise.
- If an instance appears `OutOfService` in the ELB console but **Healthy** in the Auto Scaling console, check the group's **health check type**.
- To ensure Auto Scaling responds to ELB failures, set the health check type to **ELB**.

# 20. Scaling Based on Amazon SQS

![Scaling Based on Amazon SQS](/Images/Compute/AmazonEC2ASGScalingBasedSQS.png)

# 21. Health Check Grace Period

- **Amazon EC2 Auto Scaling** waits for the **health check grace period** to expire before terminating an instance.
- This applies even if the instance fails **EC2 status checks** or **ELB health checks**.
- The grace period allows the instance time to **initialize and become healthy** before being evaluated for termination.

# 22. Health Check Replacement vs. AZ Rebalancing

## 22.1. Health Check Replacement

- **Triggered by**: Detection of an unhealthy instance (via EC2 or ELB health checks).
- **Sequence**:
  1. Terminates the unhealthy instance.
  2. Launches a new instance to replace it.
- **Impact**: May cause a temporary dip in capacity.
- **Reason**: The instance is already unhealthy and likely not serving traffic.

## 22.2. Availability Zone (AZ) Rebalancing

- **Triggered by**: Imbalance in instance distribution across AZs (e.g., due to manual changes or AZ configuration updates).
- **Sequence**:
  1. Launches a new instance in the underused AZ.
  2. Terminates an instance from the overloaded AZ.
- **Impact**: Maintains full capacity at all times.
- **Reason**: To avoid degrading performance during rebalancing.

## 22.3. Summary Table

| Feature                     | Health Check Replacement | AZ Rebalancing |
| --------------------------- | ------------------------ | -------------- |
| Trigger                     | Unhealthy instance       | AZ imbalance   |
| Terminates instance first   | Yes                      | No             |
| Launches new instance first | No                       | Yes            |
| Temporary capacity dip      | Yes                      | No             |
