

![](img/fb3bebce7b5f593bca1a7f8a1371be06_759_0.png)

下图展示了一个用户集群部署在单个子网中，捆绑式负载均衡器运行在专用工作节点上的架构示例：

图 21.7. 运行在专用工作节点上的负载均衡器

![](img/fb3bebce7b5f593bca1a7f8a1371be06_760_0.png)

# 手动负载均衡器模式

如果选择手动负载均衡器模式，Anthos 安装不会部署捆绑式负载均衡器，而你需要负责部署外部负载均衡解决方案。

下图展示了一个用户集群部署在单个子网中，并在手动负载均衡器模式下配置了外部负载均衡器的架构示例：

图 21.8. 手动负载均衡架构

来自 Anthos Ready 存储合作伙伴。你可以在此页面找到合作伙伴和已验证存储解决方案的列表：
https://cloud.google.com/anthos/docs/resources/partner-storage.

Anthos on bare metal 还捆绑了 sig-storage-local-static-provisioner，它为每个节点提供挂载点并为每个挂载点创建本地持久卷（PV）。这存在局限性，本地 PV 仅应用于非生产环境或特定的高级用例。

# 可观测性

Google Cloud Operations 可用于收集 Anthos on bare metal 的日志和监控指标，就像其他 Anthos 部署环境一样。默认情况下，系统组件日志发送到 Cloud Logging，系统组件指标发送到 Cloud Monitoring。通过在集群配置中启用，Cloud Operations 还可以收集应用日志和指标。

作为 Cloud Operations 的替代方案，如果需要，也可以使用其他解决方案，例如 Prometheus/Grafana、Elastic Stack、Sysdig、Datadog 或 Splunk。

# 身份集成

Anthos on bare metal 可以通过 Anthos Identity Service 身份验证代理集成，支持 OpenID Connect（OIDC）或 LDAP 的任何身份提供商，以使用现有用户身份和凭据管理用户和组对集群的身份验证。

通过此集成，可以使用组织中创建、启用和禁用账户的标准流程来管理对 Anthos on bare metal 集群的访问。

Kubernetes 基于角色的访问控制可用于将特定角色绑定到身份提供商中定义的用户和组，以便授权他们对特定资源执行特定操作。

# 21.3 安装和配置概述

在本节中，我们将提供部署集群所需要求的概述。在尝试部署集群之前，了解要求是一项重要步骤，我们将在下一节中讨论。

Google 已使部署 Anthos on bare metal 成为一个简单过程。与 VMware 部署类似，你在配置文件中配置集群选项，并使用名为 bmctl 的单个二进制可执行文件完成部署。

Anthos 要求你满足软件和硬件方面的要求。大多数组织都能轻松满足这些要求，但在部署之前，你应始终验证你的基础设施是否满足所有要求。

# 21.3.1 操作系统和软件要求

如介绍和架构部分所述，Anthos on bare metal 安装在由客户提供和配置的服务器上。服务器可以是物理机或虚拟机，只要它们运行受支持的操作系统之一，并配置为满足 Anthos 要求即可。

用作 Anthos on bare metal 节点的服务器需要具有以下操作系统之一：

- CentOS 8.1/8.2/8.3/8.4
- Red Hat Enterprise Linux（RHEL）8.1/8.2/8.3/8.4
- Ubuntu 18.04 & 20.04

每个受支持版本都需要略有不同的配置。如果使用 RHEL 或 CentOS，firewalld 服务必须配置为允许 TCP 和 UDP 端口的流量，这些将在网络要求部分下的内部连接要求中介绍。在这些操作系统上，如果 SELinux 以 enforcing 模式启用，则在 Anthos on Bare Metal 设置期间会配置容器隔离和安全策略。如果在 Ubuntu 上运行节点，则必须禁用 Uncomplicated Firewall（UFW）服务。

时间对集群非常重要。为确保所有节点时钟同步，所有服务器都需要配置并启用 NTP 服务。

