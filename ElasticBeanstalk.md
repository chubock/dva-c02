Overview

1. **Elastic Beanstalk** is a managed service provided by AWS which enables developers to deploy an application with a single, unified interface while abstracting away much of the underlying infrastructure management complexity.
  
2. Beanstalk interfaces with core AWS components like EC2, ASG, ELB, and RDS, handling provisioning, configuration, load balancing, and scaling, as well as application health monitoring.
  
3. Beanstalk components are:
    - **Application:** A collection of Beanstalk elements like environments, versions, and configurations.
    - **Version:** Iterations of the application code.
    - **Environment:** A collection of resources running a specific application version.
    - **Tiers:** Two available tiers, web server environment and worker environment.
  
4. Beanstalk enables easy application updates and supports a wide range of programming languages, including Go, Java SE, .NET, Node.js, PHP, Python, Ruby, etc. Even if your language isn't included, you can create a custom platform.
  
5. It offers different architectures based on the application's behavior. Two most crucial are:
    - **Web tier:** This entails the traditional model of a load balancer sending traffic to an autoscaling group comprising several EC2 instances (aka web servers).
    - **Worker tier:** Traffic isn't sent directly to EC2 instances. Instead, an SQS queue stores messages, and EC2 instances (workers) poll from this queue. This environment scales based on message volume.
  
6. Beanstalk supports two deployment modes:
    - **Single instance:** Targets development purposes, with one EC2 instance and an Elastic IP.
    - **High availability with load balancer:** Ideal for production, utilizing a load balancer, multiple EC2 instances run in an autoscaling group across multiple availability zones.
  
Elastic Beanstalk unburdens developers from managing infrastructure, allowing them to focus primarily on writing the code while it handles the infrastructure setup and scaling.


First Environment

1. Elastic Beanstalk lets you create an application environment with choices between a web server environment for hosting websites or a worker environment for processing tasks off a queue.

2. The environment information auto-fills when creating the application, but you can provide names per your requirements. You are also assigned a generated domain name for accessing your web servers.

3. Elastic Beanstalk offers different platforms. The sample uses Node.js but depending on user requirement, other platforms can be chosen. 

4. Users can either upload their application code or use a sample application. 

5. Beanstalk provides presets of configurations for a single instance (free tier eligible), high availability, or a custom configuration.

6. Beanstalk needs IAM roles for service access. These can be manually created using the AWS IAM console. The roles used in this walkthrough include the service role and the EC2 instance profile.

7. Beanstalk leverages CloudFormation to create the required resources. Within the CloudFormation console, you can see the ongoing events and the resources being created.

8. Once created, all resources (Auto Scaling groups, security groups, Elastic IPs) can be reviewed in the AWS EC2 console. 

9. When the application is successfully launched, you can access it through a generated domain name.

10. Elastic Beanstalk also allows you to upload new versions of your application, monitor application metrics, view logs, manage updates, and even modify the configuration of your environment.

11. Beanstalk is centered around code and code environments, while CloudFormation is used for deploying infrastructure stacks.

12. Beanstalk allows for multiple environments under an application, manageable separately.
Second Environment

1. Using Elastic Beanstalk, you can create multiple environments like development and production, for separate versions of your application.

2. In this example, for the production environment, a High Availability Preset is used, which offers more configuration options than Single Instance. This allows the application to run across multiple instances to ensure high availability.

3. Networking, Instance and Database settings can be configured, although they are optional. For Instance Settings, subnets across which instances launch can be chosen to ensure high availability.

4. Elastic Beanstalk allows the addition of a database to the environment. However, the lifecycle of the database is linked to the environment, implying that deleting the environment will also delete the database. If needed, the database can be snapshotted for a restore later.

5. Among other settings, you can also configure the Auto Scaling Group (ASG) size and instance type, as well as the type of Load Balancer to use.

6. Health Reporting, Email Notifications, Rolling Updates and Platform Software settings can also be configured. It's important to review these based on your specific application needs.

7. Elastic Beanstalk can do automatic configuration for majority of settings. It's useful for complex and comprehensive services, but may also require caution as wrongly set configurations can inadvertently lead to issues.

8. Finally, Elastic Beanstalk allows a quick review of parameters before launch. Post-launch, it auto-creates resources like security groups, load balancers, and auto-scaling groups, taking away some of the hassle from configuring them manually.

9. After the environment has been created, the application can be accessed via the generated URL. 
Beanstalk Deployment Modes

1. **Deployment Options:** Elastic Beanstalk offers several ways to deploy updates to your application–all at once, rolling, rolling with additional batch, immutable, blue/green, and traffic splitting. Each approach has distinct advantages and trade-offs.

2. **All at Once:** Updates are deployed on all instances simultaneously. This method is the fastest but leads to downtime since all instances are down when being updated.

3. **Rolling:** Updates are deployed a few instances at a time, or "buckets". Once a bucket is successfully updated, the next one is targeted. This approach minimizes downtime but may temporarily reduce the application's capacity.

