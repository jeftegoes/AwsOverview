# AWS CloudFormation<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction Infrastructure as Code](#1-introduction-infrastructure-as-code)
  - [1.1. What is CloudFormation](#11-what-is-cloudformation)
  - [1.2. Benefits of AWS CloudFormation](#12-benefits-of-aws-cloudformation)
  - [1.3. How CloudFormation Works](#13-how-cloudformation-works)
  - [1.4. Deploying CloudFormation templates](#14-deploying-cloudformation-templates)
  - [1.5. CloudFormation Building Blocks](#15-cloudformation-building-blocks)
- [2. YAML](#2-yaml)
  - [2.1. What are resources?](#21-what-are-resources)
  - [2.2. How do I find resources documentation?](#22-how-do-i-find-resources-documentation)
  - [2.3. FAQ for resources](#23-faq-for-resources)
- [JSON](#json)
- [3. What are parameters?](#3-what-are-parameters)
  - [3.1. When should you use a parameter?](#31-when-should-you-use-a-parameter)
  - [3.2. Parameters Settings](#32-parameters-settings)
  - [3.3. How to Reference a Parameter](#33-how-to-reference-a-parameter)
  - [3.4. Concept: Pseudo Parameters](#34-concept-pseudo-parameters)
  - [3.5. What are mappings?](#35-what-are-mappings)
  - [3.6. When would you use mappings vs parameters?](#36-when-would-you-use-mappings-vs-parameters)
  - [3.7. Fn::FindInMap Accessing Mapping Values](#37-fnfindinmap-accessing-mapping-values)
- [4. What are outputs?](#4-what-are-outputs)
  - [4.1. Outputs Example](#41-outputs-example)
- [5. Cross Stack Reference](#5-cross-stack-reference)
- [6. What are conditions used for?](#6-what-are-conditions-used-for)
  - [6.1. How to define a condition?](#61-how-to-define-a-condition)
  - [6.2. Using a Condition](#62-using-a-condition)
- [7. Must Know Intrinsic Functions](#7-must-know-intrinsic-functions)
  - [7.1. Fn::Ref](#71-fnref)
  - [7.2. Fn::GetAtt](#72-fngetatt)
  - [7.3. Fn::GetAZs](#73-fngetazs)
  - [7.4. Fn::Select](#74-fnselect)
  - [7.5. Fn::FindInMap - Accessing Mapping Values](#75-fnfindinmap---accessing-mapping-values)
  - [7.6. Fn::ImportValue](#76-fnimportvalue)
  - [7.7. Fn::Join](#77-fnjoin)
  - [7.8. Fn::Sub](#78-fnsub)
  - [7.9. Condition Functions](#79-condition-functions)
- [8. Rollbacks](#8-rollbacks)
- [9. Stack Notifications](#9-stack-notifications)
- [10. ChangeSets](#10-changesets)
- [11. Nested stacks](#11-nested-stacks)
- [12. CloudFormation - Cross vs Nested Stacks](#12-cloudformation---cross-vs-nested-stacks)
- [13. StackSets](#13-stacksets)
- [14. Drift](#14-drift)
- [15. Stack Policies](#15-stack-policies)
- [16. CloudFormation helper scripts reference](#16-cloudformation-helper-scripts-reference)

# 1. Introduction Infrastructure as Code

- Currently, we have been doing a lot of manual work.
- All this manual work will be very tough to reproduce:
  - In another region.
  - in another AWS account.
  - Within the same region if everything was deleted.
- Wouldn't it be great, if all our infrastructure was... code?
- That code would be deployed and create / update / delete our infrastructure

## 1.1. What is CloudFormation

- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).
- For example, within a CloudFormation template, you say:
  - I want a security group.
  - I want two EC2 machines using this security group.
  - I want two Elastic IPs for these EC2 machines.
  - I want an S3 bucket.
  - I want a load balancer (ELB) in front of these machines.
- Then CloudFormation creates those for you, in the right order, with the exact configuration that you specify.

## 1.2. Benefits of AWS CloudFormation

- **Infrastructure as code**
  - No resources are manually created, which is excellent for control.
  - The code can be version controlled for example using git.
  - Changes to the infrastructure are reviewed through code.
- **Cost**
  - Each resources within the stack is tagged with an identifier so you can easily see how much a stack costs you.
  - You can estimate the costs of your resources using the CloudFormation template.
  - Savings strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely.
- **Productivity**
  - Ability to destroy and re-create an infrastructure on the cloud on the fly.
  - Automated generation of Diagram for your templates!
  - Declarative programming (no need to figure out ordering and orchestration).
- Separation of concern: create many stacks for many apps, and many layers. Ex:
  - VPC stacks.
  - Network stacks.
  - App stacks.
- **Don't re-invent the wheel**
  - Leverage existing templates on the web!
  - Leverage the documentation.

## 1.3. How CloudFormation Works

- Templates have to be uploaded in S3 and then referenced in
  CloudFormation
- To update a template, we can't edit previous ones. We have to re-upload a new version of the template to AWS
- Stacks are identified by a name
- Deleting a stack deletes every single artifact that was created by CloudFormation.

## 1.4. Deploying CloudFormation templates

- Manual way:
  - Editing templates in the CloudFormation Designer
  - Using the console to input parameters, etc
- Automated way:
  - Editing templates in a YAML file
  - Using the AWS CLI (Command Line Interface) to deploy the templates
  - Recommended way when you fully want to automate your flow

## 1.5. CloudFormation Building Blocks

- Templates components (one course section for each):

1. Resources: your AWS resources declared in the template (MANDATORY)
2. Parameters: the dynamic inputs for your template
3. Mappings: the static variables for your template
4. Outputs: References to what has been created
5. Conditionals: List of conditions to perform resource creation
6. Metadata

- Templates helpers:

1. References
2. Functions

# 2. YAML

- YAML and JSON are the languages you can use for CloudFormation.
  - JSON
  - YAML (better)

## 2.1. What are resources?

- Resources are the core of your CloudFormation template (MANDATORY).
- They represent the different AWS Components that will be created and configured.
- Resources are declared and can reference each other.
- AWS figures out creation, updates and deletes of resources for us
- There are over ~224 types of resources
- Resource types identifiers are of the form:
  `AWS::aws-product-name::data-type-name`

## 2.2. How do I find resources documentation?

- I can't teach you all of the 224 resources, but I can teach you how to learn how to use them.
- All the resources can be found here: [Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
- Example here (for an EC2 instance): [Example](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html)

## 2.3. FAQ for resources

- Can I create a dynamic amount of resources?
  - No, you can't. Everything in the CloudFormation template has to be declared. You can't perform code generation there.
- Is every AWS Service supported?
  - Almost. Only a select few niches are not there yet.
  - You can work around that using AWS Lambda Custom Resources.

# JSON

- An S3 bucket policy statement is composed of several elements, and the following are required to create a valid policy:

  - `Effect` - The effect can be Allow or Deny.
  - `Action` - The specific API action for which you are granting or denying permission.
  - `Principal` - The user, account, service, or other entity that is allowed or denied access to the bucket or objects within the bucket.
  - `Resource` - The resource that's affected by the action.
    - You specify a resource using an Amazon Resource Name (ARN).

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
              "AWS": [
                "arn:aws:iam::1234567890:role/Developer"
              ]
          },
          "Action": [
              "s3:GetObject"
          ],
          "Resource": "arn:aws:s3:::jeftegoesdev/*"
        },
        {
          "Effect": "Allow",
          "Principal": {
              "AWS": [
                "arn:aws:iam::1234567890:role/QA"
              ]
          },
          "Action": [
              "s3:GetObject"
          ],
          "Resource": "arn:aws:s3:::jeftegoesdev/qa/*"
        }
    ]
  }
  ```

![Example CloudFormation in JSON](Images/AWSCloudFormationExampleJson.png)

# 3. What are parameters?

- Parameters are a way to provide inputs to your AWS CloudFormation template.
- They're important to know about if:
  - You want to reuse your templates across the company.
  - Some inputs can not be determined ahead of time.
- Parameters are extremely powerful, controlled, and can prevent errors from happening in your templates thanks to types.

## 3.1. When should you use a parameter?

- Ask yourself this:
  - Is this CloudFormation resource configuration likely to change in the future?
  - If so, make it a parameter.
- You won't have to re-upload a template to change its content.

## 3.2. Parameters Settings

- Parameters can be controlled by all these settings:
  - Type:
    - String
    - Number
    - CommaDelimitedList
    - List<Type>
    - AWS Parameter (to help catch invalid values - match against existing values in the AWS Account)
  - Description
  - Constraints
  - ConstraintDescription (String)
  - Min/MaxLength
  - Min/MaxValue
  - Defaults
  - AllowedValues (array)
  - AllowedPattern (regexp)
  - NoEcho (Boolean)

## 3.3. How to Reference a Parameter

- The `Fn::Ref` function can be leveraged to reference parameters
- Parameters can be used anywhere in a template.
- The shorthand for this in YAML is `!Ref`
- The function can also reference other elements within the template

## 3.4. Concept: Pseudo Parameters

- AWS offers us pseudo parameters in any CloudFormation template.
- These can be used at any time and are enabled by default

| Reference Value       | Example Return Value                                               |
| --------------------- | ------------------------------------------------------------------ |
| AWS::AccountId        | 1234567890                                                         |
| AWS::NotificationARNs | [arn:aws:sns:us-east-1:123456789012:MyTopic]                       |
| AWS::NoValue          | Does not return a value                                            |
| AWS::Region           | us-east-2                                                          |
| AWS::StackId          | rn:aws:cloudformation:us-east-1:123456789012:stack/MyStack/1c2fa62 |
| AWS::StackName        | MyStack                                                            |

## 3.5. What are mappings?

- Mappings are fixed variables within your CloudFormation Template.
- They're very handy to differentiate between different environments (dev vs prod), regions (AWS regions), AMI types, etc.
- All the values are hardcoded within the template.

## 3.6. When would you use mappings vs parameters?

- Mappings are great when you know in advance all the values that can be taken and that they can be deduced from variables such as:
  - Region
  - Availability Zone
  - AWS Account
  - Environment (dev vs prod)
  - Etc...
- They allow safer control over the template.
- Use parameters when the values are really user specific

## 3.7. Fn::FindInMap Accessing Mapping Values

- We use Fn::FindInMap to return a named value from a specific key
- !FindInMap [ MapName, TopLevelKey, SecondLevelKey ]

# 4. What are outputs?

- The Outputs section declares optional outputs values that we can import into other stacks (if you export them first)!
- You can also view the outputs in the AWS Console or in using the AWS CLI
- They're very useful for example if you define a network CloudFormation, and output the variables such as VPC ID and your Subnet IDs
- It's the best way to perform some collaboration cross stack, as you let expert handle their own part of the stack
- You can't delete a CloudFormation Stack if its outputs are being referenced by another CloudFormation stack.

## 4.1. Outputs Example

- Creating a SSH Security Group as part of one template.
- We create an output that references that security group.

# 5. Cross Stack Reference

- We then create a second template that leverages that security group.
- For this, we use the Fn::ImportValue function.
- You can't delete the underlying stack until all the references are deleted too.

# 6. What are conditions used for?

- Conditions are used to control the creation of resources or outputs based on a condition.
- Conditions can be whatever you want them to be, but common ones are:
  - Environment (dev / test / prod).
  - AWS Region.
  - Any parameter value.
- Each condition can reference another condition, parameter value or mapping.

## 6.1. How to define a condition?

- The logical ID is for you to choose. It's how you name condition
- The intrinsic function (logical) can be any of the following:
  - `Fn::And`
  - `Fn::Equals`
  - `Fn::If`
  - `Fn::Not`
  - `Fn::Or`

## 6.2. Using a Condition

- Conditions can be applied to resources / outputs / etc...

# 7. Must Know Intrinsic Functions

- Ref
- Fn::GetAtt
- Fn::GetAZs
- Fn::Select
- Fn::FindInMap
- Fn::ImportValue
- Fn::Join
- Fn::Sub
- Condition Functions (Fn::If, Fn::Not, Fn::Equals, etc...)

## 7.1. Fn::Ref

- The `Fn::Ref` function can be leveraged to reference.
  - Parameters => returns the value of the parameter.
  - Resources => returns the physical ID of the underlying resource (ex: EC2 ID).
- The shorthand for this in YAML is `!Ref`.

## 7.2. Fn::GetAtt

- Attributes are attached to any resources you create.
- To know the attributes of your resources, the best place to look at is the documentation.
- For example: the AZ of an EC2 machine!

## 7.3. Fn::GetAZs

- Returns an array that lists Availability Zones for a specified region in alphabetical order.

## 7.4. Fn::Select

- Returns a single object from a list of objects by index.

## 7.5. Fn::FindInMap - Accessing Mapping Values

- We use `Fn::FindInMap` to return a named value from a specific key.
- !FindInMap [ MapName, TopLevelKey, SecondLevelKey ].

## 7.6. Fn::ImportValue

- Import values that are exported in other templates.
- For this, we use the Fn::ImportValue function.

## 7.7. Fn::Join

- Join values with a delimiter.
- This creates "a:b:c".

## 7.8. Fn::Sub

- Fn::Sub, or !Sub as a shorthand, is used to substitute variables from a text. It's a very handy function that will allow you to fully customize your templates.
- For example, you can combine Fn::Sub with References or AWS Pseudo variables!
- String must contain ${VariableName} and will substitute them.

## 7.9. Condition Functions

- The logical ID is for you to choose. It's how you name condition.
- The intrinsic function (logical) can be any of the following:
  - Fn::And
  - Fn::Equals
  - Fn::If
  - Fn::Not
  - Fn::Or

# 8. Rollbacks

- Stack Creation Fails:
  - Default: everything rolls back (gets deleted). We can look at the log.
  - Option to disable rollback and troubleshoot what happened.
- Stack Update Fails:
  - The stack automatically rolls back to the previous known working state.
  - Ability to see in the log what happened and error messages.

# 9. Stack Notifications

- Send Stack events to SNS Topic (Email, Lambda, ...)
- Enable SNS Integration using Stack Options

# 10. ChangeSets

- When you update a stack, you need to know what changes before it happens for greater confidence.
- ChangeSets won't say if the update will be successful.

# 11. Nested stacks

- Nested stacks are stacks as part of other stacks.
- They allow you to isolate repeated patterns / common components in separate stacks and call them from other stacks.
- Example:
  - Load Balancer configuration that is re-used.
  - Security Group that is re-used.
- Nested stacks are considered best practice.
- To update a nested stack, always update the parent (root stack).Stephane Maarek

# 12. CloudFormation - Cross vs Nested Stacks

- Cross Stacks
  - Helpful when stacks have different lifecycles.
  - Use Outputs Export and Fn::ImportValue.
  - When you need to pass export values to many stacks (VPC Id, etc...).
- Nested Stacks
  - Helpful when components must be re-used.
  - Ex: re-use how to properly configure an Application Load Balancer.
  - The nested stack only is important to the higher level stack (it's not shared).

# 13. StackSets

- Create, update, or delete stacks across **multiple accounts and regions** with a single operation.
- Administrator account to create StackSets.
- Trusted accounts to create, update, delete stack instances from StackSets.
- When you update a stack set, all associated stack instances are updated throughout all accounts and regions.

![CloudFormation StackSet](Images/AWSCloudFormationStackSet.png)

# 14. Drift

- CloudFormation allows you to create infrastructure.
- But it doesn't protect you against manual configuration changes.
- How do we know if our resources have drifted?
- We can use CloudFormation drift!
- Not all resources are supported yet: [Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-supported-resources.html)

# 15. Stack Policies

- During a CloudFormation Stack update, all update actions are allowed on all resources (default)
- **A Stack Policy is a JSON document that defines the update actions that are allowed on specific resources during Stack updates.**
- Protect resources from unintentional updates.
- When you set a Stack Policy, all resources in the Stack are protected by default.
- Specify an explicit ALLOW for the resources you want to be allowed to be updated.

# 16. CloudFormation helper scripts reference

- AWS CloudFormation provides the following Python helper scripts that you can use to install software and start services on an [Amazon EC2](AWS%20EC2.md) instance that you create as part of your stack:
  - `cfn-init`: Use to retrieve and interpret resource metadata, install packages, create files, and start services.
  - `cfn-signal`: Use to signal with a `CreationPolicy` or `WaitCondition`, so you can synchronize other resources in the stack when the prerequisite resource or application is ready.
  - `cfn-get-metadata`: Use to retrieve metadata for a resource or path to a specific key.
  - `cfn-hup`: Use to check for updates to metadata and execute custom hooks when changes are detected.
- You call the scripts directly from your template.
- The scripts work in conjunction with resource metadata that's defined in the same template.
- The scripts run on the [Amazon EC2](AWS%20EC2.md) instance during the stack creation process.
