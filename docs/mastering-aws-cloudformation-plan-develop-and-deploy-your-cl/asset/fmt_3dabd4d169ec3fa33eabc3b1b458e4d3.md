
# Chapter 3: Validation, Linting, and Deployment of the Stack

1.  From 9,000 and above.

2.  **Linting** is the process of evaluating the template against linter rules and is performed by `cfn-lint`. **Validation** is performed by CloudFormation and checks whether the template is valid and can be processed by CloudFormation.

3.  There are several steps:

    (a) CloudFormation will check whether the stack exists or not.

    (b) CloudFormation will create a `ChangeSet` for a new or existing stack.

    (c) CloudFormation will execute the `ChangeSet` and listen to the stack events.

    (d) If there is a failure, CloudFormation rolls back the changes and informs the user about the issue.

4.  No, `ChangeSets` must have unique names, otherwise you will get a `400 AlreadyExists` error.

5.  The name of the rule file and the rule class must be the same.

6.  Just add the necessary change to the template or parameters and run the stack update.

# Chapter 4: Continuous Integration and Deployment

1.  `buildspec.yml` is a build specification file with instructions for AWS CodeBuild on how to process the build.

2.  A CloudFormation artifact is the template and, for some cases, parameter files. They are stored on S3.

3.  You can use CloudFormation signals, wait conditions, and update policies.

4.  `ListStackResources`.

5.  Source, Build, and Deploy. There are various services that can be used, such as S3, CodeBuild, CodeDeploy, and CloudFormation.

# Chapter 5: Deploying to Multiple Regions and Accounts Using StackSets

1.  Yes, but it is not recommended. You will have to then explicitly specify the names of both roles.

2.  Yes, this is managed by a property called `Maximum Concurrent Accounts`.

3.  Per Region (as they are defined from the first to last) and then per account (as they are defined from the first to last).

4.  No, you must delete all the stack instances first.

5.  The only action for the Administrator role is to assume the Execution role.

# Chapter 6: Configuration Management of the EC2 Instances Using cfn-init

1.  The `cfn-init` helper script processes these configuration sections in the following order: `packages`, `groups`, `users`, `sources`, `files`, `commands`, and then `services`. Yes, and it is recommended to do so for complex configurations or to run configuration items in a different order.

2.  No.

3.  Not using the `files` directive in the config set. You will have to use the `commands` directive, but it is possible to add a missing directory to the full path of the file, and `cfn-init` will automatically create it (similar to the `mkdir -p` command).

4.  `WaitCondition` is a CloudFormation resource that is used to make CloudFormation wait for resource completion. For example, you can use `WaitCondition` with `cfn-signal` to make CloudFormation inform the users about resource creation only when `cfn-init` has finished running.

# Chapter 7: Creating Resources outside AWS Using Custom Resources

1.  `Create`, `Update`, and `Delete`.

2.  Only using Lambda Layers. Or simply copy the module content to the Lambda function's code.

3.  CloudFormation as a requester, AWS Lambda for provisioning, CloudWatch for logs and metrics, and other AWS services, if they are used in the custom resource's logic.

4.  `responseReason`.

5.  The event is an actual payload to the Lambda function, which has to be processed. Context is metadata of the Lambda request that contains various information, such as the timestamp of request.

# Chapter 8: Dynamically Rendering the Template Using Template Macros

1.  JSON. It is either a piece of a template or a full template, depending on whether the `Transform` section is declared or the `Fn::Transform` function is being used.

2.  No, because a macro and its function must be in the same Region.

3.  The stack operation will be considered as Failed and CloudFormation will start rolling back.

4.  Yes, but you need to be aware of the macro evaluation order (https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html#template-macros-use).

# Chapter 9: Generating CloudFormation Templates Using AWS CDK

1.  Constructs are building blocks of CDK. Any block of CDK, such as `App`, `Stack`, `Asset`, or `Resource` is considered to be a Construct.

2.  You have to define each stack separately by assigning a stack class to it as a value.

3.  By adding subnet configuration to the VPC construct. Originally, CDK was developed by TypeScript, but there are libraries for Python, Java, C#, and JavaScript. The CDK development team is working on more languages to support in CDK.

4.  No, CDK will use the default credentials and config to connect to AWS. But it is recommended to provide an account ID and Region when using CDK in production.

# Chapter 10: Deploying Serverless Applications Using AWS SAM

1.  Yes, by using CloudFormation-specific resource types.

2.  `sam local start-api`.

3.  No, SAM CLI doesn't have this command. You will have to use AWS CLI to delete the stack (`aws cloudformation delete-stack`).

4.  It is, but you have to use the CloudFormation-specific type for DynamoDB, since `SimpleTable` doesn't have this property.

5.  The application's source code and its dependencies are uploaded to S3 as a ZIP file. The template is altered to point to S3 for the source code and is saved on disk or printed to the Terminal output.

# Other Books You May Enjoy

If you enjoyed this book, you may be interested in these other books by Packt:

**AWS Security Cookbook**  
Heartin Kanikathottu  
ISBN: 978-1-83882-625-3

*   Create and manage users, groups, roles, and policies across accounts
*   Use AWS Managed Services for logging, monitoring, and auditing
*   Check compliance with AWS Managed Services that use machine learning
*   Provide security and availability for EC2 instances and applications
*   Secure data using symmetric and asymmetric encryption
*   Manage user pools and identity pools with federated login

**AWS SysOps Cookbook - Second Edition**  
Eric Z. Beard, Rowan Udell, Et al  
ISBN: 978-1-83855-018-9

*   Secure your account by creating IAM users and avoiding the use of the root login
*   Simplify the creation of a multi-account landing zone using AWS Control Tower
*   Master Amazon S3 for unlimited, cost-efficient storage of data
*   Explore a variety of compute resources on the AWS Cloud, such as EC2 and AWS Lambda
*   Configure secure networks using Amazon VPC, access control lists, and security groups
*   Estimate your monthly bill by using cost estimation tools
*   Learn to host a website with Amazon Route 53, Amazon CloudFront, and S3

# Leave a review - let other readers know what you think
