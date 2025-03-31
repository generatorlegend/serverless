---
title: Monitoring and Logging Serverless Applications
description: Learn how to monitor and log serverless applications deployed with the Serverless Framework, focusing on AWS CloudWatch integration, log retrieval, and custom metrics.
---

# Monitoring and Logging Serverless Applications

When working with serverless applications deployed using the Serverless Framework, it's crucial to have proper monitoring and logging in place. This guide will walk you through how to effectively monitor and log your serverless applications, with a focus on AWS CloudWatch integration, log retrieval, and setting up custom metrics.

## AWS CloudWatch Integration

The Serverless Framework automatically integrates with AWS CloudWatch, providing you with out-of-the-box logging capabilities for your Lambda functions.

### Accessing CloudWatch Logs

To access your function's logs, you can use the Serverless Framework CLI:

```bash
serverless logs -f <function-name>
```

This command will retrieve the most recent logs for the specified function.

### Tailing Logs

To continuously stream logs in real-time, use the `--tail` option:

```bash
serverless logs -f <function-name> --tail
```

### Filtering Logs

You can filter logs using the `--filter` option:

```bash
serverless logs -f <function-name> --filter "Error"
```

This will only show log entries containing the word "Error".

## Customizing Log Retention

By default, CloudWatch Logs are set to never expire. You can customize the retention period in your `serverless.yml` file:

```yaml
provider:
  logRetentionInDays: 14
```

This sets the log retention period to 14 days for all functions in your service.

## Configuring Function-Level Logging

You can configure logging at the function level using the `logs` property:

```yaml
functions:
  hello:
    handler: handler.hello
    logs:
      logGroup:
        name: '/aws/lambda/my-custom-log-group'
      logFormat: JSON
      applicationLogLevel: INFO
      systemLogLevel: INFO
```

This configuration sets a custom log group name, specifies JSON as the log format, and sets both application and system log levels to INFO.

## Setting Up Custom Metrics

To gain more insights into your application's performance, you can set up custom metrics using CloudWatch.

### Creating a Custom Metric

1. In your Lambda function code, use the AWS SDK to publish custom metrics:

```javascript
const AWS = require('aws-sdk');
const cloudWatch = new AWS.CloudWatch();

module.exports.handler = async (event) => {
  // Your function logic here

  // Publish a custom metric
  await cloudWatch.putMetricData({
    MetricData: [{
      MetricName: 'MyCustomMetric',
      Dimensions: [
        {
          Name: 'FunctionName',
          Value: process.env.AWS_LAMBDA_FUNCTION_NAME
        }
      ],
      Unit: 'Count',
      Value: 1
    }],
    Namespace: 'MyServerlessApp'
  }).promise();

  // Return response
};
```

2. Ensure your Lambda function has the necessary IAM permissions to publish metrics to CloudWatch:

```yaml
provider:
  iam:
    role:
      statements:
        - Effect: Allow
          Action:
            - cloudwatch:PutMetricData
          Resource: '*'
```

### Viewing Custom Metrics

You can view your custom metrics in the AWS CloudWatch console or use the AWS CLI:

```bash
aws cloudwatch get-metric-statistics --namespace MyServerlessApp --metric-name MyCustomMetric --start-time 2023-05-01T00:00:00Z --end-time 2023-05-02T00:00:00Z --period 3600 --statistics Sum
```

## Best Practices

1. **Use structured logging**: Format your logs as JSON for easier parsing and analysis.
2. **Include relevant context**: Always include request IDs, function names, and other relevant information in your logs.
3. **Set appropriate log levels**: Use different log levels (DEBUG, INFO, WARN, ERROR) appropriately to make log analysis easier.
4. **Monitor cold starts**: Keep an eye on function initialization times to optimize performance.
5. **Set up alerts**: Use CloudWatch Alarms to get notified about unusual patterns or errors in your application.

By following these practices and utilizing the tools provided by the Serverless Framework and AWS CloudWatch, you can effectively monitor and log your serverless applications, ensuring better performance, easier debugging, and improved overall management of your serverless infrastructure.