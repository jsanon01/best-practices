
# AWS & Serverless Best Practices

Best Practices are ideas and tips that go along way in ensuring that you build a well-optimized serverless applications.

## AWS Lambda Best Practices
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

## API Gateway Best Practices
- Use Custom Domain in the Production environment (recommended)
- Use DevOps tools for CI/CD automation
- Deploy APIs closer to customers
- Enable logging options (to track down failures and their causes)
- Enable AWS Clouwatch logs for the API. 

## AWS DynamoDB Best Practices

## AWS Step Functions Best Practices

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

Speaking of Permisson Model, we have 2 types:

#### Invoke Permissions
This type of permission only requires caller to have access to invoke Lambda functions and no more access is needed.

#### Execution Permissions
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
 
 #### - For Lambda functions running inside a VPC
 - Use least privileges Security Groups
 - Lambda-specific subnets & Network configuration

 #### - Controlling API Gateway Access
 - API keys and usage plans
 - API Gateway Policies
 - AWS Lambda authorizers
 - AWS Cognito User Pool Authorizers
 - Client Certificates
 - CORS Headers
 - IAM policies
 - Federated Identity Access using Cognito

## Serverless Static Website hosted on S3 bucket
You can use Amazon S3 to host a static website, which includes individual webpages alond with static content. Such webpages might also contain client-side scripts.

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

## Serverless Mobile Backend
Unlike serverless static websites, serverless applications will now act as mobile frontend. Therefore, Monile App will be the Presentation Tier.

It will connect to thew Logic or Application Tier using REST API running on API Gateway which triggers the Lambda functions.


## Serverless Microservices Architecture Pattern
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