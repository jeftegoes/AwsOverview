# AWS CodePipeline<!-- omit in toc -->

# Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. How pipeline executions are started](#2-how-pipeline-executions-are-started)
- [3. Artifacts](#3-artifacts)
- [4. Troubleshooting](#4-troubleshooting)
- [5. Events vs.Webhooks vs. Polling](#5-events-vswebhooks-vs-polling)
- [6. Action types constraints for artifacts](#6-action-types-constraints-for-artifacts)
- [7. Manual Approval Stage](#7-manual-approval-stage)
- [8. CloudFormation as a target](#8-cloudformation-as-a-target)
- [9. CloudFormation integration](#9-cloudformation-integration)
- [10. Best practices](#10-best-practices)
- [11. EventBridge](#11-eventbridge)
- [12. Invoke Action](#12-invoke-action)
- [13. Multi Region (Cross-Region Actions)](#13-multi-region-cross-region-actions)
- [14. Pipeline executions](#14-pipeline-executions)

# 1. Introduction

- Visual Workflow to orchestrate your CI/CD.
- **Source:** CodeCommit, ECR, S3, Bitbucket, GitHub.
- **Build:** CodeBuild, Jenkins, CloudBees, TeamCity.
- **Test:** CodeBuild, AWS Device Farm, 3rd party tools, ...
- **Deploy:** CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3, ...
- **Invoke:** Lambda, Step Functions.
- Consists of stages:
  - Each stage can have sequential actions and/or parallel actions.
  - Example: Build -> Test -> Deploy -> Load Testing -> ...
  - Manual approval can be defined at any stage.

# 2. How pipeline executions are started

- You can trigger an execution when you **change your source code** or **manually** start the pipeline.
- You can also trigger an execution through an [Amazon CloudWatch](/Management%20&%20Governance/Amazon%20CloudWatch.md) Events rule that you schedule.

![CodePipeline](/Images/AWSCodePipeline.png)

# 3. Artifacts

- Each pipeline stage can create `artifacts`.
- `artifacts` - This element represents information about where CodeBuild can find the build output and how CodeBuild prepares it for uploading to the S3 output bucket.
  - Artifacts stored in an S3 bucket and passed on to the next stage.

# 4. Troubleshooting

- For CodePipeline Pipeline/Action/Stage Execution State Changes.
- Use **CloudWatch Events (Amazon EventBridge)**.
- Example:
  - You can create events for failed pipelines.
  - You can create events for cancelled stages.
- If CodePipeline fails a stage, your pipeline stops, and you can get information in the console.
- If pipeline can't perform an action, make sure the "IAM Service Role" attached does have enough IAM permissions (IAM Policy).
- AWS CloudTrail can be used to audit AWS API calls.

# 5. Events vs.Webhooks vs. Polling

- Events
  - CodeCommit -> `event` -> EventBridge -> `trigger` -> CodePipeline
  - GitHub -> CodeStar Source Connection (GitHub App) -> `trigger` -> CodePipeline
  - **Note: Events are the default and recommended**
- Webhooks
  - Script/Code -> `HTTP Webhook` CodePipeline
- Polling
  - GitHub -> regular checks -> CodePipeline

# 6. Action types constraints for artifacts

- **Owner**
  - **AWS:** For AWS services.
  - **3rd Party:** GitHub or Alexa Skills Kit.
  - **Custom:** Jenkins.
- **Action Type**
  - **Source:** S3, ECR, GitHub, ...
  - **Build:** CodeBuild, Jenkins.
  - **Test:** CodeBuild, Device Farm, Jenkins.
  - **Approval:** Manual.
  - **Invoke:** Lambda.
  - **Deploy:** S3, CloudFormation, CodeDeploy, Elastic Beanstalk, OpsWorks, ECS, Service Catalog, ...

# 7. Manual Approval Stage

- CodePipeline
  - CodeCommit -> `new commit` CodeBuild -> `trigger` > Manual Approval -> `deploy` -> CodeDeploy
- **Important: Owner is "AWS", Action is "Manual".**

# 8. CloudFormation as a target

- CloudFormation Deploy Action can be used to deploy AWS resources.
- Example: deploy Lambda functions using CDK or SAM (alternative to CodeDeploy).
- Works with CloudFormation StackSets to deploy across multiple AWS accounts and AWS Regions.
- Configure different settings:
  - Stack name, Change Set name, template, parameters, IAM Role, Action Mode...
- **Action Modes**
  - Create or Replace a Change Set, Execute a Change Set.
  - Create or Update a Stack, Delete a Stack, Replace a Failed Stack.
- **Template Parameter Overrides**
  - Specify a JSON object to override parameter values.
  - Retrives the parameter value from CodePipeline Input Artifact.
  - All parameter names must be present in the template.
  - **Static:** Use template configuration file (recommended).
  - **Dynamic:** Use parameter overrides.

# 9. CloudFormation integration

- `CREATE_UPDATE` - Create or update an existing stack.
- `DELETE_ONLY` - Delete a stack if it exists.

# 10. Best practices

- One CodePipeline, One CodeDeploy, Parallel deploy to multiple Deployment Groups.
  ![Multiple Deployment Groups](/Images/AWSCodePipelineBestPractices_1.png)
- Parallel Actions using in a Stage using RunOrder.
  ![Multiple Deployment Groups](/Images/AWSCodePipelineBestPractices_2.png)
- Deploy to Pre-Prod before Deploying to Prod.
  ![Multiple Deployment Groups](/Images/AWSCodePipelineBestPractices_3.png)

# 11. EventBridge

- EventBridge - Detect and react to changes in execution states (e.g., intercept failures at certain stages).

# 12. Invoke Action

- **Lambda:** Invokes a Lambda function within a Pipeline.
- **Step Functions:** Starts a State Machine within a Pipeline.

# 13. Multi Region (Cross-Region Actions)

- **Actions in your pipeline can be in different regions**
  - Example: deploy a Lambda function through CloudFormation into multiple regions.
- **S3 Artifact Stores must be defined in each region where you have actions**
  - CodePipeline must have read/write access into every **artifact buckets**.
  - If you use the console default **artifact buckets** are configured, else you must create them.
- CodePipeline handles the copying of input artifacts from one AWS Region to the other Regions when performing **cross-region actions**.
  - In your **cross-region actions**, only reference the name of the input artifacts.

![AWS CodePipeline - Multi Region](/Images/AWSCodePipelineMultiRegion.png)

# 14. Pipeline executions

- Traverse pipeline stages in order.
- Valid statuses for pipelines are:
  - InProgress
  - Stopping
  - Stopped
  - Succeeded
  - Superseded
  - Failed.
