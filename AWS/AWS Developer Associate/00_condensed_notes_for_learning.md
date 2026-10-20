<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# ALB
Application Load Balancers provide two advanced options that you may want to configure when you use ALBs with AWS Lambda: support for multi-value headers and health check configurations. You can set up these options in Target Groups section on the Amazon EC2 console. If requests from a client or responses from a Lambda function contain headers with multiple values or contains the same header multiple times or query parameters with multiple values for the same key, you can enable support for multi-value header syntax. After you enable multi-value headers, the headers and query parameters exchanged between the load balancer and the Lambda function use arrays instead of strings.

Q: A developer is managing an Application Load Balancer that targets a Lambda function. The developer needs to obtain all values of identical query parameters key that is supplied in a request. How can the developer implement this?
A: Enable the multi-value headers on the Application Load Balancer.

Q: A technology firm manages an internal portal that contains proprietary information. The firm intends to make this portal available to the general public. However, access must be restricted solely to employees authenticated through the firm’s OpenID Connect (OIDC) identity provider (IdP). This authentication mechanism must be implemented without modifying the existing website code. Which action will accomplish this requirement?
A: Set up an internet-facing Application Load Balancer. Create a listener rule for the load balancer for HTTPS on port 443. Configure the rule’s default action to authenticate users using the OIDC IdP configuration.


# Amplify
AWS Amplify is a set of purpose-built tools and features that enables frontend web and mobile developers to quickly and easily build full-stack applications on AWS.

Q: A developer is creating a React application whose source code is hosted in GitHub. To help ensure proper functionality and identify any UI issues before going live, the developer must perform end-to-end (E2E) testing using Cypress. Which combination of actions should the developer take? (Select Two)
A: Connect the Github repository to AWS Amplify Hosting +
Update the amplify.yml file with appropriate configuration settings for Cypress.


# Api Gateway
* In Amazon API Gateway, API keys by themselves do not grant access to execute an API. They need to be associated with a usage plan, and that usage plan then determines which API stages and methods the API key can access. If the API key is not associated with a usage plan, it will not have permission to access any of the resources, which will result in a “403 Forbidden” error.
* You (or your organization) probably has some existing web services that respond to older protocols such as XML-RPC or SOAP. You can use the API Gateway to modernize these services. In API Gateway, an API’s method request can take a payload in a different format from the corresponding integration request payload, as required in the backend. Similarly, the backend may return an integration response payload different from the method response payload, as expected by the frontend. API Gateway lets you use mapping templates to map the payload from a method request to the corresponding integration request and from an integration response to the corresponding method response.
* By enabling caching, API Gateway can store responses to API requests for a specified period.

Q: A company offers a Generative Artificial Intelligence (AI) service exposed through a REST API managed by Amazon API Gateway. They recently rolled out a subscription tier where users receive API keys to access premium features. The company uses the CreateApiKey API for generating these keys. During testing, developers noticed that while existing users can access the service without issues, new premium subscribers get a 403 Forbidden error when using their API keys. What must be done to give new users access to the service?
A: Associate the API keys for the premium users with the intended usage plan using the CreateUsagePlanKey operation.

Q: A media analytics company provides APIs as a service that enables users to retrieve aggregated, daily-updated data on video performance metrics, such as views, likes, and watch times. These APIs are built using Amazon API Gateway and AWS Lambda, with data sourced from a pre-computed file in Amazon S3 that updates every 24 hours. Due to a surge in traffic, users have reported increased latency when accessing the API. The company aims to enhance the API’s responsiveness without changing the backend architecture. Which approach would improve the responsiveness of the APIs?
A: Enable Amazon API Gateway caching.

Q: A developer is hosting a static website from an S3 bucket. The website makes requests to an API Gateway endpoint integrated with a Lambda function (non-proxy). The developer noticed that the requests were failing. Upon debugging, he found a: "No 'Access-Control-Allow-Origin' header is present on the requested resource" error message. What should the developer do to resolve this issue?
A: In the API Gateway Console, enable cross-origin resource sharing (CORS) for the method in the specified resource.


# AppConfig
AWS AppConfig is a specialized service that enables developers to create, manage, and deploy application configurations quickly and efficiently. It also provides the functionality to manage feature flags, a powerful technique that allows developers to test and control new features in live environments without affecting all users. With AWS AppConfig, developers can gradually introduce new features, control who can access them based on specific criteria such as user role or location, and easily update or revert features without deploying new code. This capability helps developers seamlessly control the visibility and access of new features while keeping them hidden until they’re ready for release.

Q: A web developer must deploy new features to a web application that can be turned on or off based on certain conditions. These new functionalities should not be accessible until fully developed and tested. The company wants to seamlessly control the visibility and access of these new features without revealing them until it’s officially released. Which of the following solutions would fulfill these criteria?
A: Utilize AWS AppConfig to set the feature flag by creating a feature flag configuration profile. Enable and disable the feature flags according to development progress.


# Auto Scaling Group
* A launch template is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch template, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. You can specify your launch template with multiple Auto Scaling groups. However, you can only specify one launch template for an Auto Scaling group at a time, and you can’t modify a launch template after you’ve created it. To change the launch template for an Auto Scaling group, you must create a launch template and then update your Auto Scaling group with it.


# CDK (Cloud Development Kit)
* You can think of the CDK as a cloud infrastructure “compiler”. It is an open-source software development framework to model and provision your cloud application resources using familiar programming languages. The AWS CDK has first-class support for TypeScript, JavaScript, Python, Java, and C#. The AWS CDK can also update your deployed resources after you modify your app using the appropriate CDK commands.
  (CloudFormation only allows JSON and YAML in defining cloud resources in a stack.)
* The sam local invoke command allows you to test AWS Lambda functions locally by emulating the Lambda execution environment. However, to test resources defined in AWS CDK, you must first convert the CDK constructs into a format that SAM can understand. This is where the cdk synth command comes into play. It synthesizes or “compiles” your CDK application into an AWS CloudFormation template. By running cdk synth, you generate the necessary CloudFormation template that sam local invoke can be used to locally execute and test your Lambda functions.
* The CDK uses an S3 bucket to store assets required for deployment, such as Lambda code or Docker images. When you move to a new AWS account or a new region within an account, you need to set up the environment for the CDK. This is done using the cdk bootstrap command.
* cdk bootstrap: sets up the necessary resources in the AWS account to manage the deployments of CDK applications. It’s a prerequisite step for deploying resources,

Q: A developer is building a serverless URL shortener using Amazon API Gateway, Amazon DynamoDB, and AWS Lambda. The application code as well as the stack that defines the cloud resources should be written in Python. The code should also be reusable in case an update must be done to the stack.
A: Use AWS CDK to build the stack. Then, use Python as the runtime environment in writing the application logic on Lambda.

Q: A developer is building a serverless API composed of an API Gateway and several Lambda functions. All resources are defined using Cloud Development Kit (CDK) L2 constructs. The developer wants to test some of the Lambda functions in their local environment.
A: Run the cdk synth command and indicate the stack name of Lambda functions to be tested. +
Execute the sam local invoke command and specify the location of the synthesized CloudFormation template and identifier of each function.

Q: A team is deploying a serverless app with AWS Cloud Development Kit (CDK), using the Lambda construct library to define Lambda functions. They’ve had successful deployments in a test environment using the cdk deploy command. However, when the team attempts their first deployment to a new AWS account, it fails, returning a NoSuchBucket error. The team is confident that they made no alterations to the code or CDK configurations after the last successful deployment. What AWS CDK CLI command should be run first to fix the problem?
A: cdk bootstrap


