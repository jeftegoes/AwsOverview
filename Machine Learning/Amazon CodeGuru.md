# Amazon CodeGuru <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Reviewer](#2-reviewer)
- [3. Profiler](#3-profiler)
  - [3.1. Reviewer Secrets Detector](#31-reviewer-secrets-detector)
  - [3.2. Function Decorator](#32-function-decorator)
- [4. Agent Configuration](#4-agent-configuration)

# 1. Introduction

- **Amazon CodeGuru is a developer tool that provides intelligent recommendations to improve code quality and identify an application's most expensive lines of code.**
- An ML-powered service for **automated code reviews** and **application performance recommendations**.
- **Provides two functionalities**
  - **CodeGuru Reviewer:** Automated code reviews for static code analysis (development).
  - **CodeGuru Profiler:** Visibility/recommendations about application performance during runtime (production).
    ![Amazon CodeGuru](/Images/Machine%20Learning/AmazonCodeGuru.png)

# 2. Reviewer

- Identify critical issues, security vulnerabilities, and hard-to-find bugs.
- **Example:** Common coding best practices, resource leaks, security detection, input validation.
- Uses Machine Learning and automated reasoning.
- Hard-learned lessons across millions of code reviews on 1000s of open-source and Amazon repositories.
- Supports Java and Python.
- Integrates with GitHub, Bitbucket, and AWS CodeCommit.

# 3. Profiler

- Helps understand the runtime behavior of your application.
- **Example:** Identify if your application is consuming excessive CPU capacity on a logging routine.
- **Features**
  - Identify and remove code inefficiencies.
  - Improve application performance (e.g., reduce CPU utilization).
  - Decrease compute costs.
  - Provides heap summary (identify which objects using up memory).
  - Anomaly Detection.
- Support applications running on AWS or on-premise.
- Minimal overhead on application.

## 3.1. Reviewer Secrets Detector

- Uses ML to identify hardcoded secrets embedded in your code (e.g., passwords, API keys, credentials, SSH keys...).
- Besides scanning code, it scans configuration and documentation files.
- **Suggests remediation to automatically protect your secrets with Secrets Manager.**
  ![Amazon CodeGuru - Reviewer](/Images/Machine%20Learning/AmazonCodeGuruReviewer.png)

## 3.2. Function Decorator

- Integrate and apply CodeGuru Profiler to Lambda functions either using:
  - Function Decorator `@with_lambda_profiler`
  - Add `codeguru_profiler_agent` dependency to your **Lambda function .zip** file or use **Lambda Layers**.
    ```python
      from codeguru_profiler_agent import with_lambda_profiler

      @with_lambda_profiler(profiling_group_name="MyGroupName")
      def handler_name(event, context):
        return "Hello World"
    ```
- Enable Profiling in the Lambda function configuration.
- **It automatically**
  - Starts profiling when the Lambda function begins.
  - Collects performance data during execution.
  - Sends data to CodeGuru Profiler.
  - Stops profiling when execution finishes.

# 4. Agent Configuration

- `MaxStackDepth` - The maximum depth of the stacks in the code that is represented in the profile.
  - **Example:** If CodeGuru Profiler finds a method A, which calls method B, which calls method C, which calls method D, then the depth is 4.
  - If the MaxStackDepth is set to 2, then the profiler evaluates A and B.
- `MemoryUsageLimitPercent` - The memory percentage used by the profiler.
- `MinimumTimeForReportingInMilliseconds` - The minimum time between sending reports (milliseconds).
- `ReportingIntervalInMilliseconds` - The reporting interval used to report profiles (milliseconds).
- `SamplingIntervalInMilliseconds` - The sampling interval that is used to profile samples (milliseconds).
  - Reduce to have a higher sampling rate.
