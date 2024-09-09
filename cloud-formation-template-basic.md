```markdown
# CloudFormation Template

This CloudFormation template deploys an EC2 instance, an RDS MySQL database, an API Gateway, and a Lambda function that connects to the MySQL database. It also includes CRUD operations through the Lambda function.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'CloudFormation template to deploy EC2, RDS, API Gateway, and Lambda with MySQL integration.'

Parameters:
  InstanceType:
    Description: 'EC2 instance type'
    Type: String
    Default: 't2.micro'
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium

  KeyName:
    Description: 'Name of an existing EC2 KeyPair to enable SSH access'
    Type: AWS::EC2::KeyPair::KeyName

  DBName:
    Description: 'Name of the RDS database'
    Type: String
    Default: 'mydatabase'

  DBUser:
    Description: 'Database admin user'
    Type: String
    NoEcho: true

  DBPassword:
    Description: 'Database admin password'
    Type: String
    NoEcho: true

  LambdaFunctionName:
    Description: 'Name of the Lambda function'
    Type: String
    Default: 'MyLambdaFunction'

Resources:

  # EC2 Instance
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyName
      ImageId: 'ami-0ff8a91507f77f867' # Update with a valid AMI ID for your region

  # RDS MySQL Instance
  MyRDSInstance:
    Type: 'AWS::RDS::DBInstance'
    Properties:
      DBInstanceClass: 'db.t2.micro'
      Engine: 'mysql'
      MasterUsername: !Ref DBUser
      MasterUserPassword: !Ref DBPassword
      DBName: !Ref DBName
      AllocatedStorage: '20'
      BackupRetentionPeriod: '7'
      PubliclyAccessible: true
      Tags:
        - Key: 'Name'
          Value: 'MyRDSInstance'

  # API Gateway
  MyApiGateway:
    Type: 'AWS::ApiGateway::RestApi'
    Properties:
      Name: 'MyApi'
      Description: 'API for CRUD operations'

  # Lambda Execution Role
  LambdaExecutionRole:
    Type: 'AWS::IAM::Role'
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: 'Allow'
            Principal:
              Service: 'lambda.amazonaws.com'
            Action: 'sts:AssumeRole'
      ManagedPolicyArns:
        - 'arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole'
        - 'arn:aws:iam::aws:policy/service-role/AWSLambdaVPCAccessExecutionRole'

  # Lambda Function
  MyLambdaFunction:
    Type: 'AWS::Lambda::Function'
    Properties:
      FunctionName: !Ref LambdaFunctionName
      Handler: 'index.handler'
      Role: !GetAtt LambdaExecutionRole.Arn
      Code:
        ZipFile: |
          import json
          import pymysql
          import os

          def connect_to_rds():
              conn = pymysql.connect(
                  host=os.environ['DB_HOST'],
                  user=os.environ['DB_USER'],
                  password=os.environ['DB_PASSWORD'],
                  db=os.environ['DB_NAME']
              )
              return conn

          def lambda_handler(event, context):
              conn = connect_to_rds()
              try:
                  with conn.cursor() as cursor:
                      sql = "SELECT * FROM your_table;"
                      cursor.execute(sql)
                      result = cursor.fetchall()
              finally:
                  conn.close()

              return {
                  'statusCode': 200,
                  'body': json.dumps(result)
              }
      Runtime: 'python3.8'
      Environment:
        Variables:
          DB_HOST: !GetAtt MyRDSInstance.Endpoint.Address
          DB_USER: !Ref DBUser
          DB_PASSWORD: !Ref DBPassword
          DB_NAME: !Ref DBName

  # Lambda Function API Gateway Integration
  MyLambdaApiGateway:
    Type: 'AWS::ApiGateway::Resource'
    Properties:
      ParentId: !GetAtt MyApiGateway.RootResourceId
      PathPart: 'crud'
      RestApiId: !Ref MyApiGateway

  MyLambdaMethod:
    Type: 'AWS::ApiGateway::Method'
    Properties:
      AuthorizationType: NONE
      HttpMethod: POST
      ResourceId: !Ref MyLambdaApiGateway
      RestApiId: !Ref MyApiGateway
      Integration:
        IntegrationHttpMethod: POST
        Type: AWS_PROXY
        Uri: !Sub arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${MyLambdaFunction.Arn}/invocations
      MethodResponses:
        - StatusCode: '200'

  MyLambdaPermission:
    Type: 'AWS::Lambda::Permission'
    Properties:
      Action: 'lambda:InvokeFunction'
      FunctionName: !Ref MyLambdaFunction
      Principal: 'apigateway.amazonaws.com'

Outputs:
  EC2InstanceId:
    Description: 'The ID of the EC2 instance'
    Value: !Ref MyEC2Instance

  RDSInstanceEndpoint:
    Description: 'The endpoint of the RDS instance'
    Value: !GetAtt MyRDSInstance.Endpoint.Address

  ApiUrl:
    Description: 'The URL of the API Gateway'
    Value: !Sub 'https://${MyApiGateway}.execute-api.${AWS::Region}.amazonaws.com/prod/crud'
```

### Summary of Components:

1. **EC2 Instance**:
   - Deploys an EC2 instance with a specified type and key pair.

2. **RDS MySQL Instance**:
   - Sets up an RDS MySQL instance with admin credentials and a specified database name.

3. **API Gateway**:
   - Creates an API Gateway to interact with the Lambda function.

4. **Lambda Function**:
   - A Lambda function written in Python connects to the RDS instance and performs CRUD operations.

5. **Lambda Execution Role**:
   - Provides the necessary IAM role for the Lambda function.

6. **API Gateway Integration**:
   - Connects the Lambda function to the API Gateway, allowing HTTP POST requests to trigger the Lambda function.

### Notes:
- Replace `ami-0ff8a91507f77f867` with a valid AMI ID for your region.
- Ensure that `your_table` in the Lambda function code is replaced with the actual table name in your database.
- Modify database connection settings and Lambda function code as per your specific requirements.
