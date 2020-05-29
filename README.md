# Welcome to your CDK TypeScript project!

You should explore the contents of this project. It demonstrates a CDK app with an instance of a stack (`CdkWorkshopStack`)
which contains an Amazon SQS queue that is subscribed to an Amazon SNS topic.

The `cdk.json` file tells the CDK Toolkit how to execute your app.

## Useful commands

 * `npm run build`   compile typescript to js
 * `npm run watch`   watch for changes and compile
 * `npm run test`    perform the jest unit tests
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk synth`       emits the synthesized CloudFormation template

## cdk synth
The CDK CLI requires you to be in the same directory as your cdk.json file. If you have changed directories in your terminal, please navigate back now.

### Output 

Resources:
  CdkWorkshopQueue50D9D426:
    Type: AWS::SQS::Queue
    Properties:
      VisibilityTimeout: 300
    Metadata:
      aws:cdk:path: CdkWorkshopStack/CdkWorkshopQueue/Resource
  CdkWorkshopQueuePolicyAF2494A5:
    Type: AWS::SQS::QueuePolicy
    Properties:
      PolicyDocument:
        Statement:
          - Action: sqs:SendMessage
            Condition:
              ArnEquals:
                aws:SourceArn:
                  Ref: CdkWorkshopTopicD368A42F
            Effect: Allow
            Principal:
              Service: sns.amazonaws.com
            Resource:
              Fn::GetAtt:
                - CdkWorkshopQueue50D9D426
                - Arn
        Version: "2012-10-17"
      Queues:
        - Ref: CdkWorkshopQueue50D9D426
    Metadata:
      aws:cdk:path: CdkWorkshopStack/CdkWorkshopQueue/Policy/Resource
  CdkWorkshopQueueCdkWorkshopStackCdkWorkshopTopicD7BE96438B5AD106:
    Type: AWS::SNS::Subscription
    Properties:
      Protocol: sqs
      TopicArn:
        Ref: CdkWorkshopTopicD368A42F
      Endpoint:
        Fn::GetAtt:
          - CdkWorkshopQueue50D9D426
          - Arn
    Metadata:
      aws:cdk:path: CdkWorkshopStack/CdkWorkshopQueue/CdkWorkshopStackCdkWorkshopTopicD7BE9643/Resource
  CdkWorkshopTopicD368A42F:
    Type: AWS::SNS::Topic
    Metadata:
      aws:cdk:path: CdkWorkshopStack/CdkWorkshopTopic/Resource
  CDKMetadata:
    Type: AWS::CDK::Metadata
    Properties:
      Modules: aws-cdk=1.42.0,@aws-cdk/aws-cloudwatch=1.42.0,@aws-cdk/aws-iam=1.42.0,@aws-cdk/aws-kms=1.42.0,@aws-cdk/aws-sns=1.42.0,@aws-cdk/aws-sns-subscriptions=1.42.0,@aws-cdk/aws-sqs=1.42.0,@aws-cdk/cdk-assets-schema=1.42.0,@aws-cdk/cloud-assembly-schema=1.42.0,@aws-cdk/core=1.42.0,@aws-cdk/cx-api=1.42.0,@aws-cdk/region-info=1.42.0,jsii-runtime=node.js/v10.15.0    Condition: CDKMetadataAvailable
Conditions:
  CDKMetadataAvailable:
    Fn::Or:
      - Fn::Or:
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-east-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-northeast-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-northeast-2
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-south-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-southeast-1
          - Fn::Equals:
              - Ref: AWS::Region
              - ap-southeast-2
          - Fn::Equals:
              - Ref: AWS::Region
              - ca-central-1
          - Fn::Equals:
              - Ref: AWS::Region
              - cn-north-1
          - Fn::Equals:
              - Ref: AWS::Region
              - cn-northwest-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-central-1
      - Fn::Or:
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-north-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-west-1
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-west-2
          - Fn::Equals:
              - Ref: AWS::Region
              - eu-west-3
          - Fn::Equals:
              - Ref: AWS::Region
              - me-south-1
          - Fn::Equals:
              - Ref: AWS::Region
              - sa-east-1
          - Fn::Equals:
              - Ref: AWS::Region
              - us-east-1
          - Fn::Equals:
              - Ref: AWS::Region
              - us-east-2
          - Fn::Equals:
              - Ref: AWS::Region
              - us-west-1
          - Fn::Equals:
              - Ref: AWS::Region
              - us-west-2

## Bootstrapping an environment
The first time you deploy an AWS CDK app into an environment (account/region), you’ll need to install a “bootstrap stack”. This stack includes resources that are needed for the toolkit’s operation. For example, the stack includes an S3 bucket that is used to store templates and assets during the deployment process.

You can use the cdk bootstrap command to install the bootstrap stack into an environment:

`cdk bootstrap`

# Let's Deploy
Use cdk deploy to deploy a CDK app:

`cdk deploy`

You should see a warning.


# Delete the sample code from your stack
The project created by cdk init sample-app includes an SQS queue, and an SNS topic. We’re not going to use them in our project, so remove them from your the CdkWorkshopStack constructor.

Open lib/cdk-workshop-stack.ts and clean it up. 

Eventually it should look like this:

```
        import * as cdk from '@aws-cdk/core';

        export class CdkWorkshopStack extends cdk.Stack {
        constructor(scope: cdk.App, id: string, props?: cdk.StackProps) {
            super(scope, id, props);

                // nothing here!
            }
        }
```

`cdk diff` 
Now that we modified our stack’s contents, we can ask the toolkit to show us the difference between our CDK app and what’s currently deployed. 

*This is a safe way to check what will happen once we run cdk deploy and is always good practice:
`cdk diff`


As expected, all of our resources are going to be brutally destroyed after changing this class.


# Install the AWS Lambda Construct Library
The AWS CDK is shipped with an extensive library of constructs called the AWS Construct Library. The construct library is divided into modules, one for each AWS service. For example, if you want to define an AWS Lambda function, we will need to use the AWS Lambda construct library.

To discover and learn about AWS constructs, you can browse the AWS Construct Library reference.
`npm install @aws-cdk/aws-lambda`


# Install the API Gateway construct library
`npm install @aws-cdk/aws-apigateway`

Going back to lib/cdk-workshop-stack.ts, define an API endpoint and associate it with our Lambda function:

```
    // defines an API Gateway REST API resource backed by our "hello" function.
    new apigw.LambdaRestApi(this, 'Endpoint', {
      handler: hello
    });
```