# AWS CLI
Q: A developer has been instructed to automate the creation of the snapshot of an existing Amazon EC2 instance. The engineer created a script that uses the AWS Command Line Interface (CLI) to run the necessary API call. He is getting an InvalidInstanceID.NotFound error whenever the script is run. What is the most likely cause of the error?
A: The AWS Region name used to configure the AWS CLI does not match the region where the instance lives.


# CloudFormation
* The optional Mappings section matches a key to a corresponding set of named values. For example, if you want to set values based on a region, you can create a mapping that uses the region name as a key and contains the values you want to specify for each specific region. You use the Fn::FindInMap intrinsic function to retrieve values in a map.
  (Fn::ImportValue function is used to return the value of an output exported by another stack,
  Fn::GetAtt function is used to return the value of an attribute from a resource in the template)
* The aws cloudformation package command packages the local artifacts (local paths) that your AWS CloudFormation template references. The command uploads local artifacts, such as source code for an AWS Lambda function or a Swagger file for an AWS API Gateway REST API, to an S3 bucket. The command returns a copy of your template, replacing references to local artifacts with the S3 location where the command uploaded the artifacts. Use this command to quickly upload local artifacts that might be required by your template. After you package your template’s artifacts, run the aws cloudformation deploy command to deploy the returned template.
* AWS CloudFormation StackSets extends the functionality of stacks by enabling you to create, update, or delete stacks across multiple accounts and regions with a single operation. Using an administrator account, you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts across specified regions. A stack set lets you create stacks in AWS accounts across regions by using a single AWS CloudFormation template. All the resources included in each stack are defined by the stack set’s AWS CloudFormation template. As you create the stack set, you specify the template to use, as well as any parameters and capabilities that the template requires.
* You can use the existing Parameters section of your CloudFormation template to define Systems Manager parameters, along with other parameters. Systems Manager parameters are a unique type that is different from existing parameters because they refer to actual values in the Parameter Store. The value for this type of parameter would be the Systems Manager (SSM) parameter key instead of a string or other value. CloudFormation will fetch values stored against these keys in Systems Manager in your account and use them for the current stack operation. Parameters stored in Systems Manager are mutable. Any time you use a template containing Systems Manager parameters to create/update your stacks, CloudFormation uses the values for these Systems Manager parameters at the time of the create/update operation. So, as parameters are updated in Systems Manager, you can have the new value of the parameter take effect by just executing a stack update operation. The Parameters section in the output for Describe API will show an additional ‘ResolvedValue’ field that contains the resolved value of the Systems Manager parameter that was used for the last stack operation.

Q: A serverless application consists of multiple Lambda Functions and a DynamoDB table. The application must be deployed by calling the CloudFormation APIs using AWS CLI. The CloudFormation template and the files containing the code for all the Lambda functions are located on a local computer. What should the Developer do to deploy the application?
A: Use the aws cloudformation package command and deploy using aws cloudformation deploy.

Q: An application architect manages several AWS accounts for staging, testing, and production environments, which are used by several development teams. For application deployments, the developers use the similar base CloudFormation template for their applications. Which of the following can allow the developer to effectively manage the updates on this template across all AWS accounts with minimal effort?
A: Update the stacks on multiple AWS accounts using CloudFormation StackSets.

Q: For application deployments, a company is using CloudFormation templates, which are regularly updated to map the latest AMI IDs. A developer was assigned to automate the process since the current set up takes a lot of time to execute on a regular basis. Which of the following is the MOST suitable solution that the developer should implement to satisfy this requirement?
A: Set up CloudFormation with Systems Manager Parameter Store to retrieve the latest AMI IDs for your template. Whenever you decide to update the EC2 instances, call the update-stack API in CloudFormation in your CloudFormation template.


# CloudFront
* When you update existing files in a CloudFront distribution, AWS recommends that you include some sort of version identifier either in your file names or in your directory names to give yourself better control over your content. This identifier might be a date-time stamp, a sequential number, or some other method of distinguishing two versions of the same object.
* To protect sensitive content, the company enforces client-side encryption with customer-managed encryption keys. However, the team noticed that other external websites are linking to and using the photos without permission.
  --> Use an Amazon CloudFront web distribution with signed URLs or signed cookies.

Q: A company has a latency-sensitive service running on AWS Fargate, which is fronted by an Application Load Balancer (ALB). A CloudFront distribution uses the ALB as its origin and presents a custom domain for clients to access the service. The service authenticates requests by validating the JSON Web Token (JWT) obtained from the Authorization header sent by clients. Lately, there has been a significant influx of login attempts from unauthenticated users, which increases the CPU utilization of the Fargate tasks. Which solution would reduce the load on the Fargate tasks in the most operationally efficient manner?
A: Create a CloudFront function for JWT validation. Attach it to the Viewer Request event of the CloudFront distribution.


# CloudWatch
* The Amazon CloudWatch embedded metric format (EMF) allows you to generate custom metrics asynchronously in the form of logs written to CloudWatch Logs. You can embed custom metrics alongside detailed log event data, and CloudWatch automatically extracts the custom metrics so that you can visualize and alarm on them, for real-time incident detection. Additionally, the detailed log events associated with the extracted metrics can be queried using CloudWatch Logs Insights to provide deep insights into the root causes of operational events.
* CloudWatch does not monitor the memory, swap, and disk space utilization of your instances. If you need to track these metrics, you can install a CloudWatch agent in your EC2 instances.

Q: A development team uses AWS Step Functions to orchestrate a serverless workflow that processes incoming data streams with AWS Lambda. The team requires a solution to extract custom metrics, such as processing times, directly from the logs generated by a Lambda function. These metrics must be analyzed for operational insights, with alarms set up to detect anomalies in real-time. Which approach should be used to meet this requirement?
A: Use Amazon's open-source libraries to format logs in the Amazon CloudWatch embedded metric format. Then, use CloudWatch to monitor, view, and set alarms for the custom metrics.

Q: A developer needs to view the percentage of used memory and the number of TCP connections of instances inside an Auto Scaling Group. To achieve this, the developer must send the metrics to Amazon CloudWatch. Which approach provides the MOST secure way of authenticating a CloudWatch PUT request?
A: Create an IAM role with cloudwatch:PutMetricData permission for the new Auto Scaling launch template from which you launch instances.


# CodeBuild
You can use AWS CodeBuild with a proxy server to regulate HTTP and HTTPS traffic to and from the Internet. To run CodeBuild with a proxy server, you install a proxy server in a public subnet and CodeBuild in a private subnet in a VPC. Below are possible causes of error when running CodeBuild with a proxy server:
1.	ssl-bump is not configured properly.
2.	Your organization’s security policy does not allow you to use ssl-bump.
3.	Your buildspec.yml file does not have proxy settings specified using a proxy element.
      If you do not use ssl-bump for an explicit proxy server, add a proxy configuration to your buildspec.yml using a proxy element:
      version: 0.2
      proxy:
      upload-artifacts: yes
      logs: yes

Q: A development team is building a website that displays an analytics dashboard. The team uses AWS CodeBuild to compile the website from a source code residing on Github. A member was instructed to configure CodeBuild to run with a proxy server for privacy and security reasons. A RequestError timeout error appears on CloudWatch whenever CodeBuild is accessed.
A: Modify the proxy element of the buildspec.yml file on the source code root directory.


# CodeDeploy
* In-place deployment: EC2/On-Premises
* Blue/green deployment: EC2/On-Premises, Lambda compute platform, ECS compute platform

Q: A company uses AWS CodeDeploy in their CI/CD pipeline to handle in-place deployments of their web application on EC2 instances. Recently, a new version of the application was pushed, which contained a code regression. The deployment group is configured with automatic rollback. What happens if the deployment of the new version fails?
A: CodeDeploy redeploys the last known good version of an application with a new deployment ID.


