Docker Introduction

1. **Docker:** Docker constitutes a software platform used to deploy apps in 'containers' across any operating system without compatibility issues. Its use cases include microservice architecture and lifting and shifting apps from on-premises to the cloud. Docker's behavior is predictable, making it easier to maintain, deploy, and work seamlessly with any language, operating system, or technology.

2. **Docker Vs Virtual Machines:** Unlike VMs, where resources are not shared and isolations are maintained, Docker containers can share resources on one server—networking, certain data, etc. Although slightly less 'secure', Docker allows more containers to run on a single server, being lighter and more efficient than VMs.

3. **Docker Images & Repositories:** Docker uses a repository system to store images. Docker Hub, a public repository, hosts base images for many technologies and operating systems. For private use, Amazon's Elastic Container Registry (ECR) can be used, which also features a public repository option, the Amazon ECR Public Gallery.

4. **Docker on AWS:** The process entails writing a Docker file to define the container then building an image and storing it in Docker repositories (known as a Docker 'push'). Pulling these images from repositories and running them results in Docker containers that run your code. AWS offers several services for Docker container management: Amazon ECS (Elastic Container Service) for Docker management, Amazon EKS (Elastic Kubernetes Service) to manage Kubernetes—an open-source project, AWS Fargate (serverless container platform that works with both ECS and EKS), and Amazon ECR to store container images.

Amazon ECS

1. **Elastic Container Service (ECS):**
   ECS stands for Elastic Container Service. When you launch Docker Containers on AWS, these are launched as an "ECS Task" on an "ECS Cluster." ECS Clusters are made up of EC2 instances which run the ECS Agent.

2. **EC2 Launch Type:**
   In EC2 Launch Type, the 'things' that make up the ECS Cluster are EC2 instances. It requires you to provision and maintain the infrastructure yourself. These instances run the ECS Agent, which registers each EC2 Instance into the Amazon ECS service and specified ECS Cluster.

3. **Fargate Launch Type:**
   Fargate Launch Type does not require provisioning of the infrastructure; it's serverless. Instead, it creates task definitions to define ECS tasks, and AWS runs these. The Docker Containers run without any EC2 Instance needing to be created in our account. Scaling is managed by increasing the number of tasks.

4. **IAM Roles for ECS tasks:**
   An EC2 Instance Profile is used by the ECS Agent to make API calls to ECS service, CloudWatch Logs, ECR, and to reference sensitive data in Secrets Manager or the SSM Parameter Store. ECS tasks get ECS Task Roles, which are specific task role that allows each task to make API calls to other AWS services.

5. **Load Balancer Integrations:**
  - Application Load Balancer (ALB): Supports most use cases; can be used with Fargate.
  - Network Load Balancer: Recommended for high throughput or performance cases, or with AWS Private Link.
  - Classic Load Balancer: Doesn't support advanced features and can't link to Fargate.

6. **Data Persistence:** 
  For data persistence in Amazon ECS, they use Data Volumes such as EFS (Elastic File System). EFS is a network file system compatible with both EC2 and Fargate launch types. It allows tasks running in any Availability Zone (AZ) linked to this EFS file system to share the same data.

7. **Fargate and EFS Combo:** 
  The ultimate combo is to use Fargate to launch ECS tasks in a serverless manner and Amazon EFS for file system persistence. EFS is serverless; we don't manage any servers - it is pay-as-you-go and provisioned in advance. The use case for EFS with ECS is to provide persistent multi-AZ shared storage for containers.


Creating ECS Cluster Hands On

1. **Creating Amazon ECS Cluster**: Navigate to the ECS console service, enable the new ECS experience, go in to cluster, and create your first cluster. The demo used "DemoCluster" for this exercise.

2. **Infrastructure Options**:
   - ***AWS Fargate***: This serverless option allows you to provide the containers to AWS, which will run these containers on demand. You do not provide the compute, AWS provides it for you which you don't directly see.
   - ***Amazon EC2 Instances***: Here, you provide your own Amazon EC2 instances to provide the compute to run your containers on.
   - ***External Instances using ECS Anywhere***: Enables you to use your own data center to run ECS containers. 

3. **Configuration**: In the demo cluster, Fargate and Amazon EC2 instances are enabled. An auto-scaling group with min 0 and max 5 capacity is created. The OS can be either Amazon Linux 2 or Amazon Linux 2023. For instance type, the example uses t2.micro which is free tier eligible. No SSH key pair is configured and root EBS volume size is left as is.

