# Project 1. microservice:

**Resource:** <a href="https://github.com/saha-rajdeep/serverless-lab">LINK</a>

**Services:**

1. DynamoDB - lambda-apigateway | Primary key – id (string)
2. Lambda function - lambda-function-over-https-create
3. IAM Role(For Lambda function) -  lambda-apigateway-role
4. API Gateway - DynamoDBManager | POST

REST API - DynamoDBOperations
Resource - DynamoDBManager | Resource Path - /dynamodbmanager
Method - POST



## DynamoDB - [lambda-apigateway]
 ![image](https://user-images.githubusercontent.com/124598875/233772047-1a1585e1-0fa1-499f-80c1-45117640e00e.png)


## Role [lambda-apigateway-role]

**Permissions** 

- DynamoDB
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

## API Gateway

- REST API - DynamoDBOperations
- Resource - DynamoDBManager | Resource Path - /dynamodbmanager
- Method - POST

### GET Method - https://repost.aws/knowledge-center/pass-api-gateway-rest-api-parameters

```
import boto3
import json

def lambda_handler(event, context):
    '''
        - tableName: required for operations that interact with DynamoDB
    '''
    
    client = boto3.client('dynamodb')
    
    # search_id=event["queryStringParameters"]['key']
    search_id=event['key']
    try:
        response=client.get_item(
            TableName='lambda-apigateway',
            Key={
                'id':{'S':search_id}
            }
        )
        
        data=response['Item']

        statusCode=response['ResponseMetadata']['HTTPStatusCode']
    
        return {
            'statusCode': statusCode,
            'items': json.dumps(data),
        }
    except KeyError as e:
        #Send to logs
        # print(e)
        return {
            'statusCode': 200,
            'items': json.dumps({}),
        }
```




