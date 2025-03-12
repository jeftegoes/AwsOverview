# AWS CI/CD - CodeCommit, CodePipeline, CodeBuild, CodeDeploy, CodeStar, CodeArtifact, CodeGuru, Cloud9 <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. CICD in AWS](#1-cicd-in-aws)
- [2. Continuous Integration (CI)](#2-continuous-integration-ci)
- [3. Continuous Delivery (CD)](#3-continuous-delivery-cd)
- [4. AWS CodeCommit](#4-aws-codecommit)
  - [4.1. Security](#41-security)
  - [4.2. Migrations](#42-migrations)
  - [4.3. CodeCommit vs. GitHub](#43-codecommit-vs-github)
  - [4.4. Monitoring with EventBridge](#44-monitoring-with-eventbridge)
  - [4.5. Migrate a project](#45-migrate-a-project)
  - [4.6. Cross-Region Replication](#46-cross-region-replication)
  - [4.7. Branch Security](#47-branch-security)
  - [4.8. Pull Request Approval Rules](#48-pull-request-approval-rules)
- [5. AWS CodePipeline](#5-aws-codepipeline)
- [6. AWS CodeBuild](#6-aws-codebuild)
- [7. AWS CodeDeploy](#7-aws-codedeploy)
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
    - [10.2.1. Reviewer Secrets Detector](#1021-reviewer-secrets-detector)
    - [10.2.2. Ftunction decoractors](#1022-ftunction-decoractors)
  - [10.3. Agent Configuration](#103-agent-configuration)
- [11. AWS Cloud9](#11-aws-cloud9)
- [12. Sections erros](#12-sections-erros)

# 1. CICD in AWS

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
  - **HTTPS:**
    1. Set-up a **Git credential helper** using your access key credentials specified in your AWS credential profile.
    2. Generate HTTPS Git credentials for AWS CodeCommit (IAM).
       1. Specify the credentials in the Git Credential Manager.
- **Authorization**
  - IAM policies to manage users/roles permissions to repositories.
- **Encryption**
  - Repositories are automatically encrypted at rest using AWS KMS.
  - Encrypted in transit (can only use HTTPS or SSH - both secure).
- **Cross-account Access**
  - Do NOT share your SSH keys or your AWS credentials.
  - Use an IAM Role in your AWS account and use AWS STS (`AssumeRole` API).

## 4.2. Migrations

- You can migrate to CodeCommit from other version control systems, such:
  - Perforce.
  - Subversion.
  - TFS.
- **But you must first migrate to Git.**

## 4.3. CodeCommit vs. GitHub

|                                     | CodeCommit              | GitHub                                                            |
| ----------------------------------- | ----------------------- | ----------------------------------------------------------------- |
| Support Code Review (Pull Requests) | Ok                      | Ok                                                                |
| Integration with AWS CodeBuild      | Ok                      | Ok                                                                |
| Authentication (SSH & HTTPS)        | Ok                      | Ok                                                                |
| Security                            | IAM Users & Roles       | GitHub Users                                                      |
| Hosting                             | Managed & hosted by AWS | Hosted by GitHub / GitHub Enterprise: self hosted on your servers |
| UI                                  | Minimal                 | Fully Featured                                                    |

## 4.4. Monitoring with EventBridge

- You can monitor CodeCommit events in EventBridge (near real-time).
- `pullRequestCreated`, `pullRequestStatusChanged`, `referenceCreated`, `commentOnCommitCreated`.

## 4.5. Migrate a project

- To migrate a project hosted on another Git repository to CodeCommit, you have to follow the process, below:
  1. Complete the initial setup required for CodeCommit.
  2. Create a CodeCommit repository.
  3. Clone the repository and push it to CodeCommit.
  4. View files in the CodeCommit repository.
  5. Share the CodeCommit repository with your team.

![AWS CodeCommit migration](/Images/AWSCodeCommitMigration.png)

## 4.6. Cross-Region Replication

- **Use case:** Achieve lower latency pulls for global developers, backups...
  - `us-east-1` > `us-east-2`

## 4.7. Branch Security

- By default, a user who has push permissions to a CodeCommit repository can contribute to any branch.
- Use IAM policies to restrict users to push or merge code to a specific branch.
- Example: only senior developers can push to production branch.
- **Note: Resource Policy is not supported yet.**

## 4.8. Pull Request Approval Rules

- Helps ensure the quality of your code by requiring user(s) to approve the open PRs before the code can be merged.
- Specify a pool of users to approve and number of users who must approve the PR.
- Specify IAM Principal ARN (IAM users, federated users, IAM Roles, IAM Groups).
- Approval Rule Templates.
- Automatically apply Approval Rules to PRs in specific repositories.
- Example: define different rules for dev and prod branches.

# 5. AWS CodePipeline

[AWS CodePipeline](/Developer%20Tools/AWS%20CodePipeline.md)

# 6. AWS CodeBuild

[AWS CodeBuild](/Developer%20Tools/AWS%20CodeBuild.md)

# 7. AWS CodeDeploy

[AWS CodeDeploy](/Developer%20Tools/AWS%20CodeDeploy.md)

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

- A CodeArtifact repository can have other CodeArtifact repositories as **Upstream Repositories**.
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
  - Package Manager connected to Repository A requests the package **Lodash v4.17.20**
  - The package version is not present in any of the three repositories
  - The package version will be fetched from npmjs.com
  - When **Lodash 4.17.20** is fetched, it will be retained in:
    - **Repository A** - the most-downstream repository
    - **Repository C** - has the external connection to npmjs.com
    - The Package version will not be retained in Repository B as that is an intermediate Repository

## 9.5. Domains

- **Deduplicated Storage:** Asset only needs to be stored once in a domain, even if it's available in many repositories (only pay once for storage).
- **Fast Copying:** Only metadata record are updated when you pull packages from an Upstream CodeArtifact Repository into a Downstream.
- **Easy Sharing Across Repositories and Teams:** All the assets and metadata in a domain are encrypted with a single AWS KMS Key.
- **Apply Policy Across Multiple Repositories:** Domain administrator can apply policy across the domain such as:
  - Restricting which accounts have access to repositories in the domain.
  - Who can configure connections to public repositories to use as sources of packages.

# 10. Amazon CodeGuru

- **Amazon CodeGuru is a developer tool that provides intelligent recommendations to improve code quality and identify an application's most expensive lines of code.**
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

### 10.2.1. Reviewer Secrets Detector

- Uses ML to identify hardcoded secrets embedded in your code (e.g., passwords, API keys, credentials, SSH keys...).
- Besides scanning code, it scans configuration and documentation files.
- Suggests remediation to automatically protect your secrets with Secrets Manager.

### 10.2.2. Ftunction decoractors

- Integrate and apply CodeGuru Profiler to Lambda functions either using:
  - Function Decorator `@with_lambda_profiler`
  - Add codeguru_profiler_agent dependency to your **Lambda function .zip** file or use **Lambda Layers**.
- Enable Profiling in the Lambda function configuration.

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

# 12. Sections erros

- `DownloadBundle` deployment lifecycle event will throw an error whenever:
  - The EC2 instance's IAM profile does not have permission to access the application code in the Amazon S3.
  - An Amazon S3 internal error occurs.
  - The instances you deploy to are associated with one AWS Region (for example, US West Oregon), but the Amazon S3 bucket that contains the application revision is related to another AWS Region (for example, US East N. Virginia).
