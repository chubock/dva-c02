Serverless Introduction

### Key Takeaways from the AWS Certification Preparation Course:

1. **Serverless:** Serverless is a cloud computing execution model where the cloud provider dynamically allocates machine resources. Serverless does not mean there are no servers, instead, developers don't manage them anymore.

2. **Function as a Service (FaaS):** Initially, serverless meant FaaS like AWS Lambda. This allows you to run your code without provisioning or managing servers. 

3. **AWS Lambda:** AWS Lambda was the pioneer of serverless. It can run code in response to events, such as changes to data in an Amazon S3 bucket or an Amazon DynamoDB table. 

4. **Expanding Understanding of Serverless:** Now, serverless includes any services that are remotely managed, including databases, messaging, and storage.

5. **Components of Serverless in AWS:**

   - **AWS Users and Static Content:** Users get static content from S3 buckets delivered via CloudFront or S3.
   - **Cognito:** Users' identities are stored here. Used for logging in.
   - **API Gateway:** Facilitates invoking Lambda functions through a REST API.
   - **Lambda Functions:** Invoked by the API Gateway, Lambda functions interact with DynamoDB to store and retrieve data.
   - **SQS and SNS:** Amazon Simple Queue Service (SQS) and Simple Notification Service (SNS) also fall under Serverless as they scale on their own without server management.
   - **DynamoDB:** It is a serverless database that supports key-value and document data structures.
   - **Kinesis Data Firehose:** This service enables capturing, transforming, and loading of streaming data into AWS for the analysis, and scales based on your throughput.
   - **Aurora Serverless:** Aurora Serverless is an on-demand, auto-scaling configuration for Amazon Aurora.
   - **Step Functions:** AWS Step Functions is a serverless function orchestrator that makes it easy to sequence AWS Lambda functions and multiple AWS services into business-critical applications.
   - **Fargate:** AWS Fargate is a serverless compute engine for containers that work with AWS Lambda.

6. **Exam Focus:** The AWS certification exam tests heavily on serverless knowledge. This includes understanding how AWS Lambda, DynamoDB, Cognito, API Gateway, Amazon S3, SQS, SNS, and more fit into the serverless architecture.

Overview

1. **AWS Lambda**: It's a fully managed compute service where no server management is necessary. You only have to upload your code. It's designed to run short executions up to 15 minutes, but scale automatically when needed. AWS will provision more Lambda functions when required.
   
2. **Advantages of AWS Lambda**:
   - *Simple Pricing*: Only pay for the request Lambda receives and the compute time it consumes. Generous free tier on Lambda (1 million requests and 400,000 GB-seconds of compute time).
   - *A-list Services Integration*: Lambda integrates with several AWS services.
   - *Language Support*: Wide variety of supported programming languages.
   - *Provision per Function*: Can provision up to 10 GB of RAM per function (Increasing RAM improves the power and performance of your CPU and network).
   
3. **Lambda Language Support**: Node.js, Python, Java (compatible with Java 8 and Java 11), Golang, C# for .NET Core, Powershell, Ruby, and community-supported custom runtime API enabling any language.

4. **Lambda Container Image**: The container image itself must implement the Lambda runtime API. For running arbitrary Docker images, ECS and Fargate are preferred.

5. **Lambda Integrations**: API Gateway, Kinesis, DynamoDB, Amazon S3, CloudFront, CloudWatch Events (EventBridge), CloudWatch Logs, SNS, SQS, Cognito, etc. They enable various function capacities.

6. **Serverless Thumbnail Creation**: An example of using S3 and Lambda. When an image is uploaded in S3 bucket, an event triggers a Lambda function that processes the image to create a thumbnail.

7. **Serverless CRON Jobs**: Cron jobs can be designed on virtual servers like EC2, but they waste server time when inactive. CloudWatch and Lambda enable serverless Cron Jobs.

8. **Lambda Pricing**: Pay per calls and duration in increments of 100ms. First 1M requests are free, then 20 cents per extra 1M requests. For duration, first 400,000 GB-seconds of compute time per month are free. Extra usage incurs a cost of $1 for 600,000 GB-seconds.

First Hands On

1. **AWS Lambda Overview**: AWS Lambda is a serverless service that helps you run code without worrying about servers. It can utilize various programming languages such as .Net Core, Go, Java, Node JS, Python, Ruby, etc. You can upload your code into the Lambda console, and AWS will execute it for you.

2. **Invoking Lambda Function**: Lambda functions can be invoked manually by clicking the 'Run' button, or they can be triggered to respond to events. Events can originate from various sources like streaming analytics, mobile IoT backends, or an S3 bucket for data processing. As the frequency of these events increases, Lambda automatically scales to match the load, demonstrating its efficient scalability.

3. **Lambda Cost Efficiency**: AWS Lambda operates on a pay-per-use model. However, it grants a significant number of free invocations every month. Therefore, most light workloads may not cost anything. After exceeding the significant free tier, costs will occur, but they are minuscule.

4. **Creating a Lambda Function**: You can create a Lambda function using a blueprint. The function needs a name, and a new role with basic Lambda permissions needs to be created. Inside a Lambda function, the Lambda handler is the function that handles the event of invoking the Lambda function.

5. **Testing Lambda Function**: Test events can be created to test the execution of Lambda functions. The events can be JSON documents with key-value pairs.

6. **Lambda Function Monitoring**: Monitoring of Lambda functions includes information about how many times it was invoked, how long it lasted, whether there were errors, and more. Integration with CloudWatch Metrics and CloudWatch Logs provides logs and metrics of the function's execution.

7. **Lambda Function Error Handling & Debugging**: Lambda reports any error occurrences. Errors can be simulated by modifying the code and inducing exceptions or errors. Detailed logs about these errors can also be retrieved from CloudWatch Logs to understand the root cause of the issue.

8. **Lambda Execution Role & Permissions**: Upon function creation, an IAM role is automatically created with basic permissions. This execution role has permissions to write to CloudWatch Logs.

9. **AWS Lambda Runtime and Handler**: The runtime denotes the programming language version used for the function. The handler informs Lambda to look for a specific file and function within that file when invoking the function.

Synchronous Invocation

1. **Synchronous Invocation**: This type of invocation is used when using the CLI, SDK, API Gateway, or an Application Load Balancer. Synchronous means that you wait for the results, and the result will be returned to you.

2. **Error Handling**: Any errors that occur during lambda execution using the synchronous method must be handled on the client side. It's up to the client's decision on how to handle errors, for instance, to retry or to implement an exponential backoff.

3. **Invocation Mechanism**: Tools like the CLI and the SDK, API Gateway invoke the Lambda functions, wait for the processes to execute, and then return the results. For instance, API Gateway will act as a proxy that invokes the Lambda function when clients send requests, and return the response.

4. **Synchronous Integrations with Services**: Any user-invoked actions are considered synchronous. Hence, services like Elastic Load Balancing (Application Load Balancer), API Gateway, CloudFront using Lambda@Edge are integrated synchronously with Lambda. More services such as Amazon S3 Batch, Cognito, Step Functions, Lex, Alexa, and Kinesis Data Firehose are also synchronous.
Application Load Balancer

1. Lambda functions can be invoked with CLI or SDK, or exposed to the internet via an HTTP/HTTPS endpoint using Application Load Balancer (ALB) or API Gateway.

2. To use an ALB, you need to register the lambda function in a target group.

3. The ALB synchronously invokes the lambda function in a target group and waits for a response before letting the client know.

