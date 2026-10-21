

![](img/2ab98a5ff1ec073700d902ba95287fd6_126_0.png)

Figure 4.13 Custom Crossplane Provider for your Managed Service

想象一下，你有一个托管在其他地方的服务，可以生产新的 Pos 冰淇淋。要创建新的 Pos 冰淇淋实例，你只需要该服务的账号即可。你并不关心在自己的 Kubernetes 集群内部创建 Pos 冰淇淋实例，但你希望允许用户创建 Pos 冰淇淋，无论它们在哪里创建。这正是创建一个理解如何使用你的账号凭证连接到 Pos 冰淇淋服务的 Crossplane Provider 的完美场景。

Pos 冰淇淋 Crossplane Provider 的实现将包含一个新的 Kubernetes 自定义资源定义，称为 IceCream 资源，它将负责定义要在远程服务上创建的冰淇淋的属性。与 Compositions 类似，自定义 Crossplane Provider 可以管理和公开外部平台所提供的任意数量的新资源。例如，如果 Pos 冰淇淋服务还提供咖啡，Provider 可以添加额外资源以使用户能够创建远程咖啡杯。

如果你觉得该 Provider 对整个 Crossplane 社区有用，你可以将你的 Provider 捐赠给 Crossplane Contrib 组织：https://github.com/crossplane-contrib

## 4.5 Summary

- 云原生应用依赖于更多的应用基础设施，因为每个服务可能都需要不同的持久化存储、用于发送消息的消息代理以及其他组件来运行。
- 在云提供商内部创建应用基础设施很容易，可以为我们节省大量时间，但随后我们就依赖于他们的工具和生态系统。
- 通过依赖 Kubernetes API 和 Crossplane 等工具，可以以云无关的方式配置基础设施，这些工具抽象掉了底层的云提供商。
- Crossplane 为主要云提供商提供支持，并且可以扩展以支持其他服务提供商。
- 通过为应用基础设施创建我们简化的抽象，我们可以让开发人员以自助服务的方式配置组件，而无需了解所有云提供商特定的配置要求。我们可以通过创建 Crossplane 配置包（OCI 镜像）来共享这些抽象，这些包可以分发并与其它团队共享。

## 5 Release Strategies

本章涵盖

- 使用 Deployments、Services 和 Ingress 的 Kubernetes 发布策略
- 如何实现滚动更新、金丝雀发布、蓝绿部署和 A/B 测试
- 使用 Kubernetes 内置资源高效发布软件的局限性和挑战
- Knative Serving 高级流量路由能力，用于降低发布风险并加快发布节奏

如果你和你的团队担心部署服务的全新版本，你就做错了。你正因更改或部署对运行中应用的风险而放慢了发布节奏。

降低风险并建立适当的机制以持续部署新版本，会极大提高对系统的信心。它还能减少从变更请求到变更在用户面前上线所需的时间。带有修复和新功能的新发布与业务价值直接相关，因为软件只有在为公司用户服务时才有价值。

虽然 Kubernetes 内置资源如 Deployments、Services 和 Ingresses 为我们部署服务并向用户公开服务提供了基本构建块，但要实现众所周知的发布策略，仍需要大量手动且容易出错的工作。

本章分为两个主要部分：

- 使用 Kubernetes 内置机制的发布策略
  - Rolling Updates
  - Canary Releases
  - Blue/Green Deployments
  - A/B Testing
  - 使用 Kubernetes 内置机制的局限性和复杂性
- 降低发布风险以提高交付速度
  - Introduction to Knative Serving
  - Advanced traffic-splitting features
  - Knative Serving applied to our Conference Platform application

在本章中，你将学习 Kubernetes 内置机制和 Knative Serving 以实现滚动更新、金丝雀发布、蓝绿部署和 A/B 测试等发布策略。本章首先介绍这些模式，了解其实现影响及其局限性。

本章的后半部分介绍了 Knative Serving，它允许我们对流量如何路由到应用的服务进行更细粒度的控制。我们探讨了 Knative Serving 流量拆分机制如何用于改进我们对云原生应用新版本的发布方式。

### 5.1 Kubernetes built-in mechanisms for releasing new versions

Kubernetes 带有内置机制，可随时部署和升级你的服务。Deployment 和 StatefulSets 资源都编排 ReplicaSets，在资源配置更改时运行内置的滚动更新机制。Deployment 和 StatefulSets 都会跟踪一个版本到下一个版本之间的更改，允许回滚到之前记录的版本。

Deployment 和 StatefulSets 就像饼干模具；它们包含服务容器如何配置的定义，并且基于指定的副本数，将创建相应数量的容器。为了将流量路由到这些容器，我们需要创建一个 Service，如第 2 章所述。

为每个 Deployment 使用一个 Service 是标准做法，并且足以让不同的服务通过众所周知的 Service 名称相互通信。但如果我们希望允许外部用户（来自 Kubernetes 集群外部）与我们的服务交互，我们将需要一个 Ingress 资源（加上 ingress controller）。Ingress 资源将负责配置网络基础设施，以使外部流量进入我们的 Kubernetes 集群；这通常针对少数几个服务完成。一般来说，并非每个服务都暴露给外部用户。

# Figure 5.1 Kubernetes built-in resources for routing traffic

现在，想象一下当你想要更新其中一个面向外部的服务的版本时会发生什么。我们可以同意，这对于这些服务来说是用户界面的情况相当常见。我们也可以同意，如果我们可以在没有停机的情况下升级服务，那将非常好。好消息是 Kubernetes 是为零停机升级而设计的，因此 Deployment 和 StatefulSets 都配备了滚动更新机制。

如果你有多个应用 Pod 副本，Kubernetes Service 资源充当负载均衡器。这允许集群内的其他服务使用 Service 名称，而无需关心它们正在与哪个副本交互（或每个副本的 IP 地址）。

# Figure 5.2 Kubernetes Service acting as a load balancer

为了将这些副本中的每一个附加到负载均衡器（Kubernetes Service）池，Kubernetes 使用一种称为 "Readiness Probe" 的探针（https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/）来确保 Pod 内部运行的容器已准备好接受传入请求，换句话说，它已完成引导。在图 5.2 中，Pod C 尚未就绪，因此未附加到 Service 池，也没有请求转发到该实例。

现在，如果我们想升级到使用 `my-app:1.1` 容器镜像，我们需要对这些容器进行非常详细的编排。假设我们希望在更新期间不丢失任何传入流量。我们需要使用 `my-app:1.1` 镜像启动一个新副本，并确保该新实例已启动并运行并且准备好接收流量，然后再移除旧版本。如果我们有多个副本，我们可能不希望同时启动所有新副本，因为这会导致运行该服务所需资源翻倍。

我们也不希望一次性停止旧的 `my-app:1.0` 副本。我们需要保证新版本正在工作并正确处理负载，然后再关闭之前运行良好的旧版本。幸运的是，Kubernetes 使用滚动更新策略（https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/）自动处理所有容器的启动和停止。

#### 5.1.1 Rolling updates

Deployment 和 StatefulSets 都内置了这些机制，我们需要了解这些机制的工作原理，才能知道何时依赖它们以及它们的局限性和挑战。

滚动更新由升级 Deployment 管理的任意数量副本的明确定义检查和操作组成。Deployment 资源编排 ReplicaSets 以实现滚动更新，下图展示了该机制的工作方式：

