Step Function Overview

1. AWS Step Functions allows you to model a workflow as a state machine, with one state machine per workflow. This can be used for various types of workflows like order fulfillment, data processing or running a web application.
   
2. The process includes defining the sequence of events (what happens and what happens next) within a workflow, and based on certain conditions, leading to the next steps.

3. The workflow is defined in JSON which is then translated into a visual workflow. This visualization also includes the history of the workflow's execution.

4. Each step in the workflow can be tasks such as running a Lambda function or inserting data into DynamoDB. The step function coordinates this workflow.

5. Workflows can be started using SDK API call, API Gateway, CloudWatch Events, Amazon EventBridge or manually from the console.

6. Tasks within step functions are represented as boxes in the task state of the state machine diagram. These could be invoking a Lambda function, running an ECS task, inserting an item into DynamoDB, or launching another step function workflow.

7. Different states within AWS step functions can include:
   - Choice State: Tests for a condition to send execution to one branch or another.
   - Fail or Succeed State: Stops the execution of the step function workflow with a success or failure.
   - Pass State: Passes the input to the output or injects some data without performing any work.
   - Wait State: Provides a delay for a certain amount of time or until a specified date/time.
   - Map State: Dynamically iterates over steps.
   - Parallel State: Starts parallel branches of execution. 
   
8. The AWS certification exam may primarily focus on task states and parallel states.

9. AWS Step functions provide a visual way of following the flow of execution, and are instrumental in defining how your workflow should look and function. With step functions, error handling and event orchestration are easier and more effective.
Error Handling

**1. Error Handling in AWS Step Functions:**
- Error handling should be managed by the step functions themselves, not the tasks. Errors can occur due to state machine definition issues, task failures, or transient failures.
- Types of error handling include Retry (retry a task) and Catch (transition into a failure path).

**2. Retry Strategy:**
- This defines how and when to retry after an error, based on the types of errors.
- Fields include `ErrorEquals` (specific types of errors), `IntervalSeconds` (time to wait before retrying), `BackoffRate` (increase in the delay after each retry), and `MaxAttempts` (maximum retry limit).

**3. Catch Strategy:**
- If all retries are exhausted, the catch block is activated which transitions to a fallback state to handle the error.
- Fields include `ErrorEquals` and `Next` (name of the fallback state).

**4. Predefined Error Codes:**
- AWS Step Functions have predefined error codes like `States.All` (match any error), `States.Timeout` (task ran longer than the timeout), `TaskFailed` (task execution failure), or `States.Permissions` (lack of permissions to execute some code).

**5. Advantages of External Error Handling:**
- Defining Retry and Catch in step functions keeps the application code simpler.
- This approach also provides an execution history of the retries and catches.
- Making changes to the error handling strategy does not require redeploying the application code.

**6. Understanding ResultPath:**
- `ResultPath` determines the input sent to the state specified in the `Next` field.
- It allows the catch block to include the error information into its input.
Wait for Task Token

1. AWS Step Functions can incorporate the feature known as 'wait for task token' to wait for an external event, such as the completion of a task by another AWS service, human approval, a third-party integration, or a call to legacy systems.

2. This feature is implemented by appending '.waitForTaskToken' to the resource field in your task within Step Functions. This instructs the step function to wait for a specific task token to return before proceeding with the workflow execution.

3. For instance, if you have a resource named `sqs:sendMessage`, you append `waitForTaskToken` to it. The task would then be paused until it receives a return task token via either a `SendTaskSuccess` (if everything works) or a `SendTaskFailure` API call.

4. A practical example is when you need to check client credits that rely on an external service. You call SQS with a task token using 'waitForTaskToken' in the resource. You will then pass the task token and the input to the SQS queue in the message.

5. The SQS queue is processed by whatever application you have, such as Lambda functions, ECS, EC2, or a third-party server. This application gets the message input body and the task token.

6. If the process is successful or fails, there's a `SendTaskSuccess` API call. You pass both the outputs of the processing and the original task token passed to the SQS queue into this call.

7. Once the Step Function workflow receives that API call with the correct task token and the correct output, the workflow can then proceed to its next stages.

8. The advantage of this process is that it allows the Step Function workflow to rely on an external system to carry out some processing before returning to the workflow itself. This way, it can integrate with external mechanisms.

Activity Tasks

1. **Activity Workers**: Activity workers are entities, such as EC2 instances, Lambda functions, or mobile devices, that perform tasks from your step functions. These workers regularly poll your step function's workflow to find a task using the GetActivityTask API. Upon receiving a task, they complete it and send a response using SendTaskSuccess or SendTaskFailure.
   
2. **Pull Mechanism**: Activity Tasks utilize a pull mechanism. The activity worker (e.g., an EC2 instance) pulls tasks from step functions. This mechanism simplifies the network connectivity as the worker just needs to be able to connect to step functions. 

3. **Difference from Wait for Token Pattern**: This pull mechanism differs from the wait for task token pattern where step functions push an event to an external entity, who then pulls that work and finally gets it back into step functions. Thus, the former uses a pull-based mechanism while the latter employs a push-based process. 

4. **Timeout and Heartbeat Mechanisms**: Two parameters observed in this design are TimeoutSeconds and HeartbeatSeconds. The former indicates how long a task in progress can wait until it's considered a failure. The latter is a mechanism by which the activity worker signals its alive status – as long as it makes the SendHeartBeat API call, the task is considered alive. 

5. **Activity Task Duration**: If you configure a very long TimeoutSeconds and continually send a heartbeat, the actual Activity Task can be kept alive for up to one year. 

