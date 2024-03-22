Overview

## Serverless Application: AWS Lambda and DynamoDB
- Functions can utilize DynamoDB as a database for APIs to perform create, read, update, delete operations.

## AWS API Gateway
- The API Gateway is another serverless offering from AWS.
- The client communicates with the API Gateway, which then proxies the request to Lambda functions.
- It serves as more than just an HTTP endpoint, providing features like authentication, usage plans, development stages, WebSocket Protocol support, etc.
- It also handles API versioning, supports multiple environments, and includes security measures.
- Besides Lambda function, API Gateway can also integrate with HTTP and AWS services (for instance, starting a Step Function workflow or posting a message to SQS).

## API Gateway Deployment
- There are three kinds of deployment: 
   - Edge-Optimized: For global clients; the request is routed through CloudFront Edge locations.
   - Regional Deployment: When all users are expected to be within the same region where the API Gateway was created.
   - Private API Gateway: Only accessible from within the VPC.
   
## API Gateway Security
- Identification can be done through IAM roles, Amazon Cognito, and custom authorizers.
- HTTPS security can be enabled through custom domain names by integrating with the AWS Certificate Manager (ACM).
- To point to the domain and API Gateway, a CNAME or A-alias record must be set up in Route 53.

Api Gateway Stages and Deployments

1. **API Gateway and Deployments**: AWS API Gateway requires a deployment each time changes are made to the API. A common source of errors is forgetting to deploy after making changes. 
   
2. **Stages and Configurations**: Changes are deployed to stages, and you may have as many stages as you want with any naming convention (e.g., dev, test, prod or v1, v2, v3, etc.). Each stage has its own configuration parameters which can be seamlessly rolled back as each deployment history is stored.

3. **Versioning and Stages Strategy**: In case of a breaking API change, you could create a separate stage for the new version to avoid breaking existing clients. For instance, if you have a v1 stage and you're developing a v2, create a v2 stage that links to the v2 function. This allows v1 and v2 to coexist and clients to be gradually transitioned from v1 to v2.

4. **Use of Stage Variables**: Stage variables work similarly to environment variables but for API Gateway stages. They let you change configuration values without redeploying your API. These could be used in Lambda function ARN, HTTP endpoint or parameter mapping templates. You get access to these values from within your Lambda function using '$stageVariables.variableName' format. 

5. **Linking API Gateway Stages and Lambda Function Aliases**: You can create a stage variable that indicates the corresponding Lambda alias that the API Gateway should invoke. This enables automatic invocation of the right Lambda function.

6. **Lambda Aliases and Traffic Management**: Lambda aliases can manage traffic between different versions of a function. For example, a 'prod alias' could distribute 95% of traffic to 'v1' and 5% to 'v2'. This provides a pathway for version transitions without needing to update the API Gateway, where each stage points to the right alias, which in turn redirects to the correct Lambda function. 



Api Gateway Stages and Deployments Hands On


1. **Stage Variables** in API Gateway function as environment configurations for your backend setup. These are utilized to manage different versions of a service.

2. **Lambda Functions and Versions**: Lambda functions were authored in different versions to showcase the use of stage variables. Three versions of the lambda function - v1, v2 and 'latest' were created. 'latest' is the version in active development.

3. **Lambda Function Aliases**: Aliases 'DEV', 'TEST' and 'PROD' were created referring to the latest, v2 and v1 versions of the lambda function respectively. These aliases act as pointers to specific versions.

4. **API Gateway and Stage Variable ARN**: The Amazon Resource Name (ARN) for these aliases in API Gateway contained a stage variable, allowing for the user to navigate between different versions of the Lambda function.

5. **Assigning Permissions**: This was done to ensure that API Gateway has the permissions to invoke the lambda versions corresponding to each stage. The permissions were assigned with AWS CLI commands using Cloud Shell.

6. **Creating & Managing API Gateway Stages**: API Gateway stages (DEV, TEST, PROD) were created and the necessary stage variables were configured. This meant that, for instance, the DEV stage always points to the latest alias of the Lambda function. The TEST stage points to v2 alias of the function and so on.

