
# AWS & Serverless Best Practices


## AWS Serverless Architecture

The most common "Multi-Tier Architecture" is a "3-Tier Architecture."

[]{}
### Frontend or Presetation Tier
- Frontend interacts with backend through Logic Tier
- Frontend uses API Gateway endpoints to call Lambda functions, which in turn interacts with data stores available in Data Tier.

### Logic or Application Tier

Logic Tier is also known as "Application Tier"
- AWS Lambda
- AWS API Gateway

#### ==> Logic Tier is the core serverless



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