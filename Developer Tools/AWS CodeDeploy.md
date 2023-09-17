# AWS CodeDeploy<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Steps To Make it Work](#2-steps-to-make-it-work)
- [3. Primary Components](#3-primary-components)
- [4. appspec.yml](#4-appspecyml)
  - [4.1. List of lifecycle event hooks](#41-list-of-lifecycle-event-hooks)
  - [4.2. Deployment Hooks Examples](#42-deployment-hooks-examples)
- [5. EC2/On-premises Platform](#5-ec2on-premises-platform)
  - [5.1. In-Place deployment](#51-in-place-deployment)
  - [5.2. Blue / Green Deployment](#52-blue--green-deployment)
  - [5.3. CodeDeploy Agent](#53-codedeploy-agent)
- [6. Lambda Platform](#6-lambda-platform)
- [7. ECS Platform](#7-ecs-platform)
- [Deployment to EC2](#deployment-to-ec2)
- [8. In-place](#8-in-place)
  - [8.1. In-place Deployment Hooks](#81-in-place-deployment-hooks)
- [9. Deploy to an ASG](#9-deploy-to-an-asg)
- [10. Redeploy and Rollbacks](#10-redeploy-and-rollbacks)
- [11. Troubleshooting](#11-troubleshooting)
  - [11.1. Scenario 1](#111-scenario-1)
  - [11.2. Scenario 2](#112-scenario-2)
  - [11.3. Scenario 3](#113-scenario-3)
  - [11.4. Scenario 4](#114-scenario-4)

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

# 4. appspec.yml

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

## 4.1. List of lifecycle event hooks

- `Install` - During this deployment lifecycle event, the CodeDeploy agent copies the revision files from the temporary location to the final destination folder.
  - **This event is reserved for the CodeDeploy agent and cannot be used to run scripts.**

## 4.2. Deployment Hooks Examples

- `BeforeInstall` - Used for preinstall tasks, such as decrypting files and creating a backup of the current version.
- `AfterInstall` - Used for tasks such as configuring your application or changing file permissions.
- `ApplicationStart` - Used to start services that stopped during ApplicationStop.
- `ValidateService` - Used to verify the deployment was completed successfully.
- `BeforeAllowTraffic` - Run tasks on EC2 instances before registered to the load balancer.
  - Example: perform health checks on the application and fail the deployment if the health checks are not successful.

# 5. EC2/On-premises Platform

- Can deploy to EC2 Instances & on-premises servers.
- Perform in-place deployments or blue/green deployments.
- Must run the **CodeDeploy Agent** on the target instances.
- Define deployment speed:
  - AllAtOnce: Most downtime.
  - HalfAtATime: Reduced capacity by 50%.
  - OneAtATime: Slowest, lowest availability impact.
  - Custom: Define your.

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

# Deployment to EC2

- Define how to deploy the application using appspec.yml +
  Deployment Strategy
- Will do In
  -place update to
  your fleet of EC2 instances
- Can use hooks to verify the
  deployment after each
  deployment phase

# 8. In-place

- Use EC2 Tags or ASG to identify instances you want to deploy to.
- With a Load Balancer: traffic is stopped before instance is updated, and started again after the instance is updated.

## 8.1. In-place Deployment Hooks

- Hooks - one or more scripts to be run by CodeDeploy on each EC2 instance.

# 9. Deploy to an ASG

- **In-place Deployment**
  - Updates existing EC2 instances.
  - Newly created EC2 instances by an ASG will also get automated deployments.
- **Blue/Green Deployment**
  - A new Auto-Scaling Group is created (settings are copied).
  - Choose how long to keep the old EC2 instances (old ASG).
  - Must be using an ELB.

# 10. Redeploy and Rollbacks

- Rollback = redeploy a previously deployed revision of your application.
- Deployments can be rolled back:
  - **Automatically:** Rollback when a deployment fails or rollback when a CloudWatch Alarm thresholds are met.
  - **Manually.**
- Disable Rollbacks - Do not perform rollbacks for this deployment.
- **If a roll back happens, CodeDeploy redeploys the last known good revision as a new deployment (not a restored version).**

# 11. Troubleshooting

## 11.1. Scenario 1

- **Deployment Error: "InvalidSignatureException - Signature expired: [time] is now earlier than [time]"**
  - For CodeDeploy to perform its operations, it requires accurate time references.
  - If the date and time on your EC2 instance are not set correctly, they might not match the signature date of your deployment request, which CodeDeploy rejects.
- Check log files to understand deployment issues:
  - For Amazon Linux, Ubuntu, and RHEL log files stored at `/opt/codedeploy-agent/deployment-root/deployment- logs/codedeploy-agent-deployments.log`.

## 11.2. Scenario 2

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

## 11.3. Scenario 3

- If a CodeDeploy deployment to ASG is underway and a scale-out event occurs, the new instances will be updated with the application revision that was most recently deployed (not the application revision that is currently being deployed).
- ASG will have EC2 instances hosting different versions of the application.
- By default, CodeDeploy automatically starts a follow-on deployment to update any outdated EC2 instances.

## 11.4. Scenario 4

- Issue: failed AllowTraffic lifecycle event in Blue/Green Deployments with no error reported in the Deployment Logs:
- **Reason:** incorrectly configured health checks in ELB.
- **Resolution:** review and correct any errors in ELB health checks configuration.