# Cognito
* You can use Amazon Cognito to deliver temporary, limited-privilege credentials to your application so that your users can access AWS resources. Amazon Cognito identity pools support both authenticated and unauthenticated identities. You can retrieve a unique Amazon Cognito identifier (identity ID) for your end-user immediately if you’re allowing unauthenticated users or after you’ve set the login tokens in the credentials provider if you’re authenticating users.
* When your mobile app authenticates with the Identity Provider (IdP) using Amazon Cognito, the token returned from the IdP is passed to Amazon Cognito, which then returns a Cognito ID for the user. This Cognito ID is used to provide a set of temporary, limited-privilege AWS credentials through the Cognito Identity Pool.
* A user pool is a user directory in Amazon Cognito. With a user pool, your users can sign in to your web or mobile app through Amazon Cognito. Your users can also sign in through social identity providers like Google, Facebook, Amazon, or Apple, and through SAML identity providers.
* As an alternative to using IAM roles and policies or Lambda Authorizers (formerly known as custom authorizers), you can use an Amazon Cognito User Pool to control who can access your API in Amazon API Gateway. To use an Amazon Cognito user pool with your API, you must first create an authorizer of the COGNITO_USER_POOLS type and then configure an API method to use that authorizer. After the API is deployed, the client must first sign the user into the user pool, obtain an identity or access token for the user, and then call the API method with one of the tokens, which are typically set to the request’s Authorization header. The API call succeeds only if the required token is supplied and the supplied token is valid, otherwise, the client isn’t authorized to make the call because the client did not have credentials that could be authorized.
* Amazon Cognito identity pools (federated identities) enable you to create unique identities for your users and federate them with identity providers. With an identity pool, you can obtain temporary, limited-privilege AWS credentials to access other AWS services.
* Amazon Cognito supports developer authenticated identities, in addition to web identity federation through Facebook (Identity Pools), Google (Identity Pools), Login with Amazon (Identity Pools), and Sign in with Apple (Identity Pools). With developer authenticated identities, you can register and authenticate users via your own existing authentication process, while still using Amazon Cognito to synchronize user data and access AWS resources. Using developer authenticated identities involves interaction between the end-user device, your backend for authentication, and Amazon Cognito. With developer authenticated identities, a new API, , was introduced.

Q: A developer is writing a web application that will allow users to save and retrieve images in an Amazon S3 bucket. The users are required to register and log in to access the application. Which combination of AWS Services should the Developer utilize for implementing the user authentication module of the application?
A: Amazon Cognito Identity Pools and User Pools.

Q: A company is planning to launch an online cross-platform game that expects millions of users. The developer wants to use an in-house authentication system for user identification. Each user identifier must be kept consistent across devices and platforms. How can the developer achieve this?
A: Use developer-authenticated identities in Amazon Cognito to generate unique identifiers for the users.

Q: A developer is building a ReactJS application that will be hosted on Amazon S3. Amazon Cognito handles the registration and signing of users using the AWS Software Development Kit (SDK) for JavaScript. The JSON Web Token (JWT) received upon authentication will be stored on the browser’s local storage. After signing in, the application will use the JWT as an authorizer to access an API Gateway endpoint. What are the steps needed to implement the scenario above? (Select THREE.)
A: Create an Amazon Cognito User Pool. +
Set the name of the header that will be used from the request to the Cognito User Pool as a token source for authorization. +
On the API Gateway Console, create an authorizer using the Cognito User Pool ID.

Q: A San Francisco-based tech startup is building a cross-platform mobile app that can notify the user of upcoming astronomical events. Your mobile app authenticates with the Identity Provider (IdP) using the provider’s SDK and Amazon Cognito. Once the end-user is authenticated with the IdP, the OAuth or OpenID Connect token returned from the IdP is passed by your app to Amazon Cognito. Which of the following is returned for the user to provide a set of temporary, limited-privilege AWS credentials?
A: Cognito ID


# DynamoDB
* Return after each write request the total number of write capacity units consumed, with subtotals for the table and any secondary indexes that were affected by the operation. => Add the ReturnConsumedCapacity parameter with a value of INDEXES in every write request. (TOTAL returns the total number of write capacity units consumed.)
* To read data from a table, you use operations such as GetItem, Query, or Scan. DynamoDB returns all of the item attributes by default. To get just some, rather than all of the attributes, use a projection expression. A projection expression is a string that identifies the attributes you want. To retrieve a single attribute, specify its name. For multiple attributes, the names must be comma-separated.
* Auto Scaling group of EC2 instances with a DynamoDB database, changes made by one user overwritten by another --> Implement an optimistic locking strategy in your application source code by designating one property to store the version number in the mapping class for your table.
* Optimistic locking strategy ensures that the client-side item that you are updating (or deleting) is the same as the item in DynamoDB. It doesn’t have the capability to specify certain conditions to meet. Optimistic locking is a strategy to ensure that the client-side item that you are updating (or deleting) is the same as the item in DynamoDB. If you use this strategy, then your database writes are protected from being overwritten by the writes of others — and vice-versa. But does not work for:
    - Global Tables
      – DynamoDBMapper transactional operations
* By default, the DynamoDB write operations (PutItem, UpdateItem, DeleteItem) are unconditional: each of these operations will overwrite an existing item that has the specified primary key. DynamoDB optionally supports conditional writes for these operations. A conditional write will succeed only if the item attributes meet one or more expected conditions. Otherwise, it returns an error. Conditional writes are helpful in cases where multiple users attempt to modify the same item.
* A single BatchGetItem operation can retrieve up to 16 MB of data, which can contain as many as 100 items. BatchGetItem returns a partial result if:
  – The response size limit is exceeded
  – The table’s provisioned throughput is exceeded
  – More than 1MB per partition is requested
  – An internal processing failure occurs.
  If DynamoDB returns any unprocessed items, you should retry the batch operation on those items. It’s recommended that you use an exponential backoff algorithm. Exponential backoff is a technique where, if a request to a server fails, you wait a bit before retrying. If it keeps failing, you wait longer each time. The main idea is to reduce the frequency of calls over time, which helps avoid overloading the server, giving it a better chance to recover and respond successfully. You can implement an exponential backoff yourself in your code or simply use the AWS SDK, which comes with automatic retry logic and exponential backoff.
* Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10x performance improvement – from milliseconds to microseconds – even at millions of requests per second. DAX does all the heavy lifting required to add in-memory acceleration to your DynamoDB tables without requiring developers to manage cache invalidation, data population, or cluster management. One of the key benefits of using DAX is that it works transparently with existing DynamoDB API calls, and the application code does not need to be modified to take advantage of the caching layer. This means that the developer can improve the performance of their DynamoDB applications without having to modify the code.
* With Amazon DynamoDB transactions, you can group multiple actions together and submit them as a single all-or-nothing TransactWriteItems or TransactGetItems operation. TransactWriteItems is a synchronous and idempotent write operation that groups up to 25 write actions in a single all-or-nothing operation. These actions can target up to 25 distinct items in one or more DynamoDB tables within the same AWS account and in the same Region. The aggregate size of the items in the transaction cannot exceed 4 MB. The actions are completed atomically so that either all of them succeed or none of them succeeds.

Q: A developer has a Python script that relies on the low-level BatchGetItem API to fetch large amounts of data from a DynamoDB table. The script often encounters responses with partial results. A significant portion of the data appears under UnprocessedKeys. Which approaches can the developer implement to handle data retrieval MOST reliably? (Select TWO.)
A: Use the AWS software development kit (AWS SDK) to send batch requests. +
Implement an exponential backoff algorithm with a randomized delay between retries of the batch request.

Q: A developer wants to cut down the execution time of the scan operation on a DynamoDB table during periods of low demand without interfering with typical workloads. The operation consumes half of the strongly consistent read capacity units within regular operating hours. How can the developer improve this scan operation?
A: Perform a rate-limited parallel scan operation.

