
## 设置一个排除特定文件夹中文件名的 DataSync 任务（例如 `private-folder`）

### 挑战 2

设置一个计划的 DataSync 任务，按小时计划将数据从 S3 复制到 EFS。

# 第 4 章 数据库

## 4.0 简介

您在 AWS 上使用数据库时有无数选择。在 EC2 上安装和运行数据库为您提供了最多的数据库引擎和自定义配置选择，但也带来了诸如修补、备份、配置高可用性、复制和性能调优等挑战。正如其[产品页面](https://oreil.ly/wLQPC)所述，AWS 提供托管数据库服务，帮助解决这些挑战，并涵盖广泛的数据库类型（关系型、键值/NoSQL、内存型、文档型、宽列型、图型、时间序列、账本型）。在选择数据库类型和数据模型时，您必须牢记速度、容量和访问模式。

AWS 上的托管数据库服务与许多服务集成，从安全、运维和开发的角度为您提供额外功能。在本章中，您将探索 Amazon Relational Database Service (RDS)、Amazon DynamoDB 的 NoSQL 用法，以及大规模迁移、保护和运维这些数据库类型的方法。例如，您将学习如何将 Secrets Manager 与 RDS 数据库集成以自动轮换数据库用户密码。您还将学习如何利用 IAM 身份验证完全消除应用程序对数据库密码的依赖，转而通过 IAM 权限授予 RDS 访问权限。您将探索 DynamoDB 的自动扩展，并了解为什么这从成本和性能角度很重要。

> **注意**
>
> 有人认为 [Route 53 是一个数据库](https://oreil.ly/Da83G)，但我们不同意 :-)

> **注意**
>
> 过去某些数据库引擎曾对副本配置、默认根用户名、主表等使用特定术语。我们已尽力在整章（及全书）中尽可能使用包容性术语。我们支持在商业和开源数据库引擎中使用包容性术语的运动。

## 工作站配置

遵循[“CLI 配方的通用工作站设置步骤”](#preface01.html#general_workstation_setup_steps_for_cli)以验证您的配置并设置所需的环境变量。然后，克隆章节代码仓库：

```bash
git clone https://github.com/AWSCookbook/Databases
```

> **警告**
>
> 在本章的某些步骤中，您将创建密码并暂时将它们保存为环境变量以在后续步骤中使用。完成配方后，请务必按照清理步骤取消设置环境变量。我们采用这种方法是为了便于理解。在生产环境中应使用更安全的方法（例如[配方 1.8](#ch01.html#storingcomma_encryptingcomma_and_access)中使用的方法）。

## 4.1 创建 Amazon Aurora Serverless PostgreSQL 数据库

### 问题

您有一个接收不可预测请求的 Web 应用程序，这些请求需要存储在关系数据库中。您需要一种能够随使用量扩展且具有成本效益的数据库解决方案。您希望构建一个运维开销低且必须与现有的 PostgreSQL 支持的应用程序兼容的解决方案。

### 解决方案

配置并创建一个具有复杂密码的 Aurora Serverless 数据库集群。然后，应用自定义的扩展配置并在不活动后启用自动暂停。响应策略的扩展活动如[图 4-1](#ch04.html#aurora_serverless_cluster_scaling_compu)所示。

![](assets/awsc_0401.png)

#### 先决条件

*   在两个 AZ 中创建了隔离子网并关联了路由表的 VPC。
*   已部署 EC2 实例。您需要能够连接到它以进行测试。

#### 准备工作

遵循[章节代码仓库](https://github.com/AWSCookbook/Databases)中此配方文件夹内的步骤。

#### 步骤

1.  使用 AWS Secrets Manager 生成复杂密码：

    ```bash
    ADMIN_PASSWORD=$(aws secretsmanager get-random-password \
        --exclude-punctuation \
        --password-length 41 --require-each-included-type \
        --output text \
        --query RandomPassword)
    ```

    > **注意**
    >
    > 我们从创建的密码中排除了标点字符，因为 PostgreSQL 不支持它们。请参阅“Amazon RDS 中的命名约束”[表](https://oreil.ly/z0LvJ)。

2.  创建一个数据库子网组，指定用于集群的 VPC 子网。数据库子网组简化了 RDS 弹性网络接口 (ENI) 的放置：

    ```bash
    aws rds create-db-subnet-group \
        --db-subnet-group-name awscookbook401subnetgroup \
        --db-subnet-group-description "AWSCookbook401 subnet group" \
        --subnet-ids $SUBNET_ID_1 $SUBNET_ID_2
    ```

    您应该会看到类似以下的输出：

    ```json
    {
      "DBSubnetGroup": {
      "DBSubnetGroupName": "awscookbook402subnetgroup",
      "DBSubnetGroupDescription": "AWSCookbook401 subnet group",
      "VpcId": "vpc-<<VPCID>>",
      "SubnetGroupStatus": "Complete",
      "Subnets": [
        {
        "SubnetIdentifier": "subnet-<<SUBNETID>>",
        "SubnetAvailabilityZone": {
          "Name": "us-east-1b"
        },
        "SubnetOutpost": {},
        "SubnetStatus": "Active"
        },
    ...
    ```

3.  为数据库创建一个 VPC 安全组：

    ```bash
    DB_SECURITY_GROUP_ID=$(aws ec2 create-security-group \
        --group-name AWSCookbook401sg \
        --description "Aurora Serverless Security Group" \
        --vpc-id $VPC_ID --output text --query GroupId)
    ```

4.  创建一个数据库集群，指定 `engine-mode` 为 `serverless`：

    ```bash
    aws rds create-db-cluster \
        --db-cluster-identifier awscookbook401dbcluster \
        --engine aurora-postgresql \
        --engine-mode serverless \
        --engine-version 10.14 \
        --master-username dbadmin \
        --master-user-password $ADMIN_PASSWORD \
        --db-subnet-group-name awscookbook401subnetgroup \
        --vpc-security-group-ids $DB_SECURITY_GROUP_ID
    ```

    您应该会看到类似以下的输出：

    ```json
    {
      "DBCluster": {
      "AllocatedStorage": 1,
      "AvailabilityZones": [
        "us-east-1f",
        "us-east-1b",
        "us-east-1a"
      ],
      "BackupRetentionPeriod": 1,
      "DBClusterIdentifier": "awscookbook401dbcluster",
      "DBClusterParameterGroup": "default.aurora-postgresql10",
      "DBSubnetGroup": "awscookbook401subnetgroup",
      "Status": "creating",
    ...
    ```

5.  等待状态变为 available；这将花费片刻时间：

    ```bash
    aws rds describe-db-clusters \
        --db-cluster-identifier awscookbook401dbcluster \
        --output text --query DBClusters[0].Status
    ```

6.  修改数据库以使用新的自动扩展容量目标（最小 8，最大 16）自动扩展，并在不活动五分钟后启用 `AutoPause`：

    ```bash
    aws rds modify-db-cluster \
        --db-cluster-identifier awscookbook401dbcluster --scaling-configuration \
        MinCapacity=8,MaxCapacity=16,SecondsUntilAutoPause=300,TimeoutAction='ForceApplyCapacityChange',AutoPause=true
    ```

    您应该会看到类似于步骤 4 的输出。

    > **注意**
    >
    > 实际上，您可能想要使用不同的 `AutoPause` 值。要确定适合您用例的值，请评估您的性能需求和 [Aurora 定价](https://oreil.ly/o6mAP)。

    至少等待五分钟，观察数据库容量已缩减至 0：

    ```bash
    aws rds describe-db-clusters \
        --db-cluster-identifier awscookbook401dbcluster \
        --output text --query DBClusters[0].Capacity
    ```

    > **注意**
