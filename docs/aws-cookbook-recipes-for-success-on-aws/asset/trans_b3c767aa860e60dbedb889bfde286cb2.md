

# 为 EC2 实例提供直接互联网访问

## 问题

您在 VPC 的一个子网中有一个现有的 EC2 实例。您需要让该实例能够直接访问互联网上的客户端。

## 解决方案

您将创建一个互联网网关并将其附加到您的 VPC。接下来，您将修改与 EC2 实例所在子网关联的路由表。您将添加一条将来自子网的流量发送到互联网网关的路由。最后，创建一个弹性 IP (EIP) 并将其与实例关联，如[图 2-4](#ch02.html#public_subnet_tiercomma_internet_gatewa)所示。

![](assets/awsc_0204.png)

### 先决条件

- 在两个 AZ 中创建了 VPC 和子网以及关联的路由表。
- 已部署 EC2 实例。您需要具备连接到该实例以进行测试的能力。

### 准备

按照[章节代码仓库](https://github.com/AWSCookbook/Networking)中本配方文件夹中的步骤操作。

### 步骤

1. 创建互联网网关 (IGW)：

```
INET_GATEWAY_ID=$(aws ec2 create-internet-gateway \
    --tag-specifications \
'ResourceType=internet-gateway,Tags=[{Key=Name,Value=AWSCookbook202}]' \
    --output text --query InternetGateway.InternetGatewayId)
```

2. 将互联网网关附加到现有 VPC：

```
aws ec2 attach-internet-gateway \
    --internet-gateway-id $INET_GATEWAY_ID --vpc-id $VPC_ID
```

3. 在您 VPC 的每个路由表中，创建一条将默认路由目标设置为互联网网关的路由：

```
aws ec2 create-route --route-table-id $ROUTE_TABLE_ID_1 \
    --destination-cidr-block 0.0.0.0/0 --gateway-id $INET_GATEWAY_ID

aws ec2 create-route --route-table-id $ROUTE_TABLE_ID_2 \
    --destination-cidr-block 0.0.0.0/0 --gateway-id $INET_GATEWAY_ID
```

对于步骤 3 中的每个命令，您应该会看到类似以下的输出：

```
{
  "Return": true
}
```

4. 创建 EIP：

```
ALLOCATION_ID=$(aws ec2 allocate-address --domain vpc \
    --output text --query AllocationId)
```

#### 注意

AWS 将[弹性 IP 地址 (EIP)](https://oreil.ly/FCrvT)定义为“专为动态云计算设计的静态 IPv4 地址。EIP 地址分配给您的 AWS 账户，归您所有，直到您释放它。”

5. 将 EIP 与现有的 EC2 实例关联：

```
aws ec2 associate-address \
    --instance-id $INSTANCE_ID --allocation-id $ALLOCATION_ID
```

您应该会看到类似以下的输出：

```
{
  "AssociationId": "eipassoc-<<snip>>"
}
```

#### 验证检查

使用 SSM Session Manager 连接到 EC2 实例（请参阅[配方 1.6](#ch01.html#connecting_to_ectwo_instances_using_aws)）：

```
aws ssm start-session --target $INSTANCE_ID
```

Ping 互联网上的主机以测试互联网连接：

```
ping -c 4 homestarrunner.com
```

您应该会看到类似以下的输出：

```
sh-4.2$ ping -c 4 homestarrunner.com
PING homestarrunner.com (72.10.33.178) 56(84) bytes of data.
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=1 ttl=49 time=2.12 ms
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=2 ttl=49 time=2.04 ms
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=3 ttl=49 time=2.05 ms
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=4 ttl=49 time=2.08 ms
--- homestarrunner.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 2.045/2.078/2.127/0.045 ms
sh-4.2$
```

#### 提示

公共 IP 不是操作系统配置的一部分。如果您想从实例的[元数据](https://oreil.ly/lmZ1U)中检索公共 IP，可以使用此命令：

```
curl http://169.254.169.254/latest/meta-data/public-ipv4
```

退出 Session Manager 会话：

```
exit
```

### 清理

按照[章节代码仓库](https://github.com/AWSCookbook/Networking)中本配方文件夹中的步骤操作。

### 讨论

您在路由表条目中创建的路由将所有非本地流量发送到提供 VPC 互联网连接的 IGW。因为您使用的是现有的运行中 EC2 实例，所以您需要创建一个弹性 IP 并将其与实例关联。这些步骤使实例能够进行互联网通信，而无需与其交互。有一个[选项可启用](https://oreil.ly/disUG)在子网中新启动的实例自动分配公共 IPv4 地址。但是，如果您使用自动分配，每次实例重启后公共 IP 都会改变。与实例关联的 EIP 在重启后不会改变。

#### 注意

路由表优先考虑最具体的路由。AWS 还允许您创建比默认本地路由更具体的路由。这使您能够创建非常受控的网络流。有关路由优先级的更多信息，请参阅 AWS [讨论](https://oreil.ly/DKpNO)。

与您的实例关联的安全组不允许入站访问。如果您想允许来自互联网的入站访问到公共子网中的实例，您将不得不为此配置安全组入站规则。

具有与 IGW 关联的 `0.0.0.0/0` 路由的子网被视为公共子网。将仅需要来自公共互联网入站访问的实例放置在此类层中被视为安全最佳实践。面向最终用户的负载均衡器通常放置在公共子网中。公共子网不是应用程序服务器或数据库的理想选择。在这些情况下，您可以创建私有层或隔离层，通过适当的路由来满足您的需求，并使用 NAT 网关仅在需要出站互联网访问时将该子网流量导向互联网网关。

### 挑战

在 EC2 实例上安装 Web 服务器，修改安全组，并从您的工作站连接到该实例。有关如何使用负载均衡器为私有子网中的实例配置互联网访问的示例，请参阅[配方 2.7](#ch02.html#redirecting_http_traffic_to_https_with)。

# 2.4 使用 NAT 网关为私有子网提供出站互联网访问

## 问题

您的 VPC 中有公共子网，它们具有通往互联网网关的路由。您希望利用此设置为私有子网中的实例提供仅出站的互联网访问。

## 解决方案

在其中一个公共子网中创建 NAT 网关。然后创建一个弹性 IP 并将其与 NAT 网关关联。在与私有子网关联的路由表中，添加一条针对互联网绑定流量以 NAT 网关为目标的路由（请参阅[图 2-5](#ch02.html#internet_access_for_private_subnets_pro)）。

### 先决条件

- 具有两个 AZ 中的公共子网及关联路由表的 VPC。
- 在两个 AZ 中创建了隔离子网（我们将把这些变为私有子网）及关联路由表。
- 在隔离子网中部署了两个 EC2 实例。您需要具备连接到这些实例以进行测试的能力。

### 准备

按照[章节代码仓库](https://github.com/AWSCookbook/Networking)中本配方文件夹中的步骤操作。

![](assets/awsc_0205.png)

### 步骤

1. 创建一个用于 NAT 网关的弹性 IP：

```
ALLOCATION_ID=$(aws ec2 allocate-address --domain vpc \
    --output text --query AllocationId)
```

2. 在 AZ1 的公共子网中创建 NAT 网关：

```
NAT_GATEWAY_ID=$(aws ec2 create-nat-gateway \
    --subnet-id $VPC_PUBLIC_SUBNET_1 \
    --allocation-id $ALLOCATION_ID \
    --output text --query NatGateway.NatGatewayId)
```

3. 状态变为可用需要片刻时间；检查状态：

```
aws ec2 describe-nat-gateways \
    --nat-gateway-ids $NAT_GATEWAY_ID \
    --output text --query NatGateways[0].State
```

