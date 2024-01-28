# AWS CodeBuild<!-- omit in toc -->

# Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Details](#2-details)
- [3. Supported Environments](#3-supported-environments)
- [4. How it Works](#4-how-it-works)
- [5. buildspec.yml](#5-buildspecyml)
  - [5.1. Example buildspec](#51-example-buildspec)
  - [5.2. Phases](#52-phases)
  - [5.3. Custom buildspec file name and location](#53-custom-buildspec-file-name-and-location)
- [6. Local Build](#6-local-build)
- [7. Inside VPC](#7-inside-vpc)
- [8. Environment Variables](#8-environment-variables)
- [9. CloudFormation Integration](#9-cloudformation-integration)
- [10. Timeouts](#10-timeouts)
- [11. Build Badges](#11-build-badges)
- [12. Triggers](#12-triggers)
- [13. Validate Pull Requests](#13-validate-pull-requests)
- [14. Test Reports](#14-test-reports)
- [15. Security](#15-security)
  - [15.1. CodeBuild Service Role](#151-codebuild-service-role)

# 1. Introduction

- A fully managed continuous integration (CI) service.
- Continuous scaling (no servers to manage or provision - no build queue).
- Compile source code, run tests, produce software packages, ...
- Alternative to other build tools (e.g., Jenkins).
- Charged per minute for compute resources (time it takes to complete the builds).
- Leverages Docker under the hood for reproducible builds.
- Use prepackaged Docker images or create your own custom Docker image.

# 2. Details

- **Source:** CodeCommit, S3, Bitbucket, GitHub.
- **Build instructions:** Code file `buildspec.yml` or insert manually in Console.
- **Output logs** can be stored in Amazon S3 & CloudWatch Logs.
- Use CloudWatch Metrics to monitor build statistics.
- Use EventBridge (CloudWatch Events) to detect failed builds and trigger notifications.
- Use CloudWatch Alarms to notify if you need "thresholds" for failures.
- **Build Projects can be defined within CodePipeline or CodeBuild.**

# 3. Supported Environments

- Java.
- Ruby.
- Python.
- Go.
- Node.js.
- Android.
- .NET Core.
- PHP.
- Docker - extend any environment you like.

# 4. How it Works

![CodeBuild Diagram](/Images/CodeBuildDiagram.png)

# 5. buildspec.yml

- `buildspec.yml` - File must be at the **root** of your code.
- `env` - Define environment variables
  - `variables` - Plaintext variables.
  - `parameter-store` - Variables stored in SSM Parameter Store.
  - `secrets-manager` - Variables stored in AWS Secrets Manager.
- `phases` - Specify commands to run:
  - `install` - Install dependencies you may need for your build.
  - `pre_build` - Final commands to execute before build.
  - `build` - Actual build commands.
  - `post_build` - Finishing touches (e.g., zip output).
- `artifacts` - What to upload to S3 (encrypted with KMS).
- `cache` - Files to cache (usually dependencies) to S3 for future build speedup.

## 5.1. Example buildspec

- Lambda + Dotnet scenario:

  ```
      version: 0.2

      env:
      variables:
          DOTNET_ROOT: /root/.dotnet
      secrets-manager:
          AWS_ACCESS_KEY_ID_PARAM: MySecretManager:AWS_ACCESS_KEY_ID
          AWS_SECRET_ACCESS_KEY_PARAM: MySecretManager:AWS_SECRET_ACCESS_KEY

      phases:
      install:
          runtime-versions:
          dotnet: 6.0
      pre_build:
          commands:
          - echo Restore started on `date`
          - export PATH="$PATH:/root/.dotnet/tools"
          - pip install --upgrade awscli
          - aws configure set profile $Profile
          - aws configure set region $Region
          - aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID_PARAM
          - aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY_PARAM
          - cd src
          - cd InsertBook
          - dotnet clean
          - dotnet restore
      build:
          commands:
          - echo Build started on `date`
          - dotnet new -i Amazon.Lambda.Templates::*
          - dotnet tool install -g Amazon.Lambda.Tools
          - dotnet tool update -g Amazon.Lambda.Tools
          - dotnet lambda deploy-function "InsertBook"
  ```

## 5.2. Phases

![AWS CodeBuild phases](/Images/AWSCodeBuildPhases.png)

## 5.3. Custom buildspec file name and location

- To override the default buildspec file name, location, or both, do one of the following:

1. Run the AWS CLI `create-project` or `update-project` command, setting the buildspec value to the path to the alternate buildspec file relative to the value of the built-in environment variable `CODEBUILD_SRC_DIR`.
2. Run the AWS CLI `start-build` command, setting the `buildspecOverride` value to the path to the alternate buildspec file relative to the value of the built-in environment variable `CODEBUILD_SRC_DIR`.
3. In an AWS CloudFormation template, set the BuildSpec property of Source in a resource of type `AWS::CodeBuild::Project` to the path to the alternate buildspec file relative to the value of the built-in environment variable `CODEBUILD_SRC_DIR`.

# 6. Local Build

- In case of need of deep troubleshooting beyond logs...
- You can run CodeBuild locally on your desktop (after installing Docker).
- For this, leverage the CodeBuild Agent.
- [Run builds locally with the AWS CodeBuild agent](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)

# 7. Inside VPC

- By default, your CodeBuild containers are launched outside your VPC.
  - It cannot access resources in a VPC.
- You can specify a VPC configuration:
  - VPC ID.
  - Subnet IDs.
  - Security Group IDs.
- Then your build can access resources in your VPC (e.g., RDS, ElastiCache, EC2, ALB, ...).
- Use cases: integration tests, data query, internal load balancers, ...

# 8. Environment Variables

- **Default Environment Variables**
  - Defined and provided by AWS.
  - AWS_DEFAULT_REGION, CODEBUILD_BUILD_ARN, CODEBUILD_BUILD_ID, CODEBUILD_BUILD_IMAGE...
- **Custom Environment Variables**
  - **Static:** Defined at build time (override using start-build API call).
  - **Dynamic:** Using SSM Parameter Store and Secrets Manager.

# 9. CloudFormation Integration

- CloudFormation is used to deploy complex infrastructure using an API:
  - `CREATE_UPDATE` - create or update an existing stack.
  - `DELETE_ONLY` - delete a stack if it exists.

# 10. Timeouts

- A build represents a set of actions performed by AWS CodeBuild to create output artifacts (for example, a JAR file) based on a set of input artifacts (for example, a collection of Java class files).
- A build in a queue that does not start after the number of minutes specified in its time out value is removed from the queue.
  - **The default timeout value is eight hours.**
  - You can override the build queue timeout with a value **between five minutes and eight hours** when you run your build.
- By setting the timeout configuration, the build process will automatically terminate post the expiry of the configured timeout.

# 11. Build Badges

- Dynamically generated badge that displays the status of the latest build.
- Can be accessed through a public URL for your CodeBuild project.
- Supported for **CodeCommit, GitHub, and BitBucket**.
- **Note: Badges are available at the branch level.**

# 12. Triggers

- CodeCommit -> `event` -> EventBridge -> `trigger` -> CodeBuild.
- CodeCommit -> `event` -> EventBridge -> `trigger` -> Lambda -> `trigger` -> CodeBuild.
- GitHub -> `trigger` -> Web Hook -> `trigger` -> CodeBuild.

# 13. Validate Pull Requests

- Validate proposed code changes in PRs before they get merged.
- Ensure high level of code quality and avoid code conflicts.

# 14. Test Reports

- Contains details about tests that are run during builds.
- **Unit tests, configuration tests, functional tests**.
- Create your test cases with any test framework that can create report files in the following format:
  - JUnit XML, NUnit XML, NUnit3 XML.
  - Cucumber JSON, TestNG XML, Visual Studio TRX.
- Create a test report and add a **Report Group** name in **buildspec.yml** file with information about your tests.

# 15. Security

- Integration with KMS for encryption of build artifacts.
- IAM for CodeBuild permissions, and VPC for network security.
- AWS CloudTrail for API calls logging.

## 15.1. CodeBuild Service Role

- Allows CodeBuild to access AWS resources on your behalf (assign the required permissions).
- Use cases:
  - Download code from CodeCommit repository.
  - Fetch parameters from SSM Parameter Store.
  - Upload build artifacts to S3 bucket.
  - Fetch secrets from Secrets Manager.
  - Store logs in CloudWatch Logs.
- In-transit and at-rest data encryption (cache, logs...).
- Build output artifact encryption (requires access to KMS).
