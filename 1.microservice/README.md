# Project 1. microservice:

**Resource:** <a href="https://github.com/saha-rajdeep/serverless-lab">LINK</a>

**Services:**
1. API Gateway - DynamoDBManager | POST
2. Lambda function - lambda-function-over-https-create
3. IAM Role(For Lambda function) -  lambda-apigateway-role
4. DynamoDB - lambda-apigateway | Primary key – id (string)

## Role [lambda-apigateway-role]

**Permissions** 
– DynamoDB
- CloudWatch Logs.

**Policy**
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "dynamodb:PutItem",
                "dynamodb:DeleteItem",
                "dynamodb:GetItem",
                "dynamodb:Scan",
                "dynamodb:Query",
                "dynamodb:UpdateTable"
            ],
            "Resource": "arn:aws:dynamodb:us-east-1:184905145920:table/lambda-apigateway"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:CreateLogGroup",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```