4. **Rolling with Additional Batch:** Similar to rolling updates, but new instances are spun up to maintain capacity during the update process. Ensures no drop in capacity but incurs additional costs.

5. **Immutable:** New instances are deployed in a new Auto Scaling group with the updated version of the application. Once confirmed that they're healthy, the old instances are replaced. This approach ensures zero downtime but doubles the capacity temporarily, leading to higher costs.

6. **Blue/Green:** Involves creating an entirely new environment with the updated application (green), and once it's tested and confirmed healthy, it replaces the old environment (blue). This strategy allows for comprehensive testing, achieves zero downtime, and facilitates easy rollbacks.

7. **Traffic Splitting (Canary testing):** A small percentage of application traffic is sent to a new deployment (temporary ASG). Health of the new deployment is continuously monitored. This approach allows for prompt rollbacks if the new deployment fails while maintaining zero downtime.

8. For an **overall comparison** and a way to understand these deployment methods better, refer to the [Beanstalk documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html).

Remember, choosing the right deployment method depends on the needs of your specific application and the kind of constraints and requirements defined for the deployment itself.


Beanstalk Deployment Modes Hands On

1. AWS Elastic Beanstalk offers several deployment policies that govern how application updates are rolled out:
   - **All at once:** The code update applies to all instances at once.
   - **Rolling:** Some instances are taken down and upgraded, then it moves on to the next set. The batch size could be a percentage or a fixed number of instances.
   - **Rolling with an additional batch:** It adds new EC2 instances, deploys updates to them, and then moves on with the batch. It helps maintain the same capacity but at an increased cost.
   - **Immutable:** A whole new set of instances is created to deploy updates, then the old ones are deleted.
   - **Traffic Splitting:** It splits traffic to a percentage of new application versions for a specific duration before upgrading everything.

2. These deployment policies also apply while updating EC2 instances internally or making VPC configurations, with the choice between rolling and immutable updates.

3. A comprehensive exploration of the `immutable` deployment policy was done in the tutorial. The key steps include:
   - Launching one instance with new settings.
   - Creating new Auto Scaling Groups (ASG).
   - Passing the instance through a health check.
   - Detaching new instances from the temporary ASG to the permanent ASG.
   - Performing post-deployment configuration on new instances.
   - Old instances and temporary ASGs are terminated at the end.

4. Besides deployment, the tutorial also covered how to package applications using Elastic Beanstalk, specifically for Node.js applications, along with some customization using `.ebextensions` directory.
  
5. Another feature highlighted was environment swapping, which allows the interchange of environments (for instance, making production environment temporarily a dev environment for the purpose of testing). This relies on DNS and may take some time due to DNS propagation times.
  
6. Cloning, Swapping URLs, and different Deployment options were covered in the tutorial, along with how to apply updates or new versions in Elastic Beanstalk. All these features in AWS Elastic Beanstalk provide granular control over application deployment and updates as per the organizational needs.

Beanstalk Lifecycle Policy

1. Elastic Beanstalk CLI (Command Line Interface), also known as EB CLI, is a tool that facilitates working with Elastic Beanstalk. 

2. Key EB CLI commands include `eb create`, `eb status`, `health`, `events`, `logs`, `open`, `deploy`, `config`, `terminate` and more.

3. EB CLI also enables you to automate your development pipelines.

4. Understanding the use of these commands is more crucial for the DevOps exam compared to the developer exam.

5. When deploying an Elastic Beanstalk application, you need to specify your dependencies, for example, using a requirements.txt for Python or a package.json for Node.js.

6. Your code is then packaged as a zip file, and this zip file, along with your dependency descriptions, is uploaded into Beanstalk; this process creates a new app version.

7. The uploaded app version can be deployed either using the console or CLI.

8. The underlying process works by uploading the zip onto Beanstalk which then gets uploaded onto Amazon history. 

9. Beanstalk can store up to 1000 application versions within an account.

10. To manage your old application versions, you can use a Beanstalk lifecycle policy, based on time or space, to phase out and remove older versions. 

11. Application versions being used by your environments won't be deleted, irrespective of age or space usage.

12. If you want to retain the source bundle of your application in Amazon S3 for preventing data loss, you equally have that option.

13. Application Lifecycle Policy can be activated via settings, where you can specify the maximum application versions by count or maximum age of versions.

14. Each application version remains in Amazon S3, while they are registered and controlled in Beanstalk. 

15. Lastly, Elastic Beanstalk uses a backing S3 bucket to hold all application versions.

Beanstalk Extensions

## Elastic Beanstalk Extensions
- Contained within a zip file of your deployed code, you can include Elastic Beanstalk (EB) extensions, which allow you to configure parameters that are usually set in the UI, using code files.
- EB extension files must be located in a directory labeled `.ebextensions/` at the root level of your source code.
- These configuration files should be in either YAML or JSON formats, but they must end with a `.config` extension (like `logging.config`).

