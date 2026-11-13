
## Conditions

Note that `Conditions` in a declaration cannot be set to `true` or `false` manually. The Boolean value is obtained only from a result of intrinsic functions, such as `Fn::Equals`, `Fn::If`, `Fn::Not`, `Fn::And`, and `Fn::Or`!

For example, look at the following code:

```yaml
AWSTemplateFormatVersion: '2010-09-09'

Parameters:
  Env:
    Default: dev
    Description: Define the environment (dev, test or prod)
    Type: String
    AllowedValues: [dev, test, prod]

Conditions:
  IsProd: !Equals [!Ref Env, 'prod']

Resources:
  Bucket:
    Type: "AWS::S3::Bucket"
    Condition: IsProd
```

In this case, we have to manually provide the `Env` parameter, which must equal `prod`; otherwise, the resource will *not* be created.

`Conditions` work along with specific Boolean intrinsic functions, but those functions also work outside of the conditional block. We will cover these in greater detail when we start the development of our template.

## Transform

The transform block is declared when we want to run CloudFormation-specific macros in our template. This is a deep topic that we will cover in the last part of this book, *Extending CloudFormation*.

## Resources

This is the main and only required block in the `template` file. The resources section provides, as is indicated by its name, resources that we want to provision.

The resources that we create can vary. Some think that any AWS resource can be created by CloudFormation, but there are multiple services (such as AWS Organizations, Control Tower, and many others) that do not support CloudFormation.

Before starting to create the stack, you must first review the resource and property reference in the CloudFormation documentation to make sure that the services you want to use are supported by CloudFormation. For more information, go to [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html).

Resources have multiple attributes, listed as follows:

*   `Type`: The type of the resource (instance, subnet, DynamoDB table, lambda function, and so on).
*   `Properties`: Configuration of the resource.
*   `DependsOn`: Used for adding indirect dependencies.
*   `CreationPolicy`: Used for notifying CloudFormation to wait for the signal of completion (we will cover this when we create AutoScaling groups).
*   `DeletionPolicy`: Used to instruct CloudFormation what to do with the resource when we delete it from the stack (or with the stack)—for example, from the *state*. Used for important resources, such as storage backends.
*   `UpdatePolicy`: Used to instruct CloudFormation how to handle indirect updates of AutoScaling groups, ElastiCache replication groups, and Lambda function aliases.
*   `UpdateReplacePolicy`: Similar to `DeletionPolicy`, `UpdateReplacePolicy` is used to manage leftover resources when they have to be replaced with a new one. It works within the same logical resource and default policy as `Delete`.

## Outputs

Outputs are the values we *export* from the stack after its creation.

On its own, the outputs do not bring many benefits, but in the previous chapter, we used outputs to automatically get the ARN of the IAM role we created.

Outputs can retrieve the physical name or ID of the resource or its attributes.

For example, we can retrieve the API key and secret for an IAM user when we create it and supply our end users with it:

```yaml
Resources:
  Bob:
    Type: AWS::IAM::User
    Properties:
      UserName: Bob
  BobApiKey:
    Type: AWS::IAM::AccessKey
    Properties:
      UserName: !Ref Bob

Outputs:
  BobKey:
    Value: !Ref BobApiKey
  BobSecret:
    Value: !GetAtt BobApiKey.SecretAccessKey
```

As you can see, we retrieved Bob's API key and secret and exposed it to the outputs.

### Important Note

Note that outputs are accessible to anyone who is authorized to read the stacks' description, so exposing security credentials is a risky operation. In case you need to use outputs for that, make sure that only authorized users will have access to your stacks, even if they are read-only.

Outputs also have a powerful feature called exports. Exports allow the template developer to refer to the existing resources created in another stack. Using exports is a best practice for large workloads where you often have a shared stack (resources used by many applications) and application-specific stacks.

Now, let's start developing our template. Before proceeding, make sure that you understand all the preceding sections because we will rely on them heavily in the coming sections.

# Creating reusable templates

We already know that we are going to deploy a three-tier application, but we're going to have more than three stacks.

The first stack we will create is our core stack.

This stack will consist of the following:

*   Network (VPC, subnets, and so on)
*   IAM (roles and users)

We will have two environments for our application: test and production. These environments will differ in terms of size, the amount of resources, and security settings.

The code of our templates is going to be huge, so you will only see blocks that are specific for the topic; the entire source code can be found in Packt's repository.

Before we start, let's think about how we are going to organize the template and its parameters. Since we are going to reuse the same template for two different stacks (production and test), we will need to separate the network ranges and use different naming conventions.

In terms of network, our stack will have the following:

*   1 VPC
*   3 public subnets
*   3 WebTier subnets
*   3 middleware subnets
*   3 database subnets
*   1 internet gateway
*   1 NAT gateway
*   1 public route table
*   1 private route table
*   2 IAM roles (for administrators and developers)

These resources will have tags such as `Name` and `Env`.

In this case, we want to parametrize the properties for our resources, such as CIDR ranges and tags, so we will have the following parameters:

*   VPC CIDR range
*   Public subnet 1 CIDR range
*   Public subnet 2 CIDR range
*   Public subnet 3 CIDR range
*   WebTier subnet 1 CIDR range
*   WebTier subnet 2 CIDR range
*   WebTier subnet 3 CIDR range
*   Middleware subnet 1 CIDR range
*   Middleware subnet 2 CIDR range
*   Middleware subnet 3 CIDR range
*   Database subnet 1 CIDR range
*   Database subnet 2 CIDR range
*   Database subnet 3 CIDR range
*   Environment

All these parameters will have a type `String`. Let's write our `Parameters` section:

```yaml
Parameters:
  VpcCidr:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidr1:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidr2:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidr3:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  # And so on...
  DatabaseSubnetCidr1:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  DatabaseSubnetCidr2:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  DatabaseSubnetCidr3:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  Environment:
    Type: String
    AllowedValues: ['prod', 'test']
```

This is it for our core stack. Now, in our resources, we will need to use the intrinsic function `Fn::Ref` to use the value of the parameter in the resource property. We will also use another intrinsic function, `Fn::Join`, to concatenate a nice string name for our VPC:

```yaml
Resources:
  Vpc:
    Type: "AWS::EC2::VPC"
    Properties:
      CidrBlock: !Ref 'VpcCidr'
      EnableDnsHostnames: True
      EnableDnsSupport: True
      InstanceTenancy: Default
      Tags:
        - Key: 'Name'
```
