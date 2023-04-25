# AWS CI/CD - CodeCommit, CodePipeline, CodeBuild, CodeDeploy, CodeStar, CodeArtifact, CodeGuru, Cloud9<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. CICD - Introduction](#1-cicd---introduction)
- [2. Continuous Integration (CI)](#2-continuous-integration-ci)
- [3. Continuous Delivery (CD)](#3-continuous-delivery-cd)
- [4. AWS CodeCommit](#4-aws-codecommit)
  - [4.1. Security](#41-security)
  - [4.2. CodeCommit vs. GitHub](#42-codecommit-vs-github)
- [5. AWS CodePipeline](#5-aws-codepipeline)
  - [5.1. Artifacts](#51-artifacts)
  - [5.2. Troubleshooting](#52-troubleshooting)
  - [5.3. Events vs.Webhooks vs. Polling](#53-events-vswebhooks-vs-polling)
  - [5.4. Action Types Constraints for](#54-action-types-constraints-for)
  - [5.5. Manual Approval Stage](#55-manual-approval-stage)
- [6. AWS CodeBuild](#6-aws-codebuild)
  - [6.1. Supported Environments](#61-supported-environments)
  - [6.2. buildspec.yml](#62-buildspecyml)
  - [6.3. Local Build](#63-local-build)
  - [6.4. Inside VPC](#64-inside-vpc)
  - [6.5. CloudFormation Integration](#65-cloudformation-integration)
- [7. AWS CodeDeploy](#7-aws-codedeploy)
  - [7.1. Steps To Make it Work](#71-steps-to-make-it-work)
  - [7.2. Primary Components](#72-primary-components)
  - [7.3. appspec.yml](#73-appspecyml)
  - [7.4. Deployment Configuration](#74-deployment-configuration)
  - [7.5. Deployment to EC2](#75-deployment-to-ec2)
  - [7.6. Deploy to an ASG](#76-deploy-to-an-asg)
  - [7.7. Redeploy \& Rollbacks](#77-redeploy--rollbacks)
  - [7.8. Troubleshooting](#78-troubleshooting)
- [8. AWS CodeStar](#8-aws-codestar)
- [9. AWS CodeArtifact](#9-aws-codeartifact)
  - [9.1. Resource Policy](#91-resource-policy)
  - [9.2. Upstream Repositories](#92-upstream-repositories)
  - [9.3. External Connection](#93-external-connection)
  - [9.4. Retention](#94-retention)
  - [9.5. Domains](#95-domains)
- [10. Amazon CodeGuru](#10-amazon-codeguru)
  - [10.1. Reviewer](#101-reviewer)
  - [10.2. Profiler](#102-profiler)
  - [10.3. Agent Configuration](#103-agent-configuration)
- [11. AWS Cloud9](#11-aws-cloud9)

# 1. CICD - Introduction

- We have learned how to:
  - Create AWS resources, manually (fundamentals).
  - Interact with AWS programmatically (AWS CLI).
  - Deploy code to AWS using Elastic Beanstalk.
- All these manual steps make it very likely for us to do mistakes!
- We would like our code "in a repository" and have it deployed onto AWS
  - Automatically.
  - The right way.
  - Making sure it's tested before being deployed.
  - With possibility to go into different stages (dev, test, staging, prod).
  - With manual approval where needed.
- This is all about automating the deployment we've done so far while adding increased safety:
  - **AWS CodeCommit:** Storing our code.
  - **AWS CodePipeline:** Automating our pipeline from code to Elastic Beanstalk.
  - **AWS CodeBuild:** Building and testing our code.
  - **AWS CodeDeploy:** Deploying the code to EC2 instances (not Elastic Beanstalk).
  - **AWS CodeStar:** Manage software development activities in one place.
  - **AWS CodeArtifact:** Store, publish, and share software packages.
  - **AWS CodeGuru:** Automated code reviews using Machine Learning.

# 2. Continuous Integration (CI)

- Developers push the code to a code repository often (e.g., GitHub, CodeCommit, Bitbucket...).
- A testing / build server checks the code as soon as it's pushed (CodeBuild, Jenkins CI, ...).
- The developer gets feedback about the tests and checks that have passed / failed.
- Find bugs early, then fix bugs.
- Deliver faster as the code is tested.
- Deploy often.
- Happier developers, as they're unblocked.

# 3. Continuous Delivery (CD)

- Ensures that the software can be released reliably whenever needed.
- Ensures deployments happen often and are quick.
- Shift away from "one release every 3 months" to "5 releases a day".
- That usually means automated deployment (e.g., CodeDeploy, Jenkins CD, Spinnaker, ...).

# 4. AWS CodeCommit

- **Version control** is the ability to understand the various changes that happened to the code over time (and possibly roll back).
- All these are enabled by using a version control system such as **Git**.
- A Git repository can be synchronized on your computer, but it usually is uploaded on a central online repository.
- Benefits are:
  - Collaborate with other developers.
  - Make sure the code is backed-up somewhere.
  - Make sure it's fully viewable and auditable.
- Git repositories can be expensive.
- The industry includes GitHub, GitLab, Bitbucket, ...
- And **AWS CodeCommit**:
  - Private Git repositories.
  - No size limit on repositories (scale seamlessly).
  - Fully managed, highly available.
  - Code only in AWS Cloud account => increased security and compliance.
  - Security (encrypted, access control, ...).
  - Integrated with Jenkins, AWS CodeBuild, and other CI tools.

## 4.1. Security

- Interactions are done using Git (standard).
- **Authentication**
  - **SSH Keys:** AWS Users can configure SSH keys in their IAM Console.
  - **HTTPS:** With AWS CLI Credential helper or Git Credentials for IAM user.
- **Authorization**
  - IAM policies to manage users/roles permissions to repositories.
- **Encryption**
  - Repositories are automatically encrypted at rest using AWS KMS.
  - Encrypted in transit (can only use HTTPS or SSH - both secure).
- **Cross-account Access**
  - Do NOT share your SSH keys or your AWS credentials.
  - Use an IAM Role in your AWS account and use AWS STS (AssumeRole API).

## 4.2. CodeCommit vs. GitHub

|                                     | CodeCommit              | GitHub                                                            |
| ----------------------------------- | ----------------------- | ----------------------------------------------------------------- |
| Support Code Review (Pull Requests) | Ok                      | Ok                                                                |
| Integration with AWS CodeBuild      | Ok                      | Ok                                                                |
| Authentication (SSH & HTTPS)        | Ok                      | Ok                                                                |
| Security                            | IAM Users & Roles       | GitHub Users                                                      |
| Hosting                             | Managed & hosted by AWS | Hosted by GitHub / GitHub Enterprise: self hosted on your servers |
| UI                                  | Minimal                 | Fully Featured                                                    |

# 5. AWS CodePipeline

- Visual Workflow to orchestrate your CI/CD.
- **Source:** CodeCommit, ECR, S3, Bitbucket, GitHub
- **Build:** CodeBuild, Jenkins, CloudBees, TeamCity
- **Test:** CodeBuild, AWS Device Farm, 3rd party tools, ...
- **Deploy:** CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3, ...
- Consists of stages:
  - Each stage can have sequential actions and/or parallel actions.
  - Example: Build -> Test -> Deploy -> Load Testing -> ...
  - Manual approval can be defined at any stage.

## 5.1. Artifacts

- Each pipeline stage can create artifacts.
- Artifacts stored in an S3 bucket and passed on to the next stage.

## 5.2. Troubleshooting

- For CodePipeline Pipeline/Action/Stage Execution State Changes.
- Use **CloudWatch Events (Amazon EventBridge)**.
- Example:
  - You can create events for failed pipelines.
  - You can create events for cancelled stages.
- If CodePipeline fails a stage, your pipeline stops, and you can get information in the console.
- If pipeline can't perform an action, make sure the "IAM Service Role" attached does have enough IAM permissions (IAM Policy).
- AWS CloudTrail can be used to audit AWS API calls.

## 5.3. Events vs.Webhooks vs. Polling

- Events
- Webhooks
- Polling

## 5.4. Action Types Constraints for

- **Owner**
  - **AWS:** For AWS services
  - **3rd Party:** GitHub or Alexa Skills Kit
  - **Custom:** Jenkins
- **Action Type**
  - **Source:** S3, ECR, GitHub, ...
  - **Build:** CodeBuild, Jenkins
  - **Test:** CodeBuild, Device Farm, Jenkins
  - **Approval:** Manual
  - **Invoke:** Lambda
  - **Deploy:** S3, CloudFormation, CodeDeploy, Elastic Beanstalk, OpsWorks, ECS, Service Catalog, ...

## 5.5. Manual Approval Stage

- **Important: Owner is "AWS", Action is "Manual".**

# 6. AWS CodeBuild

- A fully managed continuous integration (CI) service.
- Continuous scaling (no servers to manage or provision - no build queue).
- Compile source code, run tests, produce software packages, ...
- Alternative to other build tools (e.g., Jenkins).
- Charged per minute for compute resources (time it takes to complete the builds).
- Leverages Docker under the hood for reproducible builds.
- Use prepackaged Docker images or create your own custom Docker image.
- Security:
  - Integration with KMS for encryption of build artifacts.
  - IAM for CodeBuild permissions, and VPC for network security.
  - AWS CloudTrail for API calls logging.
- **Source:** CodeCommit, S3, Bitbucket, GitHub.
- **Build instructions:** Code file buildspec.yml or insert manually in Console.
- **Output logs** can be stored in Amazon S3 & CloudWatch Logs.
- Use CloudWatch Metrics to monitor build statistics.
- Use CloudWatch Events to detect failed builds and trigger notifications.
- Use CloudWatch Alarms to notify if you need "thresholds" for failures.
- **Build Projects can be defined within CodePipeline or CodeBuild.**

## 6.1. Supported Environments

- Java.
- Ruby.
- Python.
- Go.
- Node.js.
- Android.
- .NET Core.
- PHP.
- Docker - extend any environment you like.

## 6.2. buildspec.yml

- `buildspec.yml` file must be at the **root** of your code.
- `env` - define environment variables
  - `variables` - plaintext variables.
  - `parameter-store` - variables stored in SSM Parameter Store.
  - `secrets-manager` - variables stored in AWS Secrets Manager.
- `phases` - specify commands to run:
  - install - install dependencies you may need for your build.
  - pre_build - final commands to execute before build.
  - Build - actual build commands.
  - post_build - finishing touches (e.g., zip output).
- `artifacts` - what to upload to S3 (encrypted with KMS).
- `cache` - files to cache (usually dependencies) to S3 for future build speedup.

## 6.3. Local Build

- In case of need of deep troubleshooting beyond logs...
- You can run CodeBuild locally on your desktop (after installing Docker).
- For this, leverage the CodeBuild Agent.
- [Run builds locally with the AWS CodeBuild agent](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)

## 6.4. Inside VPC

- By default, your CodeBuild containers are launched outside your VPC.
  - It cannot access resources in a VPC.
- You can specify a VPC configuration:
  - VPC ID.
  - Subnet IDs.
  - Security Group IDs.
- Then your build can access resources in your VPC (e.g., RDS, ElastiCache, EC2, ALB, ...).
- Use cases: integration tests, data query, internal load balancers, ...

## 6.5. CloudFormation Integration

- CloudFormation is used to deploy complex infrastructure using an API
  - `CREATE_UPDATE` - create or update an existing stack
  - `DELETE_ONLY` - delete a stack if it exists

# 7. AWS CodeDeploy

- We want to deploy our application automatically to many EC2 instances.
- These EC2 instances are not managed by Elastic Beanstalk.
- There are several ways to handle deployments using open-source tools (Ansible, Terraform, Chef, Puppet, ...).
- We can use the managed service AWS CodeDeploy.

## 7.1. Steps To Make it Work

- **Each EC2 instance/on-premises server must be running the CodeDeploy Agent.**
- The agent is continuously polling AWS CodeDeploy for work to do.
- Application + `appspec.yml` is pulled from GitHub or S3.
- EC2 instances will run the deployment instructions in `appspec.yml`.
- CodeDeploy Agent will report of success/failure of the deployment.

## 7.2. Primary Components

- **Application:** A unique name functions as a container (revision, deployment configuration, ...)
- **Compute Platform:** EC2/On-Premises, AWS Lambda, or Amazon ECS
  - **Deployment Configuration:** A set of deployment rules for success/failure - EC2/On-premises - specify the minimum number of healthy instances for the deployment
  - **AWS Lambda or Amazon ECS:** specify how traffic is routed to your updated versions
- **Deployment Group:** Group of tagged EC2 instances (allows to deploy gradually, or dev, test, prod...)
- **Deployment Type:** Method used to deploy the application to a Deployment Group
  - **In-place Deployment:** Supports EC2/On-Premises
  - **Blue/Green Deployment:** Supports EC2 instances only, AWS Lambda, and Amazon ECS
- **IAM Instance Profile:** Give EC2 instances the permissions to access both S3 / GitHub
- **Application Revision:** Application code + `appspec.yml` file
- **Service Role:** An IAM Role for CodeDeploy to perform operations on EC2 instances, ASGs, ELBs...
- **Target Revision:** The most recent revision that you want to deploy to a Deployment Group

## 7.3. appspec.yml

- `files` - How to source and copy from S3 / GitHub to filesystem
  - `source`
  - `destination`
- `hooks` - Set of instructions to do to deploy the new version (hooks can have timeouts), the order is:
  - `ApplicationStop`
  - `DownloadBundle`
  - `BeforeInstall`
  - `Install`
  - `AfterInstall`
  - `ApplicationStart`
  - `ValidateService` <- important!!

## 7.4. Deployment Configuration

- Configurations:
  - **One At A Time:** One EC2 instance at a time, if one instance fails then deployment stops.
  - **Half At A Time:** 50%.
  - **All At Once:** Quick but no healthy host, downtime. Good for dev.
  - **Custom:** Min. healthy host = 75%.
- Failures:
  - EC2 instances stay in "Failed" state.
  - New deployments will first be deployed to failed instances.
  - **To rollback, redeploy old deployment or enable automated rollback for failures.**
- Deployment Groups:
  - A set of **tagged** EC2 instances.
  - Directly to an ASG.
  - Mix of ASG / Tags so you can build deployment segments.
  - Customization in scripts with `DEPLOYMENT_GROUP_NAME` environment variables.

## 7.5. Deployment to EC2

- Define **how to deploy the application** using `appspec.yml` + Deployment Strategy.
- Will do In-place update to your fleet of EC2 instances.
- Can use hooks to verify the deployment after each deployment phase.

## 7.6. Deploy to an ASG

- **In-place Deployment**
  - Updates existing EC2 instances.
  - Newly created EC2 instances by an ASG will also get automated deployments.
- **Blue/Green Deployment**
  - A new Auto-Scaling Group is created (settings are copied).
  - Choose how long to keep the old EC2 instances (old ASG).
  - Must be using an ELB.

## 7.7. Redeploy & Rollbacks

- Rollback = redeploy a previously deployed revision of your application.
- Deployments can be rolled back:
  - **Automatically:** Rollback when a deployment fails or rollback when a CloudWatch Alarm thresholds are met.
  - **Manually.**
- Disable Rollbacks - do not perform rollbacks for this deployment.
- **If a roll back happens, CodeDeploy redeploys the last known good revision as a new deployment (not a restored version).**

## 7.8. Troubleshooting

- Deployment Error: "InvalidSignatureException - Signature expired: [time] is now earlier than [time]"
  - For CodeDeploy to perform its operations, it requires accurate time references.
  - If the date and time on your EC2 instance are not set correctly, they might not match the signature date of your deployment request, which CodeDeploy rejects.
- Check log files to understand deployment issues:
  - For Amazon Linux, Ubuntu, and RHEL log files stored at `/opt/codedeploy-agent/deployment-root/deployment- logs/codedeploy-agent-deployments.log`.

# 8. AWS CodeStar

- An integrated solution that groups: GitHub, CodeCommit, CodeBuild, CodeDeploy, CloudFormation, CodePipeline, CloudWatch, ...
- Quickly create "CICD-ready" projects for EC2, Lambda, Elastic Beanstalk.
- Supported languages: C#, Go, HTML 5, Java, Node.js, PHP, Python, Ruby.
- Issue tracking integration with JIRA / GitHub Issues.
- Ability to integrate with Cloud9 to obtain a web IDE (not all regions).
- One dashboard to view all your components.
- Free service, pay only for the underlying usage of other services.
- Limited Customization.

# 9. AWS CodeArtifact

- Software packages depend on each other to be built (also called code dependencies), and new ones are created.
- Storing and retrieving these dependencies is called **artifact management**.
- Traditionally you need to setup your own artifact management system.
- **CodeArtifact** is a secure, scalable, and cost-effective **artifact management** for software development.
- Works with common dependency management tools such as Maven, Gradle, npm, yarn, twine, pip, and NuGet.
- **Developers and CodeBuild can then retrieve dependencies straight from CodeArtifact.**

## 9.1. Resource Policy

- Can be used to authorize another account to access CodeArtifact.
- A given principal can either read all the packages in a repository or none of them.

## 9.2. Upstream Repositories

- A CodeArtifact repository can have other CodeArtifact repositories as Upstream Repositories.
- Allows a package manager client to access the packages that are contained in more than one repository using a single repository endpoint.
- Up to 10 Upstream Repositories.
- Only one external connection.

## 9.3. External Connection

- An External Connection is a connection between a CodeArtifact Repository and an external/public repository (e.g., Maven, npm, PyPI, NuGet...).
- Allows you to fetch packages that are not already present in your CodeArtifact Repository.
- A repository has a maximum of 1 external connection.
- Create many repositories for many external connections.
- Example - Connect to npmjs.com:
  - Configure one CodeArtifact Repository in your domain with an external connection to npmjs.com.
  - Configure all the other repositories with an upstream to it.
  - Packages fetched from npmjs.com are cached in the Upstream Repository, rather than fetching and storing them in each Repository.

## 9.4. Retention

- If a requested package version is found in an Upstream Repository, a reference to it is retained and is always available from the Downstream Repository
- The retained package version is not affected by changes to the Upstream Repository (deleting it, updating the package, ...)
- Intermediate repositories do not keep the package
- Example - Fetching Package from npmjs.com
  - Package Manager connected to Repository A requests the package Lodash v4.17.20
  - The package version is not present in any of the three repositories
  - The package version will be fetched from npmjs.com
  - When Lodash 4.17.20 is fetched, it will be retained in:
    - Repository A - the most-downstream repository
    - Repository C - has the external connection to npmjs.com
    - The Package version will not be retained in Repository B as that is an intermediate Repository

## 9.5. Domains

- **Deduplicated Storage:** Asset only needs to be stored once in a domain, even if it's available in many repositories (only pay once for storage).
- **Fast Copying:** Only metadata record are updated when you pull packages from an Upstream CodeArtifact Repository into a Downstream.
- **Easy Sharing Across Repositories and Teams:** All the assets and metadata in a domain are encrypted with a single AWS KMS Key.
- **Apply Policy Across Multiple Repositories:** Domain administrator can apply policy across the domain such as:
  - Restricting which accounts have access to repositories in the domain.
  - Who can configure connections to public repositories to use as sources of packages.

# 10. Amazon CodeGuru

- An ML-powered service for **automated code reviews** and **application performance recommendations**.
- Provides two functionalities
  - **CodeGuru Reviewer:** Automated code reviews for static code analysis (development).
  - **CodeGuru Profiler:** Visibility/recommendations about application performance during runtime (production).

## 10.1. Reviewer

- Identify critical issues, security vulnerabilities, and hard-to-find bugs.
- Example: common coding best practices, resource leaks, security detection, input validation.
- Uses Machine Learning and automated reasoning.
- Hard-learned lessons across millions of code reviews on 1000s of open-source and Amazon repositories.
- Supports Java and Python.
- Integrates with GitHub, Bitbucket, and AWS CodeCommit.

## 10.2. Profiler

- Helps understand the runtime behavior of your application.
- Example: identify if your application is consuming excessive CPU capacity on a logging routine.
- Features:
  - Identify and remove code inefficiencies.
  - Improve application performance (e.g., reduce CPU utilization).
  - Decrease compute costs.
  - Provides heap summary (identify which objects using up memory).
  - Anomaly Detection.
- Support applications running on AWS or on-premise.
- Minimal overhead on application.

## 10.3. Agent Configuration

- `MaxStackDepth` - the maximum depth of the stacks in the code that is represented in the profile.
  - Example: if CodeGuru Profiler finds a method A, which calls method B, which calls method C, which calls method D, then the depth is 4.
  - If the MaxStackDepth is set to 2, then the profiler evaluates A and B.
- `MemoryUsageLimitPercent` - The memory percentage used by the profiler.
- `MinimumTimeForReportingInMilliseconds` - The minimum time between sending reports (milliseconds).
- `ReportingIntervalInMilliseconds` - The reporting interval used to report profiles (milliseconds).
- `SamplingIntervalInMilliseconds` - The sampling interval that is used to profile samples (milliseconds).
  - Reduce to have a higher sampling rate.

# 11. AWS Cloud9

- Cloud-based Integrated Development Environment (IDE).
- Code editor, debugger, terminal in a browser.
- Work on your projects from anywhere with an Internet connection.
- Prepackaged with essential tools for popular programming languages (JavaScript, Python, PHP, ...).
- Share your development environment with your team (pair programming).
- Fully integrated with AWS SAM & Lambda to easily build serverless applications.