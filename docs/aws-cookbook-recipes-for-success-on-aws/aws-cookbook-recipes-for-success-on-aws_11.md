# 附录：快速修复

这些有用的小代码片段将帮助你节省时间，充分发挥 AWS 的效用。

将你的 `AWS_ACCOUNT_ID` 设置为 bash 变量：

```bash
export AWS_ACCOUNT_ID=$(aws sts get-caller-identity \
--query Account --output text)
```

获取最近创建的 CloudWatch 日志组名称：

```bash
aws logs describe-log-groups --output=yaml \
--query 'reverse(sort_by(logGroups,&creationTime))[:1].{Name:logGroupName}'
```

尾随查看 CloudWatch 组的日志：

```bash
aws logs tail <<LOGGROUPNAME>> --follow --since 10s
```

删除所有匹配文本模式的日志组，并提示 yes/no 进行确认：

```bash
aws logs describe-log-groups | \
jq ".logGroups[].logGroupName" | grep -i <<pattern>> | \
xargs -p -I % aws logs delete-log-group --log-group-name %
```

停止当前工作区域中所有正在运行的实例（致谢：Curtis Rissi）：

```bash
aws ec2 stop-instances \
--instance-ids $(aws ec2 describe-instances \
--filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].[InstanceId]" \
--output text | tr '\n' ' ')
```

确定发起 CLI 调用的用户：

```bash
aws sts get-caller-identity --query UserId --output text
```

为你的 CLI 命令生成 YAML 输入并使用它：

```bash
aws ec2 create-vpc --generate-cli-skeleton yaml-input > input.yaml
# Edit input.yaml - at a minimum modify CidrBlock, DryRun, ResourceType, and Tags
aws ec2 create-vpc --cli-input-yaml file://input.yaml
```

以表格格式列出 AWS 区域名称和端点：

```bash
aws ec2 describe-regions --output table
```

查找你当前使用的区域的接口 VPC 端点：

```bash
aws ec2 describe-vpc-endpoint-services \
--query ServiceDetails[*].ServiceName
```

向 DynamoDB 表填充数据：

```bash
aws ddb put table_name '[{key1: value1}, {key2: value2}]'
```

确定特定数据库引擎（例如 aurora-postgresql）当前支持的版本：

```bash
aws rds describe-db-engine-versions --engine aurora-postgresql \
--query "DBEngineVersions[].EngineVersion"
```

删除与安全组关联的网络接口，并对每次删除提示确认（回答 yes/no 以删除或跳过）：

```bash
aws ec2 describe-network-interfaces \
--filters Name=group-id,Values=$SecurityGroup \
--query NetworkInterfaces[*].NetworkInterfaceId \
--output text | tr '\t' '\n' | xargs -p -I % \
aws ec2 delete-network-interface --network-interface-id %
```

查找某个区域的默认 VPC（如果有）：

```bash
aws ec2 describe-vpcs --vpc-ids \
--query 'Vpcs[?IsDefault==`true`]'
```

在某个区域为新的 EBS 卷默认启用加密：

```bash
aws ec2 enable-ebs-encryption-by-default
```

列出所有 AWS 区域：

```bash
aws ssm get-parameters-by-path \
--path /aws/service/global-infrastructure/regions \
--output text --query Parameters[*].Name | tr "\t" "\n"
```

列出所有 AWS 服务：

```bash
aws ssm get-parameters-by-path \
--path /aws/service/global-infrastructure/services \
--output text --query Parameters[*].Name \
| tr "\t" "\n" | awk -F "/" '{ print $6 }'
```

列出某个区域（例如 us-east-1）中可用的所有服务：

```bash
aws ssm get-parameters-by-path \
--path /aws/service/global-infrastructure/regions/us-east-1/services \
--output text --query Parameters[*].Name | tr "\t" "\n" \
| awk -F "/" '{ print $8 }'
```

列出提供特定服务（例如 SNS）的所有区域：

```bash
aws ssm get-parameters-by-path \
--path /aws/service/global-infrastructure/services/sns/regions \
--output text --query Parameters[*].Value | tr "\t" "\n"
```

为 S3 中的对象创建一个一周后过期的预签名 URL：

```bash
aws s3 presign s3://<<BucketName>>/<<FileName>> \
--expires-in 604800
```

查找跨账户一致的某个区域的可用区 ID：

```bash
aws ec2 describe-availability-zones --region $AWS_REGION
```

通过从 EC2 实例的元数据中获取值来设置区域：

```bash
export AWS_DEFAULT_REGION=$(curl --silent http://169.254.169.254/latest/dynamic/instance-identity/document \
| awk -F'"' ' /region/ {print $4}')
```
