# AWS CloudFormation<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction Infrastructure as Code (IaC)](#1-introduction-infrastructure-as-code-iac)
  - [1.1. What is CloudFormation](#11-what-is-cloudformation)
  - [1.2. Benefits of AWS CloudFormation](#12-benefits-of-aws-cloudformation)
  - [1.3. How CloudFormation Works](#13-how-cloudformation-works)
  - [1.4. Deploying CloudFormation templates](#14-deploying-cloudformation-templates)
  - [1.5. Building Blocks](#15-building-blocks)
- [2. YAML](#2-yaml)
  - [2.1. Template anatomy](#21-template-anatomy)
  - [2.2. Resources](#22-resources)
    - [2.2.1. How do I find resources documentation?](#221-how-do-i-find-resources-documentation)
    - [2.2.2. Extras](#222-extras)
  - [2.3. Parameters](#23-parameters)
    - [2.3.1. When should you use a parameter?](#231-when-should-you-use-a-parameter)
    - [2.3.2. Parameters Settings](#232-parameters-settings)
    - [2.3.3. How to Reference a Parameter](#233-how-to-reference-a-parameter)
    - [2.3.4. Concept: Pseudo Parameters](#234-concept-pseudo-parameters)
    - [2.3.5. Systems Manager parameter](#235-systems-manager-parameter)
  - [2.4. Mappings](#24-mappings)
    - [2.4.1. Accessing Mapping Values Fn::FindInMap](#241-accessing-mapping-values-fnfindinmap)
    - [2.4.2. When would you use Mappings vs Parameters?](#242-when-would-you-use-mappings-vs-parameters)
  - [2.5. Outputs](#25-outputs)
    - [2.5.1. Example](#251-example)
    - [2.5.2. Cross Stack Reference](#252-cross-stack-reference)
  - [2.6. Conditions](#26-conditions)
    - [2.6.1. How to define a condition?](#261-how-to-define-a-condition)
    - [2.6.2. Using a Condition](#262-using-a-condition)
- [3. Intrinsic Functions](#3-intrinsic-functions)
  - [3.1. Fn::Ref](#31-fnref)
  - [3.2. Fn::GetAtt](#32-fngetatt)
  - [3.3. Fn::FindInMap](#33-fnfindinmap)
  - [3.4. Fn::ImportValue](#34-fnimportvalue)
  - [3.5. Fn::Base64](#35-fnbase64)
  - [3.6. Fn::Join](#36-fnjoin)
  - [3.7. Fn::Sub](#37-fnsub)
  - [3.8. Condition Functions](#38-condition-functions)
  - [3.9. Fn::GetAZs](#39-fngetazs)
  - [3.10. Fn::Select](#310-fnselect)
- [4. Rollbacks](#4-rollbacks)
- [5. Stack Notifications](#5-stack-notifications)
- [6. ChangeSets](#6-changesets)
- [7. Retaining Data on Deletes](#7-retaining-data-on-deletes)
- [8. Termination Protection on Stacks](#8-termination-protection-on-stacks)
- [9. User Data in EC2 for CloudFormation](#9-user-data-in-ec2-for-cloudformation)
  - [9.1. CFN-Init and EC2 User Data, what?!!](#91-cfn-init-and-ec2-user-data-what)
  - [9.2. The Problems with EC2 User Data](#92-the-problems-with-ec2-user-data)
  - [9.3. CloudFormation helper scripts reference](#93-cloudformation-helper-scripts-reference)
  - [9.4. AWS::CloudFormation::Init](#94-awscloudformationinit)
    - [9.4.1. cfn-init](#941-cfn-init)
    - [9.4.2. cfn-signal \& wait conditions](#942-cfn-signal--wait-conditions)
    - [9.4.3. cfn-hup](#943-cfn-hup)
- [10. Nested stacks](#10-nested-stacks)
- [11. CloudFormation - Cross vs Nested Stacks](#11-cloudformation---cross-vs-nested-stacks)
- [12. Continue Rolling Back an Update](#12-continue-rolling-back-an-update)
- [13. Custom Resources](#13-custom-resources)
  - [13.1. How to define a Custom Resource?](#131-how-to-define-a-custom-resource)
  - [13.2. Use cases](#132-use-cases)
    - [13.2.1. Non-empty S3 bucket](#1321-non-empty-s3-bucket)
- [14. Service Role](#14-service-role)
- [15. CloudFormation Capabilities](#15-cloudformation-capabilities)
- [16. SSM Parameter Type](#16-ssm-parameter-type)
- [17. Dynamic References](#17-dynamic-references)
  - [17.1. Dynamic Reference: ssm](#171-dynamic-reference-ssm)
  - [17.2. Dynamic Reference: ssm-secure](#172-dynamic-reference-ssm-secure)
  - [17.3. Dynamic Reference: secretsmanager](#173-dynamic-reference-secretsmanager)
- [18. StackSets](#18-stacksets)
  - [18.1. StackSet Operations](#181-stackset-operations)
  - [18.2. StackSet Deployment Options](#182-stackset-deployment-options)
  - [18.3. Permission Models for StackSet](#183-permission-models-for-stackset)
  - [18.4. StackSets with AWS Organizations](#184-stacksets-with-aws-organizations)
  - [18.5. StackSet Drift Detection](#185-stackset-drift-detection)
- [19. Stack Policies](#19-stack-policies)

# 1. Introduction Infrastructure as Code (IaC)

- Currently, we have been doing a lot of manual work.
- All this manual work will be very tough to reproduce:
  - In another region.
  - in another AWS account.
  - Within the same region if everything was deleted.
- Wouldn't it be great, if all our infrastructure was... code?
- That code would be deployed and create / update / delete our infrastructure.

## 1.1. What is CloudFormation

- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).
- For example, within a CloudFormation template, you say:
  - I want a security group.
  - I want two [EC2](AWS%20EC2.md) machines using this security group.
  - I want two Elastic IPs for these [EC2](AWS%20EC2.md) machines.
  - I want an [S3](Amazon%20S3.md) bucket.
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
- **Separation of concern: create many stacks for many apps, and many layers**
  - Examples:
    - VPC stacks.
    - Network stacks.
    - App stacks.
- **Don't re-invent the wheel**
  - Leverage existing templates on the web!
  - Leverage the documentation.

## 1.3. How CloudFormation Works

- Templates have to be uploaded in S3 and then referenced in CloudFormation.
- To update a template, we can't edit previous ones.
- We have to re-upload a new version of the template to AWS.
- Stacks are identified by a name.
- Deleting a stack deletes every single artifact that was created by CloudFormation.

![How CloudFormation Works](/Images/AWSCloudformationDiagram.png)

## 1.4. Deploying CloudFormation templates

- Manual way:
  - Editing templates in the CloudFormation Designer.
  - Using the console to input parameters, etc.
- Automated way:
  - Editing templates in a YAML file.
  - Using the AWS CLI (Command Line Interface) to deploy the templates.
  - Recommended way when you fully want to automate your flow.

## 1.5. Building Blocks

- **Template's components**
  - `AWSTemplateFormatVersion`: Identifies the capabilities of the template "2010-09-09".
  - Description: Comments about the template.
  - Resources: Your AWS resources declared in the template.
  - Parameters: The dynamic inputs for your template.
  - Mappings: The static variables for your template.
  - Outputs: References to what has been created.
  - Conditionals: List of conditions to perform resource creation.
- **Templates helpers**
  - References.
  - Functions.

# 2. YAML

- YAML and JSON are the languages you can use for CloudFormation.
  - JSON
  - YAML (better)
- Key value Pairs.
- Nested objects.
- Support Arrays.
- Multi line strings.
- Can include comments.

## 2.1. Template anatomy

```
  AWSTemplateFormatVersion: "version date"

  Description:
    String

  Metadata:
    template metadata

  Parameters:
    set of parameters

  Rules:
    set of rules

  Mappings:
    set of mappings

  Conditions:
    set of conditions

  Transform:
    set of transforms

  Resources:
    set of resources

  Outputs:
    set of outputs
```

## 2.2. Resources

- Resources are the core of your CloudFormation template.
- They represent the different AWS Components that will be created and configured.
- Resources are declared and can reference each other.
- AWS figures out creation, updates and deletes of resources for us.
- There are over ~700 types of resources.
- Resource types identifiers are of the form:
  - `Type: service-provider::service-name::data-type-name`
  - Example:
    ```
      Resources:
        MyInstance:
          Type: AWS::EC2::Instance
          Properties:
            AvailabilityZone: sa-east-1a
            ImageId: ami-3sd89f07
            InstanceType: t2.micro
    ```

### 2.2.1. How do I find resources documentation?

- [All the resources can be found here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
- [Example here (for an EC2 instance)](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html)

### 2.2.2. Extras

- You can create a dynamic amount of resources using CloudFormation Macros and Transform.
- Is every AWS Service supported?
  - Almost. Only a select few niches are not there yet.
  - You can work around that using **AWS CloudFormation Custom Resources**.

## 2.3. Parameters

- Parameters are a way to provide inputs to your AWS CloudFormation template.
- They're important to know about if:
  - You want to **reuse** your templates across the company.
  - Some inputs can not be determined ahead of time.
- Parameters are extremely powerful, controlled, and can prevent errors from happening in your templates thanks to types.

### 2.3.1. When should you use a parameter?

- Ask yourself this:
  - Is this CloudFormation resource configuration likely to change in the future?
  - If so, make it a parameter.
- You won't have to re-upload a template to change its content.
- Example:
  ```
    Parameters:
      SecurityGroupDescription:
        Description: Security Group Description
        Type: String
  ```

### 2.3.2. Parameters Settings

- Parameters can be controlled by all these settings:
  - Type:
    - `String`
    - `Number`
    - `CommaDelimitedList`
    - `List<Type>`
    - `AWS-Specific` Parameter (to help catch invalid values - match against existing values in the AWS Account)
    - [SSM Parameter Store](AWS%20Systems%20Manager.md)
  - Description
  - ConstraintDescription (String)
  - Min/MaxLength
  - Min/MaxValue
  - Defaults
  - AllowedValues (array)
  - AllowedPattern (regexp)
  - NoEcho (Boolean)

### 2.3.3. How to Reference a Parameter

- The `Fn::Ref` function can be leveraged to reference parameters.
- Parameters can be used anywhere in a template.
- The shorthand for this in YAML is `!Ref`
- The function can also reference other elements within the template.
- Example:
  ```
    Parameters:
      SecurityGroupDescription:
        Description: Security Group Description
        Type: String
  ```
  ```
    ServerSecurityGroup:
      Type: AWS::EC2::SecurityGroup
      Properties:
        GroupDescription: !Ref SecurityGroupDescription
  ```

### 2.3.4. Concept: Pseudo Parameters

- AWS offers us pseudo parameters in any CloudFormation template.
- These can be used at any time and are enabled by default.
- Important pseudo parameters:
  | Reference Value | Example Return Value |
  | --------------------- | ------------------------------------------------------------------ |
  | AWS::AccountId | 1234567890 (ID Account AWS) |
  | AWS::Region | us-east-2 |
  | AWS::StackId | rn:aws:cloudformation:us-east-1:123456789012:stack/MyStack/1c2fa62 |
  | AWS::StackName | MyStack |
  | AWS::NotificationARNs | [arn:aws:sns:us-east-1:123456789012:MyTopic] |
  | AWS::NoValue | Does not return a value |

### 2.3.5. Systems Manager parameter

- CloudFormation parameters already support certain AWS specific types.
- SSM parameter types will be an addition to these types.
- New parameter types introduced in CloudFormation are:

- `AWS::SSM::Parameter::Name`
- `AWS::SSM::Parameter::Value<String>`
- `AWS::SSM::Parameter::Value<List<String>>`
- `AWS::SSM::Parameter::Value<Any AWS type>`

- Example:

  ```
    aws ssm put-parameter --name myEC2TypeDev --type String --value "t2.small"

    Parameters:
      InstanceType:
        Type: 'AWS::SSM::Parameter::Value<String>'
        Default: myEC2TypeDev
  ```

  ```
    MyIAMUser:
      Type: AWS::IAM::User
      Properties:
        UserName: 'MyUserName'
        LoginProfile:
          Password: '{{resolve:ssm-secure:IAMUserPassword:10}}'
  ```

## 2.4. Mappings

- Mappings are fixed variables within your CloudFormation Template.
- They're very handy to differentiate between different environments (dev vs prod), regions (AWS regions), AMI types, etc.
- All the values are hardcoded within the template.
- Example:
  ```
    Mappings:
      Mapping01:
        Key01:
          Name: Value01
        Key02:
          Name: Value02
        Key03:
          Name: Value03
  ```
- Real example:
  ```
    Mappings:
      RegionMap:
        us-east-1:
          "HVM64": "ami-0ff8a91507f77f867"
        us-west-1:
          "HVM64": "ami-0bdb828fd58c52235"
        eu-west-1:
          "HVM64": "ami-047bb4163c506cd98"
        ap-southeast-1:
          "HVM64": "ami-08569b978cc4dfa10"
        ap-northeast-1:
          "HVM64": "ami-06cd52961ce9f0d85"
  ```

### 2.4.1. Accessing Mapping Values Fn::FindInMap

- We use `Fn::FindInMap` to return a named value from a specific key:
  - `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
- Example with pseudo parameter `AWS::Region`:
  ```
    AWSTemplateFormatVersion: "2010-09-09"
      Mappings:
        RegionMap:
          us-east-1:
            HVM64: ami-0ff8a91507f77f867
            HVMG2: ami-0a584ac55a7631c0c
          us-west-1:
            HVM64: ami-0bdb828fd58c52235
            HVMG2: ami-066ee5fd4a9ef77f1
          eu-west-1:
            HVM64: ami-047bb4163c506cd98
            HVMG2: ami-0a7c483d527806435
          ap-northeast-1:
            HVM64: ami-06cd52961ce9f0d85
            HVMG2: ami-053cdd503598e4a9d
          ap-southeast-1:
            HVM64: ami-08569b978cc4dfa10
            HVMG2: ami-0be9df32ae9f92309
      Resources:
        myEC2Instance:
        Type: "AWS::EC2::Instance"
        Properties:
          ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
          InstanceType: m1.small
  ```

### 2.4.2. When would you use Mappings vs Parameters?

- Mappings are great when you know in advance all the values that can be taken and that they can be deduced from variables such as:
  - Region.
  - Availability Zone.
  - AWS Account.
  - Environment (dev vs prod).
  - Etc...
- They allow safer control over the template.
- Use parameters when the values are really user specific.

## 2.5. Outputs

- The Outputs section declares **optional** outputs values that we can import into other stacks (if you export them first)!
- You can also view the outputs in the AWS Console or in using the AWS CLI.
- They're very useful for example if you define a network CloudFormation, and output the variables such as VPC ID and your Subnet IDs.
- It's the best way to perform some collaboration cross stack, as you let expert handle their own part of the stack.

### 2.5.1. Example

- Creating a VPC as part of one template.
- We create an output that references that VPC.
- Example:
  ```
    Outputs:
      StackVPC:
        Description: The ID of the VPC
        Value: !Ref MyVPC
        Export:
          Name: MyBeautifulVPC
  ```

### 2.5.2. Cross Stack Reference

- We then create a second template that leverages that security group.
- For this, we use the `Fn::ImportValue` function.
- You can't delete a CloudFormation Stack if its outputs are being referenced by another CloudFormation stack.
  - You can't delete the underlying stack until all the references are deleted too.
- Example:
  ```
    Outputs:
      StackSSHSecurityGroup:
      Description: The SSH Security Group for our Company
      Value: !Ref MyBeautifulSSHSecurityGroup
      Export:
        Name: SSHMyBeautifulSecurityGroup
  ```
  ```
    Resources:
      MySecureInstance:
        Type: AWS:: EC2::Instance
        Properties:
          AvailabilityZone: us-east-1a
          ImageId: ami-980s890
          InstanceType: t2.micro
          SecurityGroups:
            - !ImportValue SSHMyBeautifulSecurityGroup
  ```
- You can use the Export Output Values to export the name of the resource output for a cross-stack reference.
  - **For each AWS account, export names must be unique within a region.**
  -

## 2.6. Conditions

- Conditions are used to control the creation of **Resources** or **Outputs** based on a condition.
  - **NOT Parameter.**
- Conditions can be whatever you want them to be, but common ones are:
  - Environment (dev / test / prod).
  - AWS Region.
  - Any parameter value.
- Each condition can reference another condition, parameter value or mapping.

### 2.6.1. How to define a condition?

- Example:
  ```
    Conditions:
      CreateProdResources: !Equals
        - !Ref EnvType
        - prod
  ```
- The logical ID is for you to choose.
  - It's how you name condition.
- The intrinsic function **(logical)** can be any of the following:
  - `Fn::And`
  - `Fn::Equals`
  - `Fn::If`
  - `Fn::Not`
  - `Fn::Or`

### 2.6.2. Using a Condition

- Conditions can be applied to **Resources**, **Outputs** and etc...
  - **NOT Parameter.**
- Example:
  ```
    Resources:
      MountPoint:
        Type: 'AWS::EC2::VolumeAttachment'
        Condition: CreateProdResources
  ```

# 3. Intrinsic Functions

- `Ref`
- `Fn::GetAtt`
- `Fn::FindInMap`
- `Fn::ImportValue`
- `Fn::Join`
- `Fn::Sub`
- `Fn::ForEach`
- `Fn::ToJsonString`
- `Fn::Base64`
- `Fn::Cidr`
- `Fn::GetAZs`
- `Fn::Select`
- `Fn::Split`
- `Fn::Transform`
- `Fn::Length`
- Condition Functions (`Fn::If`, `Fn::Not`, `Fn::Equals`, etc...).

## 3.1. Fn::Ref

- The `Fn::Ref` function can be leveraged to reference.
  - **Parameters** => returns the value of the parameter.
  - **Resources** => returns the physical ID of the underlying resource (e.g., EC2 ID).
- The shorthand for this in YAML is `!Ref`.

## 3.2. Fn::GetAtt

- Attributes are attached to any resources you create.
- To know the attributes of your resources, the best place to look at is the documentation.
- For example: the AZ of an EC2 machine!
- Example:

  ```
    AWSTemplateFormatVersion: 2010-09-09
    Resources:
      myELB:
        Type: AWS::ElasticLoadBalancing::LoadBalancer
        Properties:
          AvailabilityZones:
            - eu-west-1a
          Listeners:
            - LoadBalancerPort: '80'
              InstancePort: '80'
              Protocol: HTTP
      myELBIngressGroup:
        Type: AWS::EC2::SecurityGroup
        Properties:
          GroupDescription: ELB ingress group
          SecurityGroupIngress:
            - IpProtocol: tcp
              FromPort: 80
              ToPort: 80
              SourceSecurityGroupOwnerId: !GetAtt myELB.SourceSecurityGroup.OwnerAlias
              SourceSecurityGroupName: !GetAtt myELB.SourceSecurityGroup.GroupName

  ```

## 3.3. Fn::FindInMap

- We use `Fn::FindInMap` to return a named value from a specific key.
  - `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`.

## 3.4. Fn::ImportValue

- Import values that are exported in other templates.
- For this, we use the `Fn::ImportValue` function.

## 3.5. Fn::Base64

- Convert `String` to it's `Base64` representation.

## 3.6. Fn::Join

- Join values with a delimiter.
- This creates "a:b:c".
- Example:
  ```
    !Join [ ":", [ a, b, c ] ]
  ```

## 3.7. Fn::Sub

- `Fn::Sub`, or `!Sub` as a shorthand, is used to substitute variables from a text.
  - It's a very handy function that will allow you to fully customize your templates.
- For example, you can combine `Fn::Sub` with References or AWS Pseudo variables!
- **String** must contain `${VariableName}` and will substitute them.
- Example:
  ```
    Name: !Sub
      - 'www.${Domain}'
      - Domain: !Ref RootDomainName
  ```

## 3.8. Condition Functions

- The logical ID is for you to choose.
  - It's how you name condition.
- The intrinsic function **(logical)** can be any of the following:
  - `Fn::And`
  - `Fn::Equals`
  - `Fn::If`
  - `Fn::Not`
  - `Fn::Or`

## 3.9. Fn::GetAZs

- Returns an array that lists Availability Zones for a specified region in alphabetical order.

## 3.10. Fn::Select

- Returns a single object from a list of objects by index.

# 4. Rollbacks

- **Stack Creation Fails**
  - Default: everything rolls back (gets deleted). We can look at the log.
  - Option to disable rollback and troubleshoot what happened.
- **Stack Update Fails**
  - The stack automatically rolls back to the previous known working state.
  - Ability to see in the log what happened and error messages.
- **IMPORTANT!** Fix resources manually then issue `ContinueUpdateRollback` API from Console.
  - Or from the CLI using `continue-update-rollback` API call.
- CloudFormation to continue the stack rollback and can help resolve the `UPDATE_ROLLBACK_FAILED` state.

# 5. Stack Notifications

- Send Stack events to SNS Topic (Email, Lambda, ...)
- Enable SNS Integration using Stack Options

# 6. ChangeSets

- When you update a stack, you need to know what changes before it happens for greater confidence.
- ChangeSets won't say if the update will be successful.

# 7. Retaining Data on Deletes

- You can put a DeletionPolicy on any resource to control what happens when the CloudFormation template is deleted.
- DeletionPolicy = Retain:
  - Specify on resources to preserve / backup in case of CloudFormation deletes.
  - To keep a resource, specify Retain (works for any resource / nested stack).
- DeletionPolicy = Snapshot:
  - EBS Volume, ElastiCache Cluster, ElastiCache ReplicationGroup.
  - RDS DBInstance, RDS DBCluster, Redshift Cluster.
- DeletePolicy = Delete (default behavior):
  - Note: for `AWS::RDS::DBCluster` resources, the default policy is Snapshot.
  - Note: to delete an S3 bucket, you need to first empty the bucket of its content.

# 8. Termination Protection on Stacks

- To prevent accidental deletes of CloudFormation templates, use `TerminationProtection`.

# 9. User Data in EC2 for CloudFormation

- The important thing to pass is the entire script through the function
  `Fn::Base64`.
- Good to know, user data script log is in /var/log/cloud-init-output.log

## 9.1. CFN-Init and EC2 User Data, what?!!

- Many of the CloudFormation templates will be about provisioning computing resources in your AWS Cloud.
- These resources can be either:
  - EC2 Instances.
  - Auto Scaling Groups.
  - etc...
- Usually, you want the instances to be self configured so that they can perform the job they're supposed to perform.
- You can fully automate your EC2 fleet state with CloudFormation Init.

## 9.2. The Problems with EC2 User Data

- What if we want to have a very large instance configuration?
- What if we want to evolve the state of the EC2 instance without terminating it and creating a new one?
- How do we make EC2 user-data more readable?
- How do we know or signal that our EC2 user-data script completed successfully?

## 9.3. CloudFormation helper scripts reference

- AWS CloudFormation provides the following Python helper scripts that you can use to install software and start services on an [Amazon EC2](AWS%20EC2.md) instance that you create as part of your stack:
- We have 4 Python scripts, that come directly on Amazon Linux 2 AMI, or can be installed using `yum` on non-Amazon Linux AMIs.
  - `cfn-init`: Use to retrieve and interpret resource metadata, install packages, create files, and start services.
  - `cfn-signal`: Use to signal with a `CreationPolicy` or `WaitCondition`, so you can synchronize other resources in the stack when the prerequisite resource or application is ready.
  - `cfn-get-metadata`: Use to retrieve metadata for a resource or path to a specific key.
  - `cfn-hup`: Use to check for updates to metadata and execute custom hooks when changes are detected.
- Usual flow: `cfn-init`, then `cfn-signal`, then optionally `cfn-hup`.
- You call the scripts directly from your template.
- The scripts work in conjunction with resource metadata that's defined in the same template.
- The scripts run on the [Amazon EC2](AWS%20EC2.md) instance during the stack creation process.

## 9.4. AWS::CloudFormation::Init

- A **config** contains the following and is executed in that order:
  - **Packages:** used to download and install pre-packaged apps and components on Linux/Windows (ex. MySQL, PHP, etc...).
  - **Groups:** define user groups - Users: define users, and which group they belong to.
  - **Sources:** download files and archives and place them on the EC2 instance.
  - **Files:** create files on the EC2 instance, using inline or can be pulled from a URL.
  - **Commands:** run a series of commands.
  - **Services:** launch a list of sysvinit.
- You can have multiple **configs** in your AWS::CloudFormation::Init
- You create configSets with multiple **configs**.
- And you invoke `configSets` from your EC2 user-data.

### 9.4.1. cfn-init

- With the `cfn-init` script, it helps make complex EC2 configurations readable.
- The EC2 instance will query the CloudFormation service to get init data.
- Logs go to `/var/log/cfn-init.log`.

### 9.4.2. cfn-signal & wait conditions

- We still don't know how to tell CloudFormation that the EC2 instance got properly configured after a `cfn-init`.
- For this, we can use the `cfn-signal` script!
  - We run `cfn-signal` right after `cfn-init`.
  - Tell CloudFormation service that the resource creation success/fail to keep on going or fail.
- We need to define `WaitCondition`:
  - Block the template until it receives a signal from cfn-signal.
  - We attach a `CreationPolicy` (also works on EC2, ASG).
  - We can define a `Count` > 1 (in case you need more than 1 signal).

### 9.4.3. cfn-hup

- Can be used to tell your EC2 instance to look for Metadata changes every 15 minutes and apply the Metadata configuration again.
- It's very powerful but you really need to try it out to understand how it works.
- It relies on a `cfn-hup` configuration, see **/etc/cfn/cfn-hup.conf** and **/etc/cfn/hooks.d/cfn-auto-reloader.con**

# 10. Nested stacks

- Nested stacks are stacks as part of other stacks.
- They allow you to isolate repeated patterns / common components in separate stacks and call them from other stacks.
- Example:
  - Load Balancer configuration that is re-used.
  - Security Group that is re-used.
- Nested stacks are considered best practice.
- To update a nested stack, always update the parent (root stack).
- You create a nested stack within another stack by using the `AWS::CloudFormation::Stack` resource.

![Nested stacks](/Images/AWSCloudFormationNestedStacks.png)

# 11. CloudFormation - Cross vs Nested Stacks

- **Cross Stacks**
  - Helpful when stacks have different lifecycles.
  - Use Outputs Export and `Fn::ImportValue`.
  - When you need to pass export values to many stacks (VPC Id, etc...).
- **Nested Stacks**
  - Helpful when components must be re-used.
  - Ex: re-use how to properly configure an Application Load Balancer.
  - The nested stack only is important to the higher level stack (it's not shared).

# 12. Continue Rolling Back an Update

- A stack goes into the `UPDATE_ROLLBACK_FAILED` state when CloudFormation can't roll back all changes during an update.
- A resource can't return to its original state, causing the rollback to fail.
- Example: Roll back to an old database instance that was deleted outside CloudFormation.
- Solutions:
  - Fix the errors manually outside of CloudFormation and then continue update rollback the stack.
  - Skip the resources that can't rollback successfully (CloudFormation will mark the failed resources as `UPDATE_COMPLETE`).
- You can't update a stack in this state.
- For nested stacks, rolling back the parent stack will attempt to roll back all the child stacks as well.

# 13. Custom Resources

- Enable you to write custom provision logic in templates that AWS CloudFormation runs anytime you create, update, delete stacks.
- Defined in the template using `AWS::CloudFormation::CustomResource` or `Custom::MyCustomResourceTypeName` (recommended).
- Two types:
  - Amazon SNS-backed Custom Resources.
  - AWS Lambda-backed Custom Resources.
- Use cases:
  - An AWS resource is not covered yet (new service for example).
  - An on-premises resource.
  - Running a Lambda function to empty an S3 bucket before being deleted.
  - Fetch an AMI id.
  - Anything you want...!

## 13.1. How to define a Custom Resource?

- `ServiceToken` specifies where CloudFormation sends requests to, such as **Lambda ARN** or **SNS ARN** (required & must be in the same region).
- Input data parameters (optional).

![AWS CloudFormation Custom Resources](/Images/AWSCloudFormationCustomResources.png)

## 13.2. Use cases

### 13.2.1. Non-empty S3 bucket

- You can't delete a non-empty S3 bucket.
- To delete a non-empty S3 bucket, you must first delete all the objects inside it.

![AWS CloudFormation Custom Resource Non-empty S3 bucket case](/Images/AWSCloudFormationCustomResourceNonEmptyS3Bucket.png)

# 14. Service Role

- IAM role that allows CloudFormation to create/update/delete stack resources on your behalf.
- Give ability to users to create/update/delete the stack resources even if they don't have permissions to work with the resources in the stack.
- By default, CloudFormation uses a temporary session that it generates from your user credentials.
- Use cases:
  - You want to achieve the least privilege principle.
  - But you don't want to give the user all the required permissions to create the stack resources.
- User must have `iam:PassRole` permissions.

![Service Role](/Images/AWSCloudFormationServiceRole.png)

# 15. CloudFormation Capabilities

- `CAPABILITY_NAMED_IAM` and `CAPABILITY_IAM`.
  - Necessary to enable when you CloudFormation template is creating or updating IAM resources (IAM User, Role, Group, Policy, Access Keys, Instance Profile...).
  - Specify `CAPABILITY_NAMED_IAM` if the resources are named.
- `CAPABILITY_AUTO_EXPAND`
  - Necessary when your CloudFormation template includes Macros or Nested Stacks (stacks within stacks) to perform dynamic transformations.
  - You're acknowledging that your template may change before deploying.
- `InsufficientCapabilitiesException`
  - Exception that will be thrown by CloudFormation if the capabilities haven't been acknowledge when deploying a template (security measure).

# 16. SSM Parameter Type

- Reference parameters in Systems Manager Parameter Store.
- Specify SSM parameter key as the value.
- CloudFormation always fetches the latest value (you can't specify parameter version).
- CloudFormation doesn't store Secure String values.
- Validation done on SSM parameter keys, but not values.
- Supported SSM Parameter Types:
  - `AWS::SSM::Parameter::Name`
  - `AWS::SSM::Parameter::Value<String>`
  - `AWS::SSM::Parameter::Value<List<String>>` or `AWS::SSM::Parameter::Value<CommaDelimitedList>`
  - `AWS::SSM::Parameter::Value<AWS-Specific Parameter>`
  - `AWS::SSM::Parameter::Value<List<AWS-Specific Parameter>>`

# 17. Dynamic References

- Reference external values stored in SSM Parameter Store and AWS Secrets Manager within CloudFormation templates.
- CloudFormation retrieves the value of the specified reference during stack and change set operations.
- For example: retrieve RDS DB Instance master password from AWS Secrets Manager.
- Supports
  - `ssm` - For plaintext values stored in SSM Parameter Store
  - `ssm-secure` - For secure strings stored in SSM Parameter Store
  - `secretsmanager` - For secret values stored in AWS Secrets Manager
- Up to 60 dynamic references in a template.
- `{{resolve:service-name:reference-key}}`

## 17.1. Dynamic Reference: ssm

- Reference values stored in SSM Parameter Store of type `String` and `StringList`.
- If no version specified, CloudFormation uses the latest version.
- Doesn't support public SSM parameters (e.g., Amazon Linux 2 AMI).
- `{{resolve:ssm:parameter-name:version}}`

## 17.2. Dynamic Reference: ssm-secure

- Reference values stored in SSM Parameter Store of type `SecureString`.
- For example: passwords, license keys, etc...
- CloudFormation never stores the actual parameter value
- If no version specified, CloudFormation uses the latest version
- [Only use with supported resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/dynamic-references.html#template-parameters-dynamic-patterns-resources)
- `{{resolve:ssm-secure:parameter-name:version}}`

## 17.3. Dynamic Reference: secretsmanager

- Retrieve entire secrets or secret values stored in AWS Secrets Manager.
- For example: database credentials, passwords, 3rd party API keys, etc...
- To update a secret, you must update the resource containing the secretsmanager dynamic reference (one of the resource properties).
- `{{resolve:secretsmanager:secret-id:secret-string:json-key:version-stage:version-id}}`

# 18. StackSets

- Create, update, or delete stacks across **multiple accounts and regions** with a single operation.
- Administrator account to create StackSets.
- Trusted accounts to create, update, delete stack instances from StackSets.
- When you update a stack set, all associated stack instances are updated throughout all accounts and regions.
- Regional service.
- Can be applied into all accounts of an AWS organizations.

![CloudFormation StackSet](/Images/AWSCloudFormationStackSet.png)

## 18.1. StackSet Operations

- **Create StackSet**
  - Provide template + target accounts/regions.
- **Update StackSet**
  - Updates always affect all stacks (you can't selectively update some stacks in the StackSet but not others).
- **Delete Stacks**
  - Delete stack and its resources from target accounts/regions.
  - Delete stack from your StackSet (the stack will continue to run independently).
  - Delete all stacks from your StackSet (prepare for StackSet deletion).
- **Delete StackSet**
  - Must delete all stack instances within StackSet to delete it.

## 18.2. StackSet Deployment Options

- **Deployment Order**
  - Order of regions where stacks are deployed.
  - Operations performed one region at a time.
- **Maximum Concurrent Accounts**
  - Max. number/percentage of target accounts per region to which you can deploy stacks at one time.
- **Failure Tolerance**
  - Max. number/percentage (target accounts per region) of stack operation failures that can occur before CloudFormation stops operation in all regions.
- **Region Concurrency**
  - Whether StackSet deployed into regions `Sequential (default)` or `Parallel`
- **Retain Stacks**
  - Used when deleting StackSet to keep stacks and their resources running when removed from StackSet.

## 18.3. Permission Models for StackSet

- Self-managed Permissions
  - Create the IAM roles (with established trusted relationship) in both administrator and target accounts.
  - Deploy to any target account in which you have permissions to create IAM role.
- Service-managed Permissions
  - Deploy to accounts managed by AWS Organizations.
  - StackSets create the IAM roles on your behalf (**enable trusted access** with AWS Organizations).
  - Must **enable all features** in AWS Organizations.
  - Ability to deploy to accounts added to your organization in the future (Automatic Deployments).

## 18.4. StackSets with AWS Organizations

- Ability to **automatically** deploy Stack instances to new Accounts in an Organization.
- Can delegate StackSets administration to member accounts in AWS Organization.
- Trusted access with AWS Organizations must be enabled before delegated administrators can deploy to accounts managed by Organizations.

## 18.5. StackSet Drift Detection

- Performs drift detection on the stack associated with each stack instance in the StackSet.
- If the current state of a resource in a stack varies from the expected state:
  - The stack considered drifted.
  - And the stack instance that the stack associated with considered drifted.
  - And the StackSet is considered drifted.
- Drift detection identifies unmanaged changes (outside CloudFormation).
- Changes made through CloudFormation to a stack directly (not at the StackSet level), aren't considered drifted.
- You can stop drift detection on a StackSet.

# 19. Stack Policies

- During a CloudFormation Stack update, all update actions are allowed on all resources (default).
- **A Stack Policy is a JSON document that defines the update actions that are allowed on specific resources during Stack updates.**
- Protect resources from unintentional updates.
- When you set a Stack Policy, all resources in the Stack are protected by default.
- Specify an explicit ALLOW for the resources you want to be allowed to be updated.
