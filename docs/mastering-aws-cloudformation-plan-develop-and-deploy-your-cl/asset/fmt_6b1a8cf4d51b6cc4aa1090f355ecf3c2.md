
The last thing to learn before we start is the permissions that we need to grant to the administrator and target accounts.

# Permissions

Stack instances within a `StackSet` can be deployed only using `CloudFormation` service roles. At the same time, the administrator account must have a role that allows the creation of stack instances. Before we proceed, let's configure those permissions for our account.

**Important note**

`StackSet` management roles must have a name: `AWSCloudFormationStackSetAdministrationRole` for administrator accounts and `AWSCloudFormationStackSetExecutionRole` for target accounts. This is not a string, but is recommended, and is defined in the `CloudFormation` documentation at `https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs.html`.

To deploy these roles in our account (which will act as an administrator and target account at the same time), we will use the following:

`CloudFormation:StackSetPermissions.yaml` (administrator account)

```yaml
StackSetAdministratorRole:
  Type: AWS::IAM::Role
  Properties:
    RoleName: "AWSCloudFormationStackSetAdministrationRole"
    AssumeRolePolicyDocument:
      # ...
    Policies:
      - PolicyName: StackSetAdministratorPolicy
        PolicyDocument:
          Version: 2012-10-17
          Statement:
            Effect: Allow
            Action: "sts:AssumeRole"
            Resource:
              - "arn:aws:iam::*:role/AWSCloudFormationStackSetExecutionRole"
```

The administrator role only has one action that is allowed, and this is to assume the role in a target account:

`StackSetPermissions.yaml` (target account)

```yaml
StackSetExecutionRole:
  Type: AWS::IAM::Role
  Properties:
    RoleName: AWSCloudFormationStackSetExecutionRole
    AssumeRolePolicyDocument:
      Version: 2012-10-17
      Statement:
        Effect: Allow
        Action: "sts:AssumeRole"
        Principal:
          AWS: !Sub "arn:aws:iam::${AWS::AccountId}:root"
    ManagedPolicyArns:
      - "arn:aws:iam::aws:policy/AdministratorAccess"
```

The role in a target role will do all of the jobs, and so it must have enough permissions, so we grant administrator access to the execution role for the target account.

We also specify the `AWS::AccountId` pseudoparameter because for the next section, we will deploy `StackSet` in multiple regions, but in the same account, so we won't need to expose the AWS account ID in the template. We just need to deploy the stack using this template:

```bash
aws cloudformation deploy \
  --stack-name StackSet-Permissions \
  --template-file StackSetPermissions.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

Once stack is created, we are all set and ready to go. We've prepared IAM roles in order to allow `StackSets` to be created and provisioned. Now let's deploy our core stack to multiple regions!

# Deploying to Multiple Regions

We are now going to start creating `StackSet` for a single account, but in multiple regions. We will begin with AWS Console.

## Using AWS Console

Let's start by deploying `StackSet` with the core template in a single account (for example, our main account). We'll begin with AWS Console:

1. Once we log in to the console, we will go to **CloudFormation - StackSets** and click on **Create StackSet**. We will choose **Template is ready**, upload our `core.yaml` file (which you will find in the GitHub repository) and click **Next**. The template will be uploaded to `S3` automatically:

   ![Figure 5.2 – CloudFormation StackSet console](image/B10533_05_002.jpg)

   Figure 5.2 – CloudFormation StackSet console

2. Now we need to supply the `StackSet` name and parameters and hit **Next**:

   ![Figure 5.3 – CloudFormation StackSet console](image/B10533_05_003.jpg)

   Figure 5.3 – CloudFormation StackSet console

   **Important note**

   We insert parameter values for the `StackSet` and these parameters will be inherited by stack instances. It is not yet possible to add unique parameters per stack instance during `StackSet` creation, since all stack instances by design are meant to be the same. Alternatively, we can create one stack instance for `StackSet` in the beginning, and then use `--parameter-overrides` when we add stack instances.

3. For this example, we are not going to provide any tags, but we must specify the administrator role for `StackSet`. We will leave the execution role unchanged and hit **Next**:

   ![Figure 5.4 – CloudFormation StackSet console](image/B10533_05_004.jpg)

   Figure 5.4 – CloudFormation StackSet console

4. Now we need to enter the values for AWS accounts, regions, and deployment options. Since we only create `StackSet` in our main accounts, we will choose **Deploy stacks in accounts** and add the account ID:

   ![Figure 5.5 – CloudFormation StackSet console](image/B10533_05_005.jpg)

   Figure 5.5 – CloudFormation StackSet console

5. Next, we need to specify the regions that we will deploy our stacks to. We will choose **North Virginia** (`us-east-1`) and **Ireland** (`eu-west-1`):

   ![Figure 5.6 – CloudFormation StackSet console](image/B10533_05_006.jpg)

   Figure 5.6 – CloudFormation StackSet console

   We can change the order of regions if we wish, given that `CloudFormation` will deploy the stack instances in the order that they are specified in here. If we make Ireland our *test* region and N. Virginia our *prod* region, it will be wise to deploy first to test and then to production. But for this example, we will keep things as is.

   Now let's look at our deployment options.

6. We will set **Maximum concurrent accounts** to **1** (since we will only deploy to our account) and **Failure tolerance** to **0**—we don't want to proceed if one of the stack instances fails to be provisioned. Then we hit **Next** one last time:

   ![Figure 5.7 – CloudFormation StackSet console](image/B10533_05_007.jpg)

   Figure 5.7 – CloudFormation StackSet console

7. We need to review the summary and check **I acknowledge that AWS CloudFormation might create IAM resources** (this is like setting `CAPABILITY_IAM` in `awscli`). If there is something that we want to change, then we can click **Edit** on any of the steps in the **Summary**. If everything is set in order, then all we need to do is click **Submit**. Now we can review our `StackSet`:

   ![Figure 5.8 – CloudFormation StackSet console](image/B10533_05_008.jpg)

   Figure 5.8 – CloudFormation StackSet console

   In the `StackSet` information, we will see the summary and other properties of the `StackSet`. What we're interested in is *Stack Instances*:

   ![Figure 5.9 – CloudFormation StackSet console](image/B10533_05_009.jpg)

   Figure 5.9 – CloudFormation StackSet console

8. We can see that only one stack is being provisioned. This is because we set **Maximum concurrent accounts** to **1** and deployment to Ireland will not begin until stack instance in N. Virginia is provisioned. We can't see `CloudFormation` events form this menu, so we will have to switch to the N. Virginia region and look them up there, as shown in the following screenshot:

   ![Figure 5.10 – CloudFormation console](image/B10533_05_010.jpg)

   Figure 5.10 – CloudFormation console

9. We can see that this stack instance is created. Once both stack instances are created, we will see that their **Status** is **CURRENT** in the **Stack instances** section:

   ![Figure 5.11 – CloudFormation StackSet console](image/B10533_05_011.jpg)

   Figure 5.11 – CloudFormation StackSet console

Now, let's do the same using `awscli`. To clean up, we need to delete the stack instances from the `StackSet` and then the `StackSet` itself.

## Using AWS CLI
