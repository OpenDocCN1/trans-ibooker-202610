
# Creating StackSets using `awscli`

Creating StackSets using `awscli` introduces a few differences. One of them is that the creation of `StackSet` and stack instances is a different and independent operation. This means that we can easily use specific parameters per stack instance, which is helpful when we want to have slight changes in stack instances within a single `StackSet`.

By going through the following steps, we will be able to achieve multi-region deployment:

## 1. Create a StackSet

We begin by creating a `StackSet`:

```
aws cloudformation create-stack-set \
    --stack-set-name core \
    --template-body file://core.yaml \
    --capabilities CAPABILITY_NAMED_IAM
```

We will see `StackSetId` in response. If we go to the AWS console, we will see the active `StackSet` with no stack instances; however, the parameters will be set (with the default values, unless we specify otherwise).

## 2. Create the first stack instance

Now we create the first stack instance. We will still use our account, choose Ireland as the region, and override the parameters for `VpcCidr` and `Environment`:

```
aws cloudformation create-stack-instances \
    --stack-set-name core \
    --accounts ACCT_ID \
    --regions eu-west-1 \
    --parameter-overrides \
    ParameterKey=VpcCidr,ParameterValue=10.1.0.0/16 \
    ParameterKey=Environment,ParameterValue=test
```

**Response**

```
{
    "OperationId": "7643d973-1358-475d-b148-5e096f9a44b9"
}
```

## 3. Review the stack instance

The `awscli` will return `OperationId`, and we can review our stack instance in the AWS Console or using the command-line interface:

```
aws cloudformation list-stack-instances --profile personal --stack-set-name core
```

```
{
    "Summaries": [
        {
            "StackSetId": "core:ec0e8256-dcc9-4dd0-b074-aee9721ed949",
            "Region": "eu-west-1",
            "Account": "ACCT_ID",
            "StackId": "arn:aws:cloudformation:eu-west-1:ACCT_ID:stack/StackSet-core-f8b4d34b-abaf-4e97-b004-9309ea693803/124e9400-2bc6-11ea-8c11-0abd335268a4",
            "Status": "OUTDATED",
            "StatusReason": "User Initiated",
            "DriftStatus": "NOT_CHECKED"
        }
    ]
}
```

## 4. Create the production stack instance

We don't have to wait until this stack instance is created. Let's create the production stack instance:

```
aws cloudformation create-stack-instances \
    --stack-set-name core \
    --accounts ACCT_ID \
    --regions us-east-1 \
    --parameter-overrides \
    ParameterKey=VpcCidr,ParameterValue=10.0.0.0/16 \
    ParameterKey=Environment,ParameterValue=prod
```

Voila! Using `awscli`, we were able to work around the issue with inherited parameters from `StackSets`! Now let's check how `StackSets` work in a multi-account environment.

# Best practices for `StackSets`

Before we begin, we need to learn a bit about best practices for `StackSets`. This is crucial, because we don't want to make a monstrous zoo instead of a properly organized infrastructure, and we want to have efficiency and transparency for stack operations. The most important of these are listed as follows:

*   **Develop universal templates**: By default, `StackSet` deploys the same template and parameter set in all the regions and accounts. To allow this process to be as automated as possible, we must focus on the reusability of our template. Do not hardcode regions and availability zones; instead, use AWS SSM parameter stores in every account and region to ease the deployment of `StackSet`. Don't set names for global resources such as IAM roles, users, groups, policies, S3 bucket, and so on to avoid naming conflicts.

    Think big, start small. If you are asked to create a `StackSet` for an application that will serve tens of accounts and regions, it is wise to start with only a few of them. Get some hands-on experience, improve your template, and continue growing.

*   **Don't forget about deployment performance**: It will take a lot of time to perform rolling updates on `StackSet`. You can speed up this process if you set your maximum concurrent operations accordingly.

*   **Don't forget about validation and linting**: Even if `StackSet` will stop operation in case of failure and roll back, we don't want to slow down. Doing preflight checks using linting and validation will save us a lot of time.

Make sure you look up the best practices for `StackSets` in the AWS documentation (you will find the link in the *Further reading* section). Meanwhile, let's continue looking at how to deploy to multiple accounts.

# Deploying to multiple accounts

Now let's begin working with multiple accounts. For this section, I've created two additional accounts in AWS Organizations: testing and production.

### Important note

Since this book is not about **multi-account strategies** (**MAS**), we're not going to cover the creation of child accounts and organizational units in this book. If you need to learn about these, then you can start looking at AWS Organizations ([https://aws.amazon.com/organizations/](https://aws.amazon.com/organizations/)). If you don't use a multi-account strategy and don't plan to, then you may skip this section.

Since we will work in various accounts, we need to tweak our `StackSet` permissions template. We've separated it into two templates—one for administrator role and one for an execution role. We don't need to create an execution role in our main (or to use the terms of AWS, our payer) account, because no stack instance will be created in it.

Our `StackSet` administrator role is the same, but the `StackSet` execution role will be a bit different:

**`StackSetExecution.yaml`**

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: StackSet IAM roles

Parameters:
  AdministratorAccountId:
    Type: String
    AllowedPattern: "(\\d+)$"

Resources:
  StackSetExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      # ...
      Principal:
        AWS:
          Fn::Join:
            - ""
            - - "arn:aws:iam::"
              - !Ref AdministratorAccountId
              - ":root"
      # ...
```

Here, we add a `AdministratorAccountId` parameter, which we will enter manually. This parameter will be used to set a proper `Principal` for the execution role.

To deploy this stack in all the child accounts, we will add an API key and secret to `~/.aws/credentials` under different profiles and use those profiles to provision execution roles:

```
aws cloudformation deploy \
    --stack-name Admin-Role \
    --template-file StackSetAdmin.yaml \
    --capabilities CAPABILITY_NAMED_IAM
```

```
aws cloudformation deploy \
    --stack-name Exec-Role \
    --profile test \
    --template-file StackSetExec.yaml \
    --parameter-overrides \
    AdministratorAccountId=ACCT_ID \
    --capabilities CAPABILITY_NAMED_IAM
```

```
aws cloudformation deploy \
    --stack-name Exec-Role \
    --profile prod \
    --template-file StackSetExec.yaml \
    --parameter-overrides \
    AdministratorAccountId=ACCT_ID \
    --capabilities CAPABILITY_NAMED_IAM
```
