## 21 Anthos on bare metal

by **Giovanni Galloro**

### This chapter covers

- An introduction to Anthos on bare metal
- Deployment options
- Networking architecture
- Storage architecture
- Installing and configuring Anthos on bare metal

Anthos 的原始版本要求你在 vSphere 基础设施上部署集群。没有在不同超visor 或物理服务器上部署的选项。对于初始版本，这是有意义的，因为 vSphere 被众多企业使用，并且它允许企业利用其现有基础设施和技能。

随着容器和 Kubernetes 的用例增长，很明显组织希望并且需要更灵活的部署选项。

为了解决这些额外的用例，Google 将 Anthos 扩展到包括裸机部署模型。一个需要强调的点是，你不必将 Anthos 在裸机上部署到实际物理服务器。裸机模型允许你部署到任何受支持的操作系统，无论是物理服务器还是虚拟机——甚至运行在 Hyper-V 或 KVM 上的 VM。

你可以将裸机选项视为更像“自带 Linux”的部署模型。与 vSphere 部署模型中提供的用于部署节点的设备不同，你需要在部署 Anthos on bare metal 之前提供可用的服务器。

现在，让我们向你介绍 Anthos on bare metal。

### 21.1 Introduction to Anthos on Bare Metal

如前几章所述，Anthos 是一个为多种部署环境设计的平台，如下图所示：

Figure 21.1. Anthos deployment environments

![](img/fb3bebce7b5f593bca1a7f8a1371be06_746_0.png)

Anthos on bare metal 是一种部署选项，可在客户提供操作系统的物理服务器上运行 Anthos。Anthos on bare metal 随附内置的网络、生命周期管理、诊断、健康检查、日志记录和监控。此外它支持 CentOS、Red Hat Enterprise Linux (RHEL) 和 Ubuntu，均由 Google 验证。使用 Anthos on bare metal，可以使用组织的标准硬件和操作系统镜像，利用现有投资，这些投资会自动检查并验证是否符合 Anthos 基础设施要求。

### 21.1.1 Comparing Anthos on-prem deployment options

现在你有多个选项可以在本地部署 Anthos，如何决定哪种部署最适合你的组织？两种选项各有优缺点，决定哪种部署最适合你需要考虑你的个人需求。在下面的表格中，我们提供了每个选项的一些优缺点概述。

| Anthos on VMware | Anthos on bare metal |
|---|---|
| **Runs on VMware**<br>Best for organizations who want:<br>- vSphere as a corporate standard<br>- Share hardware across multiple teams or clusters (Dev/Test)<br>- Integrated OS lifecycle management<br>- Self Healing/Autoscaling for clusters | **Runs on bare-metal or on-prem IaaS**<br>Best for organizations who want:<br>- Reduce cost and complexity (due to elimination of vSphere license)<br>- Low latency workloads (Telco & HPC)<br>- Unlock new use cases for edge computing with simplified<br>- Use existing corporate standards (i.e., logging and monitoring standards) |
| **Deployment Disadvantages** | |
| - Requires additional VMware licensing<br>- Customization of node appliance is not supported<br>- Limited GPU supported through passthrough<br>- Requires additional training for either the VM teams, or the Kubernetes support teams<br>- The vSphere scheduler and K8s scheduler are not aware of each other<br>- Storage DRS can break your cluster | - Requires planning to right size nodes to avoid wasted resources<br>- Does not include a storage provisioner other than local host storage<br>- Difficult to scale in most enterprises<br>- No node auto-healing |

### 21.2 Anthos Bare Metal Architecture

虽然 Anthos on VMware 集群节点是从 Google 提供的预配置 VM 镜像部署的，但 Anthos on Bare metal 依赖客户提供受支持的操作系统版本，由客户自行管理和修补。如图 21.2 所示，操作系统可以直接安装在物理服务器上，或安装在运行在任何虚拟化平台（KVM、OpenStack）上的 VM 上，只要支持与 Anthos on bare metal 兼容的 Linux 发行版之一。

**Figure 21.2. Anthos on prem deployment options shared responsibility model**





## 21.2.1 集群架构

在本节中，我们将讨论裸机集群的架构。你在 Anthos on VMware 部署中了解到的许多架构在裸机上也相同，但是，裸机选项与 VMware 模型有一些架构差异。

| 客户提供并管理 | Google 提供，客户管理 |
|---|---|
| Anthos | Anthos |
| Red Hat Linux、CentOS、Ubuntu | Ubuntu、COS |
| 物理机、虚拟机、OpenStack | 虚拟机、VMware |
| Anthos on bare metal | Anthos on VMware |

### 集群角色

Anthos Bare Metal 安装包含两类集群：

- **用户集群**：应用程序部署的地方；它们包括控制平面节点和运行容器化应用实例的工作节点。
- **管理集群**：管理一个或多个用户集群的集群；它通过 Anthos on bare metal 特定的 operator 来安装、更新、升级和删除用户集群，该 operator 通过两个自定义资源配置：`Cluster` 和 `Node Pool`。

管理集群仅包含用于运行管理安装组件的控制平面节点。它还保存一些安全敏感数据，包括用于访问节点操作系统的 SSH 密钥和 GCP 服务账号密钥。

与 Anthos on VMware 不同，用户集群控制平面节点与管理集群是*解耦*的。

### 高可用性

用户集群或管理集群的控制平面可以以高可用性（HA）模式运行，这样控制平面节点故障不会影响集群操作。此模式需要三个或更多控制平面节点。

如果不需要高可用性，可以在每个集群中运行单个控制平面节点，但这只能用于非生产工作负载。

除了控制平面，还需要考虑工作节点的高可用性。对于需要高可用性的应用程序，至少需要两个工作节点。与控制平面一样，绝不应在没有 HA 的情况下为工作节点运行生产工作负载。

### 部署模型