## Modifying Defaults and Adding Resources
- Defaults can be edited using the `option_settings` document.
- You can add resources such as RDS, ElastiCache, and DynamoDB that aren't typically set through the Elastic Beanstalk console.
- It's important to note that any resources managed by EB extensions are deleted when the corresponding environment is deleted.

## Applying EB Extensions
- The configuration files created as part of the `.ebextensions/` directory in your source code must be in YAML or JSON format and should have a `.config` extension.
- You can set Elastic Beanstalk application environment variables, and define settings such as database URL and user.
- When you want to use these settings (like connecting to an external database), you can define them using EB extensions.
- These configurations are zipped with the codebase, uploaded, and deployed as a new version of your application.

## Verifying EB Extensions
- You can verify the application of the EB extensions by checking the Configuration in the Elastic Beanstalk console. The environment properties should reflect the settings defined in your `.ebextensions/` directory.

## Importance of EB Extensions
- They prove to be a powerful tool when you want to include additional configurations that aren't typically included in the Elastic Beanstalk console. It allows developers to have better control over their application environment setup. 
Beanstalk and Cloudformation

1. **Elastic Beanstalk Uses CloudFormation:** AWS Elastic Beanstalk employs AWS CloudFormation under the hood to carry out its operations, serving as the foundation for infrastructure provisioning.

2. **CloudFormation for Infrastructure as code:** CloudFormation allows for Infrastructure as Code(IAC), where infrastructure setup is codified, leading to better tracking, replication, and altering of the setup.

3. **Elastic Beanstalk with .ebextensions and CloudFormation:** The combination allows provisioning and configuration of any AWS service, not limited to the user interface of Elastic Beanstalk. For example, services like ElastiCache, DynamoDB tables or S3 buckets could be deployed.

4. **Insight into CloudFormation Stacks:** You can inspect the stacks created by CloudFormation in Elastic Beanstalk. Each stack corresponds to a CloudFormation template, detailing the resources it has created, for example, auto-scaling groups, launch configurations, scaling policies, CloudWatch alarms, EC2 security groups, elastic load balancers, and others. 

5. **Manipulating AWS Services:** Although you don't need to manually handle anything in CloudFormation while using Elastic Beanstalk, understanding the interaction between CloudFormation and Elastic Beanstalk is beneficial for expanding your applications to include any additional services you need.


Beanstalk cloning

1. **Elastic Beanstalk's Cloning Feature:** AWS Elastic Beanstalk allows you to clone an existing environment into a new environment. This feature is useful when you want to deploy a test version of your application with the same settings as your production environment.

2. **Preserved Configurations:** When cloning, all resources and configurations of the original environment are preserved, including load balancer type and configuration, RDS database type, environment variables, and so forth. Note, while the RDS configuration gets cloned, the data does not.

3. **Customizing the Cloned Environment:** After cloning an environment, you can change its settings as per your requirements. 

4. **Console Navigation for Cloning:** From the AWS console, under Elastic Beanstalk > Your Application > Actions > Clone Environment, you can clone your environment into a new one. The clone options offer minimal flexibility, such as choosing a new platform version or service role.

5. **Post-Cloning Actions:** Post-cloning, you can customize the cloned environment's settings via the Configuration tab. This is often done to deploy a new version for testing before swapping the environment name.

Remember, the primary aim of cloning is to perform tests on an environment that mirrors your production settings, assuring that any modifications won't affect your live application.

Beanstalk Migrations

## **Elastic Beanstalk Migration**

1. After creating a Beanstalk environment, the Elastic Load balancer type cannot be changed, only its configuration can be modified.

2. Upgrading from Classic Load Balancer to an Application Load Balancer or from an Application to a Network Load Balancer requires a migration, which includes the following steps:

    - Create a new environment with the same configuration except the Load Balancer. 
    - Note you cannot use the clone feature for this migration as it would copy the exact same Load Balancer type and configuration.
    - Deploy the application to the new environment.
    - Shift traffic from the old environment to the new one using a CNAME swap or Route 53 for DNS update.

## **RDS Elastic Beanstalk**

1. Provisioning RDS with your Beanstalk application is useful for development and test environments, but not ideal for production deployments due to the tie between the database lifecycle and the Beanstalk environment lifecycle.

2. In production, best practice is to separate the RDS Database from the Beanstalk environment and reference it using a connection string or an environment variable.

3. Decoupling RDS from the Beanstalk environment includes the following steps:

    - Create a snapshot of the RDS Database as a safeguard.
    - Protect the RDS Database from deletion through the RDS console.
    - Create a new Elastic Beanstalk environment, this time without RDS, and point the application to the existing RDS Database with an environment variable.
    - Perform a CNAME swap or a blue/green deployment or Route 53 DNS updates and confirm it's working.
    - Terminate the old environment. Since RDS deletion protection is enabled, the RDS will remain.
    - Delete the CloudFormation stack manually, as it would be in a 'Delete Failed' state due to the failure to delete the RDS within it.

This will result in RDS Database being separated from the Elastic Beanstalk environment.