Q: A DynamoDB table has several top-level attributes such as id, course_id, course_title, price, rating and many others. The database queries of your application returns all of the item attributes by default but you only want to fetch specific attributes such as the course_id and price per request. As the developer, how can you refactor your application to accomplish this requirement?
A: Use projection expression

Q: A mobile game has a serverless backend consisting of an API Gateway backed by Lambda functions and a DynamoDB table in provisioned capacity mode, where player data is stored. While the game has maintained a consistent level of traffic, recent growth in the player base has caused response times to slow down. To improve performance, the developer wants to reduce the number of database queries for data that rarely change. What approach can the developer take to achieve this goal cost-effectively and with less development overhead?
A: Set up an Amazon DynamoDB Accelerator (DAX) caching layer in front of the DynamoDB table.

Q: A developer is building an application with Amazon DynamoDB as its database. The application needs to group the PUT, UPDATE, and DELETE actions into a single all-or-nothing operation to make changes against multiple items in the DynamoDB table. Which DynamoDB operation should the developer use?
A: TransactWriteItems


# EC2
* On-Demand EC2 instance in VPC, shell script that fetches the instance’s associated public and private IP addresses --> Get the public and private IP addresses from the instance metadata service using the http://169.254.169.254/latest/meta-data/ endpoint.
* An EC2/On-Premises deployment hook is executed once per deployment to an instance. You can specify one or more scripts to run in a hook. Each hook for a lifecycle event is specified with a string on a separate line. DownloadBundle deployment lifecycle event will throw an error whenever:
  – The EC2 instance’s IAM profile does not have permission to access the application code in the Amazon S3.
  – An Amazon S3 internal error occurs.
  – The instances you deploy to are associated with one AWS Region, but the Amazon S3 bucket that contains the application revision is related to another AWS Region.
* When you launch an instance in Amazon EC2, you have the option of passing user data to the instance that can be used to perform common automated configuration tasks and even run scripts after the instance starts. You can pass two types of user data to Amazon EC2: shell scripts and cloud-init directives. You can also pass this data into the launch wizard as plain text, as a file (this is useful for launching instances using the command line tools), or as base64-encoded text (for API calls).
* If you terminate a container instance in the RUNNING state, that container instance is automatically removed or deregistered from the cluster. However, if you terminate a container instance in the STOPPED state, that container instance isn’t automatically removed from the cluster. To deregister your container instance from the cluster, you should deregister it after terminating it in the STOPPED state by using the Amazon ECS Console or AWS Command Line Interface.
* A launch template is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch template, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you’ve launched an EC2 instance before, you specified the same information in order to launch the instance. You can specify your launch template with multiple Auto Scaling groups. However, you can only specify one launch template for an Auto Scaling group at a time, and you can’t modify a launch template after you’ve created it. To change the launch template for an Auto Scaling group, you must create a launch template and then update your Auto Scaling group with it.

Q: A developer plans to launch an EC2 instance, with Amazon Linux 2 as its AMI, using the AWS Console. A security group with port 80 that is open to public access will be associated with the instance. He wants to quickly build and test an Apache webserver with an index.html displaying a hello world message.
A: Configure the user data at the creation of the EC2 instance to run a script that will install and create the Apache webserver after the instance starts.

Q: A developer needs to view the percentage of used memory and the number of TCP connections of instances inside an Auto Scaling Group. To achieve this, the developer must send the metrics to Amazon CloudWatch. Which approach provides the MOST secure way of authenticating a CloudWatch PUT request?
A: Create an IAM role with cloudwatch:PutMetricData permission for the new Auto Scaling launch template from which you launch instances.

Q: A developer has been instructed to automate the creation of the snapshot of an existing Amazon EC2 instance. The engineer created a script that uses the AWS Command Line Interface (CLI) to run the necessary API call. He is getting an InvalidInstanceID.NotFound error whenever the script is run. What is the most likely cause of the error?
A: The AWS Region name used to configure the AWS CLI does not match the region where the instance lives.


# ECS
* A task definition is required to run Docker containers in Amazon ECS. The following are some of the parameters you can specify in a task definition:
  – The Docker image to use with each container in your task
  – How much CPU and memory to use with each task or each container within a task
  – The launch type to use, which determines the infrastructure on which your tasks are hosted
  – The Docker networking mode to use for the containers in your task
  – The logging configuration to use for your tasks
  – Whether the task should continue to run if the container finishes or fails
  – The command the container should run when it is started
  – Any data volumes that should be used with the containers in the task
  – The IAM role that your tasks should use
  Before you can run Docker containers on Amazon ECS, you must create a task definition. You can define multiple containers and data volumes in a single task definition.
* Random places tasks on instances at random, yet still honors the other constraints that you specified, implicitly or explicitly. Specifically, it still makes sure that tasks are scheduled on instances with enough resources to run them.
* Spread: place tasks evenly across your cluster to help maximize availability. By default, ECS uses spread with the ecs.availability-zone attribute to place tasks. Accepted values are attribute key-value pairs, instanceId, or host.

Q: A developer is tasked with automating the deployment of a new microservice in an ECS cluster using AWS CodeDeploy. The developer is writing the AppSpec file to instruct CodeDeploy on how to handle the deployment. Which sets of properties are REQUIRED in the resources section to successfully deploy the microservice? (Select THREE.)
A: TaskDefinition, ContainerName, ContainerPort


# Elastic Beanstalk
* You can add AWS Elastic Beanstalk configuration files (.ebextensions) to your web application’s source code to configure your environment and customize the AWS resources that it contains. Configuration files are YAML- or JSON-formatted documents with a .config file extension that you place in a folder named .ebextensions and deploy in your application source bundle.
* Standard generic and preconfigured Docker platforms on Elastic Beanstalk support only a single Docker container per Elastic Beanstalk environment. In order to get the most out of Docker, Elastic Beanstalk lets you create an environment where your Amazon EC2 instances run multiple Docker containers side by side. Container instances—Amazon EC2 instances running Multicontainer Docker in an Elastic Beanstalk environment—require a configuration file named Dockerrun.aws.json. This file is specific to Elastic Beanstalk and can be used alone or combined with source code and content in a source bundle to create an environment on a Docker platform.
* Each time you upload a new version of your application with the Elastic Beanstalk console or the EB CLI, Elastic Beanstalk creates an application version. If you don’t delete versions that you no longer use, you will eventually reach the application version quota and be unable to create new versions of that application. You can avoid hitting the quota by applying an application version lifecycle policy to your applications. A lifecycle policy tells Elastic Beanstalk to delete application versions that are old or to delete application versions when the total number of versions for an application exceeds a specified number. Elastic Beanstalk applies an application’s lifecycle policy each time you create a new application version and deletes up to 100 versions each time the lifecycle policy is applied. Elastic Beanstalk deletes old versions after creating the new version and does not count the new version towards the maximum number of versions defined in the policy. In the Retention section of the application version lifecycle settings, you can either choose to delete the source bundle in S3 or retain it.
* With ElasticBeanstalk, you can:
  – Select the operating system that matches your application requirements (e.g., Amazon Linux or Windows Server 2016)
  – Choose from several Amazon EC2 instances, including On-Demand, Reserved Instances, and Spot Instances.
  – Choose from several available database and storage options.
  – Enable login access to Amazon EC2 instances for immediate and direct troubleshooting
  – Quickly improve application reliability by running in more than one Availability Zone.
  – Enhance application security by enabling HTTPS protocol on the load balancer
  – Access built-in Amazon CloudWatch monitoring and getting notifications on application health and other important events
  – Adjust application server settings (e.g., JVM settings) and pass environment variables
  – Run other application components, such as a memory caching service, side-by-side in Amazon EC2.
  – Access log files without logging in to the application servers