4. **Network Settings**: Default VPC is used along with the available subnets and security group. For auto-assign public IP, the default to use subnet setting is chosen.

5. **Creating the Cluster**: The cluster is then created. An auto scaling group named Infra-ECS-Cluster is also created with min capacity 0, max capacity 5. The capacity of this auto scaling group can be adjusted.

6. **Exploring the Cluster**: Once the cluster is created, it can be further explored. In the demo, the cluster has three capacity providers - Fargate, Fargate_Spot and ASGProvider. Create an ECS task and it can be launched on either a Fargate, Fargate_Spot capacity provider, or on the container instances that are launched as part of the ASGProvider. 

7. **Launching an EC2 Instance**: Changing the desired capacity to 1 will create an EC2 instance. This EC2 instance registers itself into the cluster, seen under container instances. 

8. **EC2 Instance Availability**: The instance will be available with specified CPU and memory capacity. Tasks can be launched on it till its capacity runs out.

The next steps would involve running the first service on this set up, as mentioned briefly at the end of the lecture.

Creating ECS Service Hands On

1. **ECS (Elastic Container Service)**: ECS is used to run Docker containers and has two essential parts: services and task definitions. It's important to create a task definition before setting up an ECS service.

2. **Task Definitions**: A task definition, for example 'nginxdemo-hello', specifies the Docker image to use (such as nginxdemos/hello on Docker Hub), along with other configuration details like OS, hardware requirements, task size, IAM roles etc.

3. **Fargate vs EC2**: AWS Fargate provides serverless computing for containers, allowing you to focus on designing and building your applications instead of managing the infrastructure. EC2 instances can also be used, but for simplicity, Fargate was used in the tutorial.

4. **Resource allocation**: Depending on the needs of your service, you can assign varying amounts of vCPU and memory to it. Minimums for this demo were 0.5 vCPU and 1GB of memory.

5. **Port mapping and environment options**: ECS allows various configuration details like port mappings, resource allocation limits, environment variables, and logging that can be customized according to requirements.

6. **Task Execution Role and Task Role**: These are IAM roles associated with a task. If tasks need to use AWS services, task role will be required. The task execution role, left as default here, is auto-created by ECS if not already present.

7. **Service Creation**: After task definition, you create a service in ECS. This includes selecting the task definition, naming the service, and setting up the application type and deployment options.

8. **Networking and Load Balancing**: ECS allows you to set up networking configurations and load balancing for your service. In the tutorial, a new VPC security group was created, an application load balancer was set up, and target groups were configured.

9. **Scaling**: ECS supports auto-scaling, meaning it can adjust the number of running tasks based on demand. Manually increasing the desired number of tasks initiates more instances of the Fargate task.

10. **Service Update and shutdown**: The service can be updated or shut down completely to save costs. Setting the desired number of tasks to zero achieves this. The Auto Scaling Group should also be modified to ensure no instances are running. 

11. **Events and Monitoring**: ECS provides events and logs for monitoring and debugging purposes. You can check the task status, see the associated logs, and track the various lifecycle events of a service.

ECS Auto Scaling

Here are the key takeaways from the AWS certification preparation course on ECS Service Auto Scaling:

1. **ECS Service Auto Scaling**: AWS allows automatic increase or decrease of ECS tasks using AWS Application Auto Scaling.

2. **Metrics for Scaling**: ECS Service Auto Scaling can scale based on three metrics: CPU Utilization, Memory (RAM) Utilization, and ALB Request Count Per Target. 

3. **Types of Auto Scaling**: AWS supports various types of auto scaling such as:
    - **Target Tracking**: Here, the scaling is set to track specific targets based on the three mentioned metrics.
    - **Step Scaling**: Adjusts the ECS service capacity based on step adjustments.
    - **Scheduled Scaling**: This is used when you want to scale your ECS service in response to predictable load changes.

4. **ECS Service Scaling vs Cluster Scaling**: Scaling a service at the task level is different from scaling your cluster of EC2 instances. Fargate can make service auto scaling easier due to its serverless nature.

5. **EC2 Instance Scaling**: For EC2 launch type, ECS instances can be scaled using either Auto Scaling Group Scaling or ECS Cluster Capacity Provider. The latter is the smarter and more advanced choice, as it automatically scales your Auto Scaling Group (ASG) when you lack capacity to launch new tasks. This strategy is encouraged in the exam.