4. The ALB converts HTTP requests to JSON documents that include:
   - The ELB information
   - HTTP method
   - Query string parameters as key/value pairs
   - Headers as key/value pairs
   - Body for POST, PUT. This is base64 encoded.

5. The lambda function returns a JSON document which includes a status code and description, response headers as key/value pairs, the body of the response and a flag indicating if it is base 64 encoded.

6. With multi-header values enabled in the ALB settings, multiple headers or query strings with the same value get converted into an array for the lambda function to process.

7. The exam might test on how to support multi-header values. It's just a setting on the ALB that would convert multiple headers or query parameter with the same name into array values.

Application Load Balancer Hands On

1. **Lambda and ALB**: You can create a Lambda function, with Python as the runtime, to be integrated with an Application Load Balancer (ALB).

2. **Creating an ALB**: The ALB can be set up to listen on port 80 for the HTTP protocol and the default action is set to send to a target group containing the created Lambda function.

3. **Lambda Testing**: The Lambda function can be tested using a new test event, and log creation within the function can provide useful information about passed events.

4. **Response Management**: To ensure the responses from Lambda are displayed correctly in a web browser, the document for the response should follow the AWS documentation guidance. Changing the return statement of the function according to this documentation ensures an appropriate HTTP response instead of triggered downloads.

5. **Lambda and ALB Integration**: The Lambda function sends data into the ALB, and the ALB shows the data directly in the web browser.

6. **Monitoring and Logging**: The monitoring tab of the Lambda function's log offers a glimpse of the recent requests and the type of events passed from the ALB to the Lambda function.

7. **Multi-Value Headers**: In the ALB target group attributes, you can configure multi-value headers. This setting allows the load balancer to have multiple headers. One needs to make corresponding changes in the Lambda code when this option is enabled.

8. **Permissions**: Under the permissions of the Lambda function, there is a resource-based policy statement allowing the ALB to invoke the Lambda function.

Asynchronous Invocations and DLQ

## Key Takeaways from AWS Certification Preparation Course
1. **Asynchronous Invocation**: Services like Amazon S3, SNS topics, CloudWatch Events etc. can invoke other functions behind the scenes asynchronously.

2. **Event Queue**: Events are placed in an internal Event Queue. The Lambda function reads from this Event Queue and then tries to process these events. 

    * If things go wrong, Lambda function will automatically retry, three times in total: 
        * Initially, 
        * Then a minute later,
        * And finally, two minutes after the second attempt.

3. **Function Idempotency**: Retries can lead to processing the same events multiple times which could be a problem for non-idempotent Lambda functions. Therefore, Lambda functions should ideally be idempotent for such configurations.

4. **DLQ (Dead-Letter Queue)**: A DLQ can be defined to receive events after retries fail to successfully process them. This approach allows failed events to be sent to SQS or SNS for later processing.

5. **Sync Vs Async**: Some services must use asynchronous invocations; and in scenarios where quick processing is required and waiting for individual results isn't necessary, asynchronous invocation allows for concurrent processing thus speeding up the overall operation.

6. **Asynchronous Invocations in AWS Services**: 
    * `Amazon S3` sends event notifications to trigger Lambda functions asynchronously.
    * `SNS` triggers Lambda functions upon receiving notifications.
    * `CloudWatch Events/EventBridge` triggers Lambda functions in response to events happening within the AWS infrastructure.
    
    Other services using asynchronous invocations (outside of the context of this certification) could include CodeCommit, CodePipeline, CloudWatch Logs, Simple Email Service (SES), CloudFormation, Config, IoT and IoT Events.

For this certification, focus is placed on understanding how Lambda works asynchronously with services like Amazon S3, SNS, and CloudWatch Events or EventBridge.


Asynchronous Invocations Hands On

## Key Takeaways
1. **Asynchronous Lambda Invocations:** These do not give us the results back immediately. You cannot perform such invocations via AWS console, and must use Command Line Interface (CLI) instead.

2. **Status Code 202:** When an asynchronous lambda function is invoked, you receive this status code regardless of the function's success or failure.

3. **CloudWatch Logs:** These logs can be used to observe the result of the invocation. Log details include whether the function was invoked successfully or not.

4. **Dead Letter Queue (DLQ):** This can be set up for unprocessed events from asynchronous invocations. You can send these events to SQS or SNS for further error handling.

5. **Retry Attempts:** AWS Lambda will attempt to execute the function up to 2 times by default on the failure of an asynchronous event. It can be modified as per your requirements.

6. **AWS SQS Queue:** This is where unprocessed events for a lambda function can be sent. A new standard queue was created for this purpose ("lambda-DLQ") in the course. Each failed lambda function execution will result in a message sent to this queue. 

7. **IAM Role adjustment:** It may be necessary to adjust the execution role permissions of the lambda function, such as giving it full SQS access, in case of permissions error.

8. **Verification of working DLQ:** The function was invoked again to verify that it gets another two retry attempts after which a message will appear in the SQS queue for every invocation failure.

Cloudwatch Events / EventBridge

1. **Integration of CloudWatch Events or EventBridge with Lambdas**: There are two main ways to integrate them - through a serverless CRON or a rate rule, and through a CodePipeline EventBridge rule.

2. **Creating an EventBridge Rule to trigger Lambda**: This rule can be set up to trigger the Lambda function to perform a task every hour (or at any other required rate).

3. **Creating a CodePipeline EventBridge Rule**: This enables detection of state changes in CodePipeline. Whenever a state change occurs, the Lambda function is invoked to carry out a task.

4. **Steps to integrate Lambda with EventBridge**: 
   - Create a function (like `lambda-demo-eventbridge`) 
   - Choose the runtime (Python 3.9 was used in the example)
   - Invoke the function using EventBridge
   - Setup a new rule (like `InvokeLambdaEveryMinute`)
   - Select an Event bus (default Event bus can be used)
   - Follow the steps to select the preferred rule pattern (event pattern or scheduled) and to define the schedule.
   - Make sure the target of our rule is Lambda and select the appropriate Lambda function for it.
   - EventBridge automatically configures the necessary permissions.

5. **Resource Policy for a function**: A resource policy for the Lambda function is created and configured such that only the EventBridge rule has the right to invoke our Lambda function.

6. **Monitoring the function**: Invocations and other metrics can be monitored using CloudWatch logs.

7. **Updating the function**: The function can be updated to print event details whenever it is invoked. Deploy the updated code and monitor logs to see the event details.

8. **Cleanup**: Always disable your rule after you finish the task or demo to avoid unnecessary costs.

From the exercise, AWS Lambda is integrated with AWS EventBridge to enable scheduled or event-based execution of the Lambda function. This allows for efficient task automation and serverless compute abilities to any component of your cloud architecture.

S3 Event Notifications

1. **S3 event notifications** notify us when an object is created, removed, or restored, or when a replication happens. We can filter these events by prefix and suffix. 

2. The classic use case of S3 event notifications is to generate thumbnail images of every image uploaded into Amazon S3. 

3. S3 can send these notifications to three things: 
   - **SNS (Simple Notification Service)** and from there it can do a fan-out pattern to send to multiple SQS (Simple Queue Service). 
   - Directly into **SQS** and have a Lambda function directly read off that SQS.
   - **Directly invoke our Lambda function**, this is an asynchronous invocation. If things go wrong, you can set up a dead-letter queue, such as SQS.