Q: A full-stack developer has developed an application written in Node.js to host an upcoming mobile game tournament. The developer has decided to deploy the application using AWS Elastic Beanstalk because of its ease-of-use. Upon experimenting, he learned that he could configure the webserver environment with several resources. Which of the following services can the developer configure with Elastic Beanstalk? (Select THREE.)
A: Amazon EC2 Instance, Amazon CloudWatch, and Application Load Balancer.

Q: A Software Engineer is developing a Node.js application that will be deployed using Elastic Beanstalk. The application source code is currently inside a folder called MyApp. He wants to add a configuration file named tutorialsdojo.config to the application. Where should the file be placed?
A: Inside the .ebextensions folder

Q: A developer has enabled the lifecycle policy of an application deployed in Elastic Beanstalk. The lifecycle is set to limit the application version to 15 versions. The developer wants to keep the source code in an S3 bucket, yet, it gets deleted.
A: Configure the Retention setting to retain the source bundle in the S3 bucket.

Q: An application performs various workflows and processes long-running tasks that take a long time to complete. Users are complaining that the application is unresponsive since the workflow substantially increases the time it takes to complete a user request. The development team is looking for a managed solution that can handle background tasks efficiently, scale automatically, and integrate seamlessly with the existing application deployed on Elastic Beanstalk. Which of the following is the BEST way to improve the performance of the application?
A: Use an Elastic Beanstalk worker environment to process the tasks asynchronously.


# Elasticache
Amazon ElastiCache allows you to seamlessly set up, run, and scale popular open-source compatible in-memory data stores in the cloud.
Redis is the only service in Elasticache that supports replication.
Memcached supports multi-threading.

Q: A developer is looking for a way to decrease the latency in retrieving data from an Amazon RDS MySQL database. He wants to implement a caching solution that supports Multi-AZ replication with sub-millisecond response times. What must the developer do that requires the LEAST amount of effort?
A: Set up an Elasticache for Redis cluster between the application and database. Configure it to run with replication to achieve high availability.


# IAM
* If you have resources that are running inside AWS that need programmatic access to various AWS services, then the best practice is always to use IAM roles. However, applications running outside of an AWS environment will need access keys for programmatic access to AWS resources. For example, monitoring tools running on-premises and third-party automation tools will need access keys.
* Access keys are long-term credentials for an IAM user or the AWS account root user. You can use access keys to sign programmatic requests to the AWS CLI or AWS API (directly or using the AWS SDK).
* Programmatic access is just another way of presenting yourself as an IAM User. IAM users are global entities which means it can not be associated with a particular region.
* You can implement a fine-grained access control  write an IAM permissions policy that specifies conditions for accessing security credentials and the associated permissions. You then apply the policy to IAM users, groups, or roles that you create using the IAM console. Your IAM policy can restrict access to individual items in a table, access to the attributes in those items, or both at the same time. You use the IAM Condition element to implement a fine-grained access control policy.
* Access to AWS resources requires permissions. You can create IAM roles and users that include the permissions that you need for the CloudWatch agent to write metrics to CloudWatch and for the CloudWatch agent to communicate with Amazon EC2 and AWS Systems Manager. You use IAM roles on Amazon EC2 instances, and you use IAM users with on-premises servers.
* "AccessDenied" indicates an authorization error related to permissions. Multiple policies can be attached to a single IAM role.

Q: A startup has recently opened an AWS account to develop a cloud-native web application. The CEO wants to improve the security of the account by implementing the best practices in managing access keys in AWS.
A: Use IAM roles for applications that need access to AWS services. +
Delete any access keys to your AWS account root user.

Q: A mobile game developer is using DynamoDB as a data store and a Web Identity Federation for authorization and authentication. Each item in the DynamoDB table contains the attributes for individual user’s game data such as user ID, game scores, and top score where the user ID is the partition key. The developer must control user access to specific data items based on their IDs. In doing so, users will only be able to obtain items that they own.
A: Modify the IAM Policy associated with the Identity provider’s role by setting the value of the dynamodb:LeadingKeys condition key to the user IDs.

Q: A developer tests code running on the developer's laptop. The code is using the AWS SDK for Python (Boto3) to access AWS services. The .aws/credentials file is set up with the user's IAM user name and password. The developer runs the code and receives this error message: An error occurred (InvalidAccessKeyId). Which action will resolve this error?
A: Replace the IAM user name and password with an access key ID and a secret access key.

Q: A programmer is developing a Node.js application that will be run on a Linux server in their on-premises data center. The application will access various AWS services such as S3, DynamoDB, and ElastiCache using the AWS SDK. Which of the following is the MOST suitable way to provide access for the developer to accomplish the specified task?
A: Go to the AWS Console and create a new IAM user with programmatic access. In the application server, create the credentials file at ~/.aws/credentials with the access keys of the IAM user.


# Kinesis – EC2
* Ratio of the number of Kinesis shards to EC2 worker instances? --> one worker can process any number of shards, so it’s fine if the number of shards exceeds the number of instances (4 shards --> 2 instances)


# KMS
* You can perform the following key management functions in AWS KMS:
  – Create symmetric and asymmetric keys where the key material is only ever used within the service
  – Create symmetric keys where the key material is generated and used within a custom key store under your control.
  – Import your own symmetric key for use within the service.
  – Create both symmetric and asymmetric data key pairs for local use within your applications.
  – Define which IAM users and roles can manage keys.
  – Define which IAM users and roles can use keys to encrypt and decrypt data.
  – Choose to have keys that were generated by the service to be automatically rotated on an annual basis.
  – Temporarily disable keys so they cannot be used by anyone.
  – Re-enable disabled keys.
  – Schedule the deletion of keys that you no longer use.
  – Audit the use of keys by inspecting logs in AWS CloudTrail.
* SSE-KMS: x-amz-server-side-encryption header with a value of aws:kms
* SSE-C: x-amz-server-side-encryption-customer-algorithm, x-amz-server-side-encryption-customer-key, x-amz-server-side-encryption-customer-key-MD5
* The ‘GenerateDataKey’ generates a unique symmetric data key for client-side encryption. This operation returns a plaintext copy of the data key and a copy that is encrypted under a customer master key (CMK) that you specify. You can use the plaintext key to encrypt your data outside of AWS KMS and store the encrypted data key with the encrypted data. GenerateDataKey returns a unique data key for each request. The bytes in the plaintext key are not related to the caller or the CMK.
* AWS KMS establishes quotas for the number of API operations requested in each second. You can make API requests directly or by using an integrated AWS service that makes API requests to AWS KMS on your behalf. The quota applies to both kinds of requests.

Q: A developer is writing an application that will download hundreds of media files. Each file must be encrypted with a unique encryption key within the application before storing it in an S3 bucket. The developer needs a cost-effective solution with low management overhead. Which of the following is the most suitable solution
A: Use the ‘GenerateDataKey’ API command to generate a data key for each file to encrypt them. Store the encrypted data key and the file.

Q: A company is developing a Python application that submits data to an Amazon DynamoDB table. The company requires client-side encryption of specific data items and end-to-end protection for the encrypted data in transit and at rest. Which combination of steps will meet the requirement to encrypt specific data items? (Select TWO.)
A: Generate symmetric encryption keys with AWS Key Management Service (AWS KMS) +
Use generated keys with the AWS Database Encryption SDK.
(The AWS Database Encryption SDK provides end-to-end protection for your data in transit and at rest. You can encrypt selected items or attribute values in a table. AWS managed KMS keys provides server-side encryption at rest!)

Q: A developer uses Amazon CloudWatch Logs Insights to query and analyze log data from an existing CloudWatch Logs group that has been actively receiving data for the past few weeks. The developer wants to ensure that any new log data published to the log group is encrypted using an AWS Key Management Service (AWS KMS) key. Which approach requires the least administrative effort to meet the requirements?
A: Associate an existing AWS KMS key with the log group by executing the aws logs associate-kms-key command with the key’s Amazon Resource Name.


