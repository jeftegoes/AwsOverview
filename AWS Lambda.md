# AWS Lambda<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What's serverless?](#1-whats-serverless)
  - [1.1. Serverless in AWS](#11-serverless-in-aws)
- [2. Why AWS Lambda?](#2-why-aws-lambda)
- [3. Benefits of AWS Lambda](#3-benefits-of-aws-lambda)
- [4. Language support](#4-language-support)
- [5. Function resource names](#5-function-resource-names)
- [6. Pricing: example](#6-pricing-example)
- [7. Lifecycle execution](#7-lifecycle-execution)
- [8. Synchronous Invocations](#8-synchronous-invocations)
  - [8.1. Synchronous Invocations - Services](#81-synchronous-invocations---services)
  - [8.2. Lambda Integration with ALB](#82-lambda-integration-with-alb)
    - [8.2.1. ALB to Lambda: HTTP to JSON](#821-alb-to-lambda-http-to-json)
    - [8.2.2. ALB Multi-Header Values](#822-alb-multi-header-values)
  - [8.3. Lambda@Edge](#83-lambdaedge)
    - [8.3.1. Lambda@Edge: Global application](#831-lambdaedge-global-application)
    - [8.3.2. Lambda@Edge: Use Cases](#832-lambdaedge-use-cases)
- [9. Asynchronous Invocations](#9-asynchronous-invocations)
  - [9.1. Asynchronous Invocations - Services](#91-asynchronous-invocations---services)
  - [9.2. CloudWatch Events / EventBridge](#92-cloudwatch-events--eventbridge)
  - [9.3. S3 Events Notifications](#93-s3-events-notifications)
    - [9.3.1. Simple S3 Event Pattern - Metadata Sync](#931-simple-s3-event-pattern---metadata-sync)
- [10. Lambda - Event Source Mapping](#10-lambda---event-source-mapping)
  - [10.1. Streams \& Lambda (Kinesis \& DynamoDB)](#101-streams--lambda-kinesis--dynamodb)
  - [10.2. Streams \& Lambda - Error Handling](#102-streams--lambda---error-handling)
  - [10.3. Lambda - Event Source Mapping SQS \& SQS FIFO](#103-lambda---event-source-mapping-sqs--sqs-fifo)
  - [10.4. Queues \& Lambda](#104-queues--lambda)
  - [10.5. Lambda Event Mapper Scaling](#105-lambda-event-mapper-scaling)
- [11. Event and Context Objects](#11-event-and-context-objects)
- [12. Lambda - Destinations](#12-lambda---destinations)
- [13. Lambda Execution Role (IAM Role)](#13-lambda-execution-role-iam-role)
  - [13.1. Lambda Resource Based Policies](#131-lambda-resource-based-policies)
- [14. Lambda Environment Variables](#14-lambda-environment-variables)
- [15. Lambda Logging \& Monitoring](#15-lambda-logging--monitoring)
  - [15.1. Lambda Tracing with X-Ray](#151-lambda-tracing-with-x-ray)
- [16. Customization At The Edge](#16-customization-at-the-edge)
  - [16.1. CloudFront Functions \& Lambda@Edge - Use Cases](#161-cloudfront-functions--lambdaedge---use-cases)
  - [16.2. CloudFront Functions](#162-cloudfront-functions)
  - [16.3. Lambda@Edge](#163-lambdaedge)
  - [16.4. CloudFront Functions vs. Lambda@Edge](#164-cloudfront-functions-vs-lambdaedge)
  - [16.5. CloudFront Functions vs. Lambda@Edge - Use Cases](#165-cloudfront-functions-vs-lambdaedge---use-cases)
- [17. Lambda in VPC](#17-lambda-in-vpc)
  - [17.1. Lambda in VPC - Internet Access](#171-lambda-in-vpc---internet-access)
- [18. Lambda Function Configuration](#18-lambda-function-configuration)
  - [18.1. Lambda Execution Context](#181-lambda-execution-context)
  - [18.2. Lambda Functions /tmp space](#182-lambda-functions-tmp-space)
- [19. File Systems Mounting](#19-file-systems-mounting)
- [20. Lambda Concurrency and Throttling](#20-lambda-concurrency-and-throttling)
  - [20.1. Lambda Concurrency Issue](#201-lambda-concurrency-issue)
  - [20.2. Concurrency and Asynchronous Invocations](#202-concurrency-and-asynchronous-invocations)
  - [20.3. Cold Starts \& Provisioned Concurrency](#203-cold-starts--provisioned-concurrency)
  - [20.4. Reserved and Provisioned Concurrency](#204-reserved-and-provisioned-concurrency)
- [21. Lambda Function Dependencies](#21-lambda-function-dependencies)
- [22. Lambda and CloudFormation](#22-lambda-and-cloudformation)
  - [22.1. Inline](#221-inline)
  - [22.2. Through S3](#222-through-s3)
    - [22.2.1. through S3 Multiple accounts](#2221-through-s3-multiple-accounts)
- [23. Lambda Layers](#23-lambda-layers)
- [24. Lambda Container Images](#24-lambda-container-images)
  - [24.1. Lambda Container Images](#241-lambda-container-images)
- [25. AWS Lambda Versions](#25-aws-lambda-versions)
- [26. AWS Lambda Aliases](#26-aws-lambda-aliases)
- [27. Lambda \& CodeDeploy](#27-lambda--codedeploy)
- [28. Function URL](#28-function-url)
  - [28.1. Function URL Security](#281-function-url-security)
- [29. Lambda and CodeGuru Profiling](#29-lambda-and-codeguru-profiling)
- [30. AWS Lambda Limits to Know - per region](#30-aws-lambda-limits-to-know---per-region)
- [31. AWS Lambda Best Practices](#31-aws-lambda-best-practices)

# 1. What's serverless?

- Serverless is a new paradigm in which the developers don't have to manage servers anymore.
- They just deploy code / functions.
- Initially... Serverless == FaaS (Function as a Service).
- Serverless was pioneered by AWS Lambda but now also includes anything that's managed: "databases, messaging, storage, etc."
- Serverless does not mean there are no servers... it means you just don't manage / provision / see them.

## 1.1. Serverless in AWS

- AWS Lambda
- DynamoDB
- AWS Cognito
- AWS API Gateway
- Amazon S3
- AWS SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate

# 2. Why AWS Lambda?

- Amazon EC2:
  - Virtual Servers in the Cloud.
  - Limited by RAM and CPU.
  - Continuously running.
  - Scaling means intervention to add / remove servers.
- Amazon Lambda:
  - Virtual **functions** - no servers to manage!
  - Limited by time - **short executions**.
  - Run **on-demand**.
  - **Scaling is automated!**

# 3. Benefits of AWS Lambda

- Easy Pricing:
  - Pay per request and compute time.
  - Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time.
- Integrated with the whole AWS suite of services.
- **Event-Driven**: functions get invoked by AWS when needed.
- Integrated with many programming languages.
- Easy monitoring through AWS CloudWatch.
- Easy to get more resources per functions (up to 10GB of RAM!).
- Increasing RAM will also improve CPU and network!

# 4. Language support

- Node.js (JavaScript)
- Python
- Java (Java 8 compatible)
- C# (.NET Core)
- Golang
- C# / Powershell
- Ruby
- Custom Runtime API (community supported, example Rust)
- Lambda Container Image
  - The container image must implement the Lambda Runtime API
  - ECS / Fargate is preferred for running arbitrary Docker images

# 5. Function resource names

- You reference a Lambda function in a policy statement using an **Amazon Resource Name (ARN)**.
- The format of a function ARN depends on whether you are referencing the whole **function (unqualified)** or a **function version or alias (qualified)**.
  - **Qualified**
    - `arn:aws:lambda:us-west-2:123456789012:function:myFunction:1`
    - `arn:aws:lambda:us-west-2:123456789012:function:myFunction:*`
  - **Unqualified**
    - `arn:aws:lambda:us-west-2:123456789012:function:myFunction` **$LATEST**
    - `arn:aws:lambda:us-west-2:123456789012:function:myFunction*`

# 6. Pricing: example

- You can find overall pricing information here: https://aws.amazon.com/lambda/pricing/
- Pay per **calls**:
  - First 1,000,000 requests are free.
  - $0.20 per 1 million requests thereafter ($0.0000002 per request).
- Pay per **duration**: (in increment of 1 ms)
  - 400,000 GB-seconds of compute time per month for FREE.
  - == 400,000 seconds if function is 1GB RAM.
  - == 3,200,000 seconds if function is 128 MB RAM.
  - After that $1.00 for 600,000 GB-seconds.
- It is usually **very cheap** to run AWS Lambda so it's **very popular**.

# 7. Lifecycle execution

![Lifecycle execution](Images/LambdaLifecycle.png)

# 8. Synchronous Invocations

- Synchronous:
  - CLI.
  - SDK.
  - API Gateway.
  - Application Load Balancer.
- Results is returned right away.
- Error handling must happen client side (retries, [exponential backoff](AWS%20Limits.md), etc...).

![Lambda Synchronous Invocations](/Images/LambdaSynchronousInvocations.png)

## 8.1. Synchronous Invocations - Services

- User Invoked:
  - Elastic Load Balancing (Application Load Balancer).
  - Amazon API Gateway.
  - Amazon CloudFront (Lambda@Edge).
  - Amazon S3 Batch.
- Service Invoked:
  - DynamoDB > Streams [DynamoDB Streams and AWS Lambda](AWS%20DynamoDB.md)
  - Amazon Cognito.
  - AWS Step Functions.
- Other Services:
  - Amazon Lex.
  - Amazon Alexa.
  - Amazon Kinesis Data Firehose.

## 8.2. Lambda Integration with ALB

- To expose a Lambda function as an HTTP(S) endpoint...
- You can use the Application Load Balancer (or an API Gateway).
- The Lambda function must be registered in a target group.

![Lambda integration with ALB](Images/LambdaIntegrationAlb.png)

### 8.2.1. ALB to Lambda: HTTP to JSON

- Basic example:

  ```
  {
  "path": "api/address/44077200",
  "httpMethod": "GET"
  }
  ```

### 8.2.2. ALB Multi-Header Values

- ALB can support multi header values (ALB setting).
- When you enable multi-value headers, HTTP headers and query string parameters that are sent with multiple values are shown as arrays within the AWS Lambda event and response objects.

![Lambda ALB Multi-Header values](/Images/LambdaALBMultiHeaderValues.png)

## 8.3. Lambda@Edge

- You have deployed a CDN using CloudFront.
  - What if you wanted to run a global AWS Lambda alongside?
  - Or how to implement request filtering before reaching your application?
- For this, you can use Lambda@Edge:
  - Deploy Lambda functions alongside your CloudFront CDN:
    - Build more responsive applications.
    - You don't manage servers, Lambda is deployed globally.
    - Customize the CDN content.
    - Pay only for what you use.
- You can use Lambda to change CloudFront requests and responses:
  - After CloudFront receives a request from a viewer (viewer request).
  - Before CloudFront forwards the request to the origin (origin request).
  - After CloudFront receives the response from the origin (origin response).
  - Before CloudFront forwards the response to the viewer (viewer response).
- You can also generate responses to viewers without ever sending the request to the origin.

### 8.3.1. Lambda@Edge: Global application

![Lambda@Edge Global Application](/Images/Lambda@EdgeGlobalApplication.png)

### 8.3.2. Lambda@Edge: Use Cases

- Website Security and Privacy.
- Dynamic Web Application at the Edge.
- Search Engine Optimization (SEO).
- Intelligently Route Across Origins and Data Centers.
- Bot Mitigation at the Edge.
- Real-time Image Transformation.
- A/B Testing.
- User Authentication and Authorization.
- User Prioritization.
- User Tracking and Analytics.

# 9. Asynchronous Invocations

- S3, SNS, CloudWatch Events...
- The events are placed in an **Event Queue**.
- Lambda attempts to retry on errors:
  - 3 tries total.
  - 1 minute wait after 1st , then 2 minutes wait.
- Make sure the processing is **idempotent** (in case of retries).
- If the function is retried, you will see **duplicate logs entries in CloudWatch Logs**.
- Can define a DLQ (dead-letter queue) - **SNS or SQS** - for failed processing (need correct IAM permissions).
- Asynchronous invocations allow you to speed up the processing if you don't need to wait for the result (ex: you need 1000 files processed).

![Lambda Asynchronous Invocations](/Images/LambdaAsynchronousInvocations1.png)

![Lambda Asynchronous Invocations](/Images/LambdaAsynchronousInvocations2.png)

## 9.1. Asynchronous Invocations - Services

- Amazon Simple Storage Service (S3).
- Amazon Simple Notification Service (SNS).
- Amazon CloudWatch Events / EventBridge.
- AWS CodeCommit (CodeCommitTrigger: new branch, new tag, new push).
- AWS CodePipeline (invoke a Lambda function during the pipeline, Lambda must callback).
- Other:
  - Amazon CloudWatch Logs (log processing).
  - Amazon Simple Email Service.
  - AWS CloudFormation.
  - AWS Config.
  - AWS IoT.
  - AWS IoT Events.

## 9.2. CloudWatch Events / EventBridge

![CloudWatch Events / EventBridge](Images/CloudWatchEventsEventBridge.png)

## 9.3. S3 Events Notifications

- S3 events:
  - S3:ObjectCreated
  - S3:ObjectRemoved
  - S3:ObjectRestore
  - S3:Replication...
- Object name filtering possible (\*.jpg).
- Use case: generate thumbnails of images uploaded to S3.
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer.
- If two writes are made to a single non- versioned object at the same time, it is possible that only a single event notification will be sent
- If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket.

![S3 Events Notifications](/Images/S3EventsNotifications.png)

### 9.3.1. Simple S3 Event Pattern - Metadata Sync

# 10. Lambda - Event Source Mapping

- Kinesis Data Streams.
- SQS & SQS FIFO queue.
- DynamoDB Streams.
- Common denominator: records need to be polled from the source.
- Your Lambda function is invoked synchronously.

## 10.1. Streams & Lambda (Kinesis & DynamoDB)

- An event source mapping creates an iterator for each shard, processes items in order.
- Start with new items, from the beginning or from timestamp.
- Processed items aren't removed from the stream (other consumers can read them).
- Low traffic: use batch window to accumulate records before processing.
- You can process multiple batches in parallel.
  - Up to 10 batches per shard
  - In-order processing is still guaranteed for each partition key,

![Lambda Streams Kinesis and DynamoDb](/Images/LambdaStreamsKinesisAndDynamoDb.png)

- https://aws.amazon.com/blogs/compute/new-aws-lambda-scaling-controls-for-kinesis-and-dynamodb-event-sources/

## 10.2. Streams & Lambda - Error Handling

- By default, if your function returns an error, the entire batch is reprocessed until the function succeeds, or the items in the batch expire.
- To ensure in-order processing, processing for the affected shard is paused until the error is resolved.
- You can configure the event source mapping to:
  - Discard old events.
  - Restrict the number of retries.
  - Split the batch on error (to work around Lambda timeout issues).
- Discarded events can go to a **Destination**.

## 10.3. Lambda - Event Source Mapping SQS & SQS FIFO

- Event Source Mapping will poll SQS (**Long Polling**).
- Specify batch size (1-10 messages).
- Recommended: Set the queue visibility timeout to 6x the timeout of your Lambda function.
- To use a DLQ:
  - Set-up on the SQS queue, not Lambda (DLQ for Lambda is only for async invocations).
  - Or use a Lambda destination for failures.

## 10.4. Queues & Lambda

- Lambda also supports in-order processing for FIFO (first-in, first-out) queues, scaling up to the number of active message groups.
- For standard queues, items aren't necessarily processed in order.
- Lambda scales up to process a standard queue as quickly as possible.
- When an error occurs, batches are returned to the queue as individual items and might be processed in a different grouping than the original batch.
- Occasionally, the event source mapping might receive the same item from the queue twice, even if no function error occurred.
- Lambda deletes items from the queue after they're processed successfully.
- You can configure the source queue to send items to a dead-letter queue if they can't be processed.

## 10.5. Lambda Event Mapper Scaling

- **Kinesis Data Streams & DynamoDB Streams:**
  - One Lambda invocation per stream shard
  - If you use parallelization, up to 10 batches processed per shard simultaneously
- **SQS Standard:**
  - Lambda adds 60 more instances per minute to scale up
  - Up to 1000 batches of messages processed simultaneously
- **SQS FIFO:**
  - Messages with the same GroupID will be processed in order
  - The Lambda function scales to the number of active message groups

# 11. Event and Context Objects

- **Event Object**
  - JSON-formatted document contains data for the function to process.
  - Contains information from the invoking service (e.g., EventBridge, custom, ...).
  - Lambda runtime converts the event to an object (e.g., dict type in Python).
  - Example: input arguments, invoking service arguments, ...
- **Context Object**
  - Provides methods and properties that provide information about the invocation, function, and runtime environment.
  - Passed to your function by Lambda at runtime.
  - Example: aws_request_id, function_name, memory_limit_in_mb, ...

# 12. Lambda - Destinations

- Nov 2019: Can configure to send result to a **destination**.
- Asynchronous invocations - can define destinations for successful and failed event:
  - Amazon SQS.
  - Amazon SNS.
  - AWS Lambda.
  - Amazon EventBridge bus.
- Note: AWS recommends you use destinations instead of DLQ now (but both can be used at the same time).
- Event Source mapping: for discarded event batches
  - Amazon SQS:
  - Amazon SNS
- _Note: you can send events to a DLQ directly from SQS_

![Event Source Mapping Kinesis Stream.png](Images/EventSourceMappingKinesisStream.png)

![Lambda Destinations for Asynchronous Invocation](Images/LambdaDestinationsForAsynchronousInvocation.png)

- https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventsourcemapping.html
- https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html

# 13. Lambda Execution Role (IAM Role)

- Grants the Lambda function permissions to AWS services / resources.
- Sample managed policies for Lambda:
  - AWSLambdaBasicExecutionRole - Upload logs to CloudWatch.
  - AWSLambdaKinesisExecutionRole - Read from Kinesis.
  - AWSLambdaDynamoDBExecutionRole - Read from DynamoDB Streams.
  - AWSLambdaSQSQueueExecutionRole - Read from SQS.
  - AWSLambdaVPCAccessExecutionRole - Deploy Lambda function in VPC.
  - AWSXRayDaemonWriteAccess - Upload trace data to X-Ray.
- When you use an event source mapping to invoke your function, Lambda uses the execution role to read event data.
- Best practice: create one Lambda Execution Role per function.

## 13.1. Lambda Resource Based Policies

- Use resource-based policies to give other accounts and AWS services permission to use your Lambda resources.
- Similar to S3 bucket policies for S3 bucket.
- An IAM principal can access Lambda:
  - If the IAM policy attached to the principal authorizes it (e.g. user access).
  - OR if the resource-based policy authorizes (e.g. service access).
- When an AWS service like Amazon S3 calls your Lambda function, the resource-based policy gives it access.

# 14. Lambda Environment Variables

- Environment variable = key / value pair in "String" form.
- Adjust the function behavior without updating code.
- The environment variables are available to your code.
- Lambda Service adds its own system environment variables as well.
- Helpful to store secrets (encrypted by KMS).
- Secrets can be encrypted by the Lambda service key, or your own CMK.

# 15. Lambda Logging & Monitoring

- CloudWatch Logs:
  - AWS Lambda execution logs are stored in AWS CloudWatch Logs.
  - **Make sure your AWS Lambda function has an execution role with an IAM policy that authorizes writes to CloudWatch Logs.**
- CloudWatch Metrics:
  - AWS Lambda metrics are displayed in AWS CloudWatch Metrics.
  - Invocations, Durations, Concurrent Executions.
  - Error count, Success Rates, Throttles.
  - Async Delivery Failures.
  - Iterator Age (Kinesis & DynamoDB Streams).

## 15.1. Lambda Tracing with X-Ray

- Enable in Lambda configuration **(Active Tracing)**.
- Runs the X-Ray daemon for you.
- Use AWS X-Ray SDK in Code.
- Ensure Lambda Function has a correct IAM Execution Role.
  - The managed policy is called AWSXRayDaemonWriteAccess.
- Environment variables to communicate with X-Ray
  - **\_X_AMZN_TRACE_ID:** contains the tracing header
  - **AWS_XRAY_CONTEXT_MISSING:** by default, LOG_ERROR
  - **AWS_XRAY_DAEMON_ADDRESS:** the X-Ray Daemon IP_ADDRESS:PORT

# 16. Customization At The Edge

- Many modern applications execute some form of the logic at the edge.
- **Edge Function:**
  - A code that you write and attach to CloudFront distributions.
  - Runs close to your users to minimize latency.
- CloudFront provides two types: **CloudFront Functions** & **Lambda@Edge**.
- You don't have to manage any servers, deployed globally.
- Use case: customize the CDN content.
- Pay only for what you use.
- Fully serverless.

## 16.1. CloudFront Functions & Lambda@Edge - Use Cases

- Website Security and Privacy.
- Dynamic Web Application at the Edge.
- Search Engine Optimization (SEO).
- Intelligently Route Across Origins and Data Centers.
- Bot Mitigation at the Edge.
- Real-time Image Transformation.
- A/B Testing.
- User Authentication and Authorization.
- User Prioritization.
- User Tracking and Analytics.

## 16.2. CloudFront Functions

- Lightweight functions written in JavaScript.
- For high-scale, latency-sensitive CDN customizations.
- Sub-ms startup times, **millions of requests/second**.
- Used to change Viewer requests and responses:
  - **Viewer Request:** after CloudFront receives a request from a viewer.
  - **Viewer Response:** before CloudFront forwards the response to the viewer.
- Native feature of CloudFront (manage code entirely within CloudFront).

## 16.3. Lambda@Edge

- Lambda functions written in NodeJS or Python.
- Scales to **1000s of requests/second**.
- Used to change CloudFront requests and responses:
  - **Viewer Request:** After CloudFront receives a request from a viewer.
  - **Origin Request:** Before CloudFront forwards the request to the origin.
  - **Origin Response:** After CloudFront receives the response from the origin.
  - **Viewer Response:** Before CloudFront forwards the response to the viewer.
- Author your functions in one AWS Region (us-east-1), then CloudFront replicates to its locations.

## 16.4. CloudFront Functions vs. Lambda@Edge

|                                    | CloudFront Functions                      | Lambda@Edge                                       |
| ---------------------------------- | ----------------------------------------- | ------------------------------------------------- |
| Runtime Support                    | JavaScript                                | Node.js, Python                                   |
| # of Requests                      | Millions of requests per second           | Thousands of requests per second                  |
| CloudFront Triggers                | Viewer Request/Response                   | Viewer Request/Response - Origin Request/Response |
| Max. Execution Time                | < 1 ms                                    | 5 - 10 seconds                                    |
| Max. Memory                        | 2 MB                                      | 128 MB up to 10 GB                                |
| Total Package Size                 | 10 KB                                     | 1 MB - 50 MB                                      |
| Network Access, File System Access | No                                        | Yes                                               |
| Access to the Request Body         | No                                        | Yes                                               |
| Pricing                            | Free tier available, 1/6th price of @Edge | No free tier, charged per request & duration      |

## 16.5. CloudFront Functions vs. Lambda@Edge - Use Cases

| CloudFront Functions                                                                               | Lambda@Edge                                                                       |
| -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Cache key normalization                                                                            | Longer execution time (several ms)                                                |
| Transform request attributes (headers, cookies, query strings, URL) to create an optimal Cache Key | Adjustable CPU or memory                                                          |
| Header manipulation                                                                                | Your code depends on a 3rd libraries (e.g., AWS SDK to access other AWS services) |
| Insert/modify/delete HTTP headers in the request or response                                       | Network access to use external services for processing                            |
| URL rewrites or redirects                                                                          | File system access or access to the body of HTTP requests                         |
| Request authentication & authorization                                                             |                                                                                   |
| Create and validate user-generated tokens (e.g., JWT) to allow/deny requests                       |                                                                                   |

# 17. Lambda in VPC

- By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC).
- Therefore it cannot access resources in your VPC (RDS, ElastiCache, internal ELB...).
- You must define the VPC ID, the Subnets and the Security Groups.
- Lambda will create an ENI (Elastic Network Interface) in your subnets.
- **AWSLambdaVPCAccessExecutionRole**

## 17.1. Lambda in VPC - Internet Access

- A Lambda function in your VPC does not have internet access.
- Deploying a Lambda function in a public subnet does not give it internet access or a public IP.
- Deploying a Lambda function in a private subnet gives it internet access if you have a NAT Gateway / Instance.
- You can use VPC endpoints to privately access AWS services without a NAT.

# 18. Lambda Function Configuration

- **RAM:**
  - From 128MB to 10GB in 1MB increments
  - The more RAM you add, the more vCPU credits you get
  - At 1,792 MB, a function has the equivalent of one full vCPU
  - After 1,792 MB, you get more than one CPU, and need to use multi-threading in your code to benefit from it (up to 6 vCPU).
- **If your application is CPU-bound (computation heavy), increase RAM.**
- **Timeout:** default 3 seconds, maximum is 900 seconds (15 minutes)

## 18.1. Lambda Execution Context

- The execution context is a temporary runtime environment that initializes any external dependencies of your lambda code.
- Great for database connections, HTTP clients, SDK clients...
- The execution context is maintained for some time in anticipation of another Lambda function invocation.
- The next function invocation can "re-use" the context to execution time and save time in initializing connections objects.
- The execution context includes the _/tmp_ directory.

## 18.2. Lambda Functions /tmp space

- If your Lambda function needs to download a big file to work...
- If your Lambda function needs disk space to perform operations...
- You can use the /tmp directory.
- Max size is 512MB.
- The directory content remains when the execution context is frozen, providing transient cache that can be used for multiple invocations (helpful to checkpoint your work).
- For permanent persistence of object (non temporary), use S3.

# 19. File Systems Mounting

- Lambda functions can access EFS file systems if they are running in a VPC.
- Configure Lambda to mount EFS file systems to local directory during initialization.
- Must leverage EFS Access Points.
- Limitations: watch out for the EFS connection limits (one function instance = one connection) and connection burst limits.

# 20. Lambda Concurrency and Throttling

- Concurrency limit: up to 1000 concurrent executions.
- Can set a "reserved concurrency" at the function level (=limit).
- Each invocation over the concurrency limit will trigger a "Throttle".
- Throttle behavior:
  - If synchronous invocation => return ThrottleError - 429.
  - If asynchronous invocation => retry automatically and then go to DLQ.
- If you need a higher limit, open a support ticket.

## 20.1. Lambda Concurrency Issue

- If you don't reserve (=limit) concurrency, the following can happen:

## 20.2. Concurrency and Asynchronous Invocations

- If the function doesn't have enough concurrency available to process all events, additional requests are throttled.
- For throttling errors (429) and system errors (500-series), Lambda returns the event to the queue and attempts to run the function again for up to 6 hours.
- The retry interval increases exponentially from 1 second after the first attempt to a maximum of 5 minutes.

## 20.3. Cold Starts & Provisioned Concurrency

- **Cold Start:**
  - New instance => code is loaded and code outside the handler run (init)
  - If the init is large (code, dependencies, SDK...) this process can take some time.
  - First request served by new instances has higher latency than the rest
- Provisioned Concurrency:
  - Concurrency is allocated before the function is invoked (in advance)
  - So the cold start never happens and all invocations have low latency
  - Application Auto Scaling can manage concurrency (schedule or target utilization)
- Note:
  - Note: cold starts in VPC have been dramatically reduced in Oct & Nov 2019
  - https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/

## 20.4. Reserved and Provisioned Concurrency

https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html

# 21. Lambda Function Dependencies

- If your Lambda function depends on external libraries: for example AWS X-Ray SDK, Database Clients, etc...
- You need to install the packages alongside your code and zip it together:
  - For Node.js, use npm & "node_modules" directory.
  - For Python, use pip --target options.
  - For Java, include the relevant .jar files.
- Upload the **zip** straight to Lambda if less than 50MB, else to S3 first.
- Native libraries work: they need to be compiled on Amazon Linux.
- AWS SDK comes by default with every Lambda function.

# 22. Lambda and CloudFormation

## 22.1. Inline

- Inline functions are very simple.
- Use the Code.ZipFile property.
- You cannot include function dependencies with inline functions.

## 22.2. Through S3

- You must store the Lambda zip in S3.
- You must refer the S3 zip location in the CloudFormation code:
  - S3Bucket.
  - S3Key: full path to zip.
  - S3ObjectVersion: if versioned bucket.
- If you update the code in S3, but don't update S3Bucket, S3Key or S3ObjectVersion, CloudFormation won't update your function.

### 22.2.1. through S3 Multiple accounts

# 23. Lambda Layers

- Custom Runtimes:
  - Ex: C++ https://github.com/awslabs/aws-lambda-cpp
  - Ex: Rust https://github.com/awslabs/aws-lambda-rust-runtime
- Externalize Dependencies to re-use them.

# 24. Lambda Container Images

- Deploy Lambda function as container images of up to 10GB from ECR.
- Pack complex dependencies, large dependencies in a container.
- Base images are available for Python, Node.js, Java, .NET, Go, Ruby.
- Can create your own image as long as it implements the Lambda Runtime API.
- Test the containers locally using the Lambda Runtime Interface Emulator.
- Unified workflow to build apps.

## 24.1. Lambda Container Images

- Example: build from the base images provided by AWS:

```
# Use an image that implements the Lambda Runtime API
FROM amazon/aws-lambda-nodejs:12

# Copy your applciation code and files
COPY app.js package\*.json ./

# Install the dependencies in the container
RUN npm install

# Function to run when the Lambda function is invoked
CMD ["app.lambdaHandler"]
```

# 25. AWS Lambda Versions

- When you work on a Lambda function, we work on **$LATEST**.
- When we're ready to publish a Lambda function, we create a version.
- Versions are immutable.
- Versions have increasing version numbers.
- Versions get their own ARN (Amazon Resource Name).
- Version = code + configuration (nothing can be changed - immutable).
- Each version of the lambda function can be accessed.

# 26. AWS Lambda Aliases

- Aliases are "pointers" to Lambda function versions.
- We can define a "dev", "test", "prod" aliases and have them point at different lambda versions.
- Aliases are mutable.
- Aliases enable **Blue / Green** deployment by assigning weights to lambda functions.
- Aliases enable stable configuration of our event triggers / destinations.
- Aliases have their own ARNs.
- Aliases cannot reference aliases.

# 27. Lambda & CodeDeploy

- **CodeDeploy** can help you automate traffic shift for Lambda aliases.
- Feature is integrated within the SAM framework.
- **Linear:** grow traffic every N minutes until 100%
  - Linear10PercentEvery3Minutes
  - Linear10PercentEvery10Minutes
- **Canary:** try X percent then 100%
  - Canary10Percent5Minutes
  - Canary10Percent30Minutes
- **AllAtOnce:** immediate
  - Can create Pre & Post Traffic hooks to check the health of the Lambda function.

# 28. Function URL

- Dedicated HTTP(S) endpoint for your Lambda function.
- A unique URL endpoint is generated for you (never changes).
  - `https://<url-id>.lambda-url.<region>.on.aws (dual-stack IPv4 & IPv6)`
- Invoke via a web browser, curl, Postman, or any HTTP client.
- Access your function URL through the public Internet only.
  - Doesn't support PrivateLink (Lambda functions do support).
- Supports Resource-based Policies & CORS configurations.
- Can be applied to any function alias or to $LATEST (can't be applied to other function versions).
- Create and configure using AWS Console or AWS API.
- Throttle your function by using Reserved Concurrency.

## 28.1. Function URL Security

- **Resource-based Policy**
  - Authorize other accounts / specific CIDR / IAM principals.
- **Cross-Origin Resource Sharing (CORS)**
  - If you call your Lambda function URL from a different domain.
- **AuthType NONE**
  - Allow public and unauthenticated access.
    - Resource-based Policy is always in effect (must grant public access).
- **AuthType AWS_IAM:** IAM is used to authenticate and authorize requests.
  - Both Principal's Identity-based Policy & Resource-based Policy are evaluated.
  - Principal must have `lambda:InvokeFunctionUrl` permissions.
  - **Same account:** Identity-based Policy **OR** Resource-based Policy as ALLOW.
  - **Cross account:** Identity-based Policy **AND** Resource Based Policy as ALLOW.

# 29. Lambda and CodeGuru Profiling

- Gain insights into runtime performance of your Lambda functions using CodeGuru Profiler.
- CodeGuru creates a Profiler Group for your Lambda function.
- Supported for Java and Python runtimes.
- Activate from AWS Lambda Console.
- When activated, Lambda adds:
  - CodeGuru Profiler layer to your function.
  - Environment variables to your function.
  - `AmazonCodeGuruProfilerAgentAccess` policy to your function.

# 30. AWS Lambda Limits to Know - per region

- **Execution:**
  - Memory allocation: 128 MB - 10GB (1 MB increments).
  - Maximum execution time: 900 seconds (15 minutes).
  - Environment variables (4 KB).
  - Disk capacity in the "function container" (in /tmp): 512 MB to 10GB.
  - Concurrency executions: 1000 (can be increased).
- **Deployment:**
  - Lambda function deployment size (compressed .zip): 50 MB.
  - Size of uncompressed deployment (code + dependencies): 250 MB.
  - Can use the /tmp directory to load other files at startup.
  - Size of environment variables: 4 KB.

# 31. AWS Lambda Best Practices

- Perform heavy-duty work outside of your function handler:
  - Connect to databases outside of your function handler.
  - Initialize the AWS SDK outside of your function handler.
  - Pull in dependencies or datasets outside of your function handler.
- Use environment variables for:
  - Database Connection Strings, S3 bucket, etc... don't put these values in your code.
  - Passwords, sensitive values... they can be encrypted using KMS.
- Minimize your deployment package size to its runtime necessities:
  - Break down the function if need be.
  - Remember the AWS Lambda limits.
  - Use Layers where necessary.
- **Avoid using recursive code, never have a Lambda function call itself.**
