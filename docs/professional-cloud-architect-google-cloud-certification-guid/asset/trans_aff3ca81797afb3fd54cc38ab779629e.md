

## 服务设置

服务会公开唯一的端点，并自动扩展底层基础设施以处理传入请求。部署平台和服务名称无法更改。

### 部署平台

Cloud Run（完全托管）
Cloud Run for Anthos

你没有任何启用了 Cloud Run for Anthos 的集群。创建 Anthos GKE 集群以开始。了解更多
CREATE CLUSTER

可用 Anthos GKE 集群 *
cloudrun-default-cluster (will be created) us-central1-a
此服务的集群之后无法更改。了解更多

命名空间
default

### 服务名称 *

cloudrun-test

### 连接性 *

内部
服务只能通过集群网络调用，且无法从互联网访问。
外部
服务可以通过互联网调用，除通过集群网络外。

### 图 8.19 – Cloud Run for Anthos

如果你没有任何启用了 Cloud Run 的 Anthos 集群，集群将为你创建。如果你想创建启用了 Cloud Run 的集群，请将以下标志添加到 gcloud 命令中：

```
gcloud container clusters create CLUSTER_NAME \
  --addons=HttpLoadBalancing,CloudRun \
  --machine-type=n1-standard-2 \
  --enable-stackdriver-kubernetes
```

请注意，你需要为此命令正确运行指定默认区域和区域。一旦成功执行，你就可以在集群上部署 Cloud Run 服务。

## Cloud Run（完全托管）与 Cloud Run for Anthos

在下表中，我们对 Cloud Run（完全托管）和 Cloud Run for Anthos 之间的差异进行了简单总结：

| 功能 | Cloud Run | Cloud Run for Anthos |
|---|---|---|
| 价格 | 按使用付费 | Anthos 集群成本 |
| 计算 | CPU 和内存限制 | Anthos 集群节点的性能（包括 GPU） |
| 隔离 | 使用 gVisor 沙箱 | Anthos 集群隔离 |
| 扩展 | 1,000 个容器，可扩展配额 | 基于 Anthos 集群 |
| 域名 | 可用 |  |
| VPC 访问 | 无服务器 VPC 访问 | 直接访问 VPC |
| 服务网格 | 未连接到 Istio 服务网格 | 服务连接到 Istio 服务网格 |
| 执行环境 | Google 原生基础设施 | Anthos 集群 |

正如我们所见，差异不仅在于定价模型和执行环境，还在于提供的功能。选择在哪个选项上运行工作负载需要基于你的需求。每个服务都有其优缺点。

## 配额和限制

Anthos 带有一些配额和限制。这些是为了确保你获得最佳性能，并防止你配置过多的资源。由于配额和限制可能会发生变化，最好参考文档以查找最新配额，网址为 https://cloud.google.com/anthos/clusters/docs/on-prem/concepts/scalability。

例如，你可以在撰写本书时查看适用于 VMware 上的 Anthos 集群（GKE on-prem）的配额：

- 你最多可以在每个管理集群上部署 20 个用户集群。
- 对于每个用户集群：
  - 你必须创建最少 3 个节点。
  - 你最多可以创建 250 个节点。
  - 你最多可以创建 7,500 个 Pod：
- 对于每个节点，你最多可以创建 110 个 Pod。
- 对于每个 Google Cloud 项目，你最多可以在 GKE Hub 中注册 15 个用户集群，但你可以提交请求以增加配额。

现在让我们回顾 Anthos 定价。

## 定价

Anthos 集群按用户集群中工作节点每 vCPU 计费。这不包括管理集群和主节点。你可以选择两种计费模式：

- **按需付费** 定价，你在使用 Anthos 管理的集群时按当前适用费率计费。你可以在没有任何长期承诺的情况下开始使用按需付费 Anthos
- **订阅** 定价，你为 Anthos 部署支付月度费用。你可以从长期订阅承诺中受益，获得折扣价格

由于实际价格可能会发生变化，请参考定价列表的文档，网址为 https://cloud.google.com/anthos/pricing。