4. S3 event notifications take seconds to be delivered but can sometimes take a minute or longer. To avoid data loss, enable versioning on your bucket.

5. A simple pattern: An S3 bucket has a new file event into Lambda. The Lambda function processes that file, maybe insert that data into a DynamoDB Table or even a table in an RDS database.

6. An S3 bucket can invoke your Lambda function. You need to create an appropriate **event notification** and assign it to your Lambda function in the S3 bucket settings.

7. You can modify your Lambda function as per need. The example here just prints the events to see what is being sent into it.

8. The S3 bucket invokes the Lambda function through permissions in the **Configuration** section. The role-based policy states the invocation permission.

9. The Lambda function working can be tested by uploading a file in a configured S3 bucket. Any new file added in the S3 bucket will trigger the Lambda function.

10. The event logs can be viewed in **CloudWatch**. Information such as event source, the object where the event was put, the bucket name, the object key, the size, and the E tag can be obtained from the logs.

11. Async Notifications and resource policy in Lambda are key to understanding Amazon’s event-driven architecture.
Event Source Mapping

1. **Understanding Event Source Mapping:** This is a mechanism by which AWS Lambda can process events. It applies to Kinesis Data Streams, SQS, SQS FIFO queue, and DynamoDB Streams. Here, records need to be polled from the source, which then invokes the associated Lambda function synchronously.

2. **Streams and Queues in Event Source Mapping:** There are two categories - Streams (apply to Kinesis Data Streams and DynamoDB Streams) and Queues (apply to SQS and SQS FIFO). 
    - In case of Streams, processing is done in order at the shard level and processing can start from a new item, from the beginning of the shard, or from a specific timestamp. 
    - Queues work with SQS polling and invoking the Lambda function synchronously with the event batch.

3. **Batch Processing and Error Management within Streams:** If having a low traffic stream, records can be accumulated in a batch before processing for efficiency. If there is very high stream traffic, multiple batches can be processed in parallel at the shard level for faster operation. On encountering an error, the entire batch is reprocessed until successful or until items in the batch expire.

4. **Queues Processing and Error Management:** For queues, batches are returned to the queue as individual items and might be processed in a different grouping than the original batch. Lambda deletes the processed items from the queue.

5. **Lambda Penetration in Queues:** Standard queues (unordered processing) scale fast, whereas FIFO queues (ordered processing) scale up to the number of active message groups.

6. **Capacity of Event Source Mapper:** For Kinesis Data Streams and DynamoDB Streams, it is one Lambda invocation per stream shard. With parallelization, up to 10 batches can be processed per shard simultaneously.

7. **Exam Tip:** Consider revisiting topics related to Event Source Mapping before the exam as specific questions may be asked on this subject.

Event Source Mapping Hands On

1. **Creating a Lambda Function and SQS Queue:** This course demonstrated the creation of a new Lambda function and an SQS queue from scratch. Also, naming conventions were discussed for easier identification. 

2. **Linking Lambda and SQS:** The process for linking a Lambda function to an SQS queue was explained. This involves adding a trigger and choosing the SQS queue that the Lambda function should retrieve messages from. It was also noted that you must specify the batch size, and consider the batch window to determine how long to gather records before again invoking the function.

3. **Permission Errors:** We encountered a permissions error linking the SQS to Lambda due to missing policies that allow the Lambda function to read from the SQS queue. Correcting this required going to the IAM console and attaching the appropriate policy ('AWSLambdaSQSQueueExecutionRole') to the role associated with the Lambda function.

4. **Lambda Function Code:** The course covered a simple coding example where the function simply prints any event data to get some information, and else returns the word "success". 

5. **Testing the Lambda Function:** A demonstration was given on how to test the Lambda function by sending a message via the SQS queue, and verifying that the Lambda function processed this message by observing the logs in CloudWatch.

6. **Disabling of event mapper:** To avoid continuous pulling from your SQS queue by the Lambda function, which can potentially lead to cost implications, it's important to disable the event mapper once it is no longer needed.

7. **Kinesis as an Alternative Trigger:** Kinesis was shown as an alternative trigger, with a number of options including consumer (standard or enhanced fan-out mode), batch size, starting position, on failure destination, retries, maximum age of record, splitting batch on error, concurrent batches per shard, tumbling window duration and reporting batch item failures.

8. **Adding Missing IAM Permissions:** It was shown how to add the missing IAM permissions to enable Lambda functions to read from Event Mappers.

Overall, the lecture provided a comprehensive introduction to event mappers, setting up Lambda functions and SQS, dealing with permission issues, and testing the setup.

Lambda Event & Context Objects

- Lambda functions in AWS have two important components: **Event** and **Context** objects.

- **Event Object**: 
  - This object is created when a Lambda function is invoked (e.g., by an EventBridge rule).
  - The event object includes comprehensive details about the event, including where it was emitted from. 
  - The invoking service (such as EventBridge, SQS, SNS, etc.) will include the data related to the event within this object. 
  - The data within the event object is processed by the Lambda function. 
  - This object is a JSON-formatted document that is converted into an object based on the runtime (for example, into a dictionary for Python).
  - This object contains all necessary input arguments or invoking service arguments needed for the Lambda function to process the event.

- **Context Object**: 
  - This object provides metadata about the function, such as the AWS request ID, function name, associated log group, memory limit, and other details about the runtime environment. 
  - It passes additional data about the invocation and the runtime environment to the Lambda function at runtime. 
  - It can be used to fetch AWS request ID, function name, memory limit (in MB), etc., during the execution of the Lambda function. 

- In your function code, the handler (in case of Python, for example) will have both an event and a context. The event will contain information such as the source or the region of the event, while the context will have information such as the request ID, the function ARN (Amazon Resource Name), function name, memory limits, and CloudWatch Logs data (like stream name or group name).

Lambda Destinations

- **Lambda Destinations** is a feature introduced in November 2019 to handle the result of asynchronous invocations or event mappers. It helps in tracking whether the event has failed or succeeded and in retrieving the data.

- With Lambda Destinations, you can define a destination for both successful and failed events. Specifically, for asynchronous invocations, you can select destinations such as SQS, SNS, Lambda, and the Amazon EventBridge bus (CloudWatch events).

- If an asynchronous invocation of Lambda function is successful or fails, the result can be sent to the respective event destination.

- Lambda Destinations is recommended over Dead-Letter Queues (DLQs) for asynchronous invocations. This is because:
  - Destinations are more recent features.
  - They allow for more target options as compared to DLQs which only allow sending failures to SQS and SNS.

- Event Source mapping, on the other hand, is used for event batches that couldn't be processed and have been discarded. The discarded event batch can be sent to Amazon SQS or SNS.

- If you have an Event Source mapping reading from SQS, you can set either a failed destination or set up a DLQ directly on your SQS queue.

- Lambda destinations provide better failure tracking and retrieval capabilities for both asynchronous invocations and event source mappings.

Lambda Destinations Hands On

1. **Destinations in the Lambda S3 function**: You can add destinations for processing successes and failures directly into the Lambda S3 function.

2. **Creating SQS queues for destinations**: SQS queues named 'S3-success' and 'S3-failure' were created to be used as successful and failed destinations respectively. 

3. **Setting up destinations in the Lambda function**: A destination was added for asynchronous invocation with a condition set for failure. The destination type was set as an SQS queue and the destination as 'S3-failure'. Permissions are added automatically when saved. 

4. **IAM role verification**: The permissions to write to an SQS queue ('S3-failure') are verified through IAM role of the Lambda function.

