```markdown
### Top 10 AWS API Gateway Interview Questions:

1. What is AWS API Gateway?
2. What are the key components of AWS API Gateway?
3. How do you integrate AWS API Gateway with other AWS services?
4. What are REST APIs vs HTTP APIs in AWS API Gateway?
5. What are the different API Gateway methods and how are they used?
6. How does API Gateway handle security?
7. What is CORS and how is it handled in API Gateway?
8. How can you manage different versions or stages of an API?
9. What are throttling limits in API Gateway and how do you configure them?
10. How do you monitor an API in AWS API Gateway?

---

### Answers:

1. **What is AWS API Gateway?**
   - AWS API Gateway is a fully managed service that allows developers to create, deploy, and manage APIs at scale. It acts as a front door for applications to access backend services (like Lambda, EC2, DynamoDB) and handles traffic routing, authorization, monitoring, and throttling of API requests.

2. **What are the key components of AWS API Gateway?**
   - The main components are:
     - **API**: The overall API definition.
     - **Resource**: The individual paths within an API.
     - **Method**: HTTP methods (GET, POST, etc.) associated with resources.
     - **Endpoint**: The API Gateway’s URL that serves API requests.
     - **Stage**: Versions of the API (e.g., dev, prod).
     - **Usage Plan**: Defines throttling and quota limits for API clients.
     - **Authorizers**: Define how API access is controlled.
     - **API Keys**: Used to enforce usage plans.

3. **How do you integrate AWS API Gateway with other AWS services?**
   - API Gateway integrates with various AWS services such as:
     - **Lambda** for serverless backends.
     - **DynamoDB** for database access.
     - **Step Functions** for orchestrating workflows.
     - **EC2 or Elastic Beanstalk** for traditional backends.
     - **S3** for static content.
     - Integration is done via HTTP integrations or AWS service proxy integrations.

4. **What are REST APIs vs HTTP APIs in AWS API Gateway?**
   - **REST APIs**: Feature-rich, providing extensive control over API functionalities like request validation, API keys, usage plans, etc. They are ideal for complex APIs but come with higher costs and latency.
   - **HTTP APIs**: A newer, lightweight, and cost-effective alternative for building APIs, especially for microservices. They are optimized for speed and lower cost but have fewer features compared to REST APIs.

5. **What are the different API Gateway methods and how are they used?**
   - API Gateway supports standard HTTP methods:
     - **GET**: Retrieve data from the backend.
     - **POST**: Submit data to the backend.
     - **PUT**: Update or replace data.
     - **DELETE**: Remove data.
     - **PATCH**: Partially update data.
     - **OPTIONS**: Query available HTTP methods.
     - **HEAD**: Retrieve headers without the body content.
   - Each method maps to a corresponding backend action in AWS (Lambda, DynamoDB, etc.).

6. **How does API Gateway handle security?**
   - API Gateway offers multiple layers of security:
     - **IAM roles and policies**: Restrict access using AWS Identity and Access Management.
     - **AWS Cognito**: Authenticate users via user pools.
     - **Lambda authorizers**: Custom authentication logic implemented using AWS Lambda.
     - **API keys**: Provide API access to certain clients with throttling and quota.
     - **Resource Policies**: Define access control rules for APIs.

7. **What is CORS and how is it handled in API Gateway?**
   - **CORS** (Cross-Origin Resource Sharing) allows web applications on different domains to interact securely. In API Gateway, CORS is enabled by configuring response headers that specify allowed origins, HTTP methods, and headers. API Gateway automatically responds with appropriate CORS headers when configured.

8. **How can you manage different versions or stages of an API?**
   - API Gateway supports API stages (like **dev**, **test**, **prod**) to represent different versions or deployment environments. Each stage can have its own configurations (such as logging and throttling settings). You can deploy APIs to different stages and use stage variables to modify behavior without changing the code.

9. **What are throttling limits in API Gateway and how do you configure them?**
   - Throttling limits control the number of API requests allowed per second to protect backend systems from being overwhelmed. These limits can be configured globally or at the individual API method level. They can be enforced using **Usage Plans** which define throttling (requests per second) and quotas (requests per day or month).

10. **How do you monitor an API in AWS API Gateway?**
    - Monitoring is achieved using **Amazon CloudWatch**, which tracks metrics such as:
      - **Latency**: Time taken for API requests to be processed.
      - **Integration errors**: Backend failures.
      - **API errors**: Client-side or server-side errors (4XX or 5XX status codes).
      - **Request counts**: Number of requests received.
      - Additionally, you can enable **CloudWatch Logs** to log request and response data for debugging.
```