从这里开始，你将看到 Anthos 在 VMware 和裸机上的部署方式之间的差异。Anthos on Bare Metal 提供了几种不同的部署模型，其灵活性可满足不同组织的要求：

#### 独立集群部署

在独立集群部署模型中，单个集群同时充当管理集群和用户集群。由于此模型不需要单独的管理集群，因此在 HA 设置中可以节省三个节点。这在每个集群独立管理或每个部署位置只需要一个集群的场景中很有帮助——例如，边缘用例或隔离网络。

从安全角度来看，你需要考虑到用户工作负载将与控制平面在同一个集群上运行。你需要仔细考虑保护集群，以保护节点 SSH 密钥和 GCP 服务账号密钥等信息。实施 RBAC 策略、OPA 策略、网络策略和适当的审计将有助于保护集群。

为了给这些类型的部署提供更大的灵活性，从 Anthos 1.8 版开始，Google 将每个集群支持的最小节点数从 2 个减少到 1 个，并为独立集群引入了新的 edge profile，进一步降低了硬件要求。

**图 21.3 独立集群部署**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_754_0.png)

#### 多集群部署

这与 Anthos on VMware 使用的部署模型相同。在多集群部署模型中，你有一个管理多个用户集群的管理集群；如果你需要集中管理部署在同一数据中心的一组集群，此部署模型非常有用。

**图 21.4 多集群部署**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_756_0.png)

如你所见，Anthos on bare metal 比 VMware 部署模型提供了更大的部署灵活性。不过，额外的灵活性并不止于此；在下一节中，我们将讨论网络架构的更新。

### 网络架构

裸机选项的网络架构与 VMware 模型不同，这是合理的。请记住，在 VMware 部署中，你可以使用外部负载均衡器，也可以使用捆绑的负载均衡器 Seesaw。如果使用 Seesaw 选项，Anthos 将向你的 VMware 环境部署一个预配置的虚拟机，或在 HA 模式下部署多台虚拟机。

在裸机部署中，任何组件都没有来自 Google 的设备或 VM 镜像。不过不用担心：Google 使用 HAProxy 和 MetalLB 等其他组件解决了这个问题。

### 负载均衡

Anthos on Bare Metal 需要 L4 负载均衡来公开控制平面端点、入口端点和使用 LoadBalancer 类型 Service 的应用程序。负载均衡器的职责是将流量路由并均衡到相应的节点。

Anthos on Bare Metal 可以通过捆绑的 L4 负载均衡器（捆绑负载均衡器模式）提供所需的负载均衡能力，或者，你也可以使用由客户提供和配置的外部负载均衡解决方案（手动负载均衡器模式）。

无论你为 L4 负载均衡选择哪种选项，在安装过程中都会部署基于 Envoy 的 Istio 入口网关，并通过使用 L4 负载均衡器的 LoadBalancer 类型 Service 将其公开。此 Envoy 部署用于为应用程序提供应用代理能力，通过标准 Kubernetes Ingress 对象公开它们。

#### 捆绑负载均衡器模式

首先，让我们讨论控制平面的负载均衡。如果选择捆绑负载均衡，Anthos on bare metal 会在集群安装期间部署 L4 负载均衡器，无需你提供外部负载均衡器。负载均衡器可以在专用的工作节点池上运行，也可以位于与控制平面相同的节点上；无论哪种情况，负载均衡器节点都必须位于同一子网中。

从 Anthos 1.9 开始，Google 改变了 L4 负载均衡器的部署方式。以前，HAProxy 容器镜像是作为标准 Docker 容器（即不受 Kubernetes 控制）部署到节点上，并以 systemd 服务运行。从 1.9 开始，Keepalived 和 HAProxy 容器已更新为在负载均衡器节点上以静态 Kubernetes pod 运行。

HAProxy 仅用于对控制平面进行负载均衡。为了向数据平面提供 L4，Anthos 部署了一个名为 MetalLB 的流行开源解决方案。MetalLB 将为集群中使用 LoadBalancer 类型部署的任何服务处理请求。

回顾一下捆绑负载均衡器组件：

- **控制平面负载均衡：** 控制平面虚拟 IP 地址（VIP）将流量路由到运行在控制平面节点上的 Kubernetes API 服务器，该 VIP 通过负载均衡器节点上以 Kubernetes pod 形式运行的 HAProxy 负载均衡器公开，同时还有一个容器化的 keepalived 服务来管理 HAProxy 的高可用性。
- **数据平面负载均衡：** 为应用程序创建的 LoadBalancer 类型 Service 对象以及随 Anthos on bare metal 部署的 Istio 入口网关，通过运行在负载均衡器节点上的 Anthos 管理的 MetalLB 部署公开。LoadBalancer 类型 Service 的 IP 地址可以从预定义池中自动分配，并且与负载均衡器节点部署在同一个子网中。

控制平面负载均衡组件（HAProxy + keepalived）和数据平面负载均衡组件（MetalLB）一起在指定节点（集群控制平面节点或专用负载均衡器工作节点）上运行。

下图显示了在单个子网中部署的用户集群的架构示例，其中捆绑负载均衡器运行在控制平面节点上：

**图 21.6 运行在控制平面节点上的负载均衡器**



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





# 负载均衡器节点

| 协议 | 方向 | 端口范围 | 用途 | 使用者 |
|---|---|---|---|---|
| TCP | 入站 | 22 | 用户集群节点的开通和更新 | 管理集群节点 |
| UDP | 入站 | 6081 | GENEVE 封装 | 自身 |
| TCP | 入站 | 443 | 集群管理 | 全部 |
| TCP | 双向 | 4240 | CNI 健康检查 | 全部 |
| TCP | 入站 | 7946 | Metal LB 健康检查 | 负载均衡器节点 |
| UDP | 入站 | 7946 | Metal LB 健康检查 | 负载均衡器节点 |

# 多集群端口要求