5. **Adding second destination for success**: A second destination for success was set in a similar way pointing to 'S3-success' SQS queue.

6. **Testing Destinations**: To test that the two destinations are working:

   - A success test: a file was uploaded to S3, processed successfully by the Lambda function, and then the message was verified in the 'S3-success' SQS queue.

   - A failure test: the Lambda function was modified to raise an exception, a file uploaded as before caused a failure which was confirmed by the presence of a message in the 'S3-failure' SQS queue.

7. **Insights on SQS messages**: The body of the messages in the SQS queues contains useful information about the request, event source, status code, body data, and other related information, providing a wealth of information about the process. 

8. **S3 Asynchronous Invocation**: S3 invoking the lambda function is asynchronous. If the function fails, it wouldn't directly send the message to the failure SQS queue. Instead, it will attempt retries before invoking the destination as a failure.

9. **Lambda Destinations Importance**: Lambda Destinations help conveniently handle and understand both success and failure scenarios of Lambda function invocations, aiding in debugging and improving efficiency.

Lambda Permissions - IAM Roles and Resource Policies

1. **IAM Roles for Lambda Functions**: AWS Lambda functions must have an IAM Role attached to them. This role grants the Lambda function the permission to access AWS services and resources.

2. **Managed Policies for Lambda**: AWS provides some simple, managed policies for Lambda that can be reused. Examples include `BasicExecutionRole`, `KinesisExecutionRole`, `DynamoDBExecutionRole`, `SQSQueueExecutionRole`, `LambdaVPCAccessExecutionRole`, and `XrayDaemonWriteAccess`. These roles allow the function to do specific tasks, like reading from a Kinesis stream or uploading logs to CloudWatch, for instance.

3. **Event Source Mapping**: If a Lambda function uses event source mapping to invoke the function, an execution role is required to read the event data.

4. **One Lambda Execution Role per Function**: AWS recommends creating a separate Lambda execution role for each function. 

5. **Resource-based Policy**: When a Lambda function is invoked by other services, a resource-based policy is used. This permits other AWS service accounts to access and invoke a specific Lambda function.

6. **Service-to-Service Access**: A resource-based policy is quite useful in service-to-service access cases. Under this scenario, one service like Amazon S3 can invoke the Lambda function, provided the resource-based policy grants the necessary access.

7. **Access to Lambda Function**: An IAM principle can access a Lambda function if either the IAM policy attached to the principal authorizes it or a resource-based policy authorizes access to the Lambda function.

Lambda Permissions - IAM Roles and Resource Policies Hands On

- Every Lambda function must have an assigned IAM (Identity and Access Management) role.
- AWS provides various predefined roles for Lambda such as Demo Lambda, Lambda ALB, Lambda S3, and SQS etc.

- A standard role attached to most Lambda functions created through the console is the 'Lambda Basic Execution' role, which enables the function to create log groups, and send logs to CloudWatch. 

- AWS Lambda uses resource-based policies when they are invoked by asynchronous events such as ALB, Lambda S3, and EventBridge etc.
    - The resource-based policy for ALB allows an elastic load balancer to invoke a Lambda function. 
    - For S3, it enables S3 buckets to invoke the Lambda function.
    - For EventBridge, the policy allows proper events to initiate the function.

- In the case of SQS, there is no resource-based policy because the Lambda function initiates the SQS queue to pull data messages and subsequently process them. Thus, it requires an 'execution role' that allows it to receive messages, delete the message, get queue attributes, etc., from the SQS queue.

- Understanding these permissions, and how they work is crucial for asynchronous type of invocations. In a situation where a Lambda function is being invoked, resource-based policy is used, while in cases where Lambda function is invoking a source, modifications to the IAM role attached to the function are required. 
Lambda Environment Variables

1. **Lambda Configurations and Deployments**
   - AWS Lambda supports the use of environment variables, which are key-value pairs in string form. They allow you to adapt the function behavior without changing your code.
   - Lambda service adds its own system environment variables along with those created by the programmer.
   
2. **Encryption of Environment Variables**
   - Both AWS Key Management Service and your own Customer Master Key can be used for encrypting these environment variables.
   - With this, secure information such as passwords can be stored in the environment variables.

3. **Practical Demonstration**
   - A new Lambda function called 'lambda-config-demo' was created for this demo, using Python 3.8.
   - The presenter displayed how to input unencrypted environment variables and let the Lambda function print these to the console. The encryption part will be discussed in the Security section of the course.
   - The fetched environment variable was displayed in the return statement using the 'os.getenv' function.
   - To illustrate the impact of environment variables on the function's behavior, the value of the input variable was changed without any changes being made to the code. This change was successfully reflected in the function's response — showing the power of environment variables.
Lambda Monitoring & X-Ray Tracing

1. **Lambda-CloudWatch Integration:** AWS Lambda automatically logs its execution logs into CloudWatch, provided that the execution role has the correct IAM policy authorizing this function.

2. **CloudWatch Metrics:** CloudWatch Metrics display high-level statistical data about AWS Lambda instances such as invocation count, duration, concurrent execution, error counts, success rate, throttles, and asynchronous delivery failures. If the lambda function is consuming Kinesis or DynamoDB streams, metrics about the IteratorAge can also be found here.

3. **Lambda Tracing with X-Ray:** Tracing with X-ray is enabled in Lambda through Active Tracing. Additionally, it requires using the X-ray SDK in your code and having the appropriate IAM execution role to write into X-ray. For this, AWS provides a managed policy named 'AWS x-ray daemon write access.' 

4. **Relevant Environment Variables:** There are certain environment variables that are crucial for Lambda to communicate with X-ray. These include `_X_AMZN_TRACE_ID`, `AWS_XRAY_CONTEXT_MISSING`, and `AWS_XRAY_DAEMON_ADDRESS`. These variables can be leveraged the same way as other environment variables. The `AWS_XRAY_DAEMON_ADDRESS` variable presents where the IP and the port of the x-ray daemon is running regarding your Lambda function and is especially significant.


Lambda Monitoring & X-Ray Tracing Hands On

1. **Lambda Function Monitoring**: The Lambda S3 function's monitor tab provides key information, such as function invocation, invocation duration, error counts, and success rates. 

2. **CloudWatch Metrics**: These are useful for Lambda functions. When Lambda functions are invoked, log streams will be created. These log streams can provide notable data, like the request ID, function duration, memory usage, and other relevant metrics. 

3. **AWS X-Ray Integration**: Lambda functions can log their traces into AWS X-Ray. To set this up, go to "Configuration", then "Monitoring and Operation Tools" and enable X-Ray. The console will adjust permissions so that Lambda can write into X-Ray. This can allow for better tracking of the functioning and potential issues. 

4. **Testing** Lambda functions: By uploading data (like a HTML file) into S3, the Lambda function will process these files. This allows the user to examine the data flow and possible errors in a real case scenario. 

5. **Trace Segments and Telemetry Reports**: With X-Ray enabled, Lambda function will be writing these reports, which are useful for troubleshooting and function enhancement.

6. **Simultaneous Function Execution**: If you have a high-volume Lambda function, the concurrent executions will increase. Monitoring this data gives an indication of the function’s demand and performance under load.

7. **Throttle and Async Delivery Failures**: Useful to monitor in case the Lambda function exceeds limits or doesn't get the chance to process events. Also observe the Iterator Age for stream-oriented processing.

Remember to continually monitor functions in production for potential error and successes, this will help optimize the function's performance and eliminate any recurring issues.

