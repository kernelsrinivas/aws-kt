### Top 10 Junior Level AWS API Gateway Interview Questions:

1. What is AWS API Gateway, and why would you use it?
2. What are the main features of AWS API Gateway?
3. How do you create a simple API in API Gateway?
4. What is the difference between a public and private API in API Gateway?
5. How do you integrate an API Gateway with AWS Lambda?
6. How do you secure an API using API Gateway?
7. What is a method in API Gateway, and how is it used?
8. How can you limit the number of API requests in API Gateway?
9. What is an API Gateway stage, and why is it useful?
10. How do you monitor an API in API Gateway?

---

### Answers:

1. **What is AWS API Gateway, and why would you use it?**
   - AWS API Gateway is a service that allows developers to create and manage APIs at any scale. It acts as an interface for applications to communicate with backend services (e.g., AWS Lambda or EC2). It’s used to build serverless APIs, handle API traffic, and manage API security.

2. **What are the main features of AWS API Gateway?**
   - Some key features include:
     - **API Management**: Creating, deploying, and managing APIs.
     - **Throttling and Caching**: Controlling API traffic and caching responses.
     - **Security**: Using API keys, IAM roles, and custom authorizers.
     - **Monitoring and Logging**: Integrating with CloudWatch for metrics and logs.
     - **Multiple Protocols**: Support for REST and HTTP APIs.

3. **How do you create a simple API in API Gateway?**
   - To create a simple API:
     1. Open the **API Gateway** console.
     2. Create a new API.
     3. Define resources and methods (e.g., GET, POST).
     4. Set the integration (e.g., AWS Lambda or HTTP endpoint).
     5. Deploy the API to a stage (e.g., dev or prod).

4. **What is the difference between a public and private API in API Gateway?**
   - **Public API**: Accessible over the internet and secured using IAM roles, API keys, or custom authorizers.
   - **Private API**: Can only be accessed from within a VPC using VPC endpoints. It’s used when you don’t want to expose your API publicly.

5. **How do you integrate an API Gateway with AWS Lambda?**
   - To integrate API Gateway with Lambda:
     1. Create a new API Gateway resource and method.
     2. In the method settings, select **Lambda Function** as the integration type.
     3. Specify the Lambda function to be triggered by the API.
     4. Deploy the API to make it available.

6. **How do you secure an API using API Gateway?**
   - API Gateway offers several security options:
     - **IAM Permissions**: Control who can invoke the API.
     - **API Keys**: Provide access to specific users or applications.
     - **AWS Cognito**: Authenticate users through user pools.
     - **Lambda Authorizers**: Add custom authentication logic.

7. **What is a method in API Gateway, and how is it used?**
   - A **method** defines how API Gateway interacts with a backend resource (e.g., Lambda or EC2). Methods correspond to HTTP actions like **GET**, **POST**, or **DELETE**. Each method can be configured with different integrations, security settings, and response types.

8. **How can you limit the number of API requests in API Gateway?**
   - API Gateway allows for throttling and rate limiting through **Usage Plans** and **API Keys**. You can configure the maximum number of requests per second and set burst limits to handle sudden spikes in traffic.

9. **What is an API Gateway stage, and why is it useful?**
   - A **stage** in API Gateway represents a specific deployment of your API (e.g., development, testing, or production). It allows you to manage different versions of the API and apply settings like logging, throttling, and security per stage.

10. **How do you monitor an API in API Gateway?**
    - You can monitor API Gateway using **Amazon CloudWatch**, which provides metrics like:
      - **Latency**: Time taken for requests to be processed.
      - **Error rates**: 4XX and 5XX status codes.
      - **Request counts**: Total number of API calls.
    - You can also enable **CloudWatch Logs** to record the details of each request and response for debugging purposes.