7. **Deployment & Testing**: After deployment, by changing the URL to different stages, various versions of the lambda function were invoked, demonstrating how stage variables can be used to control the behavior of the API Gateway.

8. **Key Concept**: Each stage in the API deployment is linked to a specific version of Lambda function through the use of stage variables. This demonstrates the power and flexibility of using stage variables in AWS.


Api Gateway Stages Configurations

1. **Stage Configuration:** You can edit the stage details to modify the stage description on AWS API Gateway.

2. **API Cache:** You can configure your API Gateway stages to have a cache to store requests and responses, enhancing performance by reducing the traffic and latency between the API and backend services.

3. **Throttling**: You can control the rate and burst requests per second to your API.

4. **Firewall and Client Certificate:** Configuring these helps in verifying that the requests being handled are indeed being routed via the API Gateway.

5. **Logging and Tracing**: You can implement logging and tracing using services like CloudWatch Logs. You can opt to find error logs, info logs, or even full request and response logs.

6. **Extra Tracing**: In-depth tracing is available, especially for achieving a comprehensive integration between the API Gateway and the AWS X-Ray service.

7. **Stage Variables**: These are used for maintaining a consistent setting across various stages and controlling the behaviour of the APIs during their runtime.

8. **Deployment History**: This functionality helps in keeping tabs on the number of times a deployment to the stage has occurred.

9. **Documentation History**: If you manage documentation for your API, this function is useful.

10. **Canary Release**: This strategy is utilized for deploying updates or new functionalities. More details will be covered in the subsequent lectures.

11. **Tags**: This feature helps in organizing resources by categories, like stage, for the smooth management of the AWS environment. 
Canary Deployments

1. Canary deployments allow you to test changes on a small portion of your traffic on your Amazon API Gateway, typically done in a production environment.  

2. You can specify the percentage of traffic the canary channel will receive, enabling you to test a new version of your API Gateway backed by a Lambda function.

3. Canary deployments involve creating a 'prod stage canary' for the new version allowing your client to still operate with most of the traffic (e.g., 95%) going to the existing, proven prod stage but also automatically letting a portion of the traffic (e.g., 5%) to the new version in the canary stage.

4. Metrics and logs will be kept separately for better monitoring during the testing phase through canary deployment.

5. Stage variables for the canary stage can be overridden which allows the equivalent of performing blue/green deployments with Lambda and API Gateway.

6. After testing and confidence in the new version's performance, traffic can be fully routed to the canary stage.

7. The demonstrated process involves the creation of a new resource and a method of type GET of Lambda function with proxy integration type. This points specifically to the versions of the Lambda function in question.

8. Changing Lambda function version needs an update in the integration request. After the change, API needs to be deployed again.

9. After deploying the changes to the canary stage, a portion of the traffic starts receiving responses from the updated version of the Lambda function.

10. When you're ready to use the new version entirely, you can promote the canary, which results in all traffic going to the upgraded version. 

11. Overall, Canary deployments in Amazon API Gateway involves modifying the resource, creating a canary in your stage, deploying to the canary, and promoting the canary to the final version when ready.

Integration Types and Mappings

1. Different ways to integrate API Gateway with the backend:
   - **Mock Integration**: Returns a response without sending a request to the backend, useful for development and testing.
   - **HTTP/Lambda/Other services integration**: The API Gateway forwards the request, but we can modify it. We configure an integration request and an integration response and can set up data mappings using mapping templates for the request and response.
   - **AWS Proxy (Lambda Proxy)**: The request from the client is the direct input to the Lambda. The request or response can’t be modified since the proxy passes it directly.
   - **HTTP Proxy**: The request is passed directly to the backend, and the tool proxies the returned response back to the clients. You can add HTTP headers, for instance, an API key between your API Gateway and your backend.

2. API Gateway has the power to change the request and the response. It can be helpful in changing, renaming, reordering requests so that the backend services like SQS Queue can understand the API call made by the API Gateway.

3. On a Proxy integration, the Lambda function is solely responsible for the logic of the request and the response. Since it’s a proxy, we can't use mapping templates or change the headers, or query string parameters.

4. Mapping templates only work when integrating within AWS or HTTP, but without using the proxy methods. They can be used to modify the request and the response, rename/modify query string parameters, modify the body content, add or modify headers.