Lamda@Edge & Cloudfront Functions

### 1. Customization at the Edge
- Modern applications can execute logic at the Edge, before reaching the application itself, known as Edge Functions. 
- Edge Functions are attached to CloudFront distributions and are executed close to users to minimize latency.
- These functions are serverless and globally deployed.
- Use cases include customization of CDN content from CloudFront, bot mitigation, A/B testing, user authentication, and more.

### 2. CloudFront Functions and Lambda@Edge
- Two types of Edge Functions are offered by CloudFront: CloudFront Functions and Lambda@Edge.
- CloudFront Functions are lightweight functions written in JavaScript. They modify the viewer request and response. They are used for high-scale, latency-sensitive CDN customizations with sub-millisecond startup times.
- Lambda@Edge offers more functionality and can be written in NodeJS or Python. They have the power to modify both the viewer request/response and origin request/response. These functions are authored in the us-east-1 region and replicated among all CloudFront locations.

### 3. Differences between CloudFront Functions and Lambda@Edge
- Runtime support: JavaScript for CloudFront Functions, NodeJS and Python for Lambda@Edge.
- Scale: CloudFront Functions handle millions of requests per second, Lambda@Edge handles thousands.
- Triggers: CloudFront Functions only impact the viewer request and response, whereas Lambda@Edge can affect viewer and origin.
- Execution time: Less than 1 millisecond for CloudFront Functions, whereas Lambda@Edge can be up to 5 to 10 seconds.
- Lambda@Edge has additional features such as adjustable CPU and memory, network access to external services, and access to the body of the HTTP request.

### 4. Use Cases
- CloudFront Functions: Used for quick tasks such as cache key normalization, header manipulation, URL rewrites, and request authorizations.
- Lambda@Edge: Used for tasks requiring longer execution time, such as loading large libraries, integrating with external services, and access to HTTP request bodies.

Lambda in VPC

1. **Lambda Function Default Networking**: By default, Lambda functions are launched outside of your VPC in a separate VPC owned by AWS. Consequently, they don't have access to resources within your VPC like EC2 instances, RDS databases, and Elastic Load Balancers.

2. **Lambda Within Your VPC**: Lambda functions can be deployed within your VPC. For this, you need to specify your VPC ID, the subnets, and assign a security group to your Lambda function.

3. **Elastic Network Interface (ENI)**: Deploying within your VPC prompts Lambda to create an ENI in your chosen subnet. This process requires giving your Lambda function a 'Lambda VPC Access Execution Role.' 

4. **Accessing RDS**: Deploying your Lambda function in a private subnet and configuring it properly enables access to your RDS through the ENI with the rules set in the RDS security group.

5. **Lambda Without Internet Access**: A lambda function deployed within your VPC doesn't have internet access by default. This remains true even when the function is deployed within a public subnet.

6. **Internet Access Via NAT Gateway/Instance**: Lambda functions can be given internet access by deploying them in a private subnet, together with a NAT gateway or instance.

7. **Accessing DynamoDB**: You can configure your Lambda function to access DynamoDB either through the public route (via the internet gateway), which needs a NAT, or privately through VPC endpoints.

8. **CloudWatch Logs Functionality**: CloudWatch Logs remain functional regardless of whether your Lambda function is deployed in a private subnet, and it doesn't require endpoints or a NAT gateway.

9. **Possible Exam Question**: Remember, AWS certification exams might test your understanding of Lambda functions deployed in a public subnet, particularly, they do not have internet access by default.

10. **VPC Endpoints**: They can be used for private access to AWS services like DynamoDB without accessing a NAT device or an internet gateway.
Lambda in VPC Hands On

1. **Creating a Lambda Function and Security Group:** The first step is creating an AWS Lambda function from scratch and selecting Python 3.8 as the runtime. Afterward, a security group, referred to as 'Lambda SG' (Security Group), is needed, and attached it to the VPC.

2. **Lambda Function Configuration:** A Lambda function by default has access to the internet but not to the VPC. To allow its functioning within a particular VPC, edit the configuration under 'VPC' in the configuration tab.

3. **Internet Access for Lambda Function:** A Lambda function doesn't automatically get internet access when connected to a VPC. A Lambda function can only have access to the internet through a NAT Gateway or an instance in a public subnet which will route traffic to a private subnet with the Lambda function.

4. **Assigning a Security Group to Lambda Function:** Assign the previously created security group to the Lambda function. This controls the inbound and outbound traffic.

5. **Permissions for Lambda Function:** When deploying a Lambda function in a VPC, it requires network interfaces and specific permissions to operate. Add 'Lambda ENI management access' permissions to the Lambda function, giving it the necessary permissions to create, delete, and describe network interfaces.

6. **Testing the Function:** Test your newly created and configured Lambda function within the VPC. It may take a few minutes to be updated and start at first.

7. **Viewing Network Interfaces:** The created network interfaces for the Lambda function may be viewed in the EC2 management console. These allow the Lambda function to communicate within your VPC.

8. **Use Cases:** Deploying a Lambda function within a VPC is usually done for local operations such as on an RDS database or an Elastic Cache cluster.
Lambda Function Performance

1. **Lambda Function Configuration and Performance:**
    - RAM for a lambda function can be scaled up to 10 gigabytes in 1 megabyte increments. Increasing the RAM leads to more vCPU credits, i.e., more computational resources.
    - After reaching 1,792 megabytes of RAM, the lambda function has the equivalent of one full vCPU.
    - If an application is CPU bound, improving the application’s performance will require increasing the lambda function's RAM. 
    - By default, AWS Lambda has a timeout of three seconds but can be set up to a maximum of 900 seconds (15 minutes). Any application requiring more than this should be run on Fargate, ECS, or EC2.
    
2. **Functional Execution Context:**
   - Lambda's execution context is a temporary runtime environment that initializes any external dependencies of lambda code. This can be used to establish database connections, create HTTP clients, and SDK clients, thus improving performance.
   - An execution context can reuse pre-existing database connections and clients if the function is invoked multiple times.
   - The execution context includes the `/tmp` directory where temporary files can be stored and accessed across several function executions.

3. **Best Practices:**
   - If a process takes a lot of time to initialize, AWS recommends initializing it outside of the handler for improved performance, such as initial database connections.
   - Temporary large files or files requiring disk space to operate should be stored in the `/tmp` (temporary) space. This directory has a capacity of 10GB.
   - If objects need to be permanantly persisted then they should be stored in a location like Amazon S3.
   - To encrypt content in the `/tmp` space, KMS (Key Management Service) must be used to generate data keys and encrypt the desired data accordingly. 

Note: AWS Lambda's configuration and the ability to reuse execution contexts are common topics for examination questions in AWS certification.

Lambda Layers

1. Lambda Layers are a feature of AWS Lambda that allows us to:
   - Create custom runtimes for Lambda: Languages not initially supported by Lambda can be used via Lambda Layers. Examples given are C++ and Rust.
   - Externalize and reuse dependencies: Removing the need to repeatedly upload the entirety of an application package when only a smaller part has changed.

2. A typical use case is when you need to update a Lambda function but the large dependencies don't or rarely change. Externalizing these dependencies into a Layer can save bandwidth and upload time and make deployments faster.

3. The goal is to separate the frequently changing part (the application package) from the more static part (heavier libraries).

4. Your Lambda function can reference multiple layers. This makes it easier to manage and update dependencies separately.