# Lambda
* Concurrent executions refers to the number of executions of your function code that are happening at any given time. You can estimate the concurrent execution count, but it will differ depending on whether or not your Lambda function is processing events from a poll-based event source. For Lambda functions that process Kinesis or DynamoDB streams, the NUMBER OF SHARDS is the unit of concurrency! If your stream has 100 active shards, there will be at most 100 Lambda function invocations running concurrently.
  This is different for push-based sources such as API Gateway or Amazon S3 Events.
* If you have to do certain modifications such as the way Lambda runs the function’s setup code and how the invocation events are read from the Lambda runtime API. --> take advantage of Custom Runtime. A runtime is a program that runs a Lambda function’s handler method when the function is invoked. You can include a runtime in your function’s deployment package in the form of an executable file named 'bootstrap'. A runtime is responsible for running the function’s setup code, reading the handler name from an environment variable, and reading invocation events from the Lambda runtime API. The runtime passes the event data to the function handler and posts the response from the handler back to Lambda.
* Layers enable you to use libraries and other dependencies in your function without having to include them in your deployment package.
* If you need a simple way to configure an HTTPS endpoint in front of your Lambda function without having to learn and configure additional services besides Lambda, you can use Lambda function URLs. This can be useful in cases where you need to implement a simple webhook handler or form validator that runs within an individual Lambda function and does not require additional functionality beyond processing incoming requests. There are two types of authorization available for Lambda function URLs:
- AWS_IAM  – the function URL can only be invoked by an IAM user or role with the necessary permissions. This can be useful in cases where you need to restrict access to the Lambda function to a specific set of users or roles within your organization.
- NONE – anyone can invoke the Lambda function using the URL. This approach can be useful in cases where you want to make the Lambda function publicly accessible and do not require any additional authentication or authorization beyond the URL. However, you may still need to validate the incoming requests in the Lambda function to ensure that the request comes from a trusted source.
* Step Functions can help solve the problem of timeout errors of Lambda functions. Imagine a Lambda function that has four utility functions that are run sequentially. Each of those functions takes 5 minutes to finish which translates to a total execution time of 20 minutes. This is a problem since Lambda can only run for a maximum of 15 minutes. To solve this, we can refactor the functions inside the Lambda function into individual Step Functions states. This way, each function is contained in a separate Lambda function, which has its own execution timeout.
* AWS Lambda natively supports Java, Go, PowerShell, Node.js, C#, Python, and Ruby code, and provides a Runtime API, which allows you to use any additional programming languages to author your functions.
* ModuleNotFoundError and Module cannot be loaded are common errors for Lambda functions. These errors are usually due to an incorrect folder structure or file permissions with the deployment package .zip file. To fix this error:
1. Install all dependency modules local to the function project.
2. Build the deployment package by zipping up the project folder for deployment to Lambda.
3. Upload the deployment package.
* A Lambda function’s execution role is an AWS Identity and Access Management (IAM) role that grants the function permission to access AWS services and resources. For example, you might create an execution role that has permission to send logs to Amazon CloudWatch and upload trace data to AWS X-Ray. The AWSLambdaBasicExecutionRole is a managed policy provided by AWS that includes permissions essential for a Lambda function to create and write logs to Amazon CloudWatch Logs. These permissions include permissions to Log actions such as logs:CreateLogGroup, logs:CreateLogStream, and logs:PutLogEvents.
* By creating separate aliases for Production and Development, systems can initiate the correct alias as needed. A Lambda function alias can be used to point to a specific Lambda function version. Using the functionality to update an alias and its linked version, the development team can update the required version as needed. The $LATEST version is the newest published version.
* A Lambda alias is a pointer to a specific Lambda function version.
* Instead of using ARNs for the Lambda function in event source mappings, you can use an alias ARN. You do not need to update your event source mappings when you promote a new version or roll back to a previous version.
* To add "stageVariable" to the Lambda ARN, you should use the following format: ${stageVariable.stageVariableName}.
* Environment variables for Lambda functions enable you to dynamically pass settings to your function code and libraries without making changes to your code. Environment variables are key-value pairs that you create and modify as part of your function configuration using either the AWS Lambda Console, the AWS Lambda CLI, or the AWS Lambda SDK. AWS Lambda then makes these key-value pairs available to your Lambda function code using standard APIs supported by the language, like process.env for Node.js functions. You can use environment variables to help libraries know what directory to install files in, where to store outputs, store connection and logging settings, and more. By separating these settings from the application logic, you don’t need to update your function code when changing the function behavior based on different settings.

Q: A company is developing a distributed system which will use a Lambda function that will be invoked asynchronously. In the event of failure, the function must be retried twice before sending the unprocessed events to an Amazon SQS queue through the use of Dead Letter Queue (DLQ). Which of the following is the correct way to implement a DLQ in Lambda?
A: Specify the Amazon Resource Name of the SQS Queue in the Lambda function’s DeadLetterConfig parameter.

Q: You developed a shell script which uses AWS CLI to create a new Lambda function. However, you received an InvalidParameterValueException after running the script. What is the MOST likely cause of this issue?
A: You provided an IAM role in the CreateFunction API which AWS Lambda is unable to assume.
(The InvalidParameterValueException will be returned if one of the parameters in the request is invalid. For example, if you provided an IAM role in the CreateFunction API which AWS Lambda is unable to assume.)

Q: A Lamba function has multiple sub-functions that are chained together to process large data synchronously. When invoked, the function tends to exceed its maximum timeout limit. This has prompted the developer to break the Lambda function into manageable coordinated states using Step Functions, enabling each sub-function to run in separate processes. Which of the following type of states should the developer use to run processes?
A: Task State

Q: A startup is integrating an event-driven alerting tool with a third-party platform. The platform requires a publicly accessible HTTPS endpoint to receive webhook requests, which will be processed by a Lambda function. Given that the platform signs each request with a secret key and includes it in the headers, the developer must ensure that the Lambda function executes the domain logic only when a webhook request comes from a valid user. Which action would satisfy the requirement with the least amount of development effort?
A: Create a Lambda function URL. Attach a resource-based policy to the function allowing anyone to invoke it only if the "lambda:FunctionUrlAuthType": "NONE" condition is present. Write a custom authorization logic based on a signature provided in the HTTP headers.

Q: A developer is debugging an issue in an AWS Lambda-based application. To save time searching through logs, the developer wants the function to return the corresponding log location of an invocation request. Which approach should the developer take with the least amount of effort?
A: Extract the log stream name from the Context object of the handler function.

Q: A Ruby developer is looking to offload some of the processing on his application to the AWS cloud without managing any servers. The submodules must be written in Ruby, which mainly invokes API calls to an external web service. The response from the API call is parsed and stored in a MongoDB database. What should he do to develop the Lambda function in his preferred programming language?
A: Create a Lambda function with a supported runtime version for Ruby.

Q: An application that runs on a local Linux server is migrated to a Lambda function to save costs. The Lambda function shows an Unable to import module error when invoked. As the developer, how can you fix the error?
A: Install the missing modules locally to your application’s folder. Package the folder into a ZIP file and upload it to AWS Lambda.

Q: A developer is testing a Lambda function that was created from a CloudFormation template. While the function executes without errors, it isn’t generating logs in Amazon CloudWatch Logs, and the developer cannot find associated log streams or log groups.
Upon inspection, the following observations were made:
•	The function’s code contains appropriate logging statements.
•	The Lambda function is associated with an execution role that establishes a trusted relationship with the Lambda service; however, this role has no permissions assigned.
•	The Lambda function does not have any resource-based policies.
Which configuration must be done to resolve the issue?
A: Update the execution role by adding the AWSLambdaBasicExecutionRole managed policy.

