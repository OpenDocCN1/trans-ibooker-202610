

Anthos 是一个多云平台，可以部署到多个环境，例如公共/私有云和本地数据中心。在多个环境中管理网络具有挑战性，因为每个环境都是唯一的，并且资源管理取决于提供商而有所不同。例如，GCP VPC 的配置方式与 AWS VPC 或数据中心网络不同。Anthos 在多个环境中提供统一的接口。

Anthos 平台可以通过三种主要方式部署到多个环境中。

- 1. **多云部署** - Anthos 平台可以部署到多个公共云环境，例如 GCP 以及一个或多个公共云如 AWS 和 Azure。
- 2. **混合云部署** - Anthos 平台可以部署到 GCP 以及一个或多个本地数据中心。
- 3. **多云和混合云部署** - 此部署是上述两种部署的组合。例如，Anthos 平台可以部署到 GCP、一个或多个本地数据中心以及一个或多个非 GCP 公共云。

## 多/混合网络

当 Anthos 跨多个基础设施环境部署时，这些环境需要与 GCP 具有网络连接。

有三种网络连接选项可用于连接多个基础设施环境。

## Cloud Interconnect

[Cloud Interconnect](12) 通过高可用、低延迟的连接将本地网络扩展到 Google 网络。您可以使用 Dedicated Interconnect 直接连接到 Google，或使用 Partner Interconnect 通过受支持的服务提供商连接到 Google。Dedicated Interconnect 在您的本地网络和 Google 网络之间提供直接物理连接。Dedicated Interconnect 使您能够

它由 API 服务器用于将大型子网切片为每个节点的小型子网。例如，如果您希望每个节点有 30 个 Pod，每个节点至少需要一个 /27。您的 Pod IP 范围必须足够大，以容纳 N 个每个包含 32 个地址的子网，其中 N 为集群中的最大节点数。

Pod IP 地址在集群内可路由。它们是否可以从集群外部路由，取决于集群的类型和实现方式。这将在下一节中详细讨论。

**服务或 ClusterIP 地址** 分配给每个 Kubernetes [Service](Service)。与 Pod IP 地址不同，Pod IP 地址在 Pod 在节点之间移动时可能会发生变化，[ClusterIP](ClusterIP) 地址保持静态，并充当多个 Pod 的负载均衡虚拟 IP 地址（VIP），这些 Pod 代表单个 Kubernetes 服务。顾名思义，服务 IP 或 ClusterIP 对集群本地有效，无法从集群外部访问。集群内的服务可以使用 ClusterIP 访问服务。

## 集群网络数据平面

Anthos GKE 集群提供两种网络数据平面选项。

## GKE 数据平面 v1：Kube-proxy 和 Calico

Kubernetes 使用 [kube-proxy](kube-proxy) 组件管理 Pod 和服务之间的连接性。这默认作为每个节点上的静态 Pod 部署。运行 1.16 或更高版本的任何 GKE 集群都已部署 kube-proxy 作为 [DaemonSet](DaemonSet)。

kube-proxy 不是内联代理，而是基于出站的负载均衡控制器。它监视 Kubernetes API 服务器，并通过向节点的 [iptables](iptables) 子系统添加和删除目标 NAT（DNAT）规则，不断将 ClusterIP 映射到健康的 Pod。当在 Pod 中运行的容器向服务的 ClusterIP 发送流量时，节点会随机选择一个 Pod 并将流量路由到该 Pod。

当您配置服务时，您可以可选地通过

定义 port 和 targetPort 的值来重新映射其监听端口。port 是客户端访问应用程序的位置。targetPort 是应用程序在 Pod 内实际监听流量的端口。kube-proxy 通过在节点上添加和删除 iptables 规则来管理此端口重新映射。

在 GKE 数据平面 v1 中，Kubernetes NetworkPolicies 今天使用 Calico 组件实现。Calico 是一个开源的网络和网络安全解决方案，用于容器、虚拟机和原生主机工作负载。

此实现使用严重依赖 Linux 内核中 IPtables 功能的组件。IPtables 提供灵活但不可编程的数据路径，使 K8s 网络功能成为可能。