5. Velocity Template Language (VTL) is used to modify requests and responses with mapping templates.

6. To set the mapping templates, the content type must be set as application/json or application/xml.

7. For SOAP APIs, since SOAP APIs are XML-based and REST APIs are JSON-based, you would use the API Gateway with a mapping template to convert the JSON into XML and vice versa.

8. Mapping templates can be used to rename or reorder query string parameters before passing them into the Lambda function.

API Gateway Open API

1. **API Gateway and OpenAPI Specification:** The API Gateway has a strong integration with the OpenAPI specification, which is a widely used method to define REST APIs. The definition of the API is essentially code.

2. **OpenAPI Model 3.0:** You can create, import, and define methods in the API Gateway using the OpenAPI model 3.0. This includes defining method requests, integration requests, method responses, and any AWS extension for the API gateway.

3. **Importing and Exporting in API Gateway:** The API Gateway allows you to import specifications from an OpenAPI model. Conversely, you can export an existing API from the API Gateway as an OpenAPI spec, which can then be used to generate client code.
   
4. **OpenAPI Specifications:** These can be written in YAML or JSON and can generate client SDKs.

5. **Request Validation:** The OpenAPI spec helps to perform request validation within the API Gateway. Instead of sending the raw payload to your backend, API Gateway can verify if it adheres to the proper schema. This helps in reducing unnecessary calls to the backend.

6. **Customizable Validation:** Not only can you check whether the request parameters are presented correctly, including URI, query strings, headers, and payload, but also you can customize validation rules for different methods using `x-amazon-apigateway-request-validator` in the OpenAPI definitions file.

7. **Power of OpenAPI with API Gateway:** The OpenAPI specification combined with API Gateway gives you the power to set up, customize, validate, and manage your APIs efficiently. It helps ensure your backend will have fewer issues parsing and using payloads, resulting in a better functioning API. 
API Gateway Caching

1. **What is caching?**: Caching reduces the number of calls made to the backend. First, the API Gateway checks the cache, if the result is available, it returns the cached result. If not, it fetches the data from the backend (cache miss).

2. **TTL (Time-To-Live)**: The default TTL, the lifetime of a cached result, is 300 seconds (5 minutes). TTL can range from a minimum of 0 (no caching) to a maximum of 3600 seconds (one hour). 

3. **Cache Definition**: Caches in AWS API Gateway are defined at the "stage" level - one cache per stage. It's possible to override these cache settings per method.

4. **Cache Size & Encryption**: Cache size can range from 0.5 gigabytes to 237 gigabytes. Cache data can be encrypted for enhanced security.

5. **Use Cases**: Caching can be expensive and is more suitable for production environments where it alleviates pressure on the backend. It might not be very useful in development or testing environments.

6. **Invalidating Cache**: The cache can be completely invalidated immediately via the UI, or clients can invalidate it by placing a "Cache-Control:max-age equals zero" header into a query to the API Gateway. Proper IAM authorization is required for clients to invalidate the cache.

7. **Cache Invalidation Policy**: If no cache invalidation policy is set, any client can invalidate the API cache which could be problematic. Therefore, it is crucial to impose a proper 'InvalidateCache' policy.

8. **Application**: The tutorial showed how to enable caching at the 'prod' stage by defining cache settings - enabling it, selecting cache capacity, choosing whether to encrypt, and defining cache TTL. It also showed how to handle per-key cache invalidation by setting up authorization rules.

9. **Stopping Caching**: After testing, it's suggested to disable API cache to stop incurring costs. 

10. **Hands-on Practice**: As part of the tutorial, the user is guided to implement caching on a real-life API and observe its effects on API response. It is highlighted that the effects of caching may not be directly visible but rather noticeable in repeated route calls with same arguments, where responses come from the cache rather than the Lambda function.

Usage Plans & API Keys

1. **APIs and Charging Customers**: You can create custom APIs and make them available for your customers, often charging for their use.
   
2. **Usage Plans and API Keys**: Usage plans are set up to define who has access to your API, how much they can use it, and at what speed. API keys are linked with usage plans to identify clients and track their usage.