6. **Scaling Flow**: If the service detects increased usage (like a spike in CPU usage), it triggers an alarm through CloudWatch. This leads to a scaling activity in your ECS service auto scaling, increases the desired capacity, and creates a new task. If the ECS service is operating on the EC2 launch type, the capacity providers can assist to scale the EC2 instances-backed ECS cluster.

ECS Rolling Update

1. **ECS Service Update:** ECS Service can be updated with the help of rolling updates, where you control the number of tasks to be started and stopped at a given time, and their order.

2. **Settings for ECS Update:** When selecting a new task definition number to update an ECS service, you have "minimum healthy percent" and "maximum percent" settings. 

    - By default, these are set to one and 200 respectively.

3. **Minimum Healthy Percent** and **Maximum Percent**: 

    - If an ECS service is running nine tasks (i.e., at 100% capacity), and the minimum healthy percent is set to less than 100, this allows to terminate certain tasks as long as there are enough tasks running to meet or exceed the minimum healthy percent. 

    - The maximum percent dictates how many new tasks (version 2) can be created to update the service.

4. **Scenarios of ECS Update:**

    - *Scenario 1* (Minimum at 50%, Maximum at 100%): If you're starting with 4 tasks, you'd terminate 2 tasks (50% capacity), create 2 new tasks (100% capacity), terminate another pair of old tasks (50% capacity), and finally create another 2 new tasks (100% capacity).

    - *Scenario 2* (Minimum at 100%, Maximum at 150%): If you're starting with 4 tasks, you'd create 2 new tasks (150% capacity), terminate 2 old tasks (100% capacity), create another 2 new tasks (150% capacity), and finally terminate another pair of old tasks (100% capacity).

5. **Exam Tip:** It is important to understand these ECS service update concepts, as they may appear in exam questions. The nature of the rolling updates and the influence of the minimum healthy percent and maximum percent settings on task termination and initiation could be tested.

Amazon ECS Solutions Architectures

1. **ECS Tasks invoked by Event Bridge**: You can create a serverless architecture through Amazon ECS tasks initiated by Event Bridge. When users upload objects to S3 buckets, Event Bridge can have a rule in place to run ECS tasks. These tasks, in turn, can get the objects, process them, and send the results into Amazon DynamoDB.

2. **Event Bridge Schedule**: You can use Amazon Event Bridge to schedule ECS tasks on an hourly basis. This task could, for instance, carry out batch processing against some files in Amazon S3 every hour. This architecture is entirely serverless.

3. **Using ECS and SQS queue**: ECS tasks can ingest messages sent into an Amazon SQS queue and process them. ECS service auto-scaling can be utilized, meaning that the number of messages in the SQS queue can determine the number of tasks in the ECS service.

4. **Intercepting events from ECS cluster with Event Bridge**: AWS Event Bridge can also be used to monitor and react to task lifecycle events within your ECS cluster. For example, it can monitor and alert when tasks stop through the ECS task state change. 

5. **Event-Driven architecture**: The above examples illustrate the strength of an event-driven architecture in AWS where services such as EventBridge, ECS, and SQS work together to process, respond to, and scale based on events. These patterns are at the core of serverless architecture principles.

Amazon ECS Task Definitions Deep Dive

1. **ECS Task Definitions are JSON formatted definitions** telling ECS (Elastic Container Service) how to run Docker containers. It includes crucial information such as:
   - Image Name
   - Port Binding for Container and Host
   - Required memory and CPU
   - Environment Variables
   - Networking Information
   - IAM Role
   - Log configuration (example: CloudWatch)

2. **Docker Containers** run on EC2 instances registered with an ECS cluster. These instances must be running the ECS agent.

3. **Port Binding:**  Docker Containers run within ECS task definitions. The host port (the EC2 instance) and the container port don't have to be the same. The host port connects with the internet or an external network, and this port is linked to the container port.

4. **Multiple Containers:** You can define up to 10 containers per task definition.

5. **Dynamic Host Port Mapping:** If using the EC2 launch type with load balancing, a dynamic host port mapping occurs if only the container port is defined in the task definition. An Application Load Balancer (ALB) automatically connects to different ports on different instances thanks to this feature. However, this setup does not work with a classic load balancer.

6. **Fargate Launch Type:** Each ECS task gets a unique private IP as there's no host. The task is directly connected to all instances on the same port. 

7. **IAM Roles**: IAM roles are assigned per task definition. They can allow your ECS tasks to access Amazon S3 service, DynamoDB, and others. This assignment happens at the task definition level, not at the service level.

