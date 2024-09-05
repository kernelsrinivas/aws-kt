Here’s the information you requested in Markdown format:

```markdown
# AWS CDK Serverless Example

Based on the [AWS CDK Serverless Example Documentation](https://docs.aws.amazon.com/cdk/v2/guide/serverless_example.html).

### Steps to Set Up a CDK Project

1. Create a new CDK project:
   ```bash
   mkdir cdk-hello-world && cd cdk-hello-world
   cdk init --language typescript
   ```

2. Install necessary dependencies:
   ```bash
   npm install aws-cdk-lib constructs
   ```

3. Build the project:
   ```bash
   npm run build
   ```

4. Synthesize the CloudFormation template:
   ```bash
   cdk synth
   ```

5. Deploy the stack:
   ```bash
   cdk deploy
   ```

6. Destroy the stack (if needed):
   ```bash
   cdk destroy
   ```

---

## 1. Create the Lambda Function

1. Create a new folder for the Lambda function:
   ```bash
   mkdir lambda && cd lambda
   touch hello.js
   ```

2. Add the following code in `hello.js`:
   ```javascript
   exports.handler = async (event) => {
       return {
           statusCode: 200,
           headers: { "Content-Type": "text/plain" },
           body: JSON.stringify({ message: "Hello, World!" }),
       };
   };
   ```

---

## 2. Define Lambda and API Gateway in the Stack

1. In your CDK project, modify the stack file (e.g., `lib/cdk-hello-world-stack.ts`):

   ```typescript
   import * as cdk from 'aws-cdk-lib';
   import { Construct } from 'constructs';
   import * as lambda from 'aws-cdk-lib/aws-lambda';
   import * as apigateway from 'aws-cdk-lib/aws-apigateway';

   export class CdkHelloWorldStack extends cdk.Stack {
     constructor(scope: Construct, id: string, props?: cdk.StackProps) {
       super(scope, id, props);

       // Define the Lambda function resource
       const helloWorldFunction = new lambda.Function(this, 'HelloWorldFunction', {
         runtime: lambda.Runtime.NODEJS_20_X, // Choose any supported Node.js runtime
         code: lambda.Code.fromAsset('lambda'), // Points to the lambda directory
         handler: 'hello.handler', // Points to the 'hello' file in the lambda directory
       });

       // Define the API Gateway resource
       const api = new apigateway.LambdaRestApi(this, 'HelloWorldApi', {
         handler: helloWorldFunction,
         proxy: false,
       });
        
       // Define the '/hello' resource with a GET method
       const helloResource = api.root.addResource('hello');
       helloResource.addMethod('GET');
     }
   }
   ```

---

## 3. Deploy and Test the API

After deploying the stack, you can test the API using the following commands:

### Test the API using `curl`:

```bash
curl https://<api-id>.execute-api.<region>.amazonaws.com/prod/hello
```

Example Response:
```json
{"message":"Hello World!"}
```

### Invoke the Lambda function directly:

```bash
aws lambda invoke --function-name CdkHelloWorldStack-HelloWorldFunctionunique-identifier output.txt
```

This will invoke the Lambda function and save the output to `output.txt`.
``` 
