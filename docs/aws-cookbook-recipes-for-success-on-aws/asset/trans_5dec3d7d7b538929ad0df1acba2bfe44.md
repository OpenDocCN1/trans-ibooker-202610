
```
$ curl https://awscookbook110-$RANDOM_STRING.s3.$AWS_REGION.amazonaws.com/index.html
<?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Access
Denied</Message><RequestId>0AKQD0EFJC9ZHPCC</RequestId><HostId>gfld4qKp9A93G8ee7VPBFrXBZV1HE3jiOb3bNB54fP
EPTihit/OyFh7hF2Nu4+Muv6JEc0ebLL4=</HostId></Error>
110-Optimizing-S3-with-CloudFront:$
```

使用 `curl` 观察您的 `index.html` 文件是否通过 CloudFront 从私有 S3 存储桶提供服务：

```
curl $DOMAIN_NAME
```

您应该会看到类似以下的输出：

```
$ curl $DOMAIN_NAME
AWSCookbook
$
```

### 清理

请按照 [章节代码仓库](https://github.com/AWSCookbook/Security) 中该配方文件夹内的步骤操作。

### 讨论

此配置允许您保持 S3 存储桶私有，并且仅允许 CloudFront 分发访问存储桶中的对象。您创建了一个[源访问身份](https://oreil.ly/aABzJ)并定义了一个存储桶策略，仅允许 CloudFront 访问您的 S3 内容。这为您提供了坚实的基础，借助 CloudFront 全球 CDN 的额外保护来保障 S3 存储桶的安全。

CDN 提供的针对[分布式拒绝服务 (DDoS) 攻击](https://oreil.ly/mtHSB)的保护值得注意，因为最终用户对您内容的请求会被导向 CloudFront 网络上延迟最低的边缘节点。这也能保护您免受托管在 S3 存储桶中的静态内容遭受 DDoS 攻击所产生的费用，因为通过 CloudFront 提供请求服务通常比直接通过 S3 更便宜。

默认情况下，CloudFront 会在您分发的默认主机名上附带一个 HTTPS 证书，用于保护流量安全。使用 CloudFront，您可以关联自己的自定义域名，从 Amazon Certificate Manager (ACM) 附加自定义证书，将 HTTP 重定向到 HTTPS，强制使用 HTTPS，自定义缓存行为，调用 Lambda 函数 (Lambda@Edge) 等。

### 挑战

为您的 CloudFront 分发添加[地理限制](https://oreil.ly/FMGj0)。

# 第 2 章 网络

## 2.0 简介

计算机视觉、物联网 (IoT) 和 AI 赋能的聊天机器人等许多令人兴奋的主题占据了头条新闻。这导致传统的核心技术被遗忘。虽然拥有许多触手可及的新功能很棒，但重要的是要意识到，如果没有可靠且安全的连接这一坚实基础，这些技术将无从实现。只有当结果能通过网络可靠地交付和访问时，数据处理才有用。容器是一种极好的应用程序部署方法，但只有当它们联网在一起时，才能为用户提供最佳体验。

AWS 中的网络服务和功能是本书涵盖的几乎所有大型服务的骨干。AWS 拥有许多出色的功能，让您能以您想要的方式、在您想要的位置连接您想要的资源。更好地理解网络将使您对云有更透彻的掌握，从而更自信地使用它。

#### 提示

跟上 AWS 的网络创新需要持续学习。每年在 AWS [re:Invent](https://reinvent.awsevents.com) 上，都会讨论许多网络服务、功能和方法。

建议观看两场来自 AWS re:Invent 的精彩网络演讲：Eric Brandwine 2015 年的[“Another Day, Another Billion Packets”](https://oreil.ly/oB1BN) 和 2019 年的年度演讲[“From One to Many: Evolving VPC Design”](https://oreil.ly/leKi9)。

在本章中，您将了解基本的云网络服务和功能。我们将只关注您在个人账户中切实可行的配方。一些高级操作（例如 AWS Direct Connect 设置）过于依赖外部因素，因此我们将其排除在外，以专注于更易上手的配方和结果。虽然本章中的一些配方可能看起来简单或简短，但它们让我们能够讨论至关重要的主题和概念。

## 工作站配置

遵循[“CLI 配方的通用工作站设置步骤”](#preface01.html#general_workstation_setup_steps_for_cli)来验证您的配置并设置所需的环境变量。然后，克隆章节代码仓库：

```
git clone https://github.com/AWSCookbook/Networking
```

## 2.1 通过创建 Amazon VPC 在云中定义您的私有虚拟网络

### 问题

您需要一个网络基础来托管云资源。

### 解决方案

您将创建一个 Amazon Virtual Private Cloud (VPC) 并为其配置一个[无类别域间路由 (CIDR) 块](https://oreil.ly/2GrP7)，如[图 2-1](#ch02.html#vpc_deployed_in_a_region) 所示。

![](assets/awsc_0201.png)

#### 步骤

1.  创建一个具有 `IPv4` CIDR 块的 VPC。我们将使用 `10.10.0.0/16` 作为地址范围，但您可以根据需要进行修改：

    ```
    VPC_ID=$(aws ec2 create-vpc --cidr-block 10.10.0.0/16 \
        --tag-specifications 
    'ResourceType=vpc,Tags=[{Key=Name,Value=AWSCookbook201}]' \
        --output text --query Vpc.VpcId)
    ```

    #### 注意

    创建 VPC 时，[文档](https://oreil.ly/uIfsH) 指出 VPC `IPv4` CIDR 的最大块大小为 `/16` 网络掩码（65,536 个 IP 地址）。最小为 `/28` 网络掩码（16 个 IP 地址）。

#### 验证检查

使用此命令验证 VPC 的状态是否为 `available`：

```
aws ec2 describe-vpcs --vpc-ids $VPC_ID
```

您应该会看到类似以下的输出：

```
{
  "Vpcs": [
  {
    "CidrBlock": "10.10.0.0/16",
    "DhcpOptionsId": "dopt-<<snip>>",
    "State": "available",
    "VpcId": "vpc-<<snip>>",
    "OwnerId": "111111111111",
    "InstanceTenancy": "default",
    "CidrBlockAssociationSet": [
      {
        "AssociationId": "vpc-cidr-assoc-<<snip>>",
        "CidrBlock": "10.10.0.0/16",
        "CidrBlockState": {
        "State": "associated"
        }
      }
    ],
    "IsDefault": false,
<<snip>>
...
```

### 清理

请按照 [章节代码仓库](https://github.com/AWSCookbook/Networking) 中该配方文件夹内的步骤操作。

### 讨论

#### 警告

以下是为您的 VPC 仔细选择 CIDR 块的两个重要原因：

*   一旦 CIDR 块与 VPC 关联，[它就无法修改（尽管可以扩展）](https://oreil.ly/xUB24)。如果您想修改 CIDR 块，则需要删除并重新创建它（以及其中的所有资源）。

*   如果 VPC 通过对等连接（请参阅[配方 2.11](#ch02.html#peering_two_vpcs_together_for_inter_vpc)）或网关（例如 Transit Gateway 和 VPN）连接到其他网络，拥有重叠的 IP 范围将导致不必要的问题。

您可以使用 `aws ec2 associate-vpc-cidr-block` 命令指定额外的 `IPv4` 空间，以向 VPC 添加 `IPv4` 空间。当 IP 空间因使用和规划不足而变得稀缺时，知道您不需要为 VPC 分配大块空间是件好事，特别是如果您不确定是否会用完所有空间时。

以下是将额外的 `IPv4` CIDR 块关联到您的 VPC 的示例：

```
aws ec2 associate-vpc-cidr-block \
    --cidr-block 10.11.0.0/16 \
    --vpc-id $VPC_ID
```

#### 注意

除了 `IPv4`，VPC 还支持 [IPv6](https://oreil.ly/sBL99)。您可以通过指定 `--amazon-provided-ipv6-cidr-block` 选项来配置 Amazon 提供的 `IPv6` CIDR 块。以下是创建带有 `IPv6` CIDR 块的 VPC 的示例：

```
aws ec2 create-vpc --cidr-block 10.10.0.0/16 \
    --amazon-provided-ipv6-cidr-block \
    --tag-specifications
'ResourceType=vpc,Tags=[{Key=Name,Value=AWSCookbook201-IPv6}]'
```
