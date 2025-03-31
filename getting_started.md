---
title: Getting Started with Serverless Framework
description: Learn how to install Serverless Framework, configure your first project, and deploy a simple serverless application using AWS provider.
---

# Getting Started with Serverless Framework

This guide will help you get started with Serverless Framework, walking you through the installation process, basic configuration, and deploying your first serverless application using AWS as the provider.

## Prerequisites

Before you begin, make sure you have the following:

1. Node.js (version 12 or later) installed on your machine
2. An AWS account with appropriate permissions
3. AWS CLI installed and configured with your credentials

## Installation

To install Serverless Framework globally on your machine, run the following command:

```bash
npm install -g serverless
```

Verify the installation by running:

```bash
serverless --version
```

## Creating Your First Serverless Project

1. Create a new directory for your project and navigate to it:

```bash
mkdir my-serverless-project
cd my-serverless-project
```

2. Initialize a new Serverless Framework project:

```bash
serverless create --template aws-nodejs --name my-service
```

This command creates a new project using the AWS Node.js template.

## Configuring Your Project

Open the `serverless.yml` file in your project directory. This file is the main configuration file for your Serverless Framework project. Here's a basic example of what it might look like:

```yaml
service: my-service

provider:
  name: aws
  runtime: nodejs14.x
  stage: dev
  region: us-east-1

functions:
  hello:
    handler: handler.hello
    events:
      - http:
          path: hello
          method: get
```

Let's break down the main sections:

- `service`: The name of your service
- `provider`: Specifies the cloud provider (AWS in this case) and its settings
- `functions`: Defines the Lambda functions in your service

## Writing Your First Function

The template creates a `handler.js` file with a sample function. You can modify it or create new functions as needed. Here's an example:

```javascript
'use strict';

module.exports.hello = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify(
      {
        message: 'Hello from Serverless!',
        input: event,
      },
      null,
      2
    ),
  };
};
```

## Deploying Your Service

To deploy your service to AWS, run the following command:

```bash
serverless deploy
```

This command packages your service, uploads it to AWS S3, creates the necessary AWS CloudFormation stack, and deploys your functions.

After successful deployment, you'll see output similar to this:

```
Service Information
service: my-service
stage: dev
region: us-east-1
stack: my-service-dev
resources: 11
api keys:
  None
endpoints:
  GET - https://abcdefghij.execute-api.us-east-1.amazonaws.com/dev/hello
functions:
  hello: my-service-dev-hello
layers:
  None
```

You can now test your function by accessing the provided endpoint URL.

## Next Steps

Congratulations! You've successfully created and deployed your first serverless application using Serverless Framework. Here are some next steps to explore:

1. Learn more about [serverless.yml configuration](https://www.serverless.com/framework/docs/providers/aws/guide/serverless.yml/)
2. Explore [AWS Lambda function configuration](https://www.serverless.com/framework/docs/providers/aws/guide/functions/)
3. Understand [event sources and triggers](https://www.serverless.com/framework/docs/providers/aws/guide/events/)
4. Dive into [Serverless Framework plugins](https://www.serverless.com/framework/docs/guides/plugins/)

Happy serverless coding!