8. **Environment Variables**: These can be hardcoded or fetched from the SSM Parameter Store, Secrets Manager, or directly from an Amazon S3 Bucket. When launching an ECS task, these values are fetched and resolved at runtime and injected into the task.

9. **Shared Data**: Data can be shared between ECS tasks by mounting a data volume onto both containers. A bind mount is used to create a shared storage between tasks. This works for both EC2 and Fargate tasks.

10. **EC2 vs. Fargate** : EC2 tasks use a bind mount, and the storage is tied to the lifecycle of the EC2 instance. Fargate uses ephemeral storage, and the data disappears when the Fargate task disappears. Fargate provides 20-200 GB of shared storage. The key use of this storage is to share data between multiple containers or to facilitate the usage of a sidecar container for metrics and logs.
Amazon ECS Task Definition Hands On

1. Task Definition Creation:
   - The task definition includes options like the family name and infrastructure requirements.
   - It can be launched on either Fargate, Amazon EC2 instances, or both.

2. Infrastructure Requirements:
   - For Fargate, you need to select CPU and memory increments. For EC2 instances, you can enter any value you want for memory and CPU.
   - Network mode needs to be AWS VPC for Fargate, while with EC2, there are more advanced options.

3. Task Role:
   - Task roles are crucial as they allow the container to make API calls to AWS services.

4. Task Execution Role:
   - This IAM role is specifically for the container agent to make AWS API requests on behalf of the user.

5. Containers: 
   - Multiple containers can be included in a task definition, but at least one should be deemed as an 'essential' container.

6. Port Mapping:
   - Ports can be defined for your containers along with the protocols.

7. Environment Variables:
   - They can either be added individually or from a secret manager.

8. Logging:
   - Log collection can be sent to different services like CloudWatch, Splunk, etc., via AWS FireLens or directly to CloudWatch natively.

9. Health Checks and Timeouts:
   - They ensure the health status of containers and check the start and stop time of a container. 

10. Storage: 
    - More storage can be given to the containers using bind mount or an EFS file system.

11. Monitoring:
    - Trace collection can be enabled through AWS Distro for Open Telemetry, and metrics can be sent to CloudWatch or Managed Service for Prometheus.
Amazon ECS Task Placements

1. **ECS Tasks Placements:**

   - When an EC2 ECS task is created, ECS finds a place for the task based on available memory, CPU, and ports on the target EC2 instances.
   - This placement is also valid when a service scales in (i.e., removes an ECS task).
   - ECS task placement strategies and constraints help guide where a new container should be added to or removed from.
   - However, this concept only applies to ECS launched on EC2 instances, not Fargate.

2. **Task Placement Process:**

   - ECS identifies instances that satisfy the CPU, memory, and port requirements in the task definition.
   - Then it considers the task placement constraints.
   - It then identifies the instance that best satisfies the placement strategy.
   - Finally, the task will be placed on the selected instance.

3. **Task Placement Strategies:**

   - `Binpack`: Places tasks based on the least available amount of CPU or memory. It helps minimize the number of instances in use, leading to cost savings.
   - `Random`: Tasks are placed without any particular pattern or logic. It's a simple but not optimal strategy.
   - `Spread`: Tasks are spread based on the specified value like instance ID or ECS availability zones. It maximizes high availability by spreading tasks across EC2 instances.
   - These placement strategies can be mixed together.

4. **ECS Task Placement Constraints:**

   - `distinctInstance`: Ensures that each task is placed on a different container instance. No two tasks will share the same instance.
   - `memberOf`: Helps to place tasks on instances that satisfy an expression defined in the cluster query language. For example, forcing tasks to be placed only on t2 instances.
   - These constraints help maintain the organization and division of tasks based on specific instance-related parameters.

Becoming familiar with these ECS task placements, alongside the task placement process, strategies, and constraints, is vital for anyone preparing for the AWS certification exam.

Amazon ECR

1. **Amazon ECR (Elastic Container Registry)** is used to store and manage Docker images on AWS. It provides an alternative to using an online repository such as Docker Hub.

2. You can **store images privately or publicly** in Amazon ECR. Private repositories store images for your account, and public repositories publish to the Amazon ECR public gallery.

3. ECR is **fully integrated with Amazon ECS (Elastic Container Service)**. Docker images in ECR can be pulled and used to deploy containers via ECS.

