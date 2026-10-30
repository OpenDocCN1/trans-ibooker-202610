

每个微服务负责单一的功能。微服务之间松耦合，可以单独开发和管理。它们通过 API 彼此通信。正因如此，每个微服务甚至可以用不同的编程语言开发。当你需要升级应用程序时，可以只升级单个服务，而不会影响其他组件。通过将应用程序拆分为微服务，你还可以控制应用程序的扩展方式和时机。你可以细粒度地扩展需要扩展的服务，而不影响其他服务。最后，微服务让你能够拥抱 CI/CD，并在部署可以以非常受控的方式推出时更快地交付功能。

> **重要说明**
> **持续集成和持续交付和/或持续部署（CI/CD）** 是一种简化软件交付的方法论。
> **持续集成** 是一种实践，开发人员会频繁地将代码提交到公共仓库。代码会被审查和测试。验证后，会触发自动构建。**持续交付** 允许你自动化发布流程，以便软件可以在任何时间点部署到目标环境。在 **持续部署** 中，开发人员所做的任何更改，只要通过所有测试，就会自动部署到生产环境。

## 容器

要理解容器，让我们将其与传统的虚拟机进行比较。虽然虚拟机虚拟化硬件，但容器虚拟化操作系统。它们将应用程序及其所有依赖项抽象为一个单元。多个容器可以托管在一个操作系统上，作为一个隔离的进程运行：

![](img/de2c97ab017f36b3750a5bade111edd7_157_0.png)

图 6.2 – 容器中的应用程序

容器带来以下优势：

- **隔离**：应用程序可以使用其库，而不会与来自其他应用程序的库冲突。
- **资源限制**：应用程序可以限制资源的使用。
- **可移植性**：应用程序是自包含的，包含所有依赖项，不受操作系统或云提供商的限制。
- **轻量级**：应用程序的占用空间要小得多，因为容器共享一个公共内核。

允许容器运行的软件称为 **容器运行时**。

虽然有许多容器运行时可用，但 Google 建议使用 **containerd**（`https://containerd.io`）。多年来，标准是 **Docker** 运行时，但最近发生了变化。**Containerd** 被确定为最安全和高效的，已成为 Kubernetes 乃至 GKE 的新标准。如果你使用 Docker 运行 GKE 节点，Google 建议你计划迁移。请注意，你仍然可以使用 Docker 构建容器，因为这是一个在 Kubernetes 集群外部发生的独立过程。

## Kubernetes

Kubernetes，也称为 K8s，是一个开源容器编排器，最初由 Google 开发并捐赠给 Cloud Native Computing Foundation。它允许你部署、扩展和管理容器化应用程序。作为一个开源平台，它可以在本地环境以及公共云中的多个环境上运行。它适用于无状态和有状态应用程序。

> **考试提示**
> Kubernetes 是一个重要的考试主题。幸运的是，考试并不要求你对其有深入的了解。在本节中，我们将讨论管理层和 Kubernetes 对象。我们希望你理解管理层，因为理解 GKE 本身是必要的。至于 Kubernetes 对象，请确保你至少理解本书中提到的对象，包括 Pods、Deployments、Services 和 Namespaces。Kubernetes 本身值得一本书，市面上有很多相关书籍。如果你想了解更多，请参考 *进阶阅读部分。*

## Kubernetes 架构

在下面的图中，我们可以看到 Kubernetes 集群的基本架构。集群由多个节点组成。在高层次上，master 节点负责管理集群，而 worker 节点托管工作负载。worker 节点托管所谓的 Pods，它们是 Kubernetes 的最原子单元。这些 Pods 可以包含一个或多个容器。对 Pods 中容器的访问是通过服务提供的。在下面的图中，我们可以看到 Kubernetes 集群以及托管在每种类型节点上的服务：

![](img/de2c97ab017f36b3750a5bade111edd7_159_0.png)

图 6.3 – Kubernetes 架构

现在，让我们更仔细地看看 master 和 worker 节点。

## master 节点

master 节点是一个控制平面，负责维护集群的期望状态。它监控 Kubernetes 对象定义（YAML 文件），并确保它们被调度到 worker 节点上。

> **重要说明**
> **YAML Ain't Markup Language（YAML）** 是一种人类友好的数据序列化标准，主要用于配置文件。它是 XML 或 JSON 等格式的替代方案。

它本质上是集群的控制平面。其工作方式如下：

![](img/de2c97ab017f36b3750a5bade111edd7_160_0.png)

图 6.4 – 控制平面

master 节点运行多个进程：

- **API server**：公开 Kubernetes API。它是控制平面的前端。
- **Controller manager**：多个控制器负责集群的整体健康状况。
- **etcd**：一个托管集群状态信息的数据库。
- **Scheduler**：负责在节点之间放置 Pods 以平衡资源消耗。

集群可以用一个 master 完美运行，但为了高可用性和冗余目的，应该运行多个节点。没有 master，控制平面本质上是宕机的。你执行的所有集群管理操作都通过 master API 进行。因此，对于生产工作负载，建议你使用 *多个 master* 节点配置。

```
selector:
  matchLabels:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

无论我们定义什么类型的对象，都需要在其中包含以下数据：

- apiVersion：我们想要使用的 Kubernetes API 的版本
- kind：要创建的对象的类型
- metadata：有助于唯一标识对象的数据，例如其名称
- spec：对象的规范，根据其类型而定

要创建或更新现有对象，我们可以使用以下命令：

```
kubectl apply -f definition.yaml
```

这里，definition.yaml 是包含对象定义的<|message|>。 要运行我们的第一个部署，我们必须将前面的定义保存在 definition.yaml 文件中并运行 kubectl 命令。

你可以使用多个命令来管理 Kubernetes。kubectl 是最常用的，因为它允许我们创建、删除和更新 Kubernetes 对象。查看以下链接以了解可使用 kubectl 执行的操作：https://kubernetes.io/docs/reference/kubectl/overview/。请注意，kubectl 默认已安装在 GCP Cloud Shell 控制台中，但如果你想从任何其他机器使用它，则需要安装它。

现在，让我们逐个查看每个对象，从 Pods 开始。

> **选择器和标签**
> 你会注意到 Kubernetes 对象具有称为 **labels** 和 **selectors** 的参数。
> 
> 这些参数用于将不同的对象相互关联。假设你想将 Deployment 与 Service 关联。在 Deployment 定义中，你将使用诸如 app: myapp 的 label 属性，而在 Service 定义中，你将使用诸如 app: myapp 的 selector。

## Pods

Pod 是 Kubernetes 中部署的原子单元。Pod 包含一个或多个容器和存储资源。通常，Pod 内只有一个容器。当我们需要小型辅助服务时，可以向 Pod 添加额外的容器。每个 Pod 都有一个唯一的 IP 地址，与其内部的容器共享。Pod 本质上是临时的，并在需要重新调度时重新创建。如果它们不使用持久卷，卷的内容在 Pod 重新创建时会消失。要创建 Pod，我们可以使用 kind 值 Pod 并定义我们想要使用的镜像，如下所示：

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: myapp
spec:
  containers:
  - name: my-container
    image: nginx:1.7.9
```

在这里，我们可以看到一个 Pod 的定义，其中包含从 nginx 镜像部署的容器。