3. **Throttling and Quota Limits**: You can set up throttling limits (the rate at which users can access your API) and quota limits. For example, you can restrict a user to a certain number of requests per month. If they exceed this, they need to pay more.

4. **API keys**: API keys are strings given to your customers, enabling them to securely use your API gateway and authenticate their requests.

5. **Usage Plan Flow:**
    1. Create an API and configure the methods requiring an API key, then deploy the API to your stages.
    - Generate or import API keys for your application developers (the customers using your API).
    - Create a usage plan with desired throttle and quota limits.
    - Associate these API stages and API keys with the usage plan.
    - Include an API key in the `x-api-key` header request when making calls to the API.

6. **Important Note**: If you fail to correctly associate the API stages and API keys with the usage plan, the whole process will not work.

Monitoring, Logging and Tracing

1. **API Gateway Logging and Tracing**
   - CloudWatch Logs integration with API Gateway offers information about request and response bodies.
   - The level of logs can be defined as `ERROR`, `DEBUG`, or `INFO`.
   - Beware of sensitive data being logged in CloudWatch Logs.

2. **AWS X-Ray** 
   - Provides tracing information of requests going through API Gateway.
   - When coupled with Lambda, X-Ray can give a comprehensive view of your API.

3. **CloudWatch Metrics for API Gateway**
   - Monitoring is performed per stage, and detailed metrics can be enabled.
   - Important metrics include `CacheHitCount`, `CacheMissCount` for cache efficiency, `Count` for the number of API requests, `IntegrationLatency`, and `Latency`.
   - Maximum time for an API Gateway request is 29 seconds; anything beyond this will result in a timeout.

4. **Errors Metrics**
   - `4XX` metrics (client-side errors), revealing how many errors occur on the user side.
   - `5XX` metrics (server-side errors), demonstrating how many errors take place on the backend side.

5. **Throttling and Performance**
   - By default, API Gateway throttles requests at 10,000 requests per second, but this limit can be increased upon request.
   - Excessive requests return a `429 Too Many Requests` error.
   - To improve throttling and performance, set stage and method limits or define a usage plan per customer.

6. **Errors in API Gateway**
   - `4XX` errors are client errors, like `400: Bad Request`, `403: Access Denied`, or `429: Quota Exceeded`.
   - `5XX` errors are server errors, like `502: Bad Gateway`, `503: Service Unavailable`, or `504: Gateway Timeout` which indicates a timeout due to no response from the backend within 29 seconds.

API Gateway Security

Key Takeaways:

1. **API Gateway Security**: There are three primary concepts for securing an API gateway - IAM permissions, resource policies, and authorizers.

2. **IAM Permissions**: IAM permissions can be used for API gateways accessed within AWS accounts. IAM policy is attached to a user role to invoke the API gateway. IAM credentials are passed to the API gateway using Sig v4.

3. **Resource Policies**: These are like Lambda resource policies, which allow you to set a JSON policy on your API gateway to specify who and what can access it. The main use case is for cross-account access. Resource policies can also be used to filter for specific IP addresses or allowing only for a VPC Endpoint.

4. **Cognito User Pools**: Cognito manages the user lifecycle and the connection token to it will expire automatically. The API gateway will verify the identity of the people connecting to the API using Cognito. Users must authenticate with the Cognito User Pools, and authorization is set at the API gateway methods level.

5. **Lambda Authorizers**: These are the most flexible form of authorization, but also require the most involvement. Authenticated by an external source, Lambda function evaluates passed parameters and, if satisfied, returns an IAM policy for the client.

6. **IAM Security**: Best suited for environments with already created user and role accounts. Leverages Signature v4 technology.

7. **Custom Authorizers**: Suited for environments using third party databases of users and requiring the flexibility of choosing what IAM policy to return.

8. **Cognito User Pool**: Best suited when managing your own user pool. No need for custom code, but requires custom authorization in the backend Lambda function. 
HTTP API vs REST API

1. **API Gateway Types**: The instructor discussed three types of APIs available in API Gateway: REST API, HTTP API, and WebSocket. The course primarily focuses on REST APIs.

