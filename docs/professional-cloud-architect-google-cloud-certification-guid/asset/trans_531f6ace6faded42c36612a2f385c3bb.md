

- `default-allow-rdp`: 允许来自任何源到 VPC 中任何实例的 TCP 端口 3389 入站连接
- `default-allow-icmp`: 允许来自任何源到 VPC 中任何实例的入站 ICMP 流量：

| 名称 | 类型 | 目标 | 过滤条件 | 协议/端口 | 操作 | 优先级 | 网络 |
|---|---|---|---|---|---|---|---|
| default-allow-http | 入站 | http-server | IP 范围：0.0.0.0/0 | tcp:80 | 允许 | 1000 | default |
| default-allow-https | 入站 | https-server | IP 范围：0.0.0.0/0 | tcp:443 | 允许 | 1000 | default |
| default-allow-icmp | 入站 | 应用于所有 | IP 范围：0.0.0.0/0 | icmp | 允许 | 65534 | default |
| default-allow-internal | 入站 | 应用于所有 | IP 范围：10.128.0.0/9 | tcp:0-65535 udp:0-65535 icmp | 允许 | 65534 | default |
| default-allow-rdp | 入站 | 应用于所有 | IP 范围：0.0.0.0/0 | tcp:3389 | 允许 | 65534 | default |
| default-allow-ssh | 入站 | 应用于所有 | IP 范围：0.0.0.0/0 | tcp:22 | 允许 | 65534 | default |

图 10.12 – 防火墙规则

还有针对 HTTP 和 HTTPS 流量的预定义规则。这些规则的目标是标签。您可以将这些标签分配给您的 VM 以允许此类型流量来自任何源。

## 隐含规则

有两个隐含的防火墙规则，优先级最低为 65535，具体如下：

- **拒绝入站规则** 源为 0.0.0.0/0
- **允许出站规则** 目标为 0.0.0.0/0

此时，可能有必要澄清一下，65535 是最低可能的优先级，0 是最高。此外，IP 范围 0.0.0.0/0 等同于所有 IPv4 地址。规则无法删除，但可以被具有更高优先级的任何规则覆盖。

## 始终允许的流量规则

有某些类型的流量始终允许从 VM 发出，具体如下：

- 到元数据服务器的流量
- DHCP 流量
- DNS 流量
- NTP 流量

## 始终拒绝的规则

还有一些类型的流量始终被阻止，具体如下：

- GRE 流量
- 除 TCP、UDP、ICMP、AH、ESP、SCTP 和 IPIP 之外的协议
- TCP 端口 25 上的出站 SMTP 流量

## 用户定义规则

最后，用户定义规则是您将定义的规则，以允许应用程序所需的通信，而该通信不符合默认、始终允许或隐含规则：

![](img/de2c97ab017f36b3750a5bade111edd7_314_0.png)

图 10.13 – 创建防火墙规则

这些规则可以通过转到导航菜单，VPC 网络 | 防火墙，并点击创建防火墙规则来创建。

## 防火墙日志

防火墙日志允许您验证防火墙规则是否正常运行。它可以为新的和现有的防火墙规则启用。日志注入到 Google Cloud 操作套件中（以前称为 Stackdriver）。

## 分层防火墙策略

分层防火墙策略允许我们为组织创建和执行一致的策略。它们可以分配给整个组织或单个文件夹。与 VPC 防火墙规则一样，分层防火墙策略包含可以拒绝或允许连接的规则，但还可以借助 goto_next 操作将评估委托给低一级的策略或 VPC 网络防火墙规则。这允许组织范围的管理员在一个地方管理防火墙规则。

我们应该注意，在组织或文件夹节点级别创建策略不会自动将规则应用于节点 – 策略需要与组织中的节点关联，并且单个策略可以与多个节点关联。策略是防火墙规则的容器，当我们将策略与组织或文件夹关联时，规则会立即应用。我们可以为节点交换策略，这会原子地交换应用于该节点下 VM 实例的所有防火墙规则。与组织节点关联的规则首先被评估，并且组织中的任何文件夹和 VPC 网络都会继承此策略。如果策略应用于文件夹，则该文件夹中的所有文件夹和 VPC 网络都将继承其关联的策略。分层策略不能与项目关联。可以在以下图表中看到可以应用防火墙规则的层级：