6. **Application**: The mechanism of activity tasks resembles the roles of queues in AWS, thereby providing a familiar pattern for developers familiar with queue-based workflows.

Standard vs Express

1. **Standard Workflow**:
    * Default workflow in AWS step function.
    * Maximum duration of a single workflow is up to one year. 
    * Uses an exactly-once execution model. Can execute up to 2,000 standard workflows per second. 
    * Execution history is available up to 90 days in the console history, or indefinitely with CloudWatch logs and log retention settings.
    * Pricing is based on the number of state transitions, i.e., going from one state to another. 
    * Suitable for non-idempotent actions such as payment processing.
  
2. **Express Workflow**:
    * Ideal for short workflows with execution time up to five minutes. 
    * Supports high rates with over 100,000 executions per second. 
    * Execution results and analysis can only be seen using CloudWatch logs – not available in the console. 
    * Pricing is based on the number of executions, the duration of an execution, and memory consumption.
    * Use cases include IoT data ingestion, streaming data, and mobile app backends. 

3. **Asynchronous Express Workflow**:
    * An at-least-once execution model guarantee.
    * We don't wait for result confirmation. Status/results need to be checked through CloudWatch logs. 
    * Suitable for scenarios where immediate response is not necessary, e.g., messaging services. 
    * Step functions can automatically retry in case of errors, potentially causing the same action to be performed twice - idempotence needs to be managed.
    
4. **Synchronous Express Workflow**:
    * An at-most-once execution model guarantee.
    * Waits for the workflow to complete, and provides a result.
    * Applicable in cases requiring an immediate response from the workflow, e.g., when orchestrating microservices.
    * Can be invoked from the API gateway or a Lambda function.
    * Where there is error, retry is not automatic - It is up to you to try and need to manage your own retry logic.   
  
Remember: AWS exams might test your understanding regarding the differences between asynchronous and synchronous workflow types.

AppSync

1. **What is it?** AWS AppSync is a managed service that uses GraphQL to build APIs. It's especially suitable for creating applications that need to get specific data they need without unnecessary information.

2. **GraphQL**: GraphQL allows custom queries for exact data needs, combining data from numerous sources into a graph.

    - Data sources can be NoSQL data stores, Relational databases or HTTP APIs.
    - AWS AppSync provides direct integrations with DynamoDB, Aurora, OpenSearch, and various other sources.
    - The service also enables access to any data through Lambda to extend the pattern.

3. **AppSync for Real Time Integration**: AppSync supports real-time WebSocket Integration or MQTT on WebSockets. Thus, it's useful for developing real-time applications that need to access a data field.

4. **Local Data Access and Data Synchronization** AppSync provides local data access and synchronization for mobile applications, acting as an alternative to the outdated 'cookie to sync' method.

5. **Starting With AppSync**: The user needs to upload a GraphQL schema to get started with AppSync.

6. **Integrations**: AppSync can support web applications, mobile applications, real-time dashboards, applications needing offline data synchronization, and is a replacement for 'cookie to sync'. 

7. **Resolvers**: The service uses resolvers to determine how to fetch data and has integrations with DynamoDB, Aurora, OpenSearch or various other sources through a Lambda function. 

8. **Appsync and AWS CloudWatch**: App sync is integrated with CloudWatch Metrics and CloudWatch logs for logging, tracking, and troubleshooting the activities happening in AppSync.

9. **Security**: There are four primary ways to authorize applications to interact with an AppSync GraphQL API:

    - API_KEY: Generate keys similar to API Gateway to provide user access.
    - AWS_IAM: Allow IAM users, roles or cross-account access.
    - OPENID_CONNECT: Integration with an OpenID Connect provider and a JSONWeb Token.
    - AMAZON_COGNITO_USER_POOL: Integration with existing User pools in Amazon Cognito.
    
10. **HTTPS Security**: To gain HTTPS security on AppSync with a custom domain, it's recommended to use CloudFront in front of AppSync.
Amplify

- **AWS Amplify** is a service that allows you to create mobile and web applications. It's essentially the elastic beanstalk for mobile and web applications.

- There are different components, including **Amplify Studio** for visually building full stack apps, **Amplify CLI** for the same but via the command line interface, **Amplify Libraries** to connect to AWS services like Cognito and S3, and **Amplify Hosting** to host and deliver your application on AWS.

- Amplify provides essential features such as data storage, authentication, file storage, and machine learning.

- The backend of Amplify is powered by Amazon services such as **DynamoDB**, **AppSync**, **Cognito**, **S3**, etc.

- Amplify also provides ready-to-use front end libraries for different frameworks like React, Vue, JavaScript, iOS, Android, Flutter, etc.

- AWS Amplify is set up to incorporate best practices for reliability, security and scalability.

- Amplify provides out-of-the-box authentication by leveraging Amazon Cognito. You get user registration, authentication, account recovery, multi-factor authentication, and social signing.

- It provides a Datastore which leverages Amazon AppSync for API, and Amazon DynamoDB for data storage. It offers offline and realtime capabilities, data modeling, and automatic synchronization to the cloud without needing complex code.

- Amplify Hosting allows for the building and hosting of modern web apps. It provides a way to do continuous integration/continuous deployment (CI/CD), custom domains, monitoring, pull request previews, and setup redirects and custom headers.

- For running tests, Amplify provides unit testing and end-to-end testing.

- There's an option to use the Cypress testing framework to generate end-to-end tests. It provides a UI report for your tests, and you can define web clicks. 

- To summarize, Amplify provides build level tests for verifying your code's functionality, and end-to-end tests for assuring the overall application behaves as expected post-deployment.


