# AWS Lambda Handler Example

## Code Example

Here's a high-level example of a basic AWS Lambda function using TypeScript:

```typescript
import { Context, APIGatewayProxyCallback, APIGatewayEvent } from 'aws-lambda';

export const lambdaHandler = (event: APIGatewayEvent, context: Context, callback: APIGatewayProxyCallback): void => {
    console.log(`Event: ${JSON.stringify(event, null, 2)}`);
    console.log(`Context: ${JSON.stringify(context, null, 2)}`);
    callback(null, {
        statusCode: 200,
        body: JSON.stringify({
            message: 'hello world',
        }),
    });
};
```

## Lambda Handler Arguments

### Event Object
- Contains information from the invoker.
- AWS services provide various event structures.
- Use TypeScript annotations for the event object for better type safety.

### Context Object
- Provides information about the invocation, function, and execution environment.

### Callback
- A function to send a response in non-async handlers.
- Using `async/await` is recommended for better readability and error handling.

## Context Object Details

### Methods
- **`getRemainingTimeInMillis()`**: Returns milliseconds left before execution timeout.

### Properties
- **`functionName`**: Name of the Lambda function.
- **`functionVersion`**: Version of the function.
- **`invokedFunctionArn`**: ARN used to invoke the function.
- **`memoryLimitInMB`**: Allocated memory for the function.
- **`awsRequestId`**: Invocation request ID.
- **`logGroupName`**: Log group for the function.
- **`logStreamName`**: Log stream for the function instance.
- **`identity`**: (mobile apps) Cognito identity information.
- **`cognitoIdentityId`**: Authenticated Cognito identity.
- **`cognitoIdentityPoolId`**: Cognito identity pool that authorized invocation.
- **`clientContext`**: (mobile apps) Client context from the client application.
- **`callbackWaitsForEmptyEventLoop`**: Set to false to send response immediately when the callback runs.

## Example with Async/Await

```typescript
import { Context } from 'aws-lambda';

export const lambdaHandler = async (event: string, context: Context): Promise<string> => {
  console.log('Remaining time: ', context.getRemainingTimeInMillis());
  console.log('Function name: ', context.functionName);
  return context.logStreamName;
};
```

**Note:** Ensure to install `@types/aws-lambda` as a development dependency for TypeScript type definitions.

## Services That Can Invoke Lambda Functions

| Service                                     | Method of Invocation                    |
|---------------------------------------------|-----------------------------------------|
| Amazon Managed Streaming for Apache Kafka  | Event source mapping                    |
| Self-managed Apache Kafka                   | Event source mapping                    |
| Amazon API Gateway                          | Event-driven; synchronous invocation    |
| AWS CloudFormation                          | Event-driven; asynchronous invocation   |
| Amazon CloudWatch Logs                      | Event-driven; asynchronous invocation   |
| AWS CodeCommit                              | Event-driven; asynchronous invocation   |
| AWS CodePipeline                            | Event-driven; asynchronous invocation   |
| Amazon Cognito                              | Event-driven; synchronous invocation    |
| AWS Config                                  | Event-driven; asynchronous invocation   |
| Amazon Connect                              | Event-driven; synchronous invocation    |
| Amazon DynamoDB                             | Event source mapping                    |
| Amazon Elastic File System                  | Special integration                     |
| Elastic Load Balancing (ALB)                 | Event-driven; synchronous invocation    |
| Amazon EventBridge (CloudWatch Events)      | Event-driven; asynchronous invocation (event buses), synchronous or asynchronous invocation (pipes and schedules) |
| AWS IoT                                     | Event-driven; asynchronous invocation   |
| Amazon Kinesis                              | Event source mapping                    |
| Amazon Data Firehose                        | Event-driven; synchronous invocation    |
| Amazon Lex                                  | Event-driven; synchronous invocation    |
| Amazon MQ                                   | Event source mapping                    |
| Amazon Simple Email Service                 | Event-driven; asynchronous invocation   |
| Amazon Simple Notification Service          | Event-driven; asynchronous invocation   |
| Amazon Simple Queue Service                 | Event source mapping                    |
| Amazon Simple Storage Service (S3)          | Event-driven; asynchronous invocation   |
| Amazon Simple Storage Service Batch         | Event-driven; synchronous invocation    |
| Secrets Manager                             | Special integration                     |
| AWS Step Functions                          | Event-driven; synchronous or asynchronous invocation |
| Amazon VPC Lattice                          | Event-driven; synchronous invocation    |
| AWS X-Ray                                   | Special integration                     |

For more details on each service and invocation method, consult the AWS Lambda documentation.
