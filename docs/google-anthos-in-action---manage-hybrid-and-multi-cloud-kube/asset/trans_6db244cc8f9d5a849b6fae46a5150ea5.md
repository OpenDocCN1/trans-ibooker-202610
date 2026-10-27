

## 11 Knative 无服务器扩展

by Konrad Clapa

### 本章涵盖：
- Introduction to serverless
- Knative Serving and Eventing Components
- Knative on Anthos

在深入细节之前，让我们先设定场景。本章要讨论的是 Google Cloud Platform 的托管服务，该服务基于一个名为 Knative 的开源项目。该项目启动的目的是允许在无需理解其使用的复杂 Kubernetes 概念的情况下实现 Kubernetes 应用的更高开发速度。通过此服务，Google 会在你的 Anthos GKE 集群内部署并管理 Knative serving。使用 Knative 与 Anthos 而非开源 Knative 的好处之一是，Google 的自动化和站点可靠性工程师会处理安装和维护。Anthos 与众多 GCP 服务集成，如 Cloud Load Balancing[1]、Cloud Armor[2]、Cloud CDN[3] 等，使企业级 Knative 成为现实。

### 11.1 我们试图解决的问题是什么

我们在第 4 章已经讨论过 Kubernetes，Anthos 是一个基于 Kubernetes 的计算环境，因此我们了解其安装和维护可能有多复杂。Google Kubernetes Engine 为我们解决了这个问题。在 Kubernetes 之上，我们仍然需要知道如何运行和操作云原生应用。Knative 所做的是抽象这些实现细节，并允许你在任何 Kubernetes 集群上提供基于容器的无服务器工作负载。

在本章中，我们将了解无服务器是什么，介绍 Knative，最后讨论 Anthos 如何提供企业级容器化无服务器平台。

### 11.2 无服务器简介

关于无服务器是什么有很多讨论。最常见的是将无服务器与 Function as a Service (FaaS) 进行比较。这几乎是一种意识形态争论。为了简化，让我们看看 Cloud Native Computing Foundation 的定义：

> *Serverless computing refers to a new model of cloud native computing, enabled by architectures that do not require server management to build and run applications.*

Google 完全托管的 Cloud Run 服务完美契合此定义，因为它将计算层从开发者和运维人员中抽象出来。它允许你部署将提供 http(s) 请求的容器。应用的扩展由平台本身处理。

虽然 Google Cloud Functions 提供类似功能，但它对可使用的运行时语言有更强的限制。使用 Cloud Run，你可以使用任何能够运行可响应 HTTPS 调用的服务的语言。Cloud Run 不需要使用 Anthos。

当我们思考 Cloud Run 时，可以想到以下无服务器特性集合：

- No server - developers don’t need to worry about underlying compute infrastructure
- Multi-Language - the application can be written in any language
- Event-driven - the container/function is triggered by an external event
- Autoscaling - the container can automatically scale based on requests.
- Portability - your container/application should be able to run on any Kubernetes platform.

### 11.3 Knative

正如你所知，Kubernetes 是一个用于构建平台的平台。那么为什么不将其用于基于容器的无服务器平台构建呢？Knative 在 Kubernetes 之上运行，就像任何其他 Kubernetes 应用一样。你甚至可以看到 Knative 贡献者的一些说法，即它不应该被称为 serve less，所以，让我们将 Knative 看作是在你运行 Kubernetes 的任何地方提供无服务器的平台。听起来合理吗？

#### 11.3.1 简介

假设你想构建自己的基于 Kubernetes 的无服务器平台，你可能会画出如下所示的图表，展示所有必需的组件。显然，与构建自动伸缩、可观测性、滚动发布等原语相关的努力存在重复。Knative 所做的是为你提供所有这些原语，以便在 Kubernetes 上运行无服务器工作负载时具有一致的体验。

Figure 11.1. Kubernetes Serverless stack architecture

### Kubernetes Serverless stack architecture

Products

Developer Experience

Event Sources

