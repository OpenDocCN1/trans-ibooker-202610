
# Understanding AWS CloudFormation

## Introduction

As the demand for the AWS **cloud** grew, the usual routine and operational tasks became troublesome. The AWS cloud allowed any type of business to rapidly grow and solve all the business needs regarding compute power; however, the need to maintain a certain stack of resources was hard.

DevOps culture brought a set of methodologies and ways of working, and one of those is called **infrastructure as code**. This process is about treating your infrastructure—network, virtual machines, storages, databases, and so on—as a computer program.

AWS **CloudFormation** was developed to solve this kind of problem.

You will already have some working knowledge of CloudFormation, but before we dive deep into learning advanced template development and how to provision at scale, use CloudFormation with CI/CD pipelines, and extend its features, let's quickly refresh our memory and look again at what CloudFormation is and how we use it.

In this chapter, we will learn the following:

*   The internals of AWS CloudFormation
*   Creating and updating a CloudFormation stack
*   Managing permissions for CloudFormation
*   Detecting unmanaged changes in our stack

## Technical Requirements

The code used in this chapter can be found in the book's GitHub repository at `https://github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter1`.

Check out the following video to see the Code in Action:

[https://bit.ly/2WbU5Lh](https://bit.ly/2WbU5Lh)

## Understanding the Internals of AWS CloudFormation

AWS services consist of three parts:

*   API
*   Backend
*   Storage

We interact with AWS by making calls to its API services. If we want to create an EC2 instance, then we need to perform a call, `ec2:RunInstances`.

When we develop our template and create a stack, we invoke the `cloudformation:CreateStack` API method. AWS CloudFormation will receive the command along with the template, validate it, and start creating resources, making API calls to various AWS services, depending on what we have declared for it.

If the creation of any resource fails, then CloudFormation will roll back the changes and delete the resources that were created before the failure. But if there are no mistakes during the creation process, we will see our resources provisioned across the account.

If we want to make changes to our stack, then all we need to do is update the template file and invoke the `cloudformation:UpdateStack` API method. CloudFormation will then update only those resources that have been changed. If the update process fails, then CloudFormation will roll the changes back and return the stack to the previous, healthy, state.

Now that we have this covered, let's start creating our stack.

## Creating Your First Stack

I'm sure you've done this before.

We begin by developing our template first. This is going to be a simple S3 bucket. I'm going to use **YAML** template formatting, but you may use **JSON** formatting if you wish:

`MyBucket.yaml`

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: This is my first bucket

Resources:

  MyBucket:

    Type: AWS::S3::Bucket
```

Now we just need to create the stack with `awscli`:

```bash
$ aws cloudformation create-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml
```

After a while, we will see our bucket created if we go to the AWS console or run `aws s3 ls`.

Now let's add some public access to our bucket:

`MyBucket.yaml`

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: This is my first bucket

Resources:

  MyBucket:

    Type: AWS::S3::Bucket

    Properties:

      AccessControl: PublicRead
```

Let's run the `update` operation:

```bash
$ aws cloudformation update-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml
```

To clean up your workspace, simply delete your stack using the following command:

```bash
$ aws cloudformation delete-stack --stack-name mybucket
```

Let's now look at the CloudFormation IAM permissions.

## Understanding CloudFormation IAM Permissions

We already know that CloudFormation performs API calls when we create or update the stack. Now the question is, does CloudFormation have the same powers as a `root` user?

When you work with production-grade AWS accounts, you need to control access to your environment for both humans (yourself and your coworkers) and machines (build systems, AWS resources, and so on). That is why controlling access for CloudFormation is important.

By default, when the user runs stack creation, they invoke the API method `cloudformation:CreateStack`. CloudFormation will use that user's access to invoke other API methods during the stack creation.

This means that if our user has an IAM policy with an allowed action `ec2:*`, but attempts to create an RDS instance with CloudFormation, the stack will fail to create with an error, **User is unauthorized to perform this action**.

Let's try this. We will create an IAM role with `ec2:*`, assume that role, and try to create the same `mybucket` stack:

> **Important note**
>
> We already have an IAM user `Admin` in our AWS account and we will add that user as a principal.

`MyIamRole.yaml`

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: "This is a dummy role"

Resources:

  IamRole:

    Type: AWS::IAM::Role

    Properties:

      AssumeRolePolicyDocument:

        Version: 2012-10-17

        Statement:

          - Sid: AllowAssumeRole

            Effect: Allow

            Principal:

              AWS:

                - !Join
                    - ""
                    - - "arn:aws:iam::"
                      - !Ref "AWS::AccountId"
                      - ":user/Admin"

            Action: "sts:AssumeRole"

      ManagedPolicyArns:

        - "arn:aws:iam::aws:policy/AmazonEC2FullAccess"
        - "arn:aws:iam::aws:policy/AWSCloudformationFullAccess"

Outputs:

  IamRole:

    Value: !GetAtt IamRole.Arn
```

If we create this stack, assume that role, and try to create the previous `mybucket` stack, it will fail to create with an error. Let's take a look:

```bash
$ aws cloudformation create-stack \
    --stack-name iamrole \
    --capabilities CAPABILITY_IAM \
    --template-body file://IamRole.yaml

$ IAM_ROLE_ARN=$(aws cloudformation describe-stacks \
    --stack-name iamrole \
    --query "Stacks[0].Outputs[?OutputKey=='IamRole'].OutputValue" \
    --output text)

$ aws sts assume-role --role-arn $IAM_ROLE_ARN \
    --role-session-name tmp

# Here goes the output of the command. I will store the access credentials in the env vars

$ export AWS_ACCESS_KEY_ID=…
$ export AWS_SECRET_ACCESS_KEY=…
$ export AWS_SESSION_TOKEN=…

$ aws cloudformation create-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml
```

We will see the following error on the AWS console:

![Figure 1.1 – CloudFormation console – stack events](image/B10533_01_001.jpg)

Figure 1.1 – CloudFormation console – stack events

On the other hand, we cannot provide everyone with an `AdminAccess` policy, so we need to find a way to use CloudFormation with the necessary permissions while only letting CloudFormation use those permissions.

CloudFormation supports **service roles**. Service roles are the IAM roles that are used by different AWS services (such as EC2, ECS, Lambda, and so on). CloudFormation service roles are used by CloudFormation during stacks and **StackSets** operations—creation, update, and deletion:

1.  Let's create a specific role for CloudFormation:

    `CfnIamRole.yaml`

    ```yaml
    AWSTemplateFormatVersion: "2010-09-09"

    Description: "This is a CFN role"

    Resources:

      IamRole:
    ```
