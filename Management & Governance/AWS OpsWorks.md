# AWS OpsWorks <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Ohters details](#2-ohters-details)
- [3. Stack](#3-stack)
  - [3.1. Stacks Lifecycle Events](#31-stacks-lifecycle-events)
    - [3.1.1. Lifecycle](#311-lifecycle)
- [4. Layer, Instance, \& App](#4-layer-instance--app)
- [5. Scaling](#5-scaling)
- [6. Deployment \& Customization](#6-deployment--customization)
- [7. Auto Healing](#7-auto-healing)

# 1. Introduction

- Chef & Puppet help you perform server configuration automatically, or repetitive actions.
- They work great with EC2 & On-Premises VM.
- AWS OpsWorks = Managed Chef & Puppet.
- It's an alternative to [AWS System Manager](AWS%20Systems%20Manager.md).
- Only provision standard AWS resources:
  - EC2 Instances, Databases, Load Balancers, EBS volumes...

![AWS OpsWorks](/Images/Management%20&%20Governance/AWSOpsWorksGeneralDiagram.png)

# 2. Ohters details

- They help with managing configuration as code.
- Helps in having consistent deployments.
- Works with Linux / Windows.
- Can automate: User accounts, cron, ntp, packages, services...
- They leverage "Recipes" or "Manifests".
- Chef / Puppet have similarities with SSM / Beanstalk / CloudFormation but they're open-source tools that work cross-cloud.

# 3. Stack

- Top-level OpsWorks entity.
- Represents a set of instances and applications that you want to manage collectively.
- E.g Web Server stack may contain a load balancer, server instances and database.

![AWS OpsWorks Stacks](/Images/Management%20&%20Governance/AWSOpsWorksStacks.png)

## 3.1. Stacks Lifecycle Events

- In **AWS OpsWorks Stacks Lifecycle Events**, each layer has a set of **five lifecycle events**, each of which has an associated set of recipes that are specific to the layer.
- When an event occurs on a layer's instance, AWS OpsWorks Stacks automatically runs the appropriate set of recipes.
- To provide a custom response to these events, implement custom recipes and assign them to the appropriate events for each layer.
- AWS OpsWorks Stacks runs those recipes after the event's built-in recipes.
- When AWS OpsWorks Stacks runs a command on an instance-for example, a deploy command in response to a Deploy lifecycle event-it adds a set of attributes to the instance's node object that describes the stack's current configuration.
- For Deploy events and Execute Recipes stack commands, AWS OpsWorks Stacks installs deploy attributes, which provide some additional deployment information.

### 3.1.1. Lifecycle

- There are five lifecycle events namely:
  - `Setup` - Is sent to the **instance** when it is instantiated or successfully booted.
  - `Configure` - Notifies all **instances** whenever the state of the stack changes.
  - `Deploy` - Deploy is triggered whenever an **application** is deployed.
  - `UnDeploy` - Is sent when you delete an **application**.
  - `Shutdown` - Is sent to an **instance** 45 seconds before actually stopping the **instance**.
- The Configure event occurs on all of the stack's instances when one of the following occurs:
  - An instance enters or leaves the online state.
  - You associate an Elastic IP address with an instance or disassociate one from an instance.
  - You attach an Elastic Load Balancing load balancer to a layer or detach one from a layer.

# 4. Layer, Instance, & App

- Define how to setup and configure instances and resources.
- Stack must Contains one of more layer.
- Layer must contain at least one instance.
- Instance can be member of multiple layers.
- Apps represent code to run on your server.

# 5. Scaling

- 24/7 instance added to a layer can manually start stop or reboot the corresponding EC2 instances.
- **Automatic Scaling**
  - **Time-based instances** - They allow a stack to handle loads that follow a predictable pattern by including instances that run only at certain times or on certain days.
  - **Load-based instances** - They allow a stack to handle variable loads by starting additional instances when traffic is high and stopping instances when traffic is low, based on any of several load metrics.
- Combination of all 3 types is an effective strategy.

# 6. Deployment & Customization

- App and associate infrastructure is deployed automatically.
- Chef recipes define infrasctructure as code:
  - Customization.
  - Redeployment.
  - Version Control.
  - Code Reuse.

# 7. Auto Healing

- Every instance has an AWS OpsWorks Stacks agent that communicates regularly with the service.
- AWS OpsWorks Stacks uses that communication to monitor instance health.
- If an agent does not communicate with the service for more than approximately five minutes, AWS OpsWorks Stacks considers the instance to have failed.

![AutoHealing](/Images/Management%20&%20Governance/AWSOpsWorksAutoHealing.png)

- The `initiated_by` field is only populated when the instance is in the **requested**, **terminating**, or **stopping** states.
- The `initiated_by` field can contain one of the following values.
  - `user` - A user requested the instance state change by using either the API or AWS Management Console.
  - `auto-scaling` - The AWS OpsWorks Stacks automatic scaling feature initiated the instance state change.
  - `auto-healing` - The AWS OpsWorks Stacks automatic healing feature initiated the instance state change.
