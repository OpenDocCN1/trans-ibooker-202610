

## 8 使用 Anthos 管理云原生工作负载

在前几章中，我们深入探讨了 Google Kubernetes Engine（GKE）和 Cloud Run，它们为云原生工作负载提供编排。在本章中，我们将介绍 Anthos；这是一个混合多云平台，允许您以一致且受控的方式在任何环境中运行云原生工作负载，无论是在公有云、本地还是边缘。

重要的是要理解 Anthos 采用非常敏捷的方式开发。每 3 个月发布一个新版本。因此，本章中我们不会引用 Anthos 的任何特定版本。相反，我们将向您解释主要概念并引用最新的文档，以便您保持更新并为考试做好准备。

### 考试提示
由于 Anthos 是一个非常热门的话题，它很可能出现在 PCA 考试中。请注意，Anthos 是一个非常广泛的话题，值得单独写一本书，然而，本章将为您提供一个良好的介绍，帮助您通过考试。
对于 PCA 考试，请准备对本章所述的所有 Anthos 服务有一个高层次的理解。即使您不应该期望关于 Anthos 如何工作以及如何配置相关服务的深入问题，我们也鼓励您尝试 Anthos Qwiklabs 以更好地理解它。

本章将涵盖以下主题：

- 什么是 Anthos？
- Anthos 集群部署选项
- Anthos 配置管理
- Anthos 服务网格
- Anthos 二进制授权
- 用于 Anthos 和 GKE 的迁移
- 用于 Anthos 的 Cloud Run

## Anthos 组件

Anthos 不是单一服务，而是一组组合成平台的服务。它允许您在任何部署位置管理并观察您的云原生工作负载。如图 8.1 所示，Anthos 由一组提供以下功能的服务组成：

- 基础设施管理
- 集群管理
- 服务管理
- 策略管理
- 配置管理

![](img/de2c97ab017f36b3750a5bade111edd7_237_0.png)

图 8.1 – Anthos 组件

Anthos 允许您将 Kubernetes 资源分组为舰队/环境，以便将它们作为单个实体进行管理。我们将在本章后面更详细地了解舰队。

## Anthos 集群

Anthos 集群允许您使用多个 Kubernetes 集群并为您的云原生工作负载提供一致且安全的环境。Anthos 可以在 Google Cloud Platform（GCP）中使用 GKE、在本地以及其他公有云中部署 Anthos 集群。您还可以附加已存在的 Kubernetes 集群，并在无需配置新的 Anthos 集群并迁移工作负载的情况下利用部分 Anthos 功能。

## Anthos Connect 代理

Anthos Connect 代理可以将任何 Kubernetes 集群连接到 GCP。通过将集群连接到 GKE Hub，您能够直接从 Cloud Console 访问集群并与托管的工作负载进行交互。代理作为名为 Connect Agent 的 Kubernetes Deployment 运行，并与 GKE hub 建立加密连接：

![](img/de2c97ab017f36b3750a5bade111edd7_238_0.png)

图 8.2 – Anthos Connect 代理

对于集群无法直接访问互联网的场景，代理可以配置为使用 NAT 或代理进行到 GKE hub 的出口。一旦您在 GKE hub 中注册了集群，您将获得与 Kubernetes 集群交互的类似 GKE 的体验。

## 舰队（以前称为 environs）

舰队（以前称为 environs）允许您逻辑地组织舰队感知资源，例如 Kubernetes 集群，以便更轻松地进行管理。例如，您可以在资源上应用统一的配置或策略，以实现舰队内部的一致性。

舰队具有以下限制：单个舰队只能关联到一个名为舰队主机项目的 GCP 项目，并且舰队项目只能有一个舰队。此限制导致在项目级别上对资源进行隔离。

舰队可用于与 **舰队启用组件** 配合使用，例如以下内容：

- **工作负载身份池** – 允许您统一授权池中的工作负载访问外部服务
- **Anthos 配置管理** – 允许您在舰队中的集群和资源上统一应用配置并强制执行策略
- **Anthos 服务网格** – 允许您在舰队中的资源之间创建服务网格
- **多集群入口（MCI）** – 允许您为舰队中的集群和服务端点创建通用入口以提供负载均衡

舰队的另一个重要元素是同一性概念。在不同上下文中但名称相同的对象可以被视为同一实体。这包括以下上下文：

- 命名空间同一性
- 服务同一性
- 访问外部资源时的身份同一性
- 舰队内部的身份同一性

例如，对于命名空间同一性，同一舰队中不同集群中名称相同的命名空间被视为单个实体。

## Anthos 集群部署选项

Anthos 提供了多种部署选项，允许您几乎在任何地方托管云原生工作负载。如前所述，Anthos 是一个发展非常快的平台。因此，您应始终查阅文档以查找最新的支持端点和功能列表，网址为 https://cloud.google.com/anthos/deployment-options。以下图表显示了在撰写本书时可用的和计划中的部署选项：

![](img/de2c97ab017f36b3750a5bade111edd7_239_0.png)

图 8.3 – Anthos 部署选项

另请注意，您可以附加使用托管 Kubernetes 服务在 **Azure Kubernetes Service（AKS）** 和 **Amazon Elastic Kubernetes Service（EKS）** 中配置的 Kubernetes 集群。如果您查看前面的链接，您可能会注意到新的 Anthos 功能总是首先引入 GKE（即在 GCP 上）。这是因为 GKE 非常适合 Anthos 生态系统并利用 GCP 基础设施。您也可以预期，后来附加的在 Anthos 之外配置的集群可能由于技术限制而在功能方面存在一些限制。

现在让我们对每个部署选项进行高层次的了解。

请注意，在 *第 6 章，使用 Google Kubernetes Engine 管理 Kubernetes 集群* 中，我们对 GKE 服务进行了全面回顾。如果您想获取有关此部署选项的更多信息，我们鼓励您重温该章节。要为 GKE 启用 Anthos，请按照此处可以找到的分步指南进行操作：https://cloud.google.com/anthos/docs/setup/cloud。

## Anthos 集群在 VMware 上（GKE on-prem）

Anthos 集群在 VMware 上（GKE on-prem）是第一个将 GKE 引入本地数据中心的部署选项。它可以在基于 **VMware vSphere** 的现有虚拟化平台上运行。Anthos 通过 **vCenter** API 作为基础设施管理层与 vSphere 通信，以配置托管 Kubernetes 节点的 **虚拟机（VM）**。这样我们就能为 GKE on-prem 部署配置控制平面和工作负载集群。

由于 vSphere 是市场上最成熟且被广泛采用的虚拟化平台之一，它具有非常复杂的特性集，包括软件定义网络、存储和高级 API。它也被认为是私有云的最新基础架构。由于 vSphere 通过为软件定义基础设施提供自动化来消除管理开销，因此它是 Google 选择 vSphere 作为 GKE on-prem 的第一个端点的自然选择。您必须意识到，这会带来一些成本，因为您需要在常规许可证之外购买 vSphere 许可证。

## GKE on-prem 组件

在 *第 6 章，使用 Google Kubernetes Engine 管理 Kubernetes 集群* 中，我们了解到 GKE 集群的管理平面位于 GCP，我们作为消费者无需担心它。GCP 负责 GKE Kubernetes 集群的控制平面和数据平面的配置和管理。

