
## 3.9 Replicating Data Between EFS and S3 with DataSync

### Problem

You need to replicate files from `Amazon S3` to `Amazon EFS`.

### Solution

Configure `AWS DataSync` with an `S3` source and `EFS` target; then create a `DataSync` task and start the replication task, as shown in [Figure 3-23](#ch03.html#replicating_sthree_bucket_and_efs_files).

![](assets/awsc_0323.png)

### Prerequisites

*   An `S3` bucket and `EFS` file system.
*   `VPC` with isolated subnets created in two `AZs` and associated route tables.
*   `EC2` instance deployed with `EFS` file system attached. You will need the ability to connect to it for testing.

### Preparation

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Storage).

### Steps

1.  Create an `IAM` role using the statement in the provided `assume-role-policy.json` file with this command:

    ```
    S3_ROLE_ARN=$(aws iam create-role --role-name AWSCookbookS3LocationRole \
        --assume-role-policy-document file://assume-role-policy.json \
        --output text --query Role.Arn)
    ```

2.  Attach the `AmazonS3ReadOnlyAccess` `IAM` managed policy to the `IAM` role:

    ```
    aws iam attach-role-policy --role-name AWSCookbookS3LocationRole \
        --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
    ```

3.  Create a `DataSync` `S3` location:

    ```
    S3_LOCATION_ARN=$(aws datasync create-location-s3 \
        --s3-bucket-arn $BUCKET_ARN \
        --s3-config BucketAccessRoleArn=$S3_ROLE_ARN \
        --output text --query LocationArn)
    ```

4.  Create an `IAM` role using the statement in the provided `assume-role-policy.json` file with this command:

    ```
    EFS_ROLE_ARN=$(aws iam create-role --role-name AWSCookbookEFSLocationRole \
        --assume-role-policy-document file://assume-role-policy.json \
        --output text --query Role.Arn)
    ```

5.  Attach the `AmazonElasticFileSystemClientReadWriteAccess` `IAM` managed policy to the `IAM` role:

    ```
    aws iam attach-role-policy --role-name AWSCookbookEFSLocationRole \
        --policy-arn arn:aws:iam::aws:policy/AmazonElasticFileSystemClientFullAccess
    ```

6.  Get the `ARN` of the `EFS` file system:

    ```
    EFS_FILE_SYSTEM_ARN=$(aws efs describe-file-systems \
        --file-system-id $EFS_ID \
        --output text --query FileSystems[0].FileSystemArn)
    ```

7.  Get the `ARN` of the subnet:

    ```
    SUBNET_ARN=$(aws ec2 describe-subnets \
        --subnet-ids $ISOLATED_SUBNET_1 \
        --output text --query Subnets[0].SubnetArn)
    ```

8.  Get the `ARN` of the security group:

    ```
    SG_ARN=arn:aws:ec2:$AWS_REGION:$AWS_ACCOUNT_ID:security-group/$EFS_SG
    ```

9.  Create a `DataSync` `EFS` location:

    ```
    EFS_LOCATION_ARN=$(aws datasync create-location-efs \
        --efs-filesystem-arn $EFS_FILE_SYSTEM_ARN \
        --ec2-config SubnetArn=$SUBNET_ARN,SecurityGroupArns=[$SG_ARN] \
        --output text)
    ```

10. Create a `DataSync` task:

    ```
    TASK_ARN=$(aws datasync create-task \
        --source-location-arn $S3_LOCATION_ARN \
        --destination-location-arn $EFS_LOCATION_ARN \
        --output text --query TaskArn)
    ```

11. Execute the task:

    ```
    aws datasync start-task-execution \
        --task-arn $TASK_ARN
    ```

12. Ensure the task has completed after a few seconds:

    ```
    aws datasync list-task-executions \
        --task-arn $TASK_ARN
    ```

#### Validation checks

Ensure your `EC2` instance 1 has registered with `SSM`. Use this command to check the status. This command should return the instance ID:

```
aws ssm describe-instance-information \
    --filters Key=ResourceType,Values=EC2Instance \
    --query "InstanceInformationList[].InstanceId" --output text
```

Connect to your `EC2` instance using `SSM Session Manager`:

```
aws ssm start-session --target $INSTANCE_ID
```

The `EC2` instance has the `EFS` volume mounted at `/mnt/efs`. You can browse to the directory and view that the `S3-Test-Content.txt` file has been replicated from your `S3` bucket to your `EFS` volume, as shown in the sample output:

```
sh-4.2$ cd /mnt/efs

sh-4.2$ ls

sh-4.2$ ls -al

total 12
drwxr-xr-x 3 nfsnobody nfsnobody 6144 Jan  1  1970 .
drwxr-xr-x 3 root      root        17 Sep 10 02:07 ..
drwx------ 2 root      root      6144 Sep 10 03:27 .aws-datasync
-rwxr-xr-x 1 nfsnobody nfsnobody   30 Jan  1  1970 S3-Test-Content.txt
```

Exit the `Session Manager` session:

```
exit
```

### Cleanup

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Storage).

### Discussion

You can use [AWS DataSync](https://aws.amazon.com/datasync) for both on-demand and ongoing/automated file synchronization tasks across a variety of `AWS` services. `DataSync` preserves metadata for copied items and checks file integrity during the synchronization task, supporting retries if needed. This is useful if you are a developer or cloud engineer looking to move data among a variety of sources and targets without provisioning any infrastructure or writing your own scripts to accomplish the same task. In this recipe, you used it to synchronize data between `S3` and `EFS` hosted in your `AWS` account, but you can also use it to synchronize data among your non-`AWS` servers if you have a `VPN` connection, `Direct Connect`, or among other `AWS` accounts using `VPC` peering or a transit gateway.

#### Note

At the time of this writing, the minimum automated sync schedule interval you can set is one hour. You can find other details about `DataSync` in the [user documentation](https://oreil.ly/dVnun).

Like many `AWS` services, `DataSync` uses `IAM` roles to perform actions against `S3` and `EFS` for you. You granted `DataSync` the ability to interact with `S3` and `EFS`. `DataSync` provisions network interfaces in your `VPC` to connect to your `EFS` file shares and uses the `AWS` APIs to interact with `S3`. It encrypts traffic in transit using `TLS` and also supports encryption at rest using `KMS` should your security and compliance requirements mandate encryption at rest.

### Challenge 1

[Create an AMI](https://oreil.ly/8KFje) from the snapshot you create and launch a new instance from the newly created `AMI`.