2. **HTTP API**: This type of API is cost-effective and low latency. They work as a AWS Lambda proxy, HTTP proxy API, and private integration. However, they only support proxy integration and don't offer data mapping. They support only a few kinds of authorizations like OIDC and OAuth 2.0. HTTP APIs have built-in support for CORS, but lack usage plans or API keys. Due to their simplicity, HTTP APIs are a low-cost alternative to REST APIs.

3. **REST API**: REST API supports all features discussed in the course and is more feature-rich compared to HTTP API. The only exception is that they don't natively support OIDC and OAuth 2.0.

4. **Comparison**:
   - HTTP APIs are generally cheaper than REST APIs.
   - There are minor differences in feature support. For instance, REST APIs support resource policies, while HTTP APIs don't.
   - HTTP APIs are newer and simpler, but may not fully meet the requirements of complex implementations like REST APIs can.

5. **Exam Perspective**: From an exam standpoint, understanding the differences in cost, support and features between HTTP and REST APIs is important. Specifically, realization that HTTP APIs are cheaper than REST APIs. WebSocket may appear in the exam but only at a very high level.

Websocket API

1. **WebSocket and its Functions:** WebSocket is a protocol that enables two-way, interactive communication between the user's browser and a server. It's used in real-time applications such as chat applications, collaboration platforms, multiplayer games, and financial trading platforms. It allows the server to push back information to the client without the client making requests.

2. **WebSocket API on API Gateway:** A client connects to a WebSocket API on API Gateway and establishes a persistent connection. On the first connection, a Lambda function named `onConnect` is invoked, allowing tasks like persisting the connection ID into DynamoDB. Further, clients can send messages over this persistent connection, invoking the `sendMessage` Lambda function. Finally, the `onDisconnect` function handles the user's choice to disconnect.

3. **WebSocket API URLs:** WebSocket API URLs start with `wss://`, followed by a unique ID, `.execute-api.region.amazonaws.com/the stage name` indicating an encrypted WebSocket URL.

4. **Connection ID and Data Transmission:** The connection ID remains constant as long as the client stays connected to the WebSocket API. The client sends messages, called frames, to the server over the same persistent connection ID.

5. **Server-side Response:** The server (API Gateway) communicates back to the clients using the same WebSocket URL followed by an `/@connections/connectionid` path. With HTTP POST, GET, and DELETE methods, a service can send messages back to the client, retrieve the latest connection status, and disconnect the client, respectively.

6. **Routing:** WebSocket employs routing, and in the absence of a specified route, incoming JSON messages are dispatched to the default route. API Gateway allows developers to create a "route selection expression" to select a field from the JSON for routing.

7. **Important points for the exam:**
   - WebSocket API is used for two-way communication.
   - Routing is a critical aspect to understand. An open connection uses routing as part of the incoming data message to direct the data to a specific backend based on the routing expression.

API Gateway Architecture

1. **Microservice Architecture with API Gateway:** The API Gateway allows the implementation of a Microservice Architecture. It enables a unified interface for all your microservices, simplifying interactions for the clients by hiding the complexity of the backend resources.

2. **Use of API Endpoints:** You can use API endpoints to connect to various backend resources. For example, routes like /service1 can be configured to send requests to an Elastic Load Balancer (ELB) supporting an ECS cluster of microservices.

3. **Using Different Routes to Different Services:** Routes can also be configured to other services, like /docs targeting S3 buckets containing documentation, or /service2 to connect to an Amazon EC2 Auto Scaling group fleets.

4. **Single URL for Connection:** API gateway serves as a single URL to connect to these services, offering a streamlined experience to clients.

5. **Use of Route 53 for Domain Registry:** Amazon Route 53 can be used to register a domain, replacing the use of the DNS for the API gateway. This confers the ability to create custom addresses based on different clients.

6. **SSL Certificates:** SSL Certificates can be applied based on these domains to improve security.

7. **Forwarding and Transformation Rules:** These can be applied within the API gateway, modulating and transforming incoming data before it's sent to the backends.

8. **Primary Purpose of API Gateway:** The goal of using an API Gateway is to unify your microservices and provide a simplified, unified external URL, while it handles the complexities of routing to a service, transforming data and managing SSL certificates.

