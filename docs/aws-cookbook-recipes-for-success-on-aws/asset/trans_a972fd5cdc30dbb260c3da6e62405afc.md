
使用 AWS Backup 为您的 EC2 实例创建按需备份，并通过 AWS 控制台从保管库还原备份，如 [图 3-12](#ch03.html#creating_and_restoring_ectwo_backups) 所示。

![](assets/awsc_0312.png)

### 先决条件

*   在两个 AZ 中创建了具有隔离子网的 VPC 以及关联的路由表
*   已部署 EC2 实例

### 准备工作

按照 [章节代码仓库](https://github.com/AWSCookbook/Storage) 中此配方文件夹内的步骤操作。

### 步骤

1.  导航到 AWS Backup 控制台，从左侧导航窗格中选择“受保护资源”。

2.  单击“创建按需备份”，选择您的 EC2 实例，保留默认设置，然后单击“创建按需备份”（请参阅 [图 3-13](#ch03.html#creating_on_demand_backup)）。

    ![](assets/awsc_0313.png)

    **注意**

    AWS Backup 将创建一个用于其目的的角色，该角色使用 AWS Backup 托管策略来执行备份所需的操作。如果需要，您也可以创建自己的自定义角色。有关更多信息，请参阅此 AWS [文档](https://oreil.ly/WirFO)。

    备份在备份作业视图中开始，如 [图 3-14](#ch03.html#view_of_backup_job_running) 所示。

    ![](assets/awsc_0314.png)

3.  等待备份在您的账户中完成（这可能需要片刻才能达到 [图 3-15](#ch03.html#view_of_backup_job_completed) 中显示的“已完成”状态）。

    ![](assets/awsc_0315.png)

4.  选择当前区域的默认备份保管库，查看您刚完成的镜像备份。

5.  单击您刚创建的备份恢复点，然后选择“复制”，如 [图 3-16](#ch03.html#copy_recovery_point-id000009) 所示。

    ![](assets/awsc_0316.png)

6.  选择目标区域，保留所有默认设置，然后单击“复制”（如 [图 3-17](#ch03.html#copy_recovery_point_to_a_different_aws) 所示）。您将看到复制作业进入“运行中”状态，如 [图 3-18](#ch03.html#copy_recovery_point_running) 所示。

    ![](assets/awsc_0317.png)![](assets/awsc_0318.png)

7.  复制作业在几分钟后完成后，在 AWS 控制台中，从下拉区域选择器（AWS 控制台右上角）中选择目标区域。

8.  选择您的默认备份保管库，然后选择要还原的备份，如 [图 3-19](#ch03.html#restore_recovery_point) 所示。

    ![](assets/awsc_0319.png)

9.  在“网络设置”下，选择用于还原的实例类型和 VPC，然后单击“还原备份”。输入示例如 [图 3-20](#ch03.html#copy_recovery_point-id0000035) 所示。

    ![](assets/awsc_0320.png)

10. 您可以在控制台的“作业”部分下的“还原作业”选项卡中监控还原进度，如 [图 3-21](#ch03.html#recovery_point_restore_job_running) 所示。

     ![](assets/awsc_0321.png)

#### 验证检查

浏览到 EC2 控制台，查看目标区域中正在运行的实例。此 EC2 实例是您原始实例的副本。

### 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Storage) 中此配方文件夹内的步骤操作。

## 讨论

[AWS Backup](https://aws.amazon.com/backup/faqs) 让您能够从单一位置管理和监控您使用的各 AWS 服务中的备份。您可以备份 [多种 AWS 服务](https://oreil.ly/KHpPB)，并为您使用的 AWS 服务中的云资源设置备份策略。您还可以在账户内跨区域复制备份（或复制到其他 AWS 账户），这正是您在本配方中探索的内容。如果您使用 EC2 服务运行带有您想要保护的持久数据的实例，则 [EBS 快照](https://oreil.ly/BYWAl) 是 AWS 备份策略的重要组成部分。您可以手动创建 EBS 卷快照、编写自己的自动化脚本、使用 [Data Lifecycle Manager](https://oreil.ly/eXbT7) 进行自动化，或使用 AWS Backup。

当您使用 AWS Backup 备份 EC2 实例时，该服务会将备份存储在您选择的 [备份保管库](https://oreil.ly/OnzyN) 中（如果您没有保管库，将创建一个默认备份保管库），处理构建 Amazon Machine Image (AMI) 的过程，该镜像包含所有配置参数、已备份的附加 EBS 卷和元数据。该服务将整个包存储在备份保管库中。这使您只需启动由 AWS Backup 服务生成的 AMI 即可创建 EC2 实例，从而减少了从主区域或您选择的另一区域中的备份还原实例所涉及的恢复时间目标 (RTO)。

### 挑战

使用 [备份计划](https://oreil.ly/uguyM) 配置 EC2 实例的每周自动备份计划，将备份的实例复制到另一区域中的保管库。

# 3.8 从 EBS 快照还原文件

## 问题

您需要从账户中某个卷创建的 EBS 快照还原文件。

## 解决方案

从快照创建卷，从 EC2 实例挂载该卷，然后将文件复制到实例卷中（请参阅 [图 3-22](#ch03.html#process_flow_for_file_restore_from_a_sn)）。

![](assets/awsc_0322.png)

### 先决条件

*   在两个 AZ 中创建了具有隔离子网的 VPC 以及关联的路由表。
*   已部署 EC2 实例。您需要具备连接以挂载 EBS 快照并还原文件的能力。

### 准备工作

按照 [章节代码仓库](https://github.com/AWSCookbook/Storage) 中此配方文件夹内的步骤操作。

### 步骤

1.  找到附加到 EC2 实例的 EBS 卷：

    ```
    ORIG_VOLUME_ID=$(aws ec2 describe-volumes \
        --filters Name=attachment.instance-id,Values=$INSTANCE_ID \
        --output text \
        --query Volumes[0].Attachments[0].VolumeId)
    ```

2.  对 EBS 卷创建快照（这将花费片刻时间完成）：

    ```
    SNAPSHOT_ID=$(aws ec2 create-snapshot \
        --volume-id $ORIG_VOLUME_ID \
        --output text --query SnapshotId)
    ```

3.  从快照创建卷，并将 `VOLUME_ID` 保存为环境变量：

    ```
    SNAP_VOLUME_ID=$(aws ec2 create-volume \
        --snapshot-id $SNAPSHOT_ID \
        --size 8 \
        --volume-type gp2 \
        --availability-zone us-east-1a \
        --output text --query VolumeId)
    ```

#### 验证检查

将卷作为 `/dev/sdf` 挂载到 EC2 实例：

```
aws ec2 attach-volume --volume-id $SNAP_VOLUME_ID \
    --instance-id $INSTANCE_ID --device /dev/sdf
```

等待直到卷的状态变为“已挂载”：

```
aws ec2 describe-volumes \
    --volume-ids $SNAP_VOLUME_ID
```

使用 SSM Session Manager 连接到 EC2 实例（请参阅 [配方 1.6](#ch01.html#connecting_to_ectwo_instances_using_aws)）：

```
aws ssm start-session --target $INSTANCE_ID
```

运行 `lsblk` 命令查看卷，并记下您挂载的卷名称（您将在后续步骤中使用此卷名称来挂载卷）：

```
lsblk
```

您应该会看到类似如下的输出：

```
NAME          MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
nvme0n1       259:0    0   8G  0 disk
├─nvme0n1p1   259:1    0   8G  0 part /
└─nvme0n1p128 259:2    0   1M  0 part
nvme1n1       259:3    0   8G  0 disk
├─nvme1n1p1   259:4    0   8G  0 part
└─nvme1n1p128 259:5    0   1M  0 part
```

创建一个文件夹以挂载附加的磁盘：

```
sudo mkdir /mnt/restore
```

将您挂载的卷挂载到您创建的文件夹：

```
sudo mount -t xfs -o nouuid /dev/nvme1n1p1 /mnt/restore
```

**注意**