![](img/de2c97ab017f36b3750a5bade111edd7_315_0.png)

图 10.14 – 分层策略继承（来源：https://cloud.google.com/vpc/docs/firewall-policies）

分层防火墙策略规则必须具有指定的优先级，该优先级在每个策略中是唯一的。这与 VPC 防火墙规则不同，后者可以有多个具有相同优先级的规则。此外，防火墙策略规则没有名称，而是策略本身具有 ID 和名称，每个规则都有唯一的优先级数字。然后按优先级顺序评估规则，从最高优先级规则开始，表示为最低数字。例如，优先级号为 100 的规则将覆盖优先级为 200 的规则。

要从 Google Cloud Shell 创建防火墙策略，请使用以下代码并替换您的组织 ID 和文件夹 ID：

```
gcloud compute firewall-policies create \
    [--organization ORG_ID] | --folder FOLDER_ID] \
    --short-name SHORT_NAME
```

然后我们通过使用以下命令向策略添加规则。我们使用了示例组织 ID：

```
gcloud compute firewall-policies rules create 1000 \
    --action=allow \    --description="allow-scan-probe" \
    --layer4-configs=tcp:123 \    --firewall-policy=example-firewall-policy \    --organization=123456789\    --src-ip-ranges=10.100.0.1/32
```

最后，将防火墙策略与组织节点关联。我们使用了示例组织 ID：

```
gcloud compute firewall-policies associations create \
    --firewall-policy=example-firewall-policy \
    --organization=123456789
```

在下一节中，我们将查看私有访问选项。

## 私有访问选项

Google 提供了各种选项，让 VM 实例无需外部 IP 地址即可访问受支持的 API 和服务。根据您的用例，您可以配置一个或全部可用选项，它们彼此独立运行。作为高层次概述，以下是使用每个选项的原因：

- **面向 Google API 的 Private Service Connect**：用于在本地和 GCP 资源不需要外部 IP 地址时，使用 VPC 网络中的端点连接到 Google API 和服务。
- **Private Google Access**：用于在不为 GCP 资源分配外部 IP 地址的情况下连接到 Google API 和服务。
- **面向本地主机的 Private Google Access**：用于在本地主机不需要外部 IP 地址时，通过 VPC 网络连接到 Google API 和服务。
- **面向服务的 Private Service Connect**：用于在不为 GCP 资源分配外部 IP 地址的情况下连接到另一个 VPC 网络中的受支持服务。
- **Private Services Access**：用于在不为 GPC 或第三方资源分配外部 IP 地址的情况下连接到特定的 Google 或第三方服务。
- **Serverless VPC Access**：用于使用内部 IP 地址直接从无服务器 Google 服务连接到 VPC 网络中的资源。

使用私有访问时还存在 DNS 方面的考虑，因此了解每个选项及其影响非常重要。例如，如果我们想使用 **面向本地主机的 Private Google Access** 选项，我们的本地网络必须配置 DNS 区域和记录，以便 Google 域名解析为 `private.googleapis.com` 或 `restricted.googleapis.com` 的 IP 地址集。

> **重要说明**
> 欲了解更多详情，请查看 Google 文档 https://cloud.google.com/vpc/docs/private-access-options。

## 摘要

在本章中，我们介绍了 GCP 中的基本网络概念。我们学习了 VPC、负载均衡、防火墙、DNS 和混合连接。确保您了解如何将 VM 连接到 GCP 网络，以及如何保护和负载均衡它们。现在您了解了连接到本地数据中心的各种方式，我们建议您获得一些实践经验。设置您的第一个 VPC，创建防火墙规则，并部署和管理一个带有您选择的负载均衡器的 VM 实例组。如果您在操作中遇到任何问题，请返回本书并查看本章或 *第 4 章，使用 Google Compute Engine*。

在 *第 11 章，探索 GCP 中的存储和数据库选项 – 第 1 部分* 和 *第 12 章，探索 GCP 中的存储和数据库选项 – 第 2 部分* 中，我们将看到如何存储数据。这对于有状态工作负载至关重要。

