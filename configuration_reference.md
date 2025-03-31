---
title: 'Configuration Reference'
menuText: 'Configuration Reference'
description: 'Detailed reference for all configuration options in serverless.yml'
layout: Doc
---

# Serverless Framework Configuration Reference

This document provides a comprehensive reference for all configuration options available in the `serverless.yml` file. While the primary focus is on the AWS provider, some general configurations apply to all providers.

## Table of Contents

1. [Service Configuration](#service-configuration)
2. [Provider Configuration](#provider-configuration)
3. [Functions Configuration](#functions-configuration)
4. [Events Configuration](#events-configuration)
5. [Resources Configuration](#resources-configuration)
6. [Layers Configuration](#layers-configuration)
7. [Package Configuration](#package-configuration)
8. [Custom Configuration](#custom-configuration)

## Service Configuration

The `service` section defines the name of your service and some optional configurations.

```yaml
service: my-service-name
```

Optional properties:

- `tenant`: Specify the Serverless Dashboard tenant
- `app`: Specify the Serverless Dashboard application
- `org`: Specify the Serverless Dashboard organization

## Provider Configuration

The `provider` section defines the cloud provider and its global configurations.

```yaml
provider:
  name: aws
  runtime: nodejs14.x
  stage: dev
  region: us-east-1
```

### AWS Provider Specific Configurations

- `apiGateway`: Configure API Gateway settings
- `iam`: Define IAM role statements and permissions
- `environment`: Set environment variables for all functions
- `deploymentBucket`: Configure the S3 bucket for deployments
- `stackTags`: Add tags to your CloudFormation stack
- `logs`: Configure CloudWatch logs retention and format

Example:

```yaml
provider:
  name: aws
  apiGateway:
    minimumCompressionSize: 1024
  iam:
    role:
      statements:
        - Effect: Allow
          Action:
            - s3:GetObject
          Resource: "arn:aws:s3:::my-bucket/*"
  environment:
    TABLE_NAME: my-table
  logs:
    restApi:
      accessLogging: true
      executionLogging: true
      level: INFO
      fullExecutionData: true
```

## Functions Configuration

Define your Lambda functions in the `functions` section.

```yaml
functions:
  hello:
    handler: handler.hello
    events:
      - http:
          path: users/create
          method: get
```

Function-specific configurations:

- `handler`: The function entrypoint
- `name`: Custom name for the Lambda function (optional)
- `description`: Description of the function
- `runtime`: Override the default runtime for this function
- `memorySize`: The amount of memory allocated to the function
- `timeout`: The function timeout in seconds
- `environment`: Function-specific environment variables
- `tags`: Function-specific tags

## Events Configuration

Events trigger Lambda functions. Different event types are available depending on the provider.

### AWS Event Types

- `http`: API Gateway HTTP endpoint
- `websocket`: API Gateway WebSocket
- `s3`: S3 bucket events
- `schedule`: CloudWatch Events scheduled events
- `sns`: SNS topics
- `sqs`: SQS queues
- `stream`: Kinesis or DynamoDB streams
- `alexaSkill`: Alexa Skills
- `alexaSmartHome`: Alexa Smart Home
- `iot`: IoT events
- `cloudFront`: CloudFront events
- `eventBridge`: EventBridge (CloudWatch Events) events

Example:

```yaml
functions:
  processS3Upload:
    handler: handler.processUpload
    events:
      - s3:
          bucket: my-upload-bucket
          event: s3:ObjectCreated:*
```

## Resources Configuration

Define or modify AWS resources using CloudFormation syntax in the `resources` section.

```yaml
resources:
  Resources:
    NewResource:
      Type: AWS::S3::Bucket
      Properties:
        BucketName: my-new-bucket
  Outputs:
    NewOutput:
      Description: "Description for the output"
      Value: "Some output value"
```

## Layers Configuration

Define Lambda Layers in the `layers` section.

```yaml
layers:
  hello:
    path: layer-dir
    name: ${self:provider.stage}-hello-layer
    description: "Description of the layer"
    compatibleRuntimes:
      - python3.8
```

## Package Configuration

Configure packaging behavior in the `package` section.

```yaml
package:
  individually: true
  exclude:
    - exclude-me.js
  include:
    - include-me.js
    - include-me-dir/**
```

## Custom Configuration

The `custom` section allows you to define custom variables and configurations.

```yaml
custom:
  myStage: ${opt:stage, self:provider.stage}
  myEnvironment:
    MESSAGE: "Hello World!"
```

You can reference these variables elsewhere in your `serverless.yml` using `${self:custom.VARIABLE_NAME}`.

Remember that the Serverless Framework is highly extensible, and plugins may add additional configuration options. Always refer to the specific documentation for any plugins you're using for their configuration details.