# AWS Systems Manager<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Systems Manager parameters - SSM parameters](#2-systems-manager-parameters---ssm-parameters)
  - [2.1. Policies](#21-policies)

# 1. Introduction

- AWS Systems Manager is the operations hub for your AWS applications and resources and a secure end-to-end management solution for hybrid and multicloud environments that enables secure operations at scale.

# 2. Systems Manager parameters - SSM parameters

## 2.1. Policies

- Parameter policies help you manage a growing set of parameters by allowing you to assign specific criteria to a parameter, such as an expiration date or time to live.
- Parameter policies are especially helpful in forcing you to update or delete passwords and configuration data stored in Parameter Store, a capability of AWS Systems Manager.
- Take note that parameter policies are only available for parameters in the **Advanced tier**.
- Parameter Store offers the following types of policies:
  - `Expiration` - deletes the parameter at a specific date
  - `ExpirationNotification` - sends an event to Amazon EventBridge when the specified expiration time is reached.
  - `NoChangeNotification` - sends an event to Amazon EventBridge when a parameter has not been modified for a specified period of time.