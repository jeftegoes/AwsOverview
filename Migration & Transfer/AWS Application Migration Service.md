# AWS Application Migration Service (MGN) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Final Steps](#2-final-steps)

# 1. Introduction

- The "AWS evolution" of **CloudEndure Migration**, replacing **AWS Server Migration Service (SMS)**.
- Lift-and-shift (rehost) solution which simplify migrating applications to AWS.
- Converts your physical, virtual, and cloud-based servers to run natively on AWS.
- Supports wide range of platforms, Operating Systems, and databases.
- Minimal downtime, reduced costs.
  ![AWS Application Migration Service (MGN)](/Images/Migration%20&%20Transfer/AWSApplicationMigrationService.png)

# 2. Final Steps

1. **Launch Test Instances**
   - After the initial sync, test instances are launched to verify functionality and data integrity of the migrated VMs.
2. **Perform Validation Testing**
   - Ensures the environment works correctly before production cutover.
3. **Final Sync and Cutover**
   - AWS MGN performs a final sync of any changes.
   - Then, a cutover instance is launched to go live in production.
4. **Decommission Original VM**
   - Once the EC2 instance is validated in production, the original on-premises VM can be safely decommissioned.
