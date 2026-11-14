
## 3.9 使用 DataSync 在 EFS 和 S3 之间复制数据

### 问题

您需要将文件从 `Amazon S3` 复制到 `Amazon EFS`。

### 解决方案

配置 `AWS DataSync`，将 `S3` 作为源，`EFS` 作为目标；然后创建一个 `DataSync` 任务并启动复制任务，如[图 3-23](#ch03.html#replicating_sthree_bucket_and_efs_files)所示。

![](assets/awsc_0323.png)

### 先决条件

*   一个 `S3` 存储桶和 `EFS` 文件系统。
*   在两个 `AZ` 中创建了隔离子网并关联了路由表的 `VPC`。
*   部署了挂载 `EFS` 文件系统的 `EC2` 实例。您需要具备连接到它以进行测试的能力。

### 准备工作

按照[章节代码仓库](https://github.com/AWSCookbook/Storage)中本配方文件夹内的步骤操作。

### 步骤

1.  使用提供的 `assume-role-policy.json` 文件中的语句，通过以下命令创建一个 `IAM` 角色：

    ```
    S3_ROLE_ARN=$(aws iam create-role --role-name AWSCookbookS3LocationRole \
        --assume-role-policy-document file://assume-role-policy.json \
        --output text --query Role.Arn)
    ```

2.  将 `AmazonS3ReadOnlyAccess` `IAM` 托管策略附加到 `IAM` 角色：

    ```
    aws iam attach-role-policy --role-name AWSCookbookS3LocationRole \
        --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
    ```

3.  创建一个 `DataSync` `S3` 位置：

    ```
    S3_LOCATION_ARN=$(aws datasync create-location-s3 \
        --s3-bucket-arn $BUCKET_ARN \
        --s3-config BucketAccessRoleArn=$S3_ROLE_ARN \
        --output text --query LocationArn)
    ```

4.  使用提供的 `assume-role-policy.json` 文件中的语句，通过以下命令创建一个 `IAM` 角色：

    ```
    EFS_ROLE_ARN=$(aws iam create-role --role-name AWSCookbookEFSLocationRole \
        --assume-role-policy-document file://assume-role-policy.json \
        --output text --query Role.Arn)
    ```

5.  将 `AmazonElasticFileSystemClientReadWriteAccess` `IAM` 托管策略附加到 `IAM` 角色：

    ```
    aws iam attach-role-policy --role-name AWSCookbookEFSLocationRole \
        --policy-arn arn:aws:iam::aws:policy/AmazonElasticFileSystemClientFullAccess
    ```

6.  获取 `EFS` 文件系统的 `ARN`：

    ```
    EFS_FILE_SYSTEM_ARN=$(aws efs describe-file-systems \
        --file-system-id $EFS_ID \
        --output text --query FileSystems[0].FileSystemArn)
    ```

7.  获取子网的 `ARN`：

    ```
    SUBNET_ARN=$(aws ec2 describe-subnets \
        --subnet-ids $ISOLATED_SUBNET_1 \
        --output text --query Subnets[0].SubnetArn)
    ```

8.  获取安全组的 `ARN`：

    ```
    SG_ARN=arn:aws:ec2:$AWS_REGION:$AWS_ACCOUNT_ID:security-group/$EFS_SG
    ```

9.  创建一个 `DataSync` `EFS` 位置：

    ```
    EFS_LOCATION_ARN=$(aws datasync create-location-efs \
        --efs-filesystem-arn $EFS_FILE_SYSTEM_ARN \
        --ec2-config SubnetArn=$SUBNET_ARN,SecurityGroupArns=[$SG_ARN] \
        --output text)
    ```

10. 创建一个 `DataSync` 任务：

    ```
    TASK_ARN=$(aws datasync create-task \
        --source-location-arn $S3_LOCATION_ARN \
        --destination-location-arn $EFS_LOCATION_ARN \
        --output text --query TaskArn)
    ```

11. 执行任务：

    ```
    aws datasync start-task-execution \
        --task-arn $TASK_ARN
    ```

12. 等待几秒钟确保任务已完成：

    ```
    aws datasync list-task-executions \
        --task-arn $TASK_ARN
    ```

#### 验证检查

确保您的 `EC2` 实例 1 已向 `SSM` 注册。使用此命令检查状态。此命令应返回实例 ID：

```
aws ssm describe-instance-information \
    --filters Key=ResourceType,Values=EC2Instance \
    --query "InstanceInformationList[].InstanceId" --output text
```

使用 `SSM Session Manager` 连接到您的 `EC2` 实例：

```
aws ssm start-session --target $INSTANCE_ID
```

`EC2` 实例在 `/mnt/efs` 处挂载了 `EFS` 卷。您可以浏览到该目录并查看 `S3-Test-Content.txt` 文件已从您的 `S3` 存储桶复制到您的 `EFS` 卷中，如示例输出所示：

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

退出 `Session Manager` 会话：

```
exit
```

### 清理

按照[章节代码仓库](https://github.com/AWSCookbook/Storage)中本配方文件夹内的步骤操作。

### 讨论

您可以使用 [AWS DataSync](https://aws.amazon.com/datasync) 执行按需和持续/自动化的文件同步任务，跨多种 `AWS` 服务。`DataSync` 会保留复制项目的元数据，并在同步任务期间检查文件完整性，支持在需要时重试。如果您是希望在各种源和目标之间移动数据，而无需预置任何基础设施或编写自己的脚本来完成相同任务的开发人员或云工程师，这非常有用。在本配方中，您用它来同步托管在您的 `AWS` 账户中的 `S3` 和 `EFS` 之间的数据，但如果您有 `VPN` 连接、`Direct Connect`，或者使用 `VPC` 对等连接或传输网关，也可以用它来同步您的非 `AWS` 服务器之间或其他 `AWS` 账户之间的数据。

#### 注意

在撰写本文时，您可以设置的最小自动同步计划间隔为一小时。您可以在[用户文档](https://oreil.ly/dVnun)中找到关于 `DataSync` 的其他详细信息。

与许多 `AWS` 服务一样，`DataSync` 使用 `IAM` 角色代表您对 `S3` 和 `EFS` 执行操作。您授予了 `DataSync` 与 `S3` 和 `EFS` 交互的能力。`DataSync` 在您的 `VPC` 中预置网络接口以连接到您的 `EFS` 文件共享，并使用 `AWS` API 与 `S3` 交互。它使用 `TLS` 加密传输中的流量，如果您的安全性和合规性要求强制要求静态加密，还支持使用 `KMS` 进行静态加密。

### 挑战 1

根据您创建的快照[创建一个 AMI](https://oreil.ly/8KFje)，并从新创建的 `AMI` 启动一个新实例。