5. By externalising your heavy libraries into Lambda Layers, you can reuse them across different Lambda functions, increasing efficiency and reducing redundancy.
Lambda Filesystems Mounting

# Key Takeaways from the AWS Certification Preparation Course on File System Mounting in Lambda

1. **File System Mounting**: AWS Lambda functions can access your EFS (Elastic File System) if they are running within a VPC (Virtual Private Cloud). Lambda can be configured to mount the EFS file system to a local directory during initialization. To enable this, you must use the EFS Access Points feature of EFS.

2. **EFS Access Points**: If your Lambda functions are deployed in a Private Subnet with private connectivity into your VPC, then mounting the EFS file system is possible. 

3. **Limitations**: Each Lambda instance that comes up corresponds to a distinct connection into your EFS file system. You must ensure that you do not exceed EFS connection limits. You may also hit connection burst limits if you have many Lambda functions initiating simultaneously.

4. **Comparison of storage options for Lambda**:

    - **Ephemeral Storage, /tmp**: This storage has a max size of 10 GB. It's ephemeral, meaning all stored content is lost when the Lambda function instance is destroyed. It's a modifiable file system that supports all file system operations, and only the specific function has access to it. The cost is included in your Lambda function pricing up to 512 MB, beyond which you pay more.
    
    - **Lambda Layers**: The maximum size is five layers per function, up to 250 MB total. The persistence here is durable and immutable - you can't change what's in a Lambda Layer. Access requires the right IAM permissions. Data on this layer cannot be modified.
    
    - **Amazon S3**: This can accommodate significantly large file sizes. It's durable and dynamic, requiring the S3 API for access. You pay for storage, requests, and data transfer. Proper IAM permissions are required for access.
    
    - **Amazon EFS**: The storage is elastic, durable, and dynamic. It uses standard file system operations for accessing data. It is mounted as a network file system on your Lambda function, ensuring fast data access. The cost depends on storage, data transfer, and throughput.

All storage mediums except for Ephemeral Storage (/tmp) are shared across all your Lambda function invocations.

Lambda Concurrency

- **Lambda Concurrency and Throttling**: AWS Lambda scales easily and fast, and the more you invoke Lambda functions the more concurrent executions can happen. Depending on your events' scale, it can go up to 1000 concurrent executions.

- **Reserved Concurrency**: It's possible to limit the number of concurrent executions a Lambda function can perform by setting a 'reserved concurrency' at the function level. Invocations over this limit trigger a 'throttle'. 

- **Behavior of Throttling**: For synchronous invocations, a throttle error 429 will be returned if throttled. For asynchronous invocations, it will retry automatically and then go to the Dead Letter Queue (DLQ). If you need higher than 1000 concurrent executions, you can open a support ticket to request a limit increase.

- **Concurrent Execution Limit**: This limit applies to all functions in your account, therefore it is possible for other functions to get throttled if one function goes over the limit. It's recommended to setup concurrency carefully.

- **Concurrency and Asynchronous Invocations**: If async invocations do not have enough concurrency available to scale up and reach the limit, additional requests get throttled. In this case, any throttling errors (429) and system errors (500-series), will return the event to an internal event queue with Lambda attempting to run the function again for up to six hours, increasing retry interval in an exponential fashion (from one second up to five minutes max).

- **Cold Starts and Provisioned Concurrency**: 'Cold Start' happens when a new Lambda function instance is created. With a high latency for the first request, it might affect user experience. AWS provides 'Provisioned Concurrency', where you allocate concurrency in advance, prior to the function being invoked, hence minimizing the effects of cold starts.

- Note: Improvements have been made by AWS to reduce cold starts in your VPC if you were using Lambda before, thus, cold starts now have minimal impact on your VPC. 
Lambda Concurrency Hands On

**Concurrency Settings in AWS Lambda**

1. **Unreserved Account Concurrency:**
   - Each AWS account has an unreserved account concurrency limit of 1000, which is shared across all the Lambda functions in the account.
   - Users can edit the unreserved concurrency of individual Lambda functions by reserving a specific number.

2. **Reserved Concurrency:**
   - Users can set specific reserved concurrency for each Lambda function. This number subtracts from the total unreserved account concurrency.
   - For example, if a function's reserved concurrency is set to 20, it leaves 980 unreserved account concurrency available to other functions.

3. **Testing Concurrency:**
   - Setting reserved concurrency to zero helps test the function's response to being throttled. 
   - If the function is over its reserved concurrency, it will fail with an error message about exceeding the rate.

4. **Setting Concurrency:**
   - To fix concurrency issues, users can go back into the concurrency settings and choose to use the unreserved account concurrency or reserve a specific concurrency for the function.

5. **Provisioned Concurrency:**
   - Provisioned concurrency helps eliminate cold starts, where an application takes time to initialize.
   - Users can provision concurrency to specific aliases or versions of a Lambda function to maintain a warm pool. 
   - Note that provisioned concurrency is not available for the "latest" version; users must publish a different version.
   - Provisioned concurrency incurs cost based on the set number, so users should set a number that aligns with their budget and needs.
   
6. **Testing the Lambda Function:**
   - After adjusting concurrency settings, the function can be tested directly from the Lambda panel on AWS. If the function execution works, results will appear within this window.

Lambda External Dependencies

1. **Use of Dependencies:** In real-world scenarios, AWS Lambda functions often require external dependencies beyond the core code, such as the X-Ray SDK, Database Clients, etc.

2. **Packaging Dependencies:** Depending upon the language used, different package managers could be used to bundle these dependencies with your Lambda function. For instance:
   - JavaScript uses NPM and the node_modules directory.
   - Python uses PIP management as target options.
   - Java includes relevant .jar files.

3. **Uploading the Bundle:** Once the code and the dependencies are packaged together into a zip file, it can be directly uploaded to Lambda if it is less than 50MB in size. If it is larger, it should be first uploaded to Amazon S3 and referenced from Lambda.

4. **Native Libraries:** Any native libraries that your Lambda function relies on must first be compiled on Amazon Linux to ensure compatibility.

5. **AWS SDK in Lambda:** AWS SDK is included with every Lambda function by default. If your Lambda function uses AWS SDK, you do not need to bundle it alongside your code.

Lambda External Dependencies Hands On

1. **Lambda Function with Dependencies**: The course covers creating a Lambda function with dependencies using Node.js files; index.js and steps.js.

2. **AWS CloudShell**: AWS CloudShell is used as it directly connects to AWS accounts and has NPM (Node Package Manager) installed, which is essential for handling packages in Node.js.

3. **Creating a Lambda Folder**: In CloudShell, a new Lambda directory is created, which is where the index.js file is added.

4. **Dependency Installation**: In the course, AWS X-Ray SDK is installed locally using the npm install command, generating necessary files and folders.

5. **Creating ZIP file**: After setting file permissions, all the files are put into a ZIP file called function.zip. The .zip file is then uploaded to the AWS Lambda console.

6. **Using CLI**: The course shows how to automate the upload process using the Command Line Interface (CLI) rather than manual Zen. For this to work, a role for the Lambda function is to be created in IAM (Identity and Access Management).

7. **Lambda with Dependencies**: A Lambda function with dependencies is created using the CLI in CloudShell. By following the steps, one can verify that the function, along with dependencies, has been uploaded successfully.

8. **Permissions & Testing**: When testing the function, if there's failure due to inadequate permissions (e.g., to access Amazon S3), permissions should be amended in the IAM role. Additionally, for X-ray monitoring and active tracing, enable the X-Ray in the Lambda console's configuration.