## 总结

在本章中，我们了解了 Anthos 及其最重要的功能。我们介绍了 Anthos 部署选项，包括 **GKE（Google Cloud）**、**本地（VMware）**、**本地（裸机）**、**AWS**、**附加集群** 和 **Azure**。我们查看了如何使用定义配置同步控制 Anthos 集群的配置并使用 **Anthos Config Management** 强制执行策略。我们学习了如何使用 **Anthos Service Mesh** 控制和观察微服务架构中的流量。我们还了解了如何使用 **Anthos MCI** 分发入站流量。最后，我们学习了如何使用 **Cloud Run on Anthos** 在 Anthos 集群上运行类似无服务器的云原生工作负载。我们将其与在 **Cloud Run（完全托管）** 上运行工作负载进行了比较。在下一章中，我们将了解如何使用 Google Cloud Functions 运行无服务器函数。

## 进一步阅读

你可以参考以下链接获取更多信息：

- **Anthos 主页**：https://cloud.google.com/anthos/docs/concepts/overview
- **Anthos on VMware**：https://cloud.google.com/anthos/clusters/docs/on-prem/
- **Anthos 集群 on 裸机**：https://cloud.google.com/anthos/clusters/docs/bare-metal/1.6/concepts/about-bare-metal
- **Anthos on AWS**：https://cloud.google.com/anthos/clusters/docs/aws/concepts/architecture
- **Anthos on Azure**：https://cloud.google.com/anthos/clusters/docs/azure/
- **Anthos 配置管理**：https://cloud.google.com/anthos/config-management
- **Anthos 服务网格**：https://cloud.google.com/anthos/service-mesh
- **Istio**：https://istio.io/latest/docs
- **Anthos for Anthos and GKE**：https://cloud.google.com/migrate/anthos/docs/concepts
- **Cloud Run on Anthos**：https://cloud.google.com/anthos/run/
- **多集群入口**：https://cloud.google.com/kubernetes-engine/docs/concepts/multi-cluster-ingress
- **Config Connector**：https://cloud.google.com/config-connector

## 9
使用 Google Cloud Functions 运行无服务器函数

在本章中，我们将最终讨论完全无服务器的计算选项，即 Cloud Functions。这意味着不再有服务器，也不再有容器。此服务在后端利用它们，但对最终用户不可见。我们现在唯一需要关心的是代码。Cloud Functions 是一种 Function-as-a-Service（FaaS）服务。这意味着你用 GCP 支持的一种语言编写一个函数，它可以由事件或通过 HTTP 触发。GCP 会负责配置和扩展运行函数所需的资源。

> **重要说明**
> Cloud Functions 在后端如何工作？再次说明，你无需担心运行函数的 GCP 后端基础设施。然而，作为一名工程师，我敢打赌你仍会自行寻找答案。Cloud Functions 使用容器为你的函数设置隔离环境。这些被称为 Cloud Functions 实例。如果并行执行多个函数，将创建多个实例。

## 考试提示

预计 Cloud Architect 考试中会出现 Cloud Functions 问题。你需要了解 Cloud Functions 是什么以及最常见的用例是什么。能够区分两种类型的函数，即 HTTP 和后端函数也很重要。知道何时使用 Cloud Functions 而不是其他计算选项，以及记住支持的编程语言至关重要。最后，确保你能够从 Google Cloud 控制台和命令行部署函数。

在本章中，我们将涵盖以下主题：

- 主要 Cloud Functions 特征
- 用例
- 运行时环境
- Cloud Functions 类型
- 事件和触发器
- 其他考虑因素
- 部署 Cloud Functions
- IAM
- 配额和限制
- 定价

## 主要 Cloud Functions 特征

以下是 Cloud Functions 的关键特征：

- **无服务器**：Cloud Functions 完全无服务器。底层基础设施对最终用户是抽象的。
- **事件驱动**：Cloud Functions 是事件驱动的。它们会响应事件或 HTTP 请求而触发。这意味着它们仅在需要时被调用，且在非活动时不产生任何成本。

