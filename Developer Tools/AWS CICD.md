# AWS CI/CD - CodeCommit, CodePipeline, CodeBuild, CodeDeploy, CodeStar, CodeArtifact, CodeGuru, Cloud9 <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. CICD in AWS](#1-cicd-in-aws)
- [2. Continuous Integration (CI)](#2-continuous-integration-ci)
- [3. Continuous Delivery (CD)](#3-continuous-delivery-cd)
- [4. AWS CodePipeline](#4-aws-codepipeline)
- [5. AWS CodeBuild](#5-aws-codebuild)
- [6. AWS CodeDeploy](#6-aws-codedeploy)
- [7. AWS CodeStar](#7-aws-codestar)
- [8. AWS CodeArtifact](#8-aws-codeartifact)
  - [8.1. Resource Policy](#81-resource-policy)
  - [8.2. Upstream Repositories](#82-upstream-repositories)
  - [8.3. External Connection](#83-external-connection)
  - [8.4. Retention](#84-retention)
  - [8.5. Domains](#85-domains)
- [9. Amazon CodeGuru](#9-amazon-codeguru)
  - [9.1. Reviewer](#91-reviewer)
  - [9.2. Profiler](#92-profiler)
    - [9.2.1. Reviewer Secrets Detector](#921-reviewer-secrets-detector)
    - [9.2.2. Ftunction decoractors](#922-ftunction-decoractors)
  - [9.3. Agent Configuration](#93-agent-configuration)
- [10. AWS Cloud9](#10-aws-cloud9)
- [11. Sections erros](#11-sections-erros)

# 1. CICD in AWS

- This is all about automating the deployment, while adding increased safety:
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

# 4. AWS CodePipeline

[AWS CodePipeline](/Developer%20Tools/AWS%20CodePipeline.md)

# 5. AWS CodeBuild

[AWS CodeBuild](/Developer%20Tools/AWS%20CodeBuild.md)

# 6. AWS CodeDeploy

[AWS CodeDeploy](/Developer%20Tools/AWS%20CodeDeploy.md)

# 7. AWS CodeStar

- An integrated solution that groups: GitHub, CodeCommit, CodeBuild, CodeDeploy, CloudFormation, CodePipeline, CloudWatch, ...
- Quickly create "CICD-ready" projects for EC2, Lambda, Elastic Beanstalk.
- Supported languages: C#, Go, HTML 5, Java, Node.js, PHP, Python, Ruby.
- Issue tracking integration with JIRA / GitHub Issues.
- Ability to integrate with Cloud9 to obtain a web IDE (not all regions).
- One dashboard to view all your components.
- Free service, pay only for the underlying usage of other services.
- Limited Customization.

# 8. AWS CodeArtifact

- Software packages depend on each other to be built (also called code dependencies), and new ones are created.
- Storing and retrieving these dependencies is called **artifact management**.
- Traditionally you need to setup your own artifact management system.
- **CodeArtifact** is a secure, scalable, and cost-effective **artifact management** for software development.
- Works with common dependency management tools such as Maven, Gradle, npm, yarn, twine, pip, and NuGet.
- **Developers and CodeBuild can then retrieve dependencies straight from CodeArtifact.**

## 8.1. Resource Policy

- Can be used to authorize another account to access CodeArtifact.
- A given principal can either read all the packages in a repository or none of them.

## 8.2. Upstream Repositories

- A CodeArtifact repository can have other CodeArtifact repositories as **Upstream Repositories**.
- Allows a package manager client to access the packages that are contained in more than one repository using a single repository endpoint.
- Up to 10 Upstream Repositories.
- Only one external connection.

## 8.3. External Connection

- An External Connection is a connection between a CodeArtifact Repository and an external/public repository (e.g., Maven, npm, PyPI, NuGet...).
- Allows you to fetch packages that are not already present in your CodeArtifact Repository.
- A repository has a maximum of 1 external connection.
- Create many repositories for many external connections.
- Example - Connect to npmjs.com:
  - Configure one CodeArtifact Repository in your domain with an external connection to npmjs.com.
  - Configure all the other repositories with an upstream to it.
  - Packages fetched from npmjs.com are cached in the Upstream Repository, rather than fetching and storing them in each Repository.

## 8.4. Retention

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

## 8.5. Domains

- **Deduplicated Storage:** Asset only needs to be stored once in a domain, even if it's available in many repositories (only pay once for storage).
- **Fast Copying:** Only metadata record are updated when you pull packages from an Upstream CodeArtifact Repository into a Downstream.
- **Easy Sharing Across Repositories and Teams:** All the assets and metadata in a domain are encrypted with a single AWS KMS Key.
- **Apply Policy Across Multiple Repositories:** Domain administrator can apply policy across the domain such as:
  - Restricting which accounts have access to repositories in the domain.
  - Who can configure connections to public repositories to use as sources of packages.

# 9. Amazon CodeGuru

- **Amazon CodeGuru is a developer tool that provides intelligent recommendations to improve code quality and identify an application's most expensive lines of code.**
- An ML-powered service for **automated code reviews** and **application performance recommendations**.
- Provides two functionalities
  - **CodeGuru Reviewer:** Automated code reviews for static code analysis (development).
  - **CodeGuru Profiler:** Visibility/recommendations about application performance during runtime (production).

## 9.1. Reviewer

- Identify critical issues, security vulnerabilities, and hard-to-find bugs.
- Example: common coding best practices, resource leaks, security detection, input validation.
- Uses Machine Learning and automated reasoning.
- Hard-learned lessons across millions of code reviews on 1000s of open-source and Amazon repositories.
- Supports Java and Python.
- Integrates with GitHub, Bitbucket, and AWS CodeCommit.

## 9.2. Profiler

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

### 9.2.1. Reviewer Secrets Detector

- Uses ML to identify hardcoded secrets embedded in your code (e.g., passwords, API keys, credentials, SSH keys...).
- Besides scanning code, it scans configuration and documentation files.
- Suggests remediation to automatically protect your secrets with Secrets Manager.

### 9.2.2. Ftunction decoractors

- Integrate and apply CodeGuru Profiler to Lambda functions either using:
  - Function Decorator `@with_lambda_profiler`
  - Add codeguru_profiler_agent dependency to your **Lambda function .zip** file or use **Lambda Layers**.
- Enable Profiling in the Lambda function configuration.

## 9.3. Agent Configuration

- `MaxStackDepth` - the maximum depth of the stacks in the code that is represented in the profile.
  - Example: if CodeGuru Profiler finds a method A, which calls method B, which calls method C, which calls method D, then the depth is 4.
  - If the MaxStackDepth is set to 2, then the profiler evaluates A and B.
- `MemoryUsageLimitPercent` - The memory percentage used by the profiler.
- `MinimumTimeForReportingInMilliseconds` - The minimum time between sending reports (milliseconds).
- `ReportingIntervalInMilliseconds` - The reporting interval used to report profiles (milliseconds).
- `SamplingIntervalInMilliseconds` - The sampling interval that is used to profile samples (milliseconds).
  - Reduce to have a higher sampling rate.

# 10. AWS Cloud9

- Cloud-based Integrated Development Environment (IDE).
- Code editor, debugger, terminal in a browser.
- Work on your projects from anywhere with an Internet connection.
- Prepackaged with essential tools for popular programming languages (JavaScript, Python, PHP, ...).
- Share your development environment with your team (pair programming).
- Fully integrated with AWS SAM & Lambda to easily build serverless applications.

# 11. Sections erros

- `DownloadBundle` deployment lifecycle event will throw an error whenever:
  - The EC2 instance's IAM profile does not have permission to access the application code in the Amazon S3.
  - An Amazon S3 internal error occurs.
  - The instances you deploy to are associated with one AWS Region (for example, US West Oregon), but the Amazon S3 bucket that contains the application revision is related to another AWS Region (for example, US East N. Virginia).