9. **X-Ray Tracing**: AWS X-Ray provides a service map and traces to detail the activities and performance of the Lambda function. It gives insights into errors and successful invocations, thereby helping diagnose and fix issues.

In summary, this AWS certification preparation course covers creating a Lambda function with dependencies, working with AWS services like CloudShell, NPM, IAM, and utilizing the AWS CLI and X-Ray for deployment, permissions management and tracing respectively.

Lambda and Cloudformation

1. **CloudFormation to upload Lambda function:** There are two methods for implementing this -
    - **Inline**: The Lambda code is defined inline in the CloudFormation templates. This is good for simple functions without dependencies. This uses the Code.ZipFile property in the template.
    - **Zip file through S3**: We store the Lambda function zip in an S3 bucket and refer to its location in the CloudFormation template. This is handy for managing function dependencies.

2. **S3 Bucket attributes**: These are some of the attributes related to the S3 bucket in the CloudFormation code - S3 bucket, S3 Key (represents full path to the zip in S3), and S3 object version (useful in versioned buckets). 

3. **Versioning**: In case of updating the Lambda code in S3 without updating any of the above-mentioned attributes, CloudFormation won't update the function. Thus, versioning is recommended. If a versioned bucket is used and a new S3 object version is specified, CloudFormation will pick up the change and update the Lambda function.

4. **Multi-account deployment**: If deploying a Lambda function through CloudFormation across multiple AWS accounts is desired, access controls and policies need to be properly managed. An S3 bucket policy in Account 1 (which contains the Lambda code) should allow CloudFormation in Account 2/3 to access the S3 bucket. This access can also be empowered by defining an execution role on the CloudFormation service template which allows to `GET` and list on the S3 bucket in Account 1.

This lecture was mostly focused on discussing the different ways to deploy Lambda function code using CloudFormation and the intricacy related to S3 bucket attributes for proper deployment, especially when dealing with multi-account access.

Lambda and Cloudformation Hands On

1. A CloudFormation Template was created and uploaded to setup a Lambda function.

2. The CloudFormation template contained three parameters: S3 Bucket, S3 Key, and S3 Object version which tells CloudFormation where to find the function zip in Amazon S3.

3. In the resources, a Lambda execution IAM role was created with a policy document that allows the function to assume the role. Multiple policy statements were included giving actions on CloudWatch logs, X-ray and allow operations on S3.

4. The Lambda function was defined with handlers such as 'Index.handler' and the newly created IAM role was attached to it using 'get attributes'. Lambda code was specified by references to the parameters defined at the beginning of the template.

5. The Lambda function runtime was set to NodeJS 14.X with a 10-second timer and X-ray was enabled.

6. In order to run the CloudFormation Template, a new S3 bucket was created with bucket versioning enabled.

7. A function code (function.zip) was placed in the newly created S3 bucket.

8. In the CloudFormation Console, a Stack was created using the previously crafted Template. AWS automatically recognized the integration between CloudFormation and Lambda based on the template.

9. The Lambda function was successfully created and managed by the deployed CloudFormation Stack.

10. After confirming the creation of the function and its listed permissions, X-ray was used to review traces.

11. Cleanup could be performed by deleting the stack, which would delete both the Lambda role and the function itself.
Lambda Container Images

1. **Lambda and Container Images**

   - AWS Lambda now supports container images, allowing you to deploy Lambda functions as container images up to 10GB using ECR. 
   - This helps accommodate complex dependencies or large dependencies in a container.
   - The base image used must implement the Lambda Runtime API, so that Lambda may run the virtual machine, the container. This base image exists for languages such as Python, Node.js, Java, .NET, Go, and Ruby. It's also possible to create your own Lambda base image provided it implements the Lambda Runtime API.

2. **Lambda Container Use**

   - The application code, its dependencies, and relevant datasets, all placed on top of the base image, form an image pack that Lambda can run.
   - The Lambda Runtime Interface Emulator makes it possible to test your containers locally.
   - Containers can be built and published the same way for either ECS or Lambda. Then, they are sent into Amazon ECR for deployment onto Lambda.

3. **Lambda Container Image Structure**

   - Using a base image provided by AWS that implements the Lambda Runtime API is recommended. An example here is `amazon/aws-lambda-nodejs:12`.
   - The application code and files are copied onto the container, then the necessary dependencies are installed, in this case with `npm install`. 
   - The function to run when the Lambda Function is invoked is specified with, for example, `CMD [ "app.lambdaHandler" ]`.

4. **Best Practices for Using Lambda Container Images**

   - Using AWS-provided base images built on Amazon Linux 2 which are already cached by the Lambda service is recommended to minimize information-pulling from containers.
   - Employ multi-stage builds to keep the final image small and simple.
   - Build images in different layers, with those changing most frequently placed at the end of the build phase.
   - Using a single repository for functions with large layers helps avoid uploading and storing duplicates of these layers.

5. **Use Cases for Lambda Container images**

   - For large Lambda Functions (up to 10GB), generating a large container image to serve as the basis for your Lambda Function is more beneficial than pushing code to Lambda directly.
Lambda Version and Aliases

1. **Lambda Versions:** 

   - The code development stages in AWS Lambda functions are referred to as versions. The `LATEST` version is mutable where code and environment variables can be edited.
   - When you're happy with your code state, you can publish the Lambda function thus creating a new version (like V1, V2, etc.).
   - These versions are immutable, i.e., you cannot change the code or environment variables afterwards.
   - Each version is independent, having its own Amazon Resource Name (ARN).
   - Each version (as well as the `LATEST`) of Lambda function can be accessed separately.

2. **Lambda Aliases:** 

   - Aliases act as pointers that reference particular versions of your Lambda function.
   - You can have different aliases for different stages of development - DEV, TEST, PROD and have them point to different Lambda versions.
   - The aliases are mutable, allowing for code edits in the latest version via a `DEV` alias.
   - With aliases, you can perform canary deployments by assigning weights to the Lambda versions they point to. This allows phased rollouts and testing of newer versions with a small percentage of traffic.
   - Aliases provide stable configuration for Lambda triggers but at the same time, they can point to different versions at the backend.
   - Aliases cannot reference other aliases. They can only reference versions.

Remember, Lambda Versions and Aliases are crucial concepts to understand for any AWS certification exam and are likely to be tested.
Lambda Version and Aliases Visibility

1. **Versions in AWS Lambda**: They allow us to fix an amount of code and removables, and settings in time as specific versions. They facilitate the problem of the evolution of the functions over time.

2. **Testing versions**: After creating a version, it can be tested to ensure it works as expected. Once the code of a version has been finalized and tested, it gets published and becomes immutable, meaning it cannot be edited.

3. **Publishing new version**: After creating a function and testing it, you can publish it as a new version. Multiple versions of the same function can run in parallel by modifying and publishing the function code.

4. **Aliases in AWS Lambda**: Aliases are used for pointing to different versions of a Lambda function. This means we can have an alias 'dev' which points to the latest code, a 'test' alias that points to the recently published version for testing, and a 'prod' alias that points to the most stable version.

5. **Weighted Aliases**: This feature allows the traffic to be split between two different versions of a Lambda function. This is useful for implementing canary deployments or blue/green deployments.

6. **Updating Versions**: Once you are satisfied with the performance of the newer versions in the prod alias, it can be modified to only use the new version, effectively completing an update rollout.

