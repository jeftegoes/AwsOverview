# Amazon EC2 - Spot Instance <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Spot Instance](#1-spot-instance)
- [2. Requests](#2-requests)
- [3. How to terminate Spot Instances?](#3-how-to-terminate-spot-instances)
- [4. Spot Fleets](#4-spot-fleets)

# 1. Spot Instance

- Can get a **discount of up to 90%** compared to On-demand.
- Instances that we can "lose" at any point of time if your max price is less than the current spot price.
- The **MOST cost-efficient** instances in AWS.
- **Useful for workloads that are resilient to failure:**
  - Batch jobs.
  - Data analysis.
  - Image processing.
  - Any **distributed** workloads.
  - Workloads with a flexible start and end time.
- **Not suitable for critical jobs or databases.**

# 2. Requests

- Define **max spot price** and get the instance while **current spot price < max**.
  - The hourly spot price varies based on offer and capacity.
  - If the current spot price > your max price we can choose to **stop** or **terminate** your instance with a 2 minutes grace period.
- **Other strategy:** Spot Block
  - "block" spot instance during a specified time frame (1 to 6 hours) without interruptions.
  - In rare situations, the instance may be reclaimed.
- **Not great for critical jobs or databases.**

# 3. How to terminate Spot Instances?

- **Request types**:
  - **One-time**: Not reopened after interruption.
  - **Persistent**: Reopened after interruption, or after you manually restart a stopped instance.
    ![Amazon EC2 Spot Instance Request](/Images/Compute/AmazonEC2SpotInstanceRequest1.png)
    ![Amazon EC2 Spot Instance Request](/Images/Compute/AmazonEC2SpotInstanceRequest2.png)
- We can only cancel Spot Instance requests that are open, active, or disabled.
- Cancelling a Spot Request does not terminate instances.
- We must first cancel a Spot Request, and then terminate the associated Spot Instances.

# 4. Spot Fleets

- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances.
- The Spot Fleet will try to meet the target capacity with price constraints.
  - **Define possible launch pools:** Instance type (m5.large), OS, Availability Zone.
  - Can have multiple launch pools, so that the fleet can choose.
  - Spot Fleet stops launching instances when reaching capacity or max cost.
- **Strategies to allocate Spot Instances**
  - `lowestPrice`: From the pool with the lowest price (cost optimization, short workload).
  - `diversified`: Distributed across all pools (great for availability, long workloads).
  - `capacityOptimized`: Pool with the optimal capacity for the number of instances.
  - `priceCapacityOptimized` (recommended): Pools with highest capacity available, then select the pool with the lowest price (best choice for most workloads).
- _Spot Fleets allow us to automatically request Spot Instances with the lowest price._