Runtimes

...

Primitives

Build

Observe

Invoke

Rollout

Route

Bind Events

Autoscale

...

Common effort

Platform

Kubernetes

现在从开发者的角度思考。他们所要做的就是定义依赖项，编写代码并将其放入容器中。然后他们将应用部署到 Knative。如何提供服务不是他们的关注点。然而，这并不意味着他们失去了微调服务的能力。可以设置多个参数，如并发数（每个容器可处理的请求数）、最小/最大实例数（可为 Knative 服务配置的最小/最大容器实例数）等。Knative 隐藏了 Kubernetes 中与扩展和流量管理相关的所有复杂性，并提供了观察工作负载的手段。这就是你所说的轻松开始 Kubernetes 应用开发，对吧？

### Knative vs CaaS, FaaS and PaaS

在本章第二部分我们了解到 Knative 试图解决的问题：使无服务器工作负载能够在任何地方运行，同时具有 Kubernetes 的灵活性但隐藏复杂性。下表比较了 Knative 与平台即服务（PaaS）[4]、容器即服务（CaaS）[5] 和函数即服务（FaaS）[6]。各种功能是否受支持。Do it yourself (DIY) 表示你需要进行一些开发才能使用该功能。

### Table 11.1. Knative vs PaaS, CaaS and FaaS

| Feature | Knative | PaaS | CaaS | FaaS |
|---|---|---|---|---|
| Simple UX/DX | Yes | Yes |  | Yes |
| Event-driven | Yes | Yes |  | Yes |
| Container based | Yes | Yes | Yes | Yes |
| Autoscaling | Yes | Yes | DIY | Yes |
| Scale resources to 0 | Yes | Yes |  | Yes |
| Load Balancing | Yes | Yes | DIY | Yes |
| Unrestricted execution time | Yes* | Yes | Yes |  |
| Unrestricted compute/memory limits | Yes** | Yes** | Yes** |  |
| Variety of programming languages support | Yes | Yes | Yes | Limited |

*might be restricted for Managed services like Cloud Run.

** depends on the platform

正如我们所见，Knative 赋予了你函数即服务的全部优势，同时还赋予你以几乎任何语言运行应用的能力。与 FaaS 相比，执行时间的限制要高得多。在许多需要更长请求处理时间的情况下，Knative 是一个解决方案。你最终可以访问高级功能，如卷和网络，因此如果需要可以调整工作负载。与所有具有更高灵活性的计算服务一样，责任分界线会更多地转移到你身上。你需要构建自己的容器并确保能够利用所有高级功能，但说实话，谁不喜欢在获得 FaaS 所有好处的同时对应用有更多控制权呢？

#### 11.3.2 Knative 历史

Google 启动了 Knative，但现在已有 IBM、RedHat、Pivotal、SAP 等多家公司为其贡献。完整的文档和源代码可以在这里找到：https://github.com/knative。Knative 最初是一组组件，允许构建和运行无状态工作负载以及订阅事件。目前在 Github 上有两个活跃的项目正在进行中，它们是：

- **Knative Serving** - allows to serve serverless containerised workloads

#### 11.3.3 Knative 架构

让我们看看下面图表中展示的 Knative 架构。正如你已经注意到的那样，有多个层，其中一些组件可以即插即用或为可选。

### Figure 11.3. Knative Architecture

![](img/fb3bebce7b5f593bca1a7f8a1371be06_338_0.png)

Knative 可以在任何能够运行 Kubernetes 的计算平台上运行。它可以基于虚拟机或裸机服务器。对于流量路由，使用 Service Mesh Gateway。显然最流行的是 Istio，但也支持替代解决方案，如：Gloo、Ambassador、Contour 和 Kourier，还有更多即将推出。要了解更多关于 Istio 的信息，请参阅第 5 章 Anthos Service Mesh: security and observability at scale。在此之上，我们将 Knative 组件作为 Kubernetes 应用安装。请注意，其中每个组件都可以安装和操作