7. **Agility and Flexibility**: With versioning and aliases, AWS Lambda provides the ability to quickly respond to changes in business requirements with minimal risk, by allowing to incrementally roll out changes and rollback if something goes wrong.


Lambda and CodeDeploy

1. **Integration with CodeDeploy:** AWS Lambda can be integrated with AWS CodeDeploy, which can automate the traffic shift for your Lambda aliases. This builds on the use of versions and aliases.

2. **Serverless Application Model (SAM) Framework:** CodeDeploy feature is integrated within the SAM framework, which will be used for practicing CodeDeploy deployment for Lambda functions.

3. **Updating Versions using CodeDeploy:** If you have a PROD alias and you want to upgrade from Lambda function Version 1 to Version 2, CodeDeploy can help shift the traffic gradually from V1 to V2 until 100% of the traffic falls on V2.

4. **Strategies in CodeDeploy:** CodeDeploy offers different strategies for traffic shift, including

   - Linear: Traffic increases every "N" minutes until it reaches 100%. Examples are `Linear10PercentEvery3Minutes` and `Linear10PercentEvery10Minutes`.
   - Canary: A certain percentage of traffic is tried out first and then everything is switched to 100%. Examples are `Canary10Percent5Minutes` and `Canary10PercentEvery30Minutes`.
   - AllAtOnce: An immediate traffic shift from V1 to V2. This is faster but risky without thorough testing.

5. **Rolling Back in Case of Failure:** You can create pre and post traffic hooks to check the health of your Lambda function. If something goes wrong, CodeDeploy can rollback and shift traffic back to V1.

6. **Important Parameters in AppSpec.yml:** When deploying Lambda with CodeDeploy using AppSpec.yml, some important parameters to know are:
  
   - Name: Name of the function to be deployed.
   - Alias: Name of the alias of the Lambda function which is required.
   - CurrentVersion: The version of the Lambda function that the traffic currently points to.
   - TargetVersion: The version of the Lambda function that traffic will be shifted to.

7. **Alias Update by CodeDeploy:** CodeDeploy updates the alias from CurrentVersion to TargetVersion over time, enabling a gradual transition between the two versions of Lambda functions.

Lamda Function URL

1. **Lambda Function URL**: AWS Lambda provides a unique function URL that does not change and allows your function to be accessed as an HTTP endpoint. This eliminates the need to use API Gateway or an application load balancer. The URL supports IPv4 and IPv6.

2. **Access Method**: A published Lambda function URL can be accessed and perform HTTPS requests using web browsers, command line, Postman, or any HTTP client.

3. **Access Limitations**: The function URL can only be accessed via the public internet. If you need private access, this feature does not support it.

4. **Cross-Origin Resource Sharing (CORS)**: If you access the function URL from a different domain, you need to configure CORS settings. This is similar to CORS management in Amazon S3.

5. **Security Controls**: Security of the Lambda function URL is managed using resource-based policies. These policies specify which AWS accounts, IP ranges (CIDR) or IAM principals can access your Lambda function URL.

6. **Policy Applicability**: You can apply these access policies to any function alias or your Lambda function's latest version. However, they cannot be applied directly to specific function versions.

7. **Throttling**: If you need to limit the execution of your Lambda function to a specific number, use the reserved concurrency feature of Lambda.

8. **Authentication Types (AuthType)**: 
   - `NONE` allows public and unauthenticated access to your Lambda function. Public access must be granted in the resource policy.
   - `AWS_IAM` uses IAM to authenticate and authorize request to your Lambda function. It requires evaluation and allowance of both the principal identity-based policy and the resource-based policies.

9. **Cross-account Access**: For workloads running across AWS accounts, Lambda function access requires both the identity policy and the resource-based policy to allow invoke access.

Lamda Function URL Hands On

1. **Creation of Lambda Function**: A simple Lambda function named "lambda-demo-url" is created using Python 3.9. This function, when tested, returns an HTTP-like response with the status code 200 and a body message saying "Hello from Lambda."

2. **Publishing the Function**: The function is then published as version 1.

3. **Alias Creation**: An alias 'dev' is then created for version 1 of the function. 

4. **Function URL**: The Function URL initially appears empty but can be created via the 'Create Function URL' option on the left sidebar. This URL allows someone to access the function.

5. **Authentication Type**: The authentication type is set to 'NONE', and the necessary resource policy is created automatically. This policy allows anyone to access the Lambda function with the AuthType 'NONE'.

6. **Cross-Origin Resource Sharing (CORS) Configuration**: CORS can be configured to establish the permitted origins and exposed headers. However, this wasn't done in this example.

7. **URL Permanence**: The Function URL, once created, remains the same for that alias, allowing you to consistently access the Lambda function.

8. **Version vs Alias**: You can only add a Function URL to unpublished function versions or to an alias, not to a published version. If wanted, a Function URL can be created for the latest, unpublished version of the Lambda function.
Lambda - CodeGuru Integration

- Lambda and CodeGuru can be utilized together to gain critical insights into the runtime performance of your Lambda functions.

- CodeGuru creates a profiler group for your Lambda function, compatible with Java and Python runtimes.

- The CodeGuru integration can be simply activated from the Lambda console.

- After activation, one gets access to runtime performance insights for their Lambda functions.

- The activation of this integration means that a CodeGuru Profiler layer is automatically added to your function as a Lambda layer.

- The environment variables pertaining to CodeGuru will be integrated into your function.

- It is essential to have the necessary IAM permissions for this integration to fully function.

- Importantly, the policy `AmazonCodeGuruProfilerAgentAccess` will be added to the function IAM role to grant these necessary permissions.

Lambda Limits

**Execution Limits:**

- Memory allocation for execution varies from 128 megabytes to 10 gigabytes, in 64 megabytes increments.
- Increasing memory also increases vCPU.
- The maximum execution time is 900 seconds or 15 minutes. Tasks requiring longer are not suitable for Lambda.
- Environment variable space is limited to four kilobytes.
- Temporary storage of up to 10 gigabytes is available in the `/tmp` folder for processing larger files.
- Lambda supports up to 1000 concurrent executions over Lambda functions. This number can be increased upon request.

**Deployment Limits:**

- The maximum size for a compressed zipped function is 50 megabytes.
- For uncompressed function, the limit is 250 megabytes. For larger files, consider using temporary space (`/tmp`).
- The environmental variable limit is again, four kilobytes.

Understanding these limits are crucial to indicating what workloads are feasible on AWS Lambda. If the exam poses scenarios requiring for example, 30 gigabytes of RAM, 30 minutes of execution time, or a 3 gigabyte file, these would not be suitable for Lambda according to the listed constraints.

Lambda Best Practices

1. Perform heavy duty work **outside of your function handler** to minimize its runtime. This includes tasks such as:

   - Connecting to databases
   - Initializing the AWS SDK
   - Building dependencies or datasets

2. Use **environment variables** for any parameters that will change over time, such as:

   - Database connection strings
   - S3 bucket names

   Note: Avoid hard-coding these values directly into your code.

3. For passwords and sensitive values, ensure you **encrypt the environment variables using KMS**.

4. **Minimize your deployment package size** to its runtime necessities. Remember to consider Lambda limits for your package size. Consider breaking down your function if it gets too big.

5. If you need to reuse some libraries, consider **leveraging Lambda layers**.

6. **Avoid nesting or recursive code**: refrain from having a Lambda function call itself as this can result in high costs and potentially disastrous complications. 