Q: A developer is building an AWS Lambda-based Java application that optimizes pictures uploaded to an S3 bucket. Upon running several tests, the Lambda function shows a cold start of about 5 seconds.
A: Reduce the deployment package’s size by including only the needed modules from the AWS SDK for Java. & Increase the memory allocation setting for the Lambda function.

Q: A developer needs to query a relational database from an AWS Lambda function. The Lambda function will be called frequently. The developer needs to avoid the latency introduced by the initial JDBC connection to the database in subsequent invocations of the Lambda function. How can the developer ensure the database connection is reused by Lambda?
A: Initialize the database connection within the Lambda function code but outside the handler method.
(The Lambda environment can reuse the same database connection for subsequent invocations when defining the database connection details in the code but outside the handler method.)

Q: A developer has deployed a Lambda function that runs in DEV, UAT, and PROD environments. The function uses different parameters that varies based on the environment it is running in. The parameters are currently hardcoded in the function. Which action should the developer do to reference the appropriate parameters without modifying the code every time the environment changes?
A: Use environment variables to set the parameters per environment.


# Lambda – API Gateway
* To deploy an API, you create an API deployment and associate it with a stage. A stage is a logical reference to a lifecycle state of your API (for example, dev, prod, beta, v2). API stages are identified by the API ID and stage name. They’re included in the URL that you use to invoke the API. Each stage is a named reference to an API deployment and is made available for client applications to call.
* One of its key features is the ability to create different stages for an API, allowing developers to maintain multiple versions of the same API, such as development, testing, and production.

Q: A developer is managing several microservices built using API Gateway and AWS Lambda. The Developer wants to deploy new updates to one of the APIs. He wants to ensure a smooth transition between the versions by giving users enough time to migrate to the new version before retiring the previous one. Which solution should the developer implement?
A: Implement the updates and publish a new version of the Lambda function. Specify the new version in the API Gateway target resource then redeploy it to a new stage.

Q: An update was made on an AWS Lambda-based application. It is invoked by an API Gateway endpoint with caching enabled to improve latency requests. The developer expected to get the latest data as a response when he tested the application. However, he kept getting stale data upon trying many times. What should the developer do that will require the LEAST amount of effort to resolve the issue? (Select TWO.)
A: Include Cache-Control: max-age=0 HTTP header on the API request. &
Grant permission to the client to invalidate caching when there’s a request using the IAM execution role.

Q: You have several API Gateway APIs with Lambda Integration for each release life cycle of your application. There is a requirement to consolidate multiple releases into a single API Gateway for the ALPHA, BETA, RC (Release Candidate), and PROD releases. For example, their clients can connect to their ALPHA release by using the alpha.tutorialsdojo.com endpoint and beta release through the beta.tutorialsdojo.com endpoint. As the AWS developer, how can you satisfy this requirement?
A: Set up Stage Variables for each release.


# RDS
* Monitor how the different processes on a DB instance use the percentage of the CPU or the total memory consumed by each process --> Enhanced Monitoring
* RDS Proxy helps you manage a large number of connections from Lambda to an RDS database by establishing a warm connection pool to the database. Your Lambda functions interact with RDS Proxy instead of your database instance.

Q: A developer has an application that uses a Lambda function to process data from an Aurora MySQL DB Instance in a Virtual Private Cloud (VPC). The database throws a MySQL: ERROR 1040: Too many connections error whenever there is a surge in incoming traffic. Which is the most suitable solution for resolving the issue?
A: Provision an RDS Proxy between the Lambda function and the RDS database instance.


# SQS
* To manage large Amazon Simple Queue Service (Amazon SQS) messages, you can use Amazon Simple Storage Service (Amazon S3) and the Amazon SQS Extended Client Library for Java. This is especially useful for storing and consuming messages up to 2 GB. Unless your application requires repeatedly creating queues and leaving them inactive or storing large amounts of data in your queues, consider using Amazon S3 for storing your data.
  You can use the Amazon SQS Extended Client Library for Java to do the following:
  – Specify whether messages are always stored in Amazon S3 or only when the size of a message exceeds 256 KB
  – Send a message that references a single message object stored in an S3 bucket
  – Retrieve the message object from an S3 bucket
  – Delete the message object from an S3 bucket
  You can use the Amazon SQS Extended Client Library for Java to manage Amazon SQS messages using Amazon S3 only with the AWS SDK for Java. You can’t do this with the AWS CLI, the Amazon SQS console, the Amazon SQS HTTP API, or any of the other AWS SDKs.
* Amazon SQS long polling is a way to retrieve messages from your Amazon SQS queues. While the regular short polling returns immediately, even if the message queue being polled is empty, long polling doesn’t return a response until a message arrives in the message queue or the long poll times out. Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses (when there are no messages available for a ReceiveMessage request) and false empty responses (when messages are available but aren’t included in a response). This type of polling is suitable if the new messages that are being added to the SQS queue arrive less frequently.

Q: A developer needs to build a queueing mechanism for an application that will run on AWS. The application is expected to consume SQS messages that are larger than 256 KB and up to 1 GB in size.
A: Use Amazon S3 and the Amazon SQS Extended Client Library for Java

Q: The infrastructure of an application is designed such that a producer sends data to a consumer via HTTPS. The consumer may sometimes take a while to process the messages, which can result in unexpected timeouts and cause newer messages not to be acknowledged immediately. To resolve this issue, a developer decided to introduce an Amazon SQS standard queue into the system. However, duplicate messages are still not being handled properly.
A: Create a FIFO queue as a replacement for the standard queue + Configure the producer to set deduplication IDs for the messages.

Q: A Software Engineer is developing an application that will be hosted on an Amazon EC2 instance and read messages from a standard Amazon SQS queue. The average time that it takes for the producers to send a new message to the queue is 10 seconds. Which of the following is the MOST efficient way for the application to query the new messages from the queue?
A: Configure the SQS queue to use Long Polling.


# S3
* To enable the cross-region replication feature in S3, the following items should be met:
  – The source and destination buckets must have versioning enabled.
  – The source and destination buckets must be in different AWS Regions.
  – Amazon S3 must have permissions to replicate objects from that source bucket to the destination bucket on your behalf.
* Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and your Amazon S3 bucket. Transfer Acceleration leverages Amazon CloudFront’s globally distributed AWS Edge Locations. As data arrives at an AWS Edge Location, data is routed to your Amazon S3 bucket over an optimized network path.

Q: An application uses the PutObject operation in parallel to upload hundreds of thousands of objects per second to an S3 bucket. To meet security compliance, the developer uses the server-side encryption in AWS KMS (SSE-KMS) to encrypt objects as they get stored in the S3 bucket. There is a noticeable performance degradation after making the change. Which of the following is the most likely cause of the problem?
A: The API request rate has exceeded the quota for AWS KMS API operations.

Q: A company is developing an image processing application. When an image is uploaded to an Amazon S3 bucket, a number of independent and separate services must be invoked to process the image. The services do not have to be available immediately, but they must process every image. Which application design satisfies these requirements?
A: Configure an Amazon S3 event notification that publishes to an Amazon SNS topic. Subscribe a separate Amazon SQS queue for each service to the Amazon topic.


# SAM
Here are the SAM CLI commands needed to deploy serverless applications:
-	sam init – Initializes a serverless application with an AWS SAM template. The template provides a folder structure for your Lambda functions and is connected to an event source such as APIs, S3 buckets, or DynamoDB tables. This application includes everything you need to get started and to eventually extend it into a production-scale application.
-	sam build – The sam build command builds any dependencies that your application has, and copies your application source code to folders under .aws-sam/build to be zipped and uploaded to Lambda.
-	sam deploy – performs the functionality of sam package. You can use the sam deploy command to directly package and deploy your application.

