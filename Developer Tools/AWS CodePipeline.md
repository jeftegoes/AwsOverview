# AWS CodePipeline <!-- omit in toc -->

# Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Details](#2-details)
- [3. How pipeline executions are started](#3-how-pipeline-executions-are-started)
- [4. Artifacts](#4-artifacts)
- [5. Troubleshooting](#5-troubleshooting)
- [6. Events vs.Webhooks vs. Polling](#6-events-vswebhooks-vs-polling)
- [7. Action types constraints for artifacts](#7-action-types-constraints-for-artifacts)
- [8. Manual Approval Stage](#8-manual-approval-stage)
- [9. CloudFormation as a target](#9-cloudformation-as-a-target)
- [10. CloudFormation integration](#10-cloudformation-integration)
- [11. Best practices](#11-best-practices)
- [12. EventBridge](#12-eventbridge)
- [13. Invoke Action](#13-invoke-action)
- [14. Multi Region (Cross-Region Actions)](#14-multi-region-cross-region-actions)
- [15. Pipeline executions](#15-pipeline-executions)
- [16. Custom Actions (Job Worker)](#16-custom-actions-job-worker)

# 1. Introduction

- Build serverless visual workflow to orchestrate your Lambda functions.
- **Features:** Sequence, parallel, conditions, timeouts, error handling, ...
- Can integrate with EC2, ECS, On-premises servers, API Gateway, SQS queues, etc...
- Possibility of implementing human approval feature.
- **Use cases:** Order fulfillment, data processing, web applications, any workflow.

# 2. Details

- Visual Workflow to orchestrate your CI/CD.
- **Source:** CodeCommit, ECR, S3, Bitbucket, GitHub.
- **Build:** CodeBuild, Jenkins, CloudBees, TeamCity.
- **Test:** CodeBuild, AWS Device Farm, 3rd party tools, ...
- **Deploy:** CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3, ...
- **Invoke:** Lambda, Step Functions.
- **Consists of stages**
  - Each stage can have sequential actions and/or parallel actions.
  - Example: Build -> Test -> Deploy -> Load Testing -> ...
  - Manual approval can be defined at any stage.

# 3. How pipeline executions are started

- You can trigger an execution when you **change your source code** or **manually** start the pipeline.
- You can also trigger an execution through an [Amazon CloudWatch](/Management%20&%20Governance/Amazon%20CloudWatch.md) Events rule that you schedule.

![CodePipeline](/Images/AWSCodePipeline.png)

# 4. Artifacts

- Each pipeline stage can create `artifacts`.
- `artifacts` - This element represents information about where CodeBuild can find the build output and how CodeBuild prepares it for uploading to the S3 output bucket.
  - Artifacts stored in an S3 bucket and passed on to the next stage.

# 5. Troubleshooting

- For CodePipeline Pipeline/Action/Stage Execution State Changes.
- Use **CloudWatch Events (Amazon EventBridge)**.
- Example:
  - You can create events for failed pipelines.
  - You can create events for cancelled stages.
- If CodePipeline fails a stage, your pipeline stops, and you can get information in the console.
- If pipeline can't perform an action, make sure the "IAM Service Role" attached does have enough IAM permissions (IAM Policy).
- AWS CloudTrail can be used to audit AWS API calls.

# 6. Events vs.Webhooks vs. Polling

- Events
  - CodeCommit -> `event` -> EventBridge -> `trigger` -> CodePipeline
  - GitHub -> CodeStar Source Connection (GitHub App) -> `trigger` -> CodePipeline
  - **Note: Events are the default and recommended**
- Webhooks
  - Script/Code -> `HTTP Webhook` CodePipeline
- Polling
  - GitHub -> regular checks -> CodePipeline

# 7. Action types constraints for artifacts

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

# 8. Manual Approval Stage

- CodePipeline
  - CodeCommit -> `new commit` CodeBuild -> `trigger` > Manual Approval -> `deploy` -> CodeDeploy
- **IMPORTANT! Owner is "AWS", Action is "Manual".**

# 9. CloudFormation as a target

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

# 10. CloudFormation integration

- `CREATE_UPDATE` - Create or update an existing stack.
- `DELETE_ONLY` - Delete a stack if it exists.

# 11. Best practices

- One CodePipeline, One CodeDeploy, Parallel deploy to multiple Deployment Groups.
  ![Multiple Deployment Groups](/Images/AWSCodePipelineBestPractices_1.png)
- **Parallel Actions** using in a Stage using `RunOrder`.
  ![Multiple Deployment Groups](/Images/AWSCodePipelineBestPractices_2.png)
- Deploy to Pre-Prod before Deploying to Prod.
  ![Multiple Deployment Groups](/Images/AWSCodePipelineBestPractices_3.png)

- Example using `RunOrder`:
  ```
    [
      {
        "inputArtifacts": [
          "An input artifact structure, if supported for the action category"
        ],
        "name": "ActionName",
        "region": "Region",
        "namespace": "source_namespace",
        "actionTypeId": {
          "category": "An action category",
          "owner": "AWS",
          "version": "1",
          "provider": "A provider type for the action category"
        },
        "outputArtifacts": [
          "An output artifact structure, if supported for the action category"
        ],
        "configuration": ["Configuration details appropriate to the provider type"],
        "runOrder": "A positive integer that indicates the run order within the stage"
      }
    ]
  ```

# 12. EventBridge

- EventBridge - Detect and react to changes in execution states (e.g., intercept failures at certain stages).

# 13. Invoke Action

- **Lambda:** Invokes a Lambda function within a Pipeline.
- **Step Functions:** Starts a State Machine within a Pipeline.

# 14. Multi Region (Cross-Region Actions)

- **Actions in your pipeline can be in different regions**
  - Example: deploy a Lambda function through CloudFormation into multiple regions.
- **S3 Artifact Stores must be defined in each region where you have actions**
  - CodePipeline must have read/write access into every **artifact buckets**.
  - If you use the console default **artifact buckets** are configured, else you must create them.
- CodePipeline handles the copying of input artifacts from one AWS Region to the other Regions when performing **cross-region actions**.
  - In your **cross-region actions**, only reference the name of the input artifacts.

![AWS CodePipeline - Multi Region](/Images/AWSCodePipelineMultiRegion.png)

# 15. Pipeline executions

- Traverse pipeline stages in order.
- Valid statuses for pipelines are:
  - InProgress
  - Stopping
  - Stopped
  - Succeeded
  - Superseded
  - Failed.

# 16. Custom Actions (Job Worker)

- You can create custom actions for the following AWS CodePipeline action categories:
  - A custom build action that builds or transforms the items.
  - A custom deploy action that deploys items to one or more servers, websites, or repositories.
  - A custom test action that configures and runs automated tests.
  - A custom invoke action that runs functions.

![Custom Actions](/Images/AWSCodePipelineCustomJobWorker.png)