| 协议 | 方向 | 端口范围 | 用途 | 使用者 |
|---|---|---|---|---|
| TCP | 入站 | 22 | 集群节点的开通和更新 | 所有节点 |
| TCP | 入站 | 443 | 新增集群的 Kubernetes API 服务器 | 控制平面和负载均衡器节点 |

介绍了网络要求之后，接下来我们来看配置集群负载均衡器的其他要求。

# 负载均衡要求

在裸金属上安装 Anthos 之前，你需要选择负载均衡架构（手动 vs 捆绑），并且在捆绑的情况下，确定你的负载均衡器是安装在控制平面节点上还是专用工作节点上。

无论选择哪种解决方案，都必须预留以下虚拟 IP 地址：

- **每个集群的控制平面需要 1 个 VIP。**
  如果使用捆绑负载均衡器，则会根据安装期间定义的配置自动创建该 VIP。
  如果使用手动负载均衡器，则需要手动将其关联到包含集群所有控制平面节点 IP 地址的后端服务器组。控制平面监听的后端端口为 6444。

- **每个用户集群的 Ingress 服务需要 1 个 VIP。**
  如果使用捆绑负载均衡器，则会根据安装期间定义的配置自动创建该 VIP。
  如果使用手动负载均衡器，则需要手动配置为与集群中 gke-system 命名空间内创建的 istio-ingress Service 相同的 IP 地址，并将其关联到包含集群节点 IP 地址的后端服务器组。后端端口将是 istio-ingress Service 的 NodePort。如果希望 ingress 网关同时处理 HTTP 和 HTTPS 流量，则可能需要为每个端口配置一个 VIP（和后端池）。

- **集群中创建的每个 LoadBalancer 类型的 Service 需要 1 个 VIP。**
  如果使用捆绑负载均衡器，则会根据安装期间定义的地址池自动分配这些 VIP。
  如果使用手动负载均衡器，则需要手动配置为与分配给 Service 对象的 IP 地址相同，并将其关联到包含所有集群工作节点 IP 地址的后端服务器组。后端端口将是 Service 对象的 NodePort。

如果集群部署将使用捆绑负载均衡器：

- 负载均衡节点需要处于同一 L2 网络中，而其他连接（包括工作节点）只需要 L3 连通性。
- 所有 VIP 必须位于负载均衡器机器子网内，并且完全可路由。
- 负载均衡器子网的网关必须监听免费 ARP 消息，并将 ARP 数据包转发到负载均衡器节点。

接下来，下一部分将介绍 Google Cloud Platform 的要求。

# 21.3.5 Google Cloud Platform 要求

Anthos on bare metal 安装有一些 GCP 项目要求，包括所需的 API、服务账号和角色。

### 所需的 GCP API

为了成功部署，集群将连接到的项目必须启用一些 API。这可以手动完成，也可以在执行使用 bmctl 的部署时通过一个选项自动启用。

用于安装的 GCP 项目中必须启用以下 API：

- anthos.googleapis.com
- anthosaudit.googleapis.com
- anthosgke.googleapis.com
- cloudresourcemanager.googleapis.com
- container.googleapis.com
- gkeconnect.googleapis.com
- gkehub.googleapis.com
- serviceusage.googleapis.com
- servicemanagement.googleapis.com
- stackdriver.googleapis.com
- monitoring.googleapis.com
- logging.googleapis.com
- opsconfigmonitoring.googleapis.com

如果在运行部署之前有任何 API 未启用，预检将检测到缺失的 API 并阻止部署继续。

### 所需的服务账号和角色

在裸金属上部署 Anthos 之前的另一个要求是创建所需的服务账号和所需角色。

虽然可以使用一个拥有所有角色的账号，但这被认为是不良的安全实践。你的组织会有自己的安全要求，但建议将所有账号创建为不同的服务账号。

你可以选择手动创建服务账号，也可以使用 bmctl 安装工具的某个参数在安装期间创建它们。

Anthos on bare metal 需要以下具有指定角色的 Google Cloud 服务账号：

- 用于向 GCP Console 注册集群的服务账号，具有 GKE Hub Admin IAM 角色。
- 用于维护集群与 Google Cloud 之间连接的服务账号，具有 GKE Connect Agent IAM 角色。
- 用于向 Google Cloud 运维套件发送日志和指标的服务账号，具有以下 IAM 角色：
  - Logs Writer
  - Monitoring Metric Writer
  - Stackdriver Resource Metadata Writer
  - Monitoring Dashboard Configuration Editor
  - Ops Config Monitoring Resource Metadata Writer

如果你想在安装期间使用 bmctl 工具启用上述 API 并创建所需的 GCP 服务账号，则用于安装的账号必须具有 Project Owner/Editor 角色，或者至少分配以下角色：

- Service Account Admin
- Service Account Key Admin
- Project IAM Admin
- Compute Viewer
- Service Usage Admin

最后，还有一个 Anthos 将用于集群指标的要求需要说明。

### 云指标要求

为了向 Google Cloud 的运维套件发送指标，除了上面列出的服务账号之外，还必须在 GCP 项目中创建一个 Cloud Monitoring 工作区。

# 21.4 创建集群

满足所有要求后，你就可以继续创建集群了。

以下各节假定所有安装任务都是从一台满足“管理工作站（Admin Workstation）”一节所述要求的机器上执行的。

# 21.4.1 创建管理集群、混合集群或独立集群

正如我们已经详细说明的，Anthos on bare metal 可以使用几种不同的集群模型进行部署，包括独立的管理集群/用户集群、混合集群或独立集群。在本节中，我们将讨论每种模型的部署过程。

### 安装流程概述

在特定部署环境中安装的第一个集群，无论选择哪种模型，都是使用 bmctl 工具安装的。同一环境中的其他用户集群可以通过应用 Anthos on bare metal 用户集群配置文件来创建，该文件与第一个集群配置类似，只需稍作修改。

创建第一个集群的概要步骤如下：