4. **IAM (Identity and Access Management)** regulates access to ECR. An IAM role assigned to an EC2 instance would allow the instance to pull Docker images from ECR. All access to ECR is protected by IAM.

5. Amazon ECR provides support for **image vulnerability scanning, versioning, image tags, and image lifecycle** management.

6. You can **pull and push images to Amazon ECR using the CLI (Command Line Interface)**. The 'AWS ECR get-login-password' command gives you docker credentials to use the Docker CLI.

   - **To push an image**, use 'docker push' and specify the AWS account ID, region, and image name.

   - **To pull an image**, use 'docker pull' similarly.
      
   - If you can't push or pull an image, you might have an IAM permissions issue.

7. **Public and private repositories** in ECR have different access levels. Public repositories allow anyone to pull images, while private repositories restrict access to specific IAM roles.

8. Within ECR, you have options like **tag immutability** (prevents pushing the same tag twice), **image scan** (Checks for potential security issues) and **Repository encryption** with KMS (Key Management Service).

9. Docker needs to be enabled and running on your computer, with a recent version of Docker.

10. AWS CLI (Command Line Interface) also needs to be set up to perform ECR related actions.

11. Once images are pushed to ECR, they can be used to define tasks in ECS rather than pulling images from Docker Hub.


AWS Copilot

1. AWS Copilot is a command line interface (CLI) tool used to build, release, and operate production-ready containerized applications.

2. It allows developing apps without getting into the complexity of setting up the infrastructure, by automating deployments on AppRunner, ECS, and Fargate.

3. Copilot integrates well with CodePipeline for automated container deployments using a single command.

4. It allows deployment to multiple environments, provides troubleshooting, logs, health status, using either CLI or YAML file to define architecture.

5. The Copilot CLI assists to containerize applications and enables a well-architected, right-sized, and auto-scaled infrastructure setup.

6. It supports deployments to Amazon ECS, AWS Fargate, or AWS App Runner.

7. Practice to create an environment and deploy an application using Copilot on a AWS Cloud9 environment.

8. AWS infrastructure, including VPC, ECS cluster, security groups, and others, is created using CloudFormation.

9. A sample repository with a Dockerfile is used to build and push the Docker image to ECR. Then an ECS service referencing that image is created and started.

10. Manifest.yml files allow fine-tuning of application deployment details.

**Cleaning Up:**
After completion of the tutorial, cleaning is done by deleting the entire application with the `copilot app delete` command, deleting AWS Access Keys and user, and finally deleting the Cloud9 environment.
Amazon EKS

- **Amazon EKS (Elastic Kubernetes Service)** is a service on AWS to launch and manage Kubernetes clusters. It's an alternative to ECS (Elastic Container Service) with a different API and is open-source unlike ECS.

- Kubernetes is for automatic deployments, scaling, and management of containerized applications, primarily Docker-based. Kubernetes works across different cloud providers - Azure, Google Cloud, AWS - promoting standardization.

- EKS supports two launch modes:
  - **EC2 Launch Mode**: deploy worker nodes as EC2 instances.
  - **Fargate Mode**: deploy serverless containers in an EKS cluster.
  
- Use cases for EKS include:
  - Companies using Kubernetes on-premises or on other clouds.
  - Desire to use Kubernetes API and have AWS manage the Kubernetes cluster.

- EKS architecture within a VPC involves separating into public and private subnets, creating EKS worker nodes (EC2 instances), and running EKS pods on these nodes. Nodes can be managed by an Auto Scaling group.

- EKS services can be exposed via private or public load balancers.

- There are 3 different node types for Amazon EKS:
  - **Managed Node Groups**: AWS creates and manages nodes (EC2 instances), part of an Auto Scaling group. Supports On-Demand and Spot Instances.
  - **Self-Managed Nodes**: Provides more customization and control. Users create and register nodes to an EKS cluster. Users manage their own nodes as part of an ASG. Supports On-Demand and Spot Instances. Pre-built Amazon EKS Optimized AMIs can be used, or custom AMIs can be developed.
  - **Fargate**: If users do not want to manage nodes, EKS supports the Fargate mode, where no maintenance is required and nodes are not managed.

- Data volumes can be attached to an Amazon EKS cluster by specifying a StorageClass manifest. It leverages a Container Storage Interface (CSI) compliant driver. Supported by Amazon EBS, Amazon EFS (the only type that works with Fargate), Amazon FSx for Lustre, and Amazon FSx for NetApp ONTAP.


