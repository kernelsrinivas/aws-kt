```markdown
# Top 10 Basic Core AWS Lambda Interview Questions

1. What is AWS Lambda, and what are its key features?
2. How does AWS Lambda's event-driven model work, and what are common event sources?
3. Can you explain the steps to create a simple AWS Lambda function from scratch?
4. What are AWS Lambda function memory and timeout limits, and how do they affect performance?
5. How does AWS Lambda scale automatically, and what are the limitations of this scaling?
6. What is a cold start in AWS Lambda, and when does it occur?
7. How does AWS Lambda pricing work, and what factors influence the cost of a function's execution?
8. What is the role of the execution role (IAM role) in AWS Lambda, and why is it important?
9. How do you test and debug an AWS Lambda function locally before deploying it to AWS?
10. How does AWS Lambda integrate with AWS CloudWatch for logging and monitoring function execution?
```

2. How does AWS Lambda's event-driven model work, and what are common event sources?
```markdown
**Understanding Event-Driven Architecture (EDA):**

- **What is it?**  
EDA is a design pattern where applications use events (changes or updates) to separate components, allowing them to work independently.

- **Why use it?**  
It's ideal for building modern apps with **microservices**, where different parts of the system (services) need to communicate but can scale or fail separately without affecting the whole app. This makes the app more resilient and easier to maintain.

- **How it works:**  
  1. **Event Producers**: These are sources that generate events, like a website or IoT device.
  2. **Event Ingestion**: A system that collects or routes these events.
  3. **Event Consumers**: Components that react to events, like starting a workflow or updating a database.

- **Example**:  
When a customer adds an item to their shopping cart, that action generates an event. Multiple services (like inventory or recommendations) can act on that event independently.

- **Key benefits:**
  1. **Independent Updates**: Teams can develop and deploy features for their services without disrupting the rest of the system.
  2. **Easy Expansion**: New features can be added by listening to existing events, without changing the core system.
  3. **Resilience**: Components can fail or scale independently, reducing the chances of a single failure bringing down the whole app.

EDA is great for complex, scalable systems where services need to work in sync but maintain independence.

EDA is great for complex, scalable systems where services need to work in sync but maintain independence.
Link1: https://serverlessland.com/event-driven-architecture/what-are-event-driven-architectures
Link2: https://serverlessland.com/event-driven-architecture/visuals
Link3: https://serverlessland.com/lambda
Link4: https://serverlessland.com/patterns?services=lambda&framework=CDK&language=TypeScript
```