1. 下载 bmctl 工具
2. 使用 bmctl 创建集群配置模板文件
3. 根据需要修改配置文件
4. 运行 bmctl 创建集群

下图展示了使用 bmctl 创建第一个集群的流程：

![](img/fb3bebce7b5f593bca1a7f8a1371be06_773_0.png)

![](img/fb3bebce7b5f593bca1a7f8a1371be06_774_0.png)

前四步由用户发起，当执行第 4 步（即 bmctl create cluster 命令）时，将执行以下步骤：

- **验证配置**：检查集群配置文件，以确认规格格式正确、没有 IP 地址重叠、服务账号密钥可用，并且集群尚未在 GCP Console 中注册。
- **创建 KinD 集群**：作为设置的一部分，bmctl 首先在管理工作站上创建一个临时的 KinD（Kubernetes In Docker）集群，在该集群中，会根据配置文件规格创建管理集群所需的一些资源，例如集群和节点池对象或包含 SSH 和服务账号密钥的 Secret 对象。
- **预检**：对集群机器和网络要求进行检查，例如操作系统版本和配置、文件系统可用空间、GCP API 端点的可达性。
- **开通裸金属集群**：将二进制文件复制到目标节点并


安装执行包括节点初始化和加入。

- **安装附加组件：** 附加组件如 GKE Connect Agent、日志和监控组件、裸金属操作器、MetalLB 均被安装。
- **Pivot（切换）：** 将裸金属资源从 KinD 集群迁移到已配置集群的过程。之后，K8s 资源将在 KinD 集群中被删除。

最后，让我们深入探讨如何创建集群！

## 登录 GCP 并下载 bmctl 工具

请记住，Google 提供了一个名为 bmctl 的工具，用于在裸金属上部署 Anthos。在用于部署集群的工作站上，按照以下步骤下载 bmctl 工具（在我们的示例中，我们假设工作目录为 ~/baremetal）

- 1. 使用 gcloud 登录，执行 `gcloud auth application-default login`，使用一个拥有“安装要求”段落中“安装账户角色要求”部分所描述角色的用户。
- 2. 从文档中提供的 URL 或存储桶下载 bmctl 工具——在撰写本文时，bmctl 可用于 Anthos 1.9，地址为：

```
gsutil cp gs://anthos-baremetal-release/bmctl/1.9.0/linux-amd64/b
```

- 3. 使 bmctl 可执行：
- 4. `chmod a+x bmctl`

现在我们已经有了可执行的 bmctl，可以继续创建集群配置了。

## 创建集群配置

要部署集群，我们需要一个包含所有部署参数和选项的集群配置文件。bmctl 工具可以通过 *create config* 选项为我们创建一个新的配置文件：

```
bmctl create config -c CLUSTER_NAME --project-id=CLOUD_PROJECT_ID
```

其中，_CLUSTER_NAME_ 是您要赋予集群的名称，_CLOUD_PROJECT_ID_ 是您要在 Anthos on bare metal 中使用的项目的项目 ID。

如果您尚未启用所需的 API，可以在上述命令中添加 `--enable-apis` 选项来启用它们；如果您之前尚未创建所需的服务账户，可以添加 `--create-service-accounts` 选项，使其连同所需的角色一起创建。

## 填充集群配置文件

在创建集群之前，我们需要妥善准备由 bmctl 创建的配置文件。该文件默认保存在一个名为 _bmctl-workspace_ 的文件夹内、以集群名称命名的子文件夹中。

在本节中，我们将解释配置文件中的各项选项。

### SSH 私钥

这是集群部署期间用于连接节点的 SSH 私钥。

添加 `sshPrivateKeyPath:` 规范，指定有权以 root 身份访问所有目标节点的 SSH 私钥的完整路径，例如：

```
sshPrivateKeyPath: /root/.ssh/anthoskey
```

### GCP 服务账户密钥

如果您在运行 bmctl 之前手动创建了 GCP 服务账户，则需要按照下面的示例，使用服务账户密钥的完整路径填写相关字段；如果您使用了 bmctl 的 `--create-service-accounts` 参数，这些字段将已经填充完成。

```
gkeConnectAgentServiceAccountKeyPath: /root/bmctl-workspace/.sa-k
gkeConnectRegisterServiceAccountKeyPath: /root/bmctl-workspace/.s
cloudOperationsServiceAccountKeyPath: /root/bmctl-workspace/.sa-k
```

### 集群类型

根据所选的集群部署模型，相应地在 Cluster 自定义资源中设置 type 规范的值，在 admin、hybrid 或 standalone 之间选择，如下例所示：

```yaml
---
apiVersion: baremetal.cluster.gke.io/v1
kind: Cluster
metadata:
  name: admin-cluster
  namespace: cluster-admin-cluster
spec:
  type: hybrid
```

### 控制平面配置

根据所选的控制平面架构，在 `controlPlane:` 部分的 `nodePoolSpec:` 规范中添加目标控制平面节点的 IP 地址。以下示例展示了基于三个控制平面节点的高可用架构。请记住，如果您想启用高可用控制平面，至少需要提供三个 IP 地址。

```yaml
controlPlane:
  nodePoolSpec:
    nodes:
      # 控制平面节点池。通常，如果使用高可用部署，这可以是
      # 单个或 3 台机器。
      - address: 172.16.0.3
      - address: 172.16.0.4
      - address: 172.16.0.5
```

### Pod 和 Service 的 CIDR 块

`clusterNetwork:` 部分包含分配给集群内 Pod 和 Kubernetes Service 对象的 CIDR 范围；这些范围仅在集群内部可见，永远不会在外部使用。仅当这些默认值与您的网络上已有服务（集群内任何正在运行的 Pod 可能需要联系的服务）存在重叠时，才更改默认值。

### 负载均衡器配置

您需要根据所选的负载均衡器模式（bundled 或 manual）以及该模式所需的配置选项来填充 `loadBalancer:` 部分。以下是各种规范的说明：

- **mode**：负载均衡器模式，您需要在 bundled 或 manual 之间选择
- **ports.controlPlaneLBPort**：负载均衡器为 Kubernetes API server 提供服务的端口
- **vips.controlPlaneVIP**：分配给集群上 Kubernetes API server 的 VIP
- **vips.ingressVIP**：分配给集群上 L7 Istio Ingress 网关的 VIP，必须属于下面定义的地址池。此 VIP 仅在集群为 hybrid、standalone 或 user 时需要，在 admin 集群上不需要，该情况下可以保持注释状态。
- **addressPools**：数据平面负载均衡器用来向 Ingress 网关和 LoadBalancer 类型 Kubernetes Service 对象分配 VIP 的地址池，必须包含上面定义的 Ingress VIP。在 admin 集群上不需要，该情况下可以保持注释状态。
- **nodePoolSpec**：列出您想在其中部署捆绑负载均衡器的节点地址。仅当您想为捆绑负载均衡器指定专用工作节点时才需要使用，如果保持注释状态，负载均衡器将部署在控制平面节点上。

需要记住，如果部署的是捆绑负载均衡器，所有 VIP（控制平面 VIP 以及包含 Ingress 网关的地址池）必须与负载均衡器节点在同一子网中。

以下示例展示了一个 hybrid 集群的配置，捆绑负载均衡器部署在两个 IP 地址为 172.16.0.7 和 172.16.0.8 的专用工作节点上。

```yaml
# 负载均衡器配置
loadBalancer:
  mode: bundled
  ports:
    controlPlaneLBPort: 443
  vips:
    controlPlaneVIP: 172.16.0.16
    ingressVIP: 172.16.0.17
  addressPools:
  - name: pool1
    addresses:
    - 172.16.0.17-172.16.0.26
  nodePoolSpec:
    nodes:
    - address: 172.16.0.7
    - address: 172.16.0.8
```

### 代理配置

如果节点需要通过 http 代理连接到互联网，请使用所需信息填充 `proxy:` 部分：

- url：代理服务器可访问的 URL，格式为 http://username:password@fqdn:port
- noProxy：不应通过代理访问的 IP、主机名或域名列表

以下示例为代理服务器配置了一个条目，该代理服务器可通过 http://172.16.0.101:3128 访问，无需身份验证，并为 172.16.0.0/16 范围添加了一个 noProxy 条目，告知系统将该范围内的 IP 排除在代理服务器之外。

```
proxy:
  noProxy:
    # 指定 IP、主机名和域名的列表
    - 172.16.0.0/16
  url: http://172.16.0.101:3128 # 代理服务器的地址。
```

### Cloud Operations 日志与监控

要配置日志和监控选项，您需要在 `clusterOperations:` 部分中添加 projectID 和 location：

- **projectID**：您希望接收指标和日志的项目 ID
- **location**：您希望存储日志和指标的 Google Cloud 区域。建议选择靠近您本地数据中心的区域。





+   - **enableApplication:** 如果你想从应用工作负载中收集应用指标和日志，请设为 true。

默认情况下，Cloud Operations 仅收集管理集群中工作负载的日志和指标，对于用户集群，则仅收集系统命名空间中的工作负载日志和指标，如：*kube-system*、*gke-system*、*gke-connect*、*istio-system*，以及 *config-management-system*。系统组件的日志和指标也用于 Google 支持以在支持案例中排查问题。除系统命名空间的指标外，它还收集所有 Pod 的节点资源使用情况指标。

将 *enableApplicationspec* 设为 true 可启用对集群中任何 Pod 的 stdout 日志收集，以及通过 prometheus 端点公开的自定义指标收集。

为了抓取自定义应用指标，你需要创建一个带有注解 `prometheus.io/scrape: "true"` 的 Service，该 Service 选择应用 Pod 并指向应用公开的 prometheus 端点端口。下面你将找到一个名为 “monitoring-example” 的应用示例，该应用在 9090 端口公开 prometheus 指标。

### Note

这不是 Anthos on bare metal 配置文件的一部分，而是为特定应用创建的 Service 对象以便抓取指标

```
kind: Service
apiVersion: v1
metadata:
  name: "monitoring-example"
  namespace: "default"
  annotations:
    prometheus.io/scrape: "true"
spec:
  selector:
    app: "monitoring-example"
  ports:
  - name: http
    port: 9090
```

Anthos bare metal 设置工具会在 Cloud Monitoring 工作区中自动创建几个仪表板以监控系统组件：

+   - **Control plane status**: 显示集群核心控制平面组件如 API server、scheduler、controller manager、etcd 的可用性。
+   - **Pod status**: 显示 Pod 的指标值如 CPU usage、memory usage、network traffic、restarts 等...
+   - **Node status**: 显示节点指标如 CPU usage、memory usage、allocatable resources、node condition、filesystem size。

## Storage Configuration

storage: 部分包含本地卷供应器 LVP 的配置，可用于使用节点上的挂载点提供持久卷。

使用本地持久卷会将 Pod 绑定到特定的磁盘和节点。如果该磁盘或节点变得不可用，则 Pod 也会变得不可用。因此，使用本地 PV 的工作负载需要对这种故障具有弹性，这就是为什么本地持久卷通常适用于概念验证或高级用例，在这些用例中数据持久性不关键或数据已复制到其他卷以便在节点/磁盘不可用时可恢复。

Anthos on bare metal 集群中本地 PV 有三种类型的存储类：

+   - LVP node mounts: 此存储类为指定目录中挂载的每个磁盘创建一个本地 PV。每个 PV 映射到一个磁盘，其容量等于底层磁盘容量。集群中创建的本地 PV 总数是所有节点上在该路径下挂载的磁盘数量。
+   - **LVP share**: 此存储类在集群中每个节点上的本地共享文件系统中创建由子目录支持的本地 PV。这些子目录在集群创建期间自动创建。使用此存储类的工作负载会共享容量和 IOPS，因为 PV 由同一个共享文件系统支持。
+   - **Anthos system**: 此存储类在集群创建期间创建预配置的本地 PV，用于 Anthos 系统 Pod。不要更改或删除此存储类，也不要将此存储类用于有状态应用。

`lvpNodeMounts:` 部分包含配置 LVP node mounts 的参数：

+   - **path:** 本地节点目录路径，在该路径下将磁盘挂载为本地持久卷
+   - **storageClassName:** PV 将创建时使用的 StorageClass。StorageClass 在集群创建期间创建。

`lvpShare:` 部分包含配置 LVP share 的参数：

+   - **path:** 本地节点目录路径，在每个主机上将在该路径下创建子目录。每个子目录将创建一个本地 PV。
+   - **storageClassName:** PV 将创建时使用的 StorageClass。StorageClass 在集群创建期间创建。
+   - **numPVUnderSharedPath:** 在 path 下创建的子目录数量，集群中 LVP share 持久卷的总数量为该数字乘以节点数量

下面的示例配置使用混合集群的默认参数：

```
storage:
  lvpNodeMounts:
    path: /mnt/localpv-disk
    storageClassName: local-disks
  lvpShare:
    path: /mnt/localpv-share
    storageClassName: local-shared
    numPVUnderSharedPath: 5
```

## Authentication

如本章所述，Anthos on bare metal 利用 Anthos Identity Service 身份验证代理通过 OpenID Connect OIDC 与现有身份提供者集成。

Anthos Identity Service 允许用户通过 GCP Console 和 kubectl 使用现有企业凭据进行身份验证（在这种情况下使用 gcloud CLI 进行身份验证并创建包含用于 kubectl 的 id token 的 kubeconfig 文件）。

集群配置文件中的 `authentication:` 部分包含以下参数，用于使用 OIDC 配置对集群的身份验证：

+   - **name**: 你想要用来标识此配置的名称，通常为身份提供者名称。
+   - **issuerURI**: OpenID 提供者的 url，gcloud CLI 和 GCP Console 发送身份验证请求的位置，也是 Kubernetes API server 用于发现用于验证 token 的公钥的位置。例如，对于 Active Directory Federation Server，这通常是 [https://SERVERFQDN/adfs](https://SERVERFQDN/adfs)，而对于 Google Cloud Identity 则为 [https://accounts.google.com](https://accounts.google.com)
+   - **clientID**: 在 OIDC 提供者配置中分配给应用的客户端 ID。这通常在你在 OIDC 提供者上创建应用时自动创建。
+   - **clientSecret**: 在 OIDC 提供者配置中分配给客户端应用的客户端密钥。这通常在你在 OIDC 提供者上创建应用时自动创建。
+   - **kubectlRedirectURI**: gcloud CLI 的回调/重定向 url，为 `http://localhost:PORT_NUMBER/callback`，其中 `PORT_NUMBER` 是你可选择的任何大于 1024 的端口号。这也需要在 OIDC 提供者配置中添加
+   - **userClaim**: 用作用户名的 JWT 声明，这是可选的，如果未设置则使用 "sub"。
+   - **userPrefix**: 可选。添加到用户声明前缀以防止与现有名称冲突。
+   - **groupPrefix**: 可选。添加到组声明前缀以防止与现有名称冲突。
+   - **scopes**: 作为逗号分隔列表发送到 OpenID 提供者的额外范围。
+   - **extraParams**: 可选。作为逗号分隔列表发送到 OpenID 提供者的额外键值参数。

+   - **proxy**: 可选。用于集群连接到 OIDC 提供者 issuer URL 的代理服务器
+   - **deployCloudConsoleProxy**: 可选。指定是否在集群中部署反向代理以允许 Google Cloud Console 访问本地 OIDC 提供者以对用户进行身份验证。
+   - **certificateAuthorityData**: 可选。OIDC 提供者的 base64 编码 PEM 编码证书颁发机构证书。如果身份提供者证书不是由知名公共 CA 签发，或者在任何情况下如果 `deployCloudConsoleProxy:` 设为 `true` 时需要。

身份验证配置可在集群创建时或在现有集群上集群创建后设置。

## Node pools for worker nodes

如果安装中的第一个集群是混合集群或独立集群，旨在托管用户工作负载，你需要在集群配置文件中配置 worker 节点的 Node Pool 资源，提供目标 worker 节点的 IP 地址。下面是一个使用三个 worker 节点的 node pool 的示例：

```
```yaml
# Node pools for worker nodes
apiVersion: baremetal.cluster.gke.io/v1
kind: NodePool
metadata:
  name: node-pool-1
  namespace: cluster-hybrid-cluster
spec:
  clusterName: hybrid-cluster
  nodes:
  - address: 172.16.0.8
  - address: 172.16.0.9
  - address: 172.16.0.10
```
```

### Note

如果集群是 admin 集群，则不需要此部分。

## Create the cluster





现在我们已经拥有一个完全填充的配置文件，可以使用 bmctl *create cluster* 选项部署集群：

```
```bash
bmctl create cluster -c CLUSTER_NAME
```
```

将 **CLUSTER_NAME** 替换为你在创建集群配置文件时定义的集群名称。

此过程需要一些时间，一旦集群成功创建，你将能够使用生成的 kubeconfig 文件连接它。

## 连接到集群

集群创建完成后，你可以使用安装工具在 bmctl-workspace/CLUSTER_NAME 文件夹中创建的 kubeconfig 文件，通过 kubectl 连接到它。

你也可以使用 bearer token 从 GCP 控制台连接。支持多种类型的 bearer token。最简单的方法是在集群中创建一个 Kubernetes 服务账号（KSA），并使用其 bearer token 登录。

如果为集群配置了与身份提供者的身份集成，则可以授权现有用户和组对特定资源执行特定操作，通过创建 RoleBindings 或 ClusterRoleBindings 将其分配给具有所需权限的角色。

在创建所需的绑定后，你可以通过选择“使用为集群配置的身份提供者进行身份验证”选项，从 GCP 控制台登录到集群。

为了通过 kubectl 对集群进行身份验证以执行操作，在创建所需的 RoleBindings 和/或 ClusterRoleBindings 后，你需要执行以下步骤：

## 创建和分发身份验证配置文件

你需要创建一个身份验证配置文件，该文件将分发给需要使用 kubectl 访问集群的客户端，此文件包含 gcloud cli 所需的 OIDC 配置，以便从客户端发起身份验证和令牌请求。

从管理员工作站或任何可以访问安装创建的 kubeconfig 文件的机器执行以下命令：

```
gcloud anthos create-login-config --kubeconfig CLUSTER_KUBECONFIG
```

将 **CLUSTER_KUBECONFIG** 替换为安装创建的 kubeconfig 文件。

如果命令成功完成，身份验证配置文件 `kubectl-anthos-config.yaml` 将在当前目录中创建。

此 kubeconfig 文件提供对集群的管理访问权限，应仅提供给需要使用 kubectl 执行管理任务的人员。大多数组织应使用“应急访问”流程的一部分且属于现有安全标准的安全措施来保护此文件。

## 使用集群进行身份验证

用于访问集群的客户端机器需要安装 kubectl 和 gcloud cli 包括 anthos-auth 组件。

从客户端机器执行以下命令以从 OIDC 提供者获取 id token 并相应地配置本地 kubeconfig，以便成功使用集群进行身份验证：

```
gcloud anthos auth login \
--cluster CLUSTER_NAME \
--user USER_NAME \
--login-config AUTH_CONFIG_FILE_PATH \
--login-config-cert CA_CERT_PEM_FILE \
--kubeconfig CLUSTER_KUBECONFIG
```

登录选项说明如下：

+   - **CLUSTER_NAME**：可选。是集群在目标 kubeconfig 文件中定义的名称。如果省略此标志，将提示你从身份验证配置文件中指定的集群中进行选择。
+   - **USER_NAME**：可选。是要在 kubeconfig 文件中使用的用户名，如果省略则默认为 CLUSTER_NAME-anthos-default-user
+   - **AUTH_CONFIG_FILE_PATH**：指定身份验证配置文件的路径
+   - **CA_CERT_PEM_FILE**：指定 CA 的 PEM 证书文件的路径，如果身份验证配置文件存储在 https 服务器上则需要。
+   - **CLUSTER_KUBECONFIG**：是写入 oidc id token 的目标 kubeconfig 文件，如果省略则默认为 kubectl 默认位置

该命令将在 OIDC 提供者的同意登录页面上打开浏览器，在那里你需要输入凭据。

你的 kubeconfig 文件现在包含一个 ID token，你的 kubectl 命令将使用它来向集群的 Kubernetes API 服务器进行身份验证。

## 21.4.2 创建用户集群

一旦你创建了 admin 或 hybrid 集群，你就可以向其添加用户集群。你通过应用一个仅包含新集群的 Cluster 和 Node Pool 自定义资源清单的新配置文件来完成此操作。

创建第一个集群的高级步骤如下：

+   1. 使用 bmctl 创建集群配置模板文件
+   2. 使用所需设置修改配置文件
+   3. 使用 bmctl 应用配置文件

为用户集群执行的任务是为创建第一个集群而已执行任务的子集。在下一节中，我们将详细介绍部署用户集群的配置文件。

## 创建集群配置

与第一次集群创建一样，运行以下命令创建集群配置文件：

```
bmctl create config -c CLUSTER_NAME
```

其中 **CLUSTER_NAME** 是你要赋予用户集群的名称。

## 填充集群配置文件

与第一次集群创建一样，你需要准备由 create config 命令创建的配置文件，该文件默认保存在名为集群名称的文件夹内，位于名为 bmctl-workspace 的文件夹中。

对于配置文件的许多部分，第一次集群创建中已给出的相同说明也适用于用户集群，因此请遵循本章 *填充集群配置文件* 部分中给出的说明。

## 注意

需要注意的是，控制平面和负载均衡器部分以及节点池资源中使用的 IP 地址不能与你为第一个集群已使用的地址重叠。

在下一节中，你将找到特定于用户集群的任务。

## 删除凭据部分

用户集群将使用在 admin/hybrid 集群创建期间提供的凭据，因此我们不需要为 GCP 提供凭据。由于这些不是必需的，我们需要从文件中删除这些行。

删除包含密钥指针的部分：

```
gcrKeyPath: <path to GCR service account key>
sshPrivateKeyPath: <path to SSH private key, used for node access
gkeConnectAgentServiceAccountKeyPath: <path to Connect agent serv
gkeConnectRegisterServiceAccountKeyPath: <path to Hub registratio
cloudOperationsServiceAccountKeyPath: <path to Cloud Operations s
```

## 集群类型

将 Cluster 中的类型规范值设置为 user，如下例所示：

```
---
apiVersion: baremetal.cluster.gke.io/v1
kind: Cluster
metadata:
  name: user-cluster
  namespace: cluster-user-cluster
spec:
  # Cluster type. This can be:
  type: user
```

## 负载均衡器配置

接下来，你需要提供用户集群负载均衡器的配置。此处使用的地址不能与第一个集群的负载均衡分配的 IP 重叠。

```
# Sample user cluster config for load balancer and address pools
loadBalancer:
  vips:
    controlPlaneVIP: 10.200.0.71
    ingressVIP: 10.200.0.72
  addressPools:
  - name: pool1
    addresses:
  - 10.200.0.72-10.200.0.90
```

## 创建集群

完成集群配置文件后，你可以使用以下命令创建第一个集群：

```
bmctl create cluster -c CLUSTER_NAME --kubeconfig ADMIN_KUBECONFI
```

用户集群部署完成后，你可以使用生成的 kubeconfig 连接到新集群。

## 连接到集群

集群创建后，你可以从安装过程中在用户集群命名空间中创建的 secret 中获取 kubeconfig 以使用 kubectl 连接到它。以下是提取 kubeconfig 的示例命令：

```
kubectl --kubeconfig ADMIN_KUBECONFIG get secret USER_CLUSTER_NAM
```

你也可以使用 bearer token 从 GCP 控制台连接，或者，如果你为集群配置了身份集成，用户将能够按照 *admin/hybrid 集群部分* 中 *连接到集群* 部分所述的步骤，从 GCP 控制台和 gcloud CLI 使用集群进行身份验证。

## 21.5 升级集群

当发布新版本的 Anthos on bare metal 时，你可以升级你的集群：在非独立集群安装中，你需要先升级 admin/hybrid 集群，然后再升级用户集群。

## 21.5.1 升级 admin、standalone 或 hybrid 集群

对 admin、standalone 或 hybrid 集群执行升级的步骤如下所述：

+   1. 修改集群创建期间使用的集群配置文件，将 Anthos on bare metal 集群版本从现有版本更改为要升级到的版本。以下是一个升级到版本 1.9 的示例配置：

```
apiVersion: baremetal.cluster.gke.io/v1
kind: Cluster
metadata:
  name: admin-cluster
  namespace: cluster-admin-cluster
spec:
  type: hybrid
  # Anthos cluster version.
  anthosBareMetalVersion: 1.9.0
```

+   2. 下载所需版本的 bmctl 工具（你想要的版本



要将集群升级到 1.9，撰写本文时 1.9 版本的路径为：

```
gs://anthos-baremetal-release/bmctl/1.9.0/linux-amd64/bmctl
```

3. 执行以下命令升级集群：

   ```
   bmctl upgrade cluster -c CLUSTER_NAME --kubeconfig ADMIN_KUBECONFIG
   ```

4. 其中 CLUSTER_NAME 是集群的名称，ADMIN_KUBECONFIG 是安装时创建的 kubeconfig 文件。

### 22.5.2 升级用户集群

在升级完管理员或混合集群后，您可以使用以下步骤升级用户集群：

1. 如对管理员/混合集群配置文件所做的那样，修改用户集群配置文件，将 Anthos on bare metal 集群版本从现有版本更改为您要升级到的版本。

2. 执行以下命令升级集群版本：

   ```
   bmctl upgrade cluster -c CLUSTER_NAME --kubeconfig ADMIN_KUBECONFIG
   ```

   其中 CLUSTER_NAME 是要升级的用户集群名称，而 **ADMIN_KUBECONFIG** 是第一个管理员/混合集群安装时创建的 kubeconfig 文件。

至此，Anthos on bare metal 章节结束。

### 22.6 总结

- Anthos on bare metal 允许组织在非 VMware 平台上部署 Anthos，包括裸机或替代型超visor。
- 使用裸机安装时提供了不同的部署选项，包括管理员/用户、混合和独立集群。
- Anthos on bare metal 包含多种负载均衡选择，包括使用外部负载均衡器，即手动模式，或内置选项，即捆绑模式。捆绑模式将为控制平面部署 HAProxy 解决方案，并为工作负载部署 MetalLB。
- 裸机安装提供的默认存储选项仅限于本地主机存储，应仅用于开发集群。
- 安装 Anthos on bare metal 提供了易于部署和升级的流程，利用少量自文档化的配置文件，并使用单个可执行文件 bmctl 进行部署。

## 欢迎

感谢您购买 *Google Anthos in Action* 的 MEAP 版本。我希望您获得的内容能立即用于开发云软件，并且在您的帮助下，最终的书籍会很棒！

要从本书中获得最大收益，您需要对云计算有一定了解。别担心太多，因为我们在本书的初始章节中会回顾微服务和服务网格等基本概念。

无论您是软件开发人员、对技术感兴趣的企业人士、技术研究人员，还是仅仅想了解计算机和人类世界将走向何方的人，了解 Anthos 都至关重要。本书是了解关键技术以及导致我们创建 Anthos 的趋势和思路的绝佳方式。因此，它也能让您很好地感知未来。

当我们在 Google 开始研究 Anthos 时，我们受到了一个单一原则的启发：让我们抽象掉运行计算基础设施运营中不那么高效的大部分工作，并消除那些并非构建和运行软件服务核心的习惯性限制。Kubernetes 和用于使用 DevOps 运行托管服务的 SRE 原则是构建我们新产品基础的根本概念。Anthos 是迈向让开发者工作更轻松的又一大步，得益于创建了一个以客户为中心、可在任何地方运行的分布式开发平台，该平台可以自动化利用率、减少昂贵的依赖并促进生产状态。

这真是一本大书，我希望您阅读它和作者、策展人以及审阅者制作它一样有趣。我的建议是深入研究最初的五到六章，然后按您喜欢的顺序选择相关的后续主题。

> 请务必在 [liveBook discussion forum](#) 中发布您对本书的任何问题、评论或建议。您的反馈对于开发最好的书籍至关重要。
>
> —Antonio Gulli

## 本书内容

Copyright 2022 Manning Publications

1. Welcome
2. Brief contents
3. Overview of Google Anthos
4. Cloud is a new computing stack
5. Anthos, the one single pane-of-glass
6. Anthos, the computing environment built on Kubernetes
7. Anthos Service Mesh: security and observability at scale
8. Operations management in Anthos
9. Bringing it all together
10. Hybrid applications in Anthos
11. Anthos, the compute environment running on VMware
12. Anthos, at the edge and telco world
13. Knative serverless extension
14. Anthos—the networking environment
15. Anthos Config Management
16. Anthos, integrations with CI/CD
17. Anthos, data and analytics
18. Using Anthos for security and policies
19. Google Cloud Marketplace
20. Migrate for Anthos and GKE
21. Anthos for breaking the monolith
22. Anthos on bare metal