最后，由于安装会建立到节点的 SSH 连接，因此需要 SSH 密钥对以 root 权限访问每个节点。

# 21.3.2 硬件容量要求

Anthos on bare metal 可在与受支持操作系统之一兼容的任何硬件上运行。

Anthos on bare metal 安装所需的节点数量取决于所选的部署和负载均衡模型，如架构段落所述。所需的工作节点数量将取决于集群将托管的应用的容量要求。

下表描述了每个节点（无论其角色如何）使用默认配置文件时的最低和推荐硬件要求，不包括应额外添加的应用容量要求。

| 资源 | 最低 | 推荐 |
|---|---|---|
| CPUs / vCPUs | 4 核 | 8 核 |
| RAM | 16 GB | 32 GB |
| 存储 | 128 GB | 256 GB |

下表描述了在 Anthos on Bare Metal 版本 1.8 中引入的 **边缘配置文件** 的硬件要求

| 资源 | 最低 | 推荐 |
|---|---|---|
| CPUs / vCPUs | 2 核 | 8 核 |
| RAM | Ubuntu: 4 GB<br>CentOS/RHEL: 6 GB | Ubuntu: 8 GB<br>CentOS/RHEL: 12 GB |
| 存储 | 128 GB | 256 GB |

# 21.3.3 管理工作站

除了节点外，建议额外配备一台工作站来运行安装工具。

该工作站必须运行与集群节点相同的 Linux 操作系统，并配置 Docker 19.03 或更高版本以由非 root 用户管理。

除 Docker 外，该机器还必须具有：

- 已安装 gcloud、anthos-auth 和 kubectl
- 超过 50 GB 的可用磁盘空间
- 到所有集群节点机器的 L3 连接
- 通过 SSH 使用私钥以无密码 root 访问方式访问所有集群节点机器。访问可以是直接的，也可以是通过 sudo。
- 访问控制平面 VIP。

# 21.3.4 网络要求

Anthos 对外部和内部网络连接有不同的要求。

# 外部连接要求

所有 Anthos on bare metal 节点都需要出站 https 连接到互联网，以便：

- 注册到 GCP 控制台并通过 GKE Connect 从那里进行管理
- 将指标和日志发送到 Cloud Operations 端点
- 从 Google Container Registry 拉取镜像

此连接可以使用公共互联网、http 代理或 Google Cloud VPN 或专用互连等私有连接。

# 内部连接要求

本节将详细介绍集群的内部网络要求。集群的每个组件都有不同的要求，下表列出了集群节点流量使用的特定连接端口：

# 控制平面节点

| 协议 | 方向 | 端口范围 | 用途 | 使用者 |
|---|---|---|---|---|
| UDP | 入站 | 6081 | GENEVE 封装 | 自身 |
| TCP | 入站 | 22 | 管理集群节点的配置和更新 | 管理工作站 |
| TCP | 入站 | 6444 | Kubernetes API 服务器 | 所有 |
| TCP | 入站 | 2379 - 2380 | etcd 服务器客户端 API | kube-apiserver 和 etcd |
| TCP | 入站 | 10250 | kubelet API | 自身和控制平面 |
| TCP | 入站 | 10251 | kube-scheduler | 自身 |
| TCP | 入站 | 10252 | kube-controller-manager | 自身 |
| TCP | 双向 | 4240 | CNI 健康检查 | 所有 |

# 工作节点

| 协议 | 方向 | 端口范围 | 用途 | 使用者 |
|---|---|---|---|---|
| TCP | 入站 | 22 | 用户集群节点的配置和更新 | 管理集群节点 |
| UDP | 入站 | 6081 | GENEVE 封装 | 自身 |
| TCP | 入站 | 10250 | kubelet API | 自身和控制平面 |
| TCP | 入站 | 30000 - 32767 | NodePort 服务 | 自身 |

