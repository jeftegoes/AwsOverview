# AWS API Gateway<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Integrations High Level](#2-integrations-high-level)
- [3. Endpoint Types](#3-endpoint-types)
- [4. General Security](#4-general-security)
- [5. Deployment Stages](#5-deployment-stages)
  - [5.1. API Gateway Stages v1 and v2 API breaking change](#51-api-gateway-stages-v1-and-v2-api-breaking-change)
- [6. Stage Variables](#6-stage-variables)
  - [6.1. Stage Variables and Lambda Aliases](#61-stage-variables-and-lambda-aliases)
- [7. Canary Deployment](#7-canary-deployment)
- [8. Integration Types (Methods)](#8-integration-types-methods)
  - [8.1. MOCK](#81-mock)
  - [8.2. HTTP / AWS (Lambda \& AWS Services)](#82-http--aws-lambda--aws-services)
  - [8.3. AWS\_PROXY (Lambda Proxy)](#83-aws_proxy-lambda-proxy)
  - [8.4. HTTP\_PROXY](#84-http_proxy)
- [9. Mapping Templates (AWS and HTTP Integration)](#9-mapping-templates-aws-and-http-integration)
  - [9.1. Mapping Example: JSON to XML with SOAP](#91-mapping-example-json-to-xml-with-soap)
  - [9.2. Mapping Example: Query String parameters](#92-mapping-example-query-string-parameters)
  - [9.3. Velocity Template Language (VTL)](#93-velocity-template-language-vtl)
- [10. Open API spec](#10-open-api-spec)
  - [10.1. Request Validation](#101-request-validation)
  - [10.2. RequestValidation - OpenAPI](#102-requestvalidation---openapi)
- [11. Caching API responses](#11-caching-api-responses)
  - [11.1. Cache invalidations](#111-cache-invalidations)
- [12. Usage Plans \& API Keys](#12-usage-plans--api-keys)
  - [12.1. Correct Order for API keys](#121-correct-order-for-api-keys)
- [13. Logging \& Tracing](#13-logging--tracing)
  - [13.1. CloudWatch metrics](#131-cloudwatch-metrics)
- [14. Throttling](#14-throttling)
- [15. API Gateway - Errors](#15-api-gateway---errors)
- [16. CORS](#16-cors)
- [17. Security](#17-security)
  - [17.1. IAM Permissions](#171-iam-permissions)
    - [17.1.1. Resource Policies](#1711-resource-policies)
  - [17.2. Cognito User Pools](#172-cognito-user-pools)
  - [17.3. Lambda Authorizer (formerly Custom Authorizers)](#173-lambda-authorizer-formerly-custom-authorizers)
  - [17.4. Summary](#174-summary)
- [18. HTTP API vs REST API](#18-http-api-vs-rest-api)
- [19. WebSocket API - Overview](#19-websocket-api---overview)
  - [19.1. Routing](#191-routing)
- [20. Architecture](#20-architecture)

# 1. Introduction

![Example: API Gateway a Serveless API](/Images/APIGatewayServelessAPI.png)

- AWS Lambda + API Gateway: No infrastructure to manage.
- Support for the WebSocket Protocol.
- Handle API versioning (v1, v2...).
- Handle different environments (dev, test, prod...).
- Handle security (Authentication and Authorization).
- Create API keys, handle request throttling.
- Swagger / Open API import to quickly define APIs.
- Transform and validate requests and responses.
- Generate SDK and API specifications.
- Cache API responses.

# 2. Integrations High Level

- Lambda Function:
  - Invoke Lambda function.
  - Easy way to expose REST API backed by AWS Lambda.
- HTTP:
  - Expose HTTP endpoints in the backend.
  - Example: internal HTTP API on premise, Application Load Balancer...
  - Why? Add rate limiting, caching, user authentications, API keys, etc...
- AWS Service
  - Expose any AWS API through the API Gateway.
  - Example: start an AWS Step Function workflow, post a message to SQS.
  - Why? Add authentication, deploy publicly, rate control...

# 3. Endpoint Types

- Edge-Optimized (default): For global clients
  - Requests are routed through the CloudFront Edge locations (improves latency).
  - The API Gateway still lives in only one region.
- Regional:
  - For clients within the same region.
  - Could manually combine with CloudFront (more control over the caching strategies and the distribution).
- Private:
  - Can only be accessed from your VPC using an interface VPC endpoint (ENI).
  - Use a resource policy to define access.

# 4. General Security

- **User Authentication through:**
  - IAM Roles (useful for internal applications).
  - Cognito (identity for external users - example mobile users).
  - Custom Authorizer (your own logic).
- **Custom Domain Name HTTPS** security through integration with AWS Certificate Manager (ACM):
  - If using Edge-Optimized endpoint, then the certificate must be in us-east-1.
  - If using Regional endpoint, the certificate must be in the API Gateway region.
  - Must setup CNAME or A-alias record in Route 53.

# 5. Deployment Stages

- Making changes in the API Gateway does not mean they're effective.
- You need to make a "deployment" for them to be in effect.
  - It's a common source of confusion.
- Changes are deployed to "Stages" (as many as you want).
- Use the naming you like for stages (dev, test, prod).
- Each stage has its own configuration parameters.
- Stages can be rolled back as a history of deployments is kept.

## 5.1. API Gateway Stages v1 and v2 API breaking change

![API Gateway - API breaking change](/Images/APIGatewayAPIBreakingChange.png)

# 6. Stage Variables

- Stage variables are like environment variables for API Gateway.
- Use them to change often changing configuration values.
- They can be used in:
  - Lambda function ARN.
  - HTTP Endpoint.
  - Parameter mapping templates.
- Use cases:
  - Configure HTTP endpoints your stages talk to (dev, test, prod...).
  - Pass configuration parameters to AWS Lambda through mapping templates.
- Stage variables are passed to the "context" object in AWS Lambda.
- Examples:
  - A full URI without protocol - `http://${stageVariables.<variable_name>}`
  - A full domain - `http://${stageVariables.<variable_name>}/resource/operation`
  - A subdomain - `http://${stageVariables.<variable_name>}.example.com/resource/operation`
  - A path - `http://example.com/${stageVariables.<variable_name>}/bar`
  - A query string - `http://example.com/foo?q=${stageVariables.<variable_name>}`

## 6.1. Stage Variables and Lambda Aliases

- We create a stage variable to indicate the corresponding Lambda alias.
- Our API gateway will automatically invoke the right Lambda function!

![API Gateway Stage Variables and Lambda Aliases](/Images/APIGatewayStageVariablesAndLambdaAliases.png)

# 7. Canary Deployment

- Possibility to enable canary deployments for any stage (usually prod).
- Choose the % of traffic the canary channel receives.
- Metrics & Logs are separate (for better monitoring).
- Possibility to override stage variables for canary.
- This is blue / green deployment with [AWS Lambda](AWS%20Lambda.md) and API Gateway.

![API Gateway - Canary Deployment](/Images/APIGatewayCanaryDeployment.png)

# 8. Integration Types (Methods)

## 8.1. MOCK

- API Gateway returns a response without sending the request to the backend.

## 8.2. HTTP / AWS (Lambda & AWS Services)

- You must configure both the integration request and integration response.
- Setup data mapping using mapping templates for the request & response.
- `HTTP` is commonly used in AWS Services like EC2.
- **`AWS` is only used for Lambda custom integration**.

## 8.3. AWS_PROXY (Lambda Proxy)

- Incoming request from the client is the input to Lambda.
- The function is responsible for the logic of request / response.
- No mapping template, headers, query string parameters... are passed as arguments.

## 8.4. HTTP_PROXY

- No mapping template.
- The HTTP request is passed to the backend.
- The HTTP response from the backend is forwarded by API Gateway.

# 9. Mapping Templates (AWS and HTTP Integration)

- Mapping templates can be used to modify request / responses.
- Rename / Modify query string parameters.
- Modify body content.
- Add headers.
- Uses Velocity Template Language (VTL): for loop, if etc...
- Filter output results (remove unnecessary data).

## 9.1. Mapping Example: JSON to XML with SOAP

- SOAP API are XML based, whereas REST API are JSON based.
- In this case, API Gateway should:
  - Extract data from the request: either path, payload or header.
  - Build SOAP message based on request data (mapping template).
  - Call SOAP service and receive XML response.
  - Transform XML response to desired format (like JSON), and respond to the user.

## 9.2. Mapping Example: Query String parameters

## 9.3. Velocity Template Language (VTL)

- You should use VTL for data mapping and transformations rather than complex business logic.
- There are exceptions but the drawbacks of using VTL for other use cases often outweigh the benefits.

# 10. Open API spec

- Common way of defining REST APIs, using API definition as code.
- Import existing OpenAPI 3.0 spec to API Gateway:
  - Method
  - Method Request
  - Integration Request
  - Method Response
  - +AWS extensions for API gateway and setup every single option
- Can export current API as OpenAPI spec.
- OpenAPI specs can be written in YAML or JSON.
- Using OpenAPI we can generate SDK for our applications.

## 10.1. Request Validation

- You can configure API Gateway to perform basic validation of an API request before proceeding with the integration request.
- When the validation fails, API Gateway immediately fails the request.
- Returns a 400-error response to the caller.
- This reduces unnecessary calls to the backend.
- Checks:
  - The required request parameters in the URI, query string, and headers of an incoming request are included and non-blank.
  - The applicable request payload adheres to the configured JSON Schema request model of the method.

## 10.2. RequestValidation - OpenAPI

- Setup request validation by importing OpenAPI definitions file.

# 11. Caching API responses

- Caching reduces the number of calls made to the backend.
- Default **TTL** (time to live) is 300 seconds (min: 0s, max: 3600s).
- **Caches are defined per stage.**
  - Possible to override cache settings **per method**.
- Cache encryption option.
- Cache capacity between 0.5GB to 237GB.
- Cache is expensive, makes sense in production, may not make sense in dev / test.

## 11.1. Cache invalidations

- Able to flush the entire cache (invalidate it) immediately.
- Clients can invalidate the cache with header: `Cache-Control: max-age=0` (with proper IAM authorization).
- If you don't impose an InvalidateCache policy (or choose the Require Authorization check box in the console), any client can invalidate the API cache.

![API Gateway cache parameter](Images/APIGatewayCacheParameter.png)

# 12. Usage Plans & API Keys

- If you want to make an API available as an offering ($) to your customers.
- Usage Plan:
  - Who can access one or more deployed API stages and methods.
  - How much and how fast they can access them.
  - Uses API keys to identify API clients and meter access.
  - Configure throttling limits and quota limits that are enforced on individual client.
- API Keys:
  - alphanumeric string values to distribute to your customers
  - Ex: WBjHxNtoAb4WPKBC7cGm64CBibIb24b4jt8jJHo9
  - Can use with usage plans to control access
  - Throttling limits are applied to the API keys
  - Quotas limits is the overall number of maximum requests

## 12.1. Correct Order for API keys

- To configure a usage plan:

1. Create one or more APIs, configure the methods to require an API key, and deploy the APIs to stages.
2. Generate or import API keys to distribute to application developers (your customers) who will be using your API.
3. Create the usage plan with the desired throttle and quota limits.
4. Associate API stages and API keys with the usage plan.

- Callers of the API must supply an assigned API key in the x-api-key header in requests to the API.

# 13. Logging & Tracing

- CloudWatch Logs:
  - Enable CloudWatch logging at the Stage level (with Log Level).
  - Can override settings on a per API basis (ex: ERROR, DEBUG, INFO).
  - Log contains information about request / response body.
- X-Ray:
  - Enable tracing to get extra information about requests in API Gateway.
  - X-Ray API Gateway + AWS Lambda gives you the full picture.

## 13.1. CloudWatch metrics

- Metrics are by stage, Possibility to enable detailed metrics.
- `CacheHitCount` and `CacheMissCount` - Efficiency of the cache.
- `Count` - The total number API requests in a given period.
- `IntegrationLatency` - The time between when API Gateway relays a request to the backend and when it receives a response from the backend.
- `Latency` - The time between when API Gateway receives a request from a client and when it returns a response to the client.
  - The latency includes the integration latency and other API Gateway overhead.
- **4XX Error** (client-side) & **5XX Error** (server-side).

# 14. Throttling

- **Account Limit**
  - API Gateway throttles requests at10000 rps across all API.
  - Soft limit that can be increased upon request.
- In case of throttling => **429 Too Many Requests** (retriable error).
- Can set **Stage limit & Method limits** to improve performance.
- Or you can define **Usage Plans** to throttle per customer.
- **Just like Lambda Concurrency, one API that is overloaded, if not limited, can cause the other APIs to be throttled.**

# 15. API Gateway - Errors

- 4xx means Client errors:
  - **400:** Bad Request.
  - **403:** Access Denied, WAF filtered.
  - **429:**
    - Quota exceeded
    - Throttle.
- 5xx means Server errors:
  - **502:** Bad Gateway Exception, usually for an incompatible output returned from a Lambda proxy integration backend and occasionally for out-of-order invocations due to heavy loads.
  - **503:** Service Unavailable Exception.
  - **504:** Integration Failure - ex Endpoint Request Timed-out Exception API Gateway requests time out after 29 second maximum.

# 16. CORS

- CORS must be enabled when you receive API calls from another domain.
- The OPTIONS pre-flight request must contain the following headers:
  - Access-Control-Allow-Methods
  - Access-Control-Allow-Headers
  - Access-Control-Allow-Origin
- CORS can be enabled through the console.

![API Gateway CORS](/Images/APIGatewayCORS.png)

# 17. Security

## 17.1. IAM Permissions

- Create an IAM policy authorization and attach to User / Role.
- **Authentication = IAM** | _Authorization = IAM Policy_.
- Good to provide access within AWS (EC2, Lambda, IAM users...).
- Leverages "Sig v4" capability where IAM credential are in headers.

### 17.1.1. Resource Policies

- Resource policies (similar to Lambda Resource Policy).
- Allow for Cross Account Access (combined with IAM Security).
- Allow for a specific source IP address.
- Allow for a VPC Endpoint.

## 17.2. Cognito User Pools

- Cognito fully manages user lifecycle, token expires automatically.
- API gateway verifies identity automatically from AWS Cognito.
- No custom implementation required.
- **Authentication = Cognito User Pools** | _Authorization = API Gateway Methods_.

## 17.3. Lambda Authorizer (formerly Custom Authorizers)

- Token-based authorizer (bearer token) - ex JWT (JSON Web Token) or Oauth.
- A request parameter-based Lambda authorizer (headers, query string, stage var).
- Lambda must return an IAM policy for the user, result policy is cached.
- **Authentication = External** | _Authorization = Lambda function_.

## 17.4. Summary

- IAM:
  - Great for users / roles already within your AWS account, + resource policy for cross account.
  - Handle authentication + authorization.
  - Leverages Signature v4.
- Custom Authorizer (Lambda Authorizer):
  - Great for 3rd party tokens.
  - Very flexible in terms of what IAM policy is returned.
  - Handle Authentication verification + Authorization in the Lambda function.
  - Pay per Lambda invocation, results are cached.
- Cognito User Pool:
  - You manage your own user pool (can be backed by Facebook, Google login etc...)
  - No need to write any custom code.
  - Must implement authorization in the backend.

# 18. HTTP API vs REST API

- HTTP APIs:
  - low-latency, cost-effective AWS Lambda proxy, HTTP proxy APIs and private integration (no data mapping).
  - Support OIDC and OAuth 2.0 authorization, and built-in support for CORS.
  - No usage plans and API keys.
- REST APIs:
  - All features (except Native OpenID Connect / OAuth 2.0).

[Full list here](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html)

# 19. WebSocket API - Overview

- What's WebSocket?
  - Two-way interactive communication between a user's browser and a server.
  - Server can push information to the client.
  - This enables stateful application use cases.
- WebSocket APIs are often used in **real-time applications** such as chat applications, collaboration platforms, multiplayer games, and financial trading platforms.
- Works with AWS Services (Lambda, DynamoDB) or HTTP endpoints.

## 19.1. Routing

- Incoming JSON messages are routed to different backend.
- If no routes => sent to $default.
- You request a route selection expression to select the field on JSON to route from.
- Sample expression: $request.body.action.
- The result is evaluated against the route keys available in your API Gateway.
- The route is then connected to the backend you've setup through API Gateway.

# 20. Architecture

- Create a single interface for all the microservices in your company.
- Use API endpoints with various resources.
- Apply a simple domain name and SSL certificates.
- Can apply forwarding and transformation rules at the API Gateway level.
