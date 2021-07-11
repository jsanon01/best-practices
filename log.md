
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
- Frontend interacts with backend through Logic Tier
- Frontend uses API Gateway endpoints to call Lambda functions, which in turn interacts with data stores available in Data Tier.

### Logic or Application Tier

Logic Tier is also known as "Application Tier"
- AWS Lambda
- AWS API Gateway

#### ==> Logic Tier is the core of serverless serverless



###  Databse or Data Tier

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
This static websote could be a fully-fledged JS-based application liek Angular or React App.

## Serverless Mobile Backend
Unlike serverless static websites, serverless applications will now act as mobile frontend. Therefore, Monile App will be the Presentation Tier.

It will connect to thew Logic or Application Tier using REST API running on API Gateway which triggers the Lambda functions.


## Serverless Microservices Architecture Pattern
Microservices Architecture is a suite of small services  that are independently deployable.

### Microservices Advantages
- Breaking down large complex system into independent, decoupled services easier to manage and extend.