Q: A developer uses AWS Serverless Application Model (SAM) in a local machine to create a serverless Python application. After defining the required dependencies in the requirements.txt file, the developer is now ready to test and deploy. What are the steps to successfully deploy the application?
A: Build the SAM template in the local machine and call the sam deploy command to package and deploy the SAM template from an S3 bucket.
(Since the application’s runtime and dependencies are already defined, the next step is to call the sam build command to install and build the dependencies of the application.)


# Step Functions
* Task and Parallel states can have a field named Retry, whose value must be an array of objects known as retriers. An individual retrier represents a certain number of retries, usually at increasing time intervals. A retrier contains the following fields: ErrorEquals, IntervalSeconds, MaxAttempts, BackoffRate, Next, ResultPath
* You can use the Catch and Retry fields inside the state machine definition to capture an exception error and attempt to recover from it by automatically retrying the state.
* In AWS Step Functions, you define your workflows in the Amazon States Language. The Step Functions console provides a graphical representation of that state machine to help visualize your application logic.
  States are elements in your state machine. A state is referred to by its name, which can be any string, but must be unique within the scope of the entire state machine. States can perform a variety of functions in your state machine:
  Task State – Do some work in your state machine
  Choice State – Make a choice between branches of execution
  Fail or Succeed State – Stop execution with failure or success
  Pass State – Simply pass its input to its output or inject some fixed data, without performing work.
  Wait State – Provide a delay for a certain amount of time or until a specified time/date.
  Parallel State – Begin parallel branches of execution.
  Map State – Dynamically iterate steps.
  Out of all the types of State, only the Task State and the Parallel State can be used to run processes in the state machine. In the given scenario, the application logic inside the Lambda function process data synchronously. In this case, Task State should be used.
* Both the InputPath and Parameters fields provide a way to manipulate JSON as it moves through your workflow. InputPath can limit the input that is passed by filtering the JSON notation by using a path. The Parameters field enables you to pass a collection of key-value pairs, where the values are either static values that you define in your state machine definition, or that are selected from the input using a path. AWS Step Functions applies the InputPath field first, and then the Parameters field. You can first filter your raw input to a selection you want using InputPath, and then apply Parameters to manipulate that input further, or add new values. The output of a state can be a copy of its input, the result it produces (for example, the output from a Task state’s Lambda function), or a combination of its input and result. Use ResultPath to control which combination of these is passed to the state output. OutputPath enables you to select a portion of the state output to pass to the next state. This enables you to filter out unwanted information, and pass only the portion of JSON that you care about.
* The activity worker must first call the CreateActivity API action to obtain the activity Amazon Resource Name (ARN). The GetActivityTask API action then retrieves a scheduled task. The SendTaskHeartbeat API action periodically reports on task progress. The SendTaskFailure or SendTaskSuccess API actions report success or failure.
* The HeartbeatSeconds attribute defines the maximum interval that the task will wait for a heartbeat signal. If an activity worker fails to send heartbeats within this interval, the state is failed.

Q: A developer is building a serverless workflow using Step Functions. The developer has to implement a solution that will help the State Machine handle and recover from State exception errors. Which of the following actions should the developer do?
A: Use Catch and Retry fields in the state machine definition.

Q: A transcoding media service is being developed in AWS. Photos uploaded to Amazon S3 will trigger Step Functions to coordinate a series of processes that will perform image analysis tasks. The final output should contain the input plus the result of the final state to conform to the application’s logic flow.
A: Declare a ResultPath field filter on the Amazon States Language specification.

Q: An engineer is developing a cloud-native application on AWS designed to handle a large volume of data. Within this application, a state machine in AWS Step Functions calls upon multiple AWS Lambda functions for processing. Occasionally, one of these Lambda functions does not complete its task within the allocated time, particularly during spikes in data volume, leading to timeout errors. The engineer must set up the system so that any invocation of the Lambda function that fails due to these timeouts is automatically attempted again. Which approach would ensure the system meets this requirement?
A: Integrate a Retry field into the AWS Step Functions state machine's configuration. Specify the maximum attempts for retries and the target timeout error type as the retry trigger.

Q: A Lamba function has multiple sub-functions that are chained together to process large data synchronously. When invoked, the function tends to exceed its maximum timeout limit. This has prompted the developer to break the Lambda function into manageable coordinated states using Step Functions, enabling each sub-function to run in separate processes. Which of the following type of states should the developer use to run processes?
A: Task State
(In the given scenario, the application logic inside the Lambda function process data synchronously. In this case, Task State should be used.)


# VPC
* A developer is launching a Lambda function that requires access to a MySQL RDS instance that is in a private subnet. Which of the following is the MOST secure way to achieve this? --> Configure the Lambda function to connect to your VPC.


# X-Ray
* EC2: Using a user data script to run the daemon automatically
* Elastic Beanstalk: include the xray-daemon.config configuration file in the .ebextensions directory of your source code
* ECS: Create a Docker image that runs the X-Ray daemon

Annotations and metadata are aggregated at the trace level and can be added to any segment or subsegment.
Annotations are simple key-value pairs that are indexed for use with filter expressions. Use annotations to record data that you want to use to group traces in the console, or when calling the GetTraceSummaries API.
With Metadata you can not group traces. Metadata are key-value pairs with values of any type, including objects and lists, but that is not indexed. You commonly use metadata to record data that you want to store in the trace but don’t need to search for traces.
Sampling just ensures efficient tracing and provides a representative sample of the requests that your application serves. Additionally, sampling will help you save money by reducing the amount of traces for high-volume and unimportant requests.
Subsegment is used to provide more granular timing information and details about downstream calls that your application made to fulfill the original request. It cannot group traces from recorded data.

Q: A developer has instrumented an application using the X-Ray SDK to collect all data about the requests that an application serves. There is a new requirement to develop a custom debug tool which will enable them to view the full traces of their application without using the X-Ray console.
A: Use the GetTraceSummaries API to get the list of trace IDs of the application and then retrieve the list of traces using BatchGetTraces API.

Q: An application executes GET operations to various AWS services. The development team is using AWS X-Ray to trace all the calls made to AWS. As one of the developers, you are responsible for maintaining a particular block of code on the application. To save time, you only want to record data associated with the code to group the traces in the AWS console. Which of the following X-Ray features should you use?
A: Annotations

Q: An enterprise deployed a new serverless application on AWS designed for real-time data processing. This application uses AWS Lambda functions to fetch data from Amazon S3, process it, and then store the results in a DynamoDB database. After deployment, users began experiencing intermittent slow responses. The development team suspects the issue might be related to the interaction between the Lambda function and Amazon S3, as this is where the data fetching occurs. Which approach should a developer take to identify the root cause of the latency issue while following operational excellence principles?
A: Enhance the Lambda function with AWS X-Ray SDK integration, including the setup for HTTP/HTTPS requests and SDK client handlers. Redeploy the function with the modifications. Activate X-Ray tracing and, upon collecting sufficient operational data, utilize the X-Ray service maps to analyze average response timings and pinpoint probable sources of latency.


# Moving into the cloud
## LAMP
Q: A company uses a Linux, Apache, MySQL, and PHP (LAMP) web service stack to host an on-premises application for its car rental business. The manager wants to move its operation into the Cloud using Amazon Web Services. Which combination of services could be used to run the application that will require the least amount of configuration?
A: Amazon EC2 and Amazon Aurora

## S3
Q: A university is gradually migrating some of its physical documents to the AWS cloud. They will start by moving their alumnus’ historical records to Amazon S3. The storage solution should provide a secure and durable object storage with the lowest cost. Which of the following types of S3 storage should you recommend?
A: Amazon S3 Glacier Deep Archive
