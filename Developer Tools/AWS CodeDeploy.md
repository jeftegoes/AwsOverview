# AWS CodeDeploy<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Steps To Make it Work](#2-steps-to-make-it-work)
- [3. Primary Components](#3-primary-components)
- [4. Hooks](#4-hooks)
  - [Examples](#examples)
  - [4.1. appspec.yml](#41-appspecyml)
  - [4.2. List of lifecycle event hooks](#42-list-of-lifecycle-event-hooks)
  - [4.3. Deployment Hooks Examples](#43-deployment-hooks-examples)
- [5. EC2/On-premises Platform](#5-ec2on-premises-platform)
  - [5.1. In-Place deployment](#51-in-place-deployment)
  - [5.2. Blue / Green Deployment](#52-blue--green-deployment)
  - [5.3. CodeDeploy Agent](#53-codedeploy-agent)
- [6. Lambda Platform](#6-lambda-platform)
- [7. ECS Platform](#7-ecs-platform)
- [8. Deployment to EC2](#8-deployment-to-ec2)
- [9. In-place](#9-in-place)
  - [9.1. In-place Deployment Hooks](#91-in-place-deployment-hooks)
- [10. Deploy to an ASG](#10-deploy-to-an-asg)
- [11. Redeploy and Rollbacks](#11-redeploy-and-rollbacks)
- [12. Troubleshooting](#12-troubleshooting)
  - [12.1. Scenario 1](#121-scenario-1)
  - [12.2. Scenario 2](#122-scenario-2)
  - [12.3. Scenario 3](#123-scenario-3)
  - [12.4. Scenario 4](#124-scenario-4)

# 1. Introduction

- CodeDeploy is a deployment service that automates application deployments to:
  - Amazon EC2 instances.
  - On-premises instances.
  - Serverless Lambda functions.
  - Amazon ECS services.
- Automated Rollback capability in case of failed deployments, or trigger CloudWatch Alarm.
- Gradual deployment control.
- A file named appspec.yml defines how the deployment happens.
- There are several ways to handle deployments using open-source tools (Ansible, Terraform, Chef, Puppet, ...).

# 2. Steps To Make it Work

- **Each EC2 instance/on-premises server must be running the CodeDeploy Agent.**
- The agent is continuously polling AWS CodeDeploy for work to do.
- Application + `appspec.yml` is pulled from GitHub or S3.
- EC2 instances will run the deployment instructions in `appspec.yml`.
- CodeDeploy Agent will report of success/failure of the deployment.

# 3. Primary Components

- **Application:** A unique name functions as a container (revision, deployment configuration, ...).
- **Compute Platform:** EC2/On-Premises, AWS Lambda, or Amazon ECS
  - **Deployment Configuration:** A set of deployment rules for success/failure - EC2/On-premises
  - Specify the minimum number of healthy instances for the deployment.
  - **AWS Lambda or Amazon ECS:** specify how traffic is routed to your updated versions.
- **Deployment Group:** Group of tagged EC2 instances (allows to deploy gradually, or dev, test, prod...).
- **Deployment Type:** Method used to deploy the application to a Deployment Group.
  - **In-place Deployment:** Supports EC2 / On-Premises.
  - **Blue/Green Deployment:** Supports EC2 instances only, AWS Lambda, and Amazon ECS.
- **IAM Instance Profile:** Give EC2 instances the permissions to access both S3 / GitHub.
- **Application Revision:** Application code + `appspec.yml` file.
- **Service Role:** An IAM Role for CodeDeploy to perform operations on EC2 instances, ASGs, ELBs...
- **Target Revision:** The most recent revision that you want to deploy to a Deployment Group.

# 4. Hooks

- The content in the `hooks` section of the `AppSpec.yml` file varies, depending on the compute platform for your deployment.
- The `hooks` section for an **EC2/On-Premises** deployment contains mappings that link deployment lifecycle event hooks to one or more scripts.
- The `hooks` section for a **Lambda or an Amazon ECS deployment** specifies Lambda validation functions to run during a deployment lifecycle event.
- **If an event hook is not present, no operation is executed for that event.**
- There is also a set of available environment variables for the hooks.
- During each deployment lifecycle event, hook scripts can access the following environment variables:
- `APPLICATION_NAME` - The name of the application in CodeDeploy that is part of the current deployment (for example, WordPress_App).
- `DEPLOYMENT_ID` - The ID CodeDeploy has assigned to the current deployment (for example, d-AB1CDEF23).
- `DEPLOYMENT_GROUP_NAME` - The name of the deployment group in CodeDeploy that is part of the current deployment (for example, WordPress_DepGroup).
- `DEPLOYMENT_GROUP_ID` - The ID of the deployment group in CodeDeploy that is part of the current deployment (for example, b1a2189b-dd90-4ef5-8f40-4c1c5EXAMPLE).
- `LIFECYCLE_EVENT` - The name of the current deployment lifecycle event (for example, AfterInstall).
- These environment variables are local to each deployment lifecycle event.

## Examples

- The following script changes the listening port on an Apache HTTP server to 9090 instead of 80 if the value of `DEPLOYMENT_GROUP_NAME` is equal to Staging.
- This script must be invoked during the `BeforeInstall` deployment lifecycle event:

  ```
  if [ "$DEPLOYMENT_GROUP_NAME" == "Staging" ]
  then
    sed -i -e 's/Listen 80/Listen 9090/g' /etc/httpd/conf/httpd.conf
  fi
  ```

- The following script example changes the verbosity level of messages recorded in its error log from warning to debug if the value of the `DEPLOYMENT_GROUP_NAME` environment variable is equal to Staging.
- This script must be invoked during the `BeforeInstall` deployment lifecycle event:

  ```
  if [ "$DEPLOYMENT_GROUP_NAME" == "Staging" ]
  then
      sed -i -e 's/LogLevel warn/LogLevel debug/g' /etc/httpd/conf/httpd.conf
  fi
  ```

- **There is no such thing as custom environment variable in CodeDeploy.**

## 4.1. appspec.yml

- `files` - How to source and copy from S3 / GitHub to filesystem
  - `source`
  - `destination`
- `hooks` - The content in the `hooks` section of the AppSpec file varies, depending on the compute platform for your deployment:
  - `ApplicationStop`
  - EC2
    - `DownloadBundle`
    - `Install`
    - `ValidateService` <- important!!
    - `BeforeInstall`
    - `AfterInstall`
    - `ApplicationStart`
    - `AllowTrafic`
    - `BeforeAllowTraffic`
    - `AfterAllowTraffic`
  - Lambda
    - `Start` - Cannot be scripted or used.
    - `BeforeAllowTraffic` - Use to run tasks before traffic is shifted to the deployed Lambda function version.
    - `AllowTrafic`
    - `AfterAllowTraffic` - Use to run tasks after all traffic is shifted to the deployed Lambda function version.
  - ECS
    - `BeforeInstall`
    - `AfterInstall`
    - `AfterAllowTestTraffic`
    - `AfterAllowTraffic`
    - `BeforeAllowTraffic`

## 4.2. List of lifecycle event hooks

- `Install` - During this deployment lifecycle event, the CodeDeploy agent copies the revision files from the temporary location to the final destination folder.
  - **This event is reserved for the CodeDeploy agent and cannot be used to run scripts.**

## 4.3. Deployment Hooks Examples

- `BeforeInstall` - You can use this deployment lifecycle event for preinstall tasks, such as decrypting files and creating a backup of the current version.
- `Install` - During this deployment lifecycle event, the CodeDeploy agent copies the revision files from the temporary location to the final destination folder. This event is reserved for the CodeDeploy agent and cannot be used to run scripts.
- `AfterInstall` - You can use this deployment lifecycle event for tasks such as configuring your application or changing file permissions.
- `ApplicationStart` - You typically use this deployment lifecycle event to restart services that were stopped during ApplicationStop.
- `ValidateService` - This is the last deployment lifecycle event. It is used to verify the deployment was completed successfully.
- `BeforeBlockTraffic` - You can use this deployment lifecycle event to run tasks on instances before they are deregistered from a load balancer.
- `BlockTraffic` - During this deployment lifecycle event, internet traffic is blocked from accessing instances that are currently serving traffic. This event is reserved for the CodeDeploy agent and cannot be used to run scripts.
- `AfterBlockTraffic` - You can use this deployment lifecycle event to run tasks on instances after they are deregistered from a load balancer.
- `BeforeAllowTraffic` - You can use this deployment lifecycle event to run tasks on instances before they are registered with a load balancer.
- `AllowTraffic` - During this deployment lifecycle event, internet traffic is allowed to access instances after a deployment. This event is reserved for the CodeDeploy agent and cannot be used to run scripts.
- `AfterAllowTraffic` - You can use this deployment lifecycle event to run tasks on instances after they are registered with a load balancer.

# 5. EC2/On-premises Platform

- Can deploy to EC2 Instances & on-premises servers.
- Perform in-place deployments or blue/green deployments.
- Must run the **CodeDeploy Agent** on the target instances.
- Define deployment speed:
  - `AllAtOnce` - Most downtime.
  - `HalfAtATime` - Reduced capacity by 50%.
  - `OneAtATime` - Slowest, lowest availability impact.
  - `Custom` - Define your.

## 5.1. In-Place deployment

![Aws CodeDeploy In-Place Deployment](/Images/AwsCodeDeployInPlaceDeployment.png)

## 5.2. Blue / Green Deployment

![Aws CodeDeploy Blue/Green Deployment](/Images/AwsCodeDeployBlueGreenDeployment.png)

## 5.3. CodeDeploy Agent

- The CodeDeploy Agent must be running on the EC2 instances as a pre-requisites.
- It can be installed and updated automatically if you're using Systems Manager.
- The EC2 Instances must have sufficient permissions to access Amazon S3 to get deployment bundles.

# 6. Lambda Platform

- **CodeDeploy** can help you automate traffic shift for Lambda aliases.
- Feature is integrated within the SAM framework.
- **Linear:** grow traffic every N minutes until 100%
  - `LambdaLinear10PercentEvery3Minutes`
  - `LambdaLinear10PercentEvery10Minutes`
- **Canary:** try X percent then 100%
  - `LambdaCanary10Percent5Minutes`
  - `LambdaCanary10Percent30Minutes`
- `AllAtOnce` - Immediate.

# 7. ECS Platform

- CodeDeploy can help you automate the deployment of a new **ECS Task Definition**.
- **Only Blue/Green Deployments**.
- **Linear:** grow traffic every N minutes until 100%.
  - `ECSLinear10PercentEvery3Minutes`
  - `ECSLinear10PercentEvery10Minutes`
- **Canary:** try X percent then 100%:
  - `ECSCanary10Percent5Minutes`
  - `ECSCanary10Percent30Minutes`
- `AllAtOnce` - Immediate.

# 8. Deployment to EC2

- Define how to deploy the application using appspec.yml +
  Deployment Strategy
- Will do In
  -place update to
  your fleet of EC2 instances
- Can use hooks to verify the
  deployment after each
  deployment phase

# 9. In-place

- Use EC2 Tags or ASG to identify instances you want to deploy to.
- With a Load Balancer: traffic is stopped before instance is updated, and started again after the instance is updated.

## 9.1. In-place Deployment Hooks

- Hooks - one or more scripts to be run by CodeDeploy on each EC2 instance.

# 10. Deploy to an ASG

- **In-place Deployment**
  - Updates existing EC2 instances.
  - Newly created EC2 instances by an ASG will also get automated deployments.
- **Blue/Green Deployment**
  - A new Auto-Scaling Group is created (settings are copied).
  - Choose how long to keep the old EC2 instances (old ASG).
  - Must be using an ELB.

# 11. Redeploy and Rollbacks

- Rollback = redeploy a previously deployed revision of your application.
- Deployments can be rolled back:
  - **Automatically:** Rollback when a deployment fails or rollback when a CloudWatch Alarm thresholds are met.
  - **Manually.**
- Disable Rollbacks - Do not perform rollbacks for this deployment.
- **If a roll back happens, CodeDeploy redeploys the last known good revision as a new deployment (not a restored version).**

# 12. Troubleshooting

## 12.1. Scenario 1

- **Deployment Error: "InvalidSignatureException - Signature expired: [time] is now earlier than [time]"**
  - For CodeDeploy to perform its operations, it requires accurate time references.
  - If the date and time on your EC2 instance are not set correctly, they might not match the signature date of your deployment request, which CodeDeploy rejects.
- Check log files to understand deployment issues:
  - For Amazon Linux, Ubuntu, and RHEL log files stored at `/opt/codedeploy-agent/deployment-root/deployment- logs/codedeploy-agent-deployments.log`.

## 12.2. Scenario 2

- **When the Deployment or all Lifecycle Events are skipped (EC2/On- Premises)**, you get one of the following errors:
  - "The overall deployment failed because too many individual instances failed deployment".
  - "Too few healthy instances are available for deployment".
  - "Some instances in your deployment group are experiencing problems. (Error code: `HEALTH_CONSTRAINTS`)".
- **Reasons:**
  - CodeDeploy Agent might not be installed, running, or it can't reach CodeDeploy.
  - CodeDeploy Service Role or IAM instance profile might not have the required permissions.
  - You're using an HTTP Proxy, configure CodeDeploy Agent with `:proxy_uri:` parameter.
  - Date and Time mismatch between CodeDeploy and Agent.

![Deployment failure](/Images/AWSCodeDeployDeploymentFailure.png)

## 12.3. Scenario 3

- If a CodeDeploy deployment to ASG is underway and a scale-out event occurs, the new instances will be updated with the application revision that was most recently deployed (not the application revision that is currently being deployed).
- ASG will have EC2 instances hosting different versions of the application.
- By default, CodeDeploy automatically starts a follow-on deployment to update any outdated EC2 instances.

## 12.4. Scenario 4

- Issue: failed AllowTraffic lifecycle event in Blue/Green Deployments with no error reported in the Deployment Logs:
- **Reason:** incorrectly configured health checks in ELB.
- **Resolution:** review and correct any errors in ELB health checks configuration.

```

```
