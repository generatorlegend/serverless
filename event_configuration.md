# Event Configuration

This guide explains how to configure different event sources for serverless functions in AWS, focusing on API Gateway, S3, and CloudWatch Events (Schedule). Each event type has its own configuration options and best practices.

## API Gateway Events

API Gateway events allow you to trigger your Lambda functions in response to HTTP requests. To configure an API Gateway event, use the `http` event in your function configuration.

### Basic Configuration

```yaml
functions:
  myFunction:
    handler: handler.hello
    events:
      - http:
          path: users/create
          method: post
```

### Advanced Configuration

You can specify additional options such as CORS, authorization, and request parameters:

```yaml
functions:
  myFunction:
    handler: handler.hello
    events:
      - http:
          path: users/{id}
          method: get
          cors: true
          authorizer:
            name: myAuthorizer
            type: COGNITO_USER_POOLS
          request:
            parameters:
              paths:
                id: true
```

### Best Practices

- Use clear and descriptive paths for your API endpoints.
- Implement proper authorization for sensitive operations.
- Enable CORS if your API will be accessed from web browsers.
- Use request validation to ensure incoming data meets your requirements.

## S3 Events

S3 events allow your functions to respond to changes in S3 buckets, such as object creation or deletion.

### Basic Configuration

```yaml
functions:
  processUpload:
    handler: handler.processUpload
    events:
      - s3:
          bucket: my-bucket
          event: s3:ObjectCreated:*
```

### Advanced Configuration

You can specify rules to filter events based on object prefixes or suffixes:

```yaml
functions:
  processImage:
    handler: handler.processImage
    events:
      - s3:
          bucket: my-image-bucket
          event: s3:ObjectCreated:*
          rules:
            - prefix: uploads/
            - suffix: .jpg
```

### Best Practices

- Use event filtering to process only relevant objects.
- Consider using S3 bucket notifications for existing buckets.
- Be mindful of potential processing delays for large objects.

## CloudWatch Events (Schedule)

CloudWatch Events allow you to trigger functions on a schedule, similar to cron jobs.

### Basic Configuration

```yaml
functions:
  dailyReport:
    handler: handler.generateReport
    events:
      - schedule: rate(1 day)
```

### Advanced Configuration

You can use cron expressions for more complex schedules and add additional options:

```yaml
functions:
  weeklyCleanup:
    handler: handler.cleanup
    events:
      - schedule:
          rate: cron(0 2 ? * MON *)
          enabled: true
          input:
            key1: value1
            key2: value2
```

### Best Practices

- Use descriptive names for your scheduled events.
- Consider time zones when setting up schedules.
- Use the `enabled` flag to easily enable/disable scheduled events.
- Provide input data to your functions when necessary.

## General Best Practices

1. Keep your event configurations in the same file as your function definitions for better organization.
2. Use environment variables for sensitive information or values that might change between deployments.
3. Test your event configurations thoroughly, including error cases and edge scenarios.
4. Monitor your functions' performance and adjust configurations as needed.
5. Use IAM roles and policies to ensure your functions have the necessary permissions to interact with other AWS services.

Remember to refer to the AWS documentation for the most up-to-date information on event source configuration options and best practices.