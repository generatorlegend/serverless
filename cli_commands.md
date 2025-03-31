# CLI Commands

This document provides a comprehensive list of commonly used CLI commands in the Serverless Framework. Each command includes a brief description, usage syntax, and examples.

## deploy

The `deploy` command is used to deploy your service to AWS.

### Usage

```
serverless deploy [options]
```

### Options

- `--stage`: Specify the stage for deployment (e.g., dev, prod)
- `--region`: Specify the AWS region for deployment
- `--verbose`: Show verbose output

### Examples

```
serverless deploy
serverless deploy --stage prod --region us-east-1
```

## invoke

The `invoke` command allows you to invoke a deployed function.

### Usage

```
serverless invoke [options]
```

### Options

- `--function` or `-f`: Specify the function name to invoke
- `--path`: Path to JSON file containing input data
- `--data` or `-d`: Input data as a string

### Examples

```
serverless invoke --function myFunction
serverless invoke -f myFunction --data '{"key": "value"}'
```

## invoke local

The `invoke local` command lets you invoke your function locally.

### Usage

```
serverless invoke local [options]
```

### Options

- `--function` or `-f`: Specify the function name to invoke
- `--path`: Path to JSON file containing input data
- `--data` or `-d`: Input data as a string

### Examples

```
serverless invoke local --function myFunction
serverless invoke local -f myFunction --path event.json
```

## logs

The `logs` command retrieves the logs of a deployed function.

### Usage

```
serverless logs [options]
```

### Options

- `--function` or `-f`: Specify the function name
- `--tail` or `-t`: Continuously stream logs
- `--startTime`: Specify the start time for the logs (e.g., 5m, 2h, 1d)

### Examples

```
serverless logs --function myFunction
serverless logs -f myFunction --tail
serverless logs -f myFunction --startTime 30m
```

## config

The `config` command is used to manage Serverless configuration.

### Usage

```
serverless config [options]
```

### Options

- `credentials`: Configure provider credentials

### Examples

```
serverless config credentials --provider aws --key YOUR_KEY --secret YOUR_SECRET
```

## create

The `create` command creates a new Serverless service.

### Usage

```
serverless create [options]
```

### Options

- `--template` or `-t`: Specify a template for the service
- `--path`: Specify a custom path for the service

### Examples

```
serverless create --template aws-nodejs
serverless create --template aws-python3 --path myService
```

Remember to check the Serverless Framework documentation for the most up-to-date and comprehensive information on CLI commands and their options.