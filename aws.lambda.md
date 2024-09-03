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


Here's a small TypeScript code snippet for handling basic operations for each type of AWS Lambda event. These examples assume that you're using AWS Lambda with the Node.js runtime and the `aws-lambda` types package.

```typescript
// Example TypeScript handlers for various AWS Lambda events

import { APIGatewayEvent, Context, APIGatewayProxyResult, SQSEvent, SNSMessage, S3Event, KinesisStreamEvent, DynamoDBStreamEvent, CloudWatchLogsEvent, EventBridgeEvent } from 'aws-lambda';

// API Gateway
export const apiGatewayHandler = async (event: APIGatewayEvent, context: Context): Promise<APIGatewayProxyResult> => {
    console.log('API Gateway Event:', JSON.stringify(event, null, 2));
    return {
        statusCode: 200,
        body: JSON.stringify({ message: 'Hello from API Gateway!' })
    };
};

// S3
export const s3Handler = async (event: S3Event, context: Context): Promise<void> => {
    event.Records.forEach(record => {
        console.log('S3 Object:', record.s3.object.key);
    });
};

// DynamoDB Streams
export const dynamoDBHandler = async (event: DynamoDBStreamEvent, context: Context): Promise<void> => {
    event.Records.forEach(record => {
        console.log('DynamoDB Record:', JSON.stringify(record.dynamodb, null, 2));
    });
};

// Kinesis
export const kinesisHandler = async (event: KinesisStreamEvent, context: Context): Promise<void> => {
    event.Records.forEach(record => {
        console.log('Kinesis Record:', Buffer.from(record.kinesis.data, 'base64').toString('ascii'));
    });
};

// SQS
export const sqsHandler = async (event: SQSEvent, context: Context): Promise<void> => {
    event.Records.forEach(record => {
        console.log('SQS Message:', record.body);
    });
};

// SNS
export const snsHandler = async (event: SNSMessage, context: Context): Promise<void> => {
    console.log('SNS Message:', JSON.stringify(event, null, 2));
};

// CloudWatch Logs
export const cloudWatchLogsHandler = async (event: CloudWatchLogsEvent, context: Context): Promise<void> => {
    event.awslogs.data = Buffer.from(event.awslogs.data, 'base64').toString('ascii');
    console.log('CloudWatch Logs:', JSON.stringify(JSON.parse(event.awslogs.data), null, 2));
};

// EventBridge (CloudWatch Events)
export const eventBridgeHandler = async (event: EventBridgeEvent<string, any>, context: Context): Promise<void> => {
    console.log('EventBridge Event:', JSON.stringify(event, null, 2));
};
```

## Explanation of Each Handler

1. **API Gateway**
   - Logs the event and returns a simple response with a status code of 200.

2. **S3**
   - Logs the S3 object key from the event.

3. **DynamoDB Streams**
   - Logs changes from DynamoDB Streams.

4. **Kinesis**
   - Decodes and logs Kinesis records.

5. **SQS**
   - Logs messages from an SQS queue.

6. **SNS**
   - Logs SNS messages.

7. **CloudWatch Logs**
   - Decodes and logs CloudWatch log data.

Certainly! Here’s the sample TypeScript code for sending an email using AWS Lambda with Amazon SES, formatted in Markdown:

```markdown
# Sending an Email Using AWS Lambda with Amazon SES

## Prerequisites

Ensure you have the AWS SDK for JavaScript installed:

```bash
npm install aws-sdk
```

## Email Code

Here is a TypeScript code snippet for sending an email using Amazon SES within an AWS Lambda function:

```typescript
import { SES } from 'aws-sdk';
import { Context, APIGatewayEvent, APIGatewayProxyResult } from 'aws-lambda';

const ses = new SES({ region: 'us-east-1' }); // Specify your region

export const lambdaHandler = async (event: APIGatewayEvent, context: Context): Promise<APIGatewayProxyResult> => {
    const toAddress = 'recipient@example.com'; // Replace with the recipient's email address
    const fromAddress = 'sender@example.com'; // Replace with a verified email address in SES
    const subject = 'Test Email from AWS Lambda';
    const body = 'This is a test email sent from an AWS Lambda function using Amazon SES.';

    try {
        const params = {
            Destination: {
                ToAddresses: [toAddress],
            },
            Message: {
                Body: {
                    Text: {
                        Charset: 'UTF-8',
                        Data: body,
                    },
                },
                Subject: {
                    Charset: 'UTF-8',
                    Data: subject,
                },
            },
            Source: fromAddress,
        };

        await ses.sendEmail(params).promise();
        
        return {
            statusCode: 200,
            body: JSON.stringify({
                message: 'Email sent successfully!',
            }),
        };
    } catch (error) {
        console.error('Error sending email:', error);
        
        return {
            statusCode: 500,
            body: JSON.stringify({
                message: 'Failed to send email.',
                error: error.message,
            }),
        };
    }
};
```

## Explanation

1. **Import Dependencies:**
   - Import the `SES` class from the AWS SDK and Lambda types.

2. **Initialize SES Client:**
   - Create an instance of the SES client, specifying the AWS region. 

3. **Handler Function:**
   - Define `lambdaHandler` to handle the incoming event and context.
   - Set up email parameters including `Destination`, `Message`, and `Source`.
   - Call `ses.sendEmail` to send the email and handle the response.

4. **Error Handling:**
   - Catch and log any errors that occur during the email sending process.

5. **Return Response:**
   - Return an HTTP response with status code 200 if successful or 500 if there's an error.

**Note:** Ensure that you have verified the sender email address in Amazon SES, and if you are in the SES sandbox environment, you will need to verify the recipient's email address as well.

For more information on configuring Amazon SES, refer to the [Amazon SES documentation](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html).
```

This Markdown provides a clear and formatted guide on sending an email using AWS Lambda and Amazon SES in TypeScript.

8. **EventBridge (CloudWatch Events)**
   - Logs EventBridge events.

These handlers provide a basic starting point for processing each type of event. You can expand upon them based on your application's needs.
