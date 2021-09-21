
# AWS Best Practices

Best Practices are ideas and tips that go along way in ensuring that you build a well-optimized serverless applications.

## AWS Lambda Best Practices

![AWS LAmbda Best Practices](https://github.com/jsanon01/best-practices/blob/main/images/lambda_best_practices.png)

- Grant only the necessary IAM Permissions to Lambda functions
- Always use the Programming Best Prcatices
- Make use of error handling mechanisms
- Make use of the framework like SAM or Serverless Framework
- Make use of GIT, CI/CD tools or DevOps tools that are available that are available and follow a structred process.
- One function, one task (one function should perform one task and nothing more)
- Keep the Lambda handler clean
- Keep an eye on Lambda logs
- Keep declarations & instantiations outside the Lambda handler.
- Use VPC only if necessary

![Services invoking Lambda Functions](https://github.com/jsanon01/best-practices/blob/main/images/services_invoking_lambda.png)

## API Gateway Best Practices

Used across businesses and organizations, from enterprises to startups, API Gateway makes it easy to define, secure, deploy, share, and operate APIs at any scale. 

- Use Custom Domain in the Production environment (recommended)
- Use DevOps tools for CI/CD automation
- Deploy APIs closer to customers
- Enable logging options (to track down failures and their causes)
- Enable AWS Clouwatch logs for the API. 

### RESTFul API Design

![API Design](https://github.com/jsanon01/best-practices/blob/main/images/api_flow.png)

### THE API LIFECYCLE

![API Lifecycle](https://github.com/jsanon01/best-practices/blob/main/images/api_lifecycle.png)

## AWS DynamoDB Best Practices

![DynamoDB](https://github.com/jsanon01/best-practices/blob/main/images/dynamodb.png)

### Table Design

- First and the most important is the Table Design

- DynamoDB tables provide the best performance when designed for Uniform Data Access.

 - DynamoDB divides the provisioned throughput equally between all the table partitions in order to achieve maximum utilization of capacity units.

-  Read and Write loads must be uniform across partitions, or partition keys.
    - we don't quite get the optimal performance for the price we pay, if we don't pick the table partition keys thoughtfully.

- Non-uniform data access pattern results in "hot partitions" meaning idle provisioned capacity that is not only heavily accessed but also waste  while paying for them.

### Caching services

![DAX](https://github.com/jsanon01/best-practices/blob/main/images/dax.png)
- You can use caching services like DAX to cache the hot data (not cheap)
    - However, we should still focus on having the tables designed for uniform access because DAX is expensive.

### Provision Throughtput

when changing the provisioned throughput for any DynamoDB table, i.e. scaling up or down:

- Avoid temporary substantial scaling up of the provisioned capacity

- It's a good idea to keep the attribute names shorter (This helps reduce the item size and the costs as well).
- We should consider compressing the non-key attributes if we must store large values in our items.
    - GZIP
    - Store large items in S3, and then only the pointers to those items can be stored in DynamoDB.
- Avoid Scan operations (as far as possible)
    - Important to note that that filters always get applied after the query and scan operations are complete.
     - The applicable RCUs will be calculated before applying the filters.

- Always opt for eventual consistency (Unless your application demands strongly consistent reads)
    - That will save you lots of money
![DynamoDB Throughput](https://github.com/jsanon01/best-practices/blob/main/images/db_throughput_1.png)

![DynamoDB Throughput 2](https://github.com/jsanon01/best-practices/blob/main/images/db_throughput_2.png)

##  Local Secondary Indexes (LSIs)   

- Use Local Secondary Indexes (LSIs) sparingly.
    - LSIs share the same partition, same physical partition, same space that is used by the table.
    - Adding more indexes will reduce the available size for the table
        - Use them as per your application's needs.
    - Project fewer attributes on to secondary indexes
        - Project up to maximum of 20 attributes per index
        - choose them carefully
    - Specify ALL if you want your queries to return the entire table item
    - Use sort to sort the table by a different sort key

![LSI vs GSI](https://github.com/jsanon01/best-practices/blob/main/images/lsi_gsi.png)

##  Global Secondary Indexes (GSIs)

- Choose the keys for global secondary indexes resulting in uniform worlkoads
    - Reading an entire table item to retrieve just a few attributes may result in consumption of a lot of read capacity.
    - create a global secondary index by projecting just the needed attributes
        - This creates a much smaller index and can result in considerable amount of savings.
    - Use GSIs to creatr an eventually consistent read replica
    - we can control the throughput of this index separately
        - useful if you have two applications that read from this table
        - You can control the table throughput individually for each application.
            - In this case, one app could use the table and the other could use the global secondary index to perform reads.

## The difference between GSIs vs LSIs

![LSI vs GSI](https://github.com/jsanon01/best-practices/blob/main/images/difference.png)


## AWS Step Functions Best Practices

Step Functions is a serverless orchestration service that lets you combine AWS Lambda functions and other AWS services to build business-critical applications. Through Step Functions' graphical console, you see your application’s workflow as a series of event-driven steps.

Step Functions is based on state machines and tasks. A state machine is a workflow. A task is a state in a workflow that represents a single unit of work that another AWS service performs.

![Step Function](https://github.com/jsanon01/best-practices/blob/main/images/step_function.png)

Each step in a workflow is a state.

- Use timeouts to avoid stuck executions
- Use Amazon S3 ARNs instead of passing large payloads
- Avoid Reaching the history quota
- Avoid latency when polling for activity tasks
- Handle Lambda service exceptions
- Choosing Standard or Express Workflows
- Amazon CloudWatch Logs resource policy size restrictions


## AWS Serverless Architecture

The most common "Multi-Tier Architecture" is a "3-Tier Architecture."

![Multi-Tier](https://github.com/jsanon01/best-practices/blob/main/images/multi_tier.png)

### Frontend or Presetation Tier

![Presentation Tier](https://github.com/jsanon01/best-practices/blob/main/images/presentation_tier.png)
- Frontend interacts with backend through Logic Tier
- Frontend uses API Gateway endpoints to call Lambda functions, which in turn interacts with data stores available in Data Tier.

### Logic or Application Tier

![Application Tier](https://github.com/jsanon01/best-practices/blob/main/images/application.png)

Logic Tier includes:
- AWS Lambda
- AWS API Gateway

#### => Also known as "Application Tier," Logic Tier is the core of Serverless Architecture.



###  Database or Data Tier

![Data Tier](https://github.com/jsanon01/best-practices/blob/main/images/data_tier.png)

## Serverless Security Best Practices

We can not talk about "serverless" without mentioning "Lambda" functions and "API Gateway" services. We know that AWS Lambda uses a "decoupled permission model."

![AWS Lambda Serverless Architecture](https://github.com/jsanon01/best-practices/blob/main/images/lambda_serverless.png)

Speaking of Permisson Model, we have 2 types:

### Invoke Permissions

This type of permission only requires caller to have access to invoke Lambda functions and no more access is needed.

### Execution Permissions

 They are used by AWS Lambda to execute the function codes.

 - Give each Lambda function its own execution role
 - Avoid using the same role across multiple Lamda functions
 - Avoid setting wildcard permissions
 - Avoid giving full access to Lambda functions
 - Ensure appropriate access control for CI/CD
 - Choose only the required actions in the IAM policy (as restrictive as possible)
     - Always make use of "ENVIRONMENT VARIABLES" in Lambda functions
     - Never hardcoded these "ENVIRONMENT VARIABLES" in Lambda functions
 - Make use of KMS to encrypy sensitive data (either at rest or in transit)
 - Never log decrypted values to console or any persistent storage
 
 ### - For Lambda functions running inside a VPC

 - Use least privileges Security Groups
 - Lambda-specific subnets & Network configuration

 ### - Controlling API Gateway Access

 - API keys and usage plans
 - API Gateway Policies
 - AWS Lambda authorizers
 - AWS Cognito User Pool Authorizers
 - Client Certificates
 - CORS Headers
 - IAM policies
 - Federated Identity Access using Cognito

## Serverless Static Website hosted on S3 bucket
You can use Amazon S3 to host a static website, which includes individual webpages along with static content. Such webpages might also contain client-side scripts.

![Static Website](https://github.com/jsanon01/best-practices/blob/main/images/static_website.png)

#### When you configure a bucket as a static website, you must enable:

- Static website hosting, 
- Configure an index document, 
- Set permissions
- Use the Amazon S3 console, 
    - REST API, 
    - the AWS SDKs, 
    - the AWS CLI, 
    - or AWS CloudFormation.

[Reference: AWS Enabling website hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EnableWebsiteHosting.html)

## Serverless Mobile Backend

Unlike serverless static websites, serverless applications will now act as mobile frontend. Therefore, Mobile App will be the Presentation Tier.

It will connect to the Logic or Application Tier using REST API running on API Gateway which triggers the Lambda functions.


## Serverless Microservices Architecture Pattern

![Microservices Architecture Pattern](https://github.com/jsanon01/best-practices/blob/main/images/micro_services.png)

Microservices Architecture is a suite of small services  that are independently deployable.

### Microservices vs Monoliths

- Microservice application typically runs in a separate process,
- Each microservice will talk to other microservices to perform work: 
    - directly via REST API, 
    - over a message bus, 
    - or a number of other protocols, depending on implementation.

##### whereas
- A monolithic architecture-based application consists of fewer and larger parts, perhaps running as a single process
- Due to the interdependence and complexity of Technology, one architecture doesn’t fit all situations.

#### => Simply said, Microservices use "loosely and coupled services" to interact with each other whereas Monolyths run in a constricted environment as a "straight jacket."

### Microservices Advantages

The following characteristics describe the fundamental building blocks of a microservices architecture which are necessary to meet the design goals and obtain its benefits. Each microservice should be: 

#### - Easier to manage and extend

    - Breaking down large complex system into independent, decoupled services

#### - Independently deployable 

    - e.g., the front-end service can be deployed independently of the authentication service
    
#### - Highly observable 

    - through logging, monitoring, tracing, etc, one can determine what the service is doing

#### - Loosely coupled 

    - the service can perform its work without being overly dependent on how any other service is defined or implemented

#### - Decentralized 

    - spread across many different “systems,” potentially across many different geographies
    
#### - Highly testable 
    - developed from the beginning to be testable through automated test frameworks with high coverage rates
#### - Highly maintainable 
    - well-structured, well-commented, easy to understand, easy to change, easy and fast to build, etc (small size helps with all of this)
#### - Fungible
    - Easily replaced or reimplemented as requirements dictate
#### - Focused and specialized 
    - similar to the Unix philosophy of “do one thing well”
#### - Contractual 
    - having regimented, well-defined interfaces with deliberate life cycles

### 7 Microservices Benefits

1. Asymmetric Service Scaling (well-suited for massive scalability)
2. Zero Downtime (independently deployable)
3. Intelligent Deployment (Unwanted behavior can be detected before the old version is completely replaced)
4. Innovation Through Polyglot Programming (If one experiment doesn’t work, no problem--microservices are “fungible” so just rewrite the microservice)
5. Refactoring, Rewriting, and Decomposing (a legacy, monolithic application can be decomposed into microservices over time)
6. Separation of Logic and Responsibilities (A “loosely coupled,” “independently deployable,” “specialized,” and “contractual” microservice allows teams to focus their efforts on the business logic in their area of expertise or access)
7. Error Handling and Resiliency Design Patterns (The circuit breaker pattern is often used to prevent systemic failure due to a root cause in an isolated component)

# Serverless Best Practices


# AWS Serverless CLI Commands

- [Create VPC](https://jsanon01.github.io/serverless/index.html#vpc_command)
- [Create Subnets](https://jsanon01.github.io/serverless/index.html#subnet_command)
 
- [Create Internet Gateway](https://jsanon01.github.io/serverless/index.html#igw_command)

- [Create NAT Gateway](https://jsanon01.github.io/serverless/index.html#nat_gateway_command)

- [Create Elastic IP](https://jsanon01.github.io/serverless/index.html#eip_command)


- Create NAT Gateway

- Create Route Tables

- Adding Internet Routes

-  Route Table Associations
  
  <!--     Create Security Groups
    Adding Rules to Security Group
    Create Key-Pairs
    Create EC2 instance
    SSH to newly EC2
    Apache Installation
    Risk Management Overview
-->