数据平面 v2 解决了并解决了其中一些问题。

## GKE 数据平面 v2：eBPF 和 Cilium

GKE 数据平面 V2 是一个有主见的数据平面，利用 eBPF 和 Cilium 的强大功能，Cilium 是一个开源项目，使用 eBPF 使 Linux 内核对 Kubernetes 感知。

数据平面 V2 通过提供可编程数据路径来解决可观察性、可扩展性和功能需求。扩展伯克利包过滤器（eBPF）是一种新的 Linux 网络范式，它向 Linux 内核内部的网络栈公开可编程钩子。无需在用户空间和内核空间之间来回跳转即可丰富内核的用户空间信息的能力，使得能够以高速对网络数据包进行上下文感知操作。

新的数据平面添加了两个新的集群组件 - 编程 eBPF 数据路径的 cilium-agent DaemonSet，以及管理 Cilium 内部 CRD 并帮助 cilium-agent 避免监视每个 pod 的 cilium-operator Deployment。

数据平面还消除了两个 calico 集群组件 - calico-node DaemonSet 和 calico-typha Deployment。这些组件提供 NetworkPolicy 强制执行功能，由 cilium-agent

daemonset 提供。

数据平面还从节点中移除了 kube-proxy 静态 pod。Kube-proxy 为集群提供服务解析功能，该功能也由 cilium-agent 提供。

此外，cilium-cni 由 Netd 编写的 CNI 配置链接到现有的 GKE CNI。Netd 通过从 netd-config configmap 读取添加链接到 cilium-cni 的选项。

## 图 12.6 GKE 数据平面 v2 架构

![](img/fb3bebce7b5f593bca1a7f8a1371be06_376_0.png)

数据平面 V2 为 Anthos 集群提供网络可编程性和可扩展性。一个用例是 Network Policy 流日志记录。企业使用 Kubernetes 网络策略来声明 pod 如何相互通信。然而，没有可扩展的方式来排查和审计这些策略的行为。有了 GKE 中的 eBPF，您现在可以实时强制执行策略，并在几乎不影响节点 CPU 和内存资源的情况下，以线速将策略操作（允许/拒绝）与 pod、命名空间和策略名称相关联。当数据包进入 VM 时，专门的 eBPF 程序可以安装在内核中以决定如何路由数据包。与 IPTables 不同，eBPF 程序可以访问 Kubernetes 特定元数据，包括网络策略信息。这样，它们不仅可以允许或拒绝数据包，还可以将带注释的操作报告回用户空间。这些事件使您能够生成网络策略日志。

图 12.7 数据平面 v2：网络策略流日志记录

![](img/fb3bebce7b5f593bca1a7f8a1371be06_377_0.png)

下表显示了数据平面 V1 和 V2 之间网络功能比较。

| 网络功能 | 现有 | 新数据平面 |
|---|---|---|
| ClusterIP 服务解析 | Kube-proxy 使用 IPTables | Cilium-agent 使用 eBPF 在套接字上 |
| NodePort 服务解析 | Kube-proxy 使用 IPTables | Cilium-agent 使用 eBPF 在 eth0 tc 钩子上 |
| LoadBalancer 服务解析 | Kube-proxy 使用 IPTables 重定向到服务链 | Cilium-agent 使用 eBPF 在 eth0 tc 钩子上（与上面相同的钩子） |
| 网络策略强制执行 | Calico 使用 IPTables | Cilium-agent 使用 eBPF 在套接字以及 eth0 tc 钩子上。 |

根据 Anthos 集群的类型和实现方式，网络设计和要求各不相同。在下一节中，我们将根据网络要求和最佳实践查看每种类型的 Anthos 集群。

## Anthos GKE on GCP

Anthos GKE 集群在 GCP 上运行并利用 GCP VPC 功能进行 Kubernetes 网络。有两种类型的 Anthos GKE on GCP 实现。

## VPC 原生集群

这是 Anthos GKE on GCP 集群的默认且推荐的实现。使用别名 IP 地址范围的集群称为 VPC-

