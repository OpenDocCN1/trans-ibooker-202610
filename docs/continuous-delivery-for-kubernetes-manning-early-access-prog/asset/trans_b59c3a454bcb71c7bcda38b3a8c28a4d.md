

查看以下博客文章以了解可供您使用的 Ingress 控制器有哪些不同选项：https://docs.google.com/spreadsheets/u/2/d/191WWNpJj2za6-nbG4ZoUMXMpUK8KICIosvQB0f-og3k/edit#gid=907731238

使用 Ingress，您可以配置单个入口点并使用基于路径的路由将流量重定向到您需要公开的每个服务。图 2.20 中的前一个 Ingress 资源将发送到 `/` 的所有流量路由到 `fmtok8s-api-gateway` 服务。请注意，Ingress 规则非常简单，您不应该在此级别添加任何业务逻辑路由。

## TROUBLESHOOTING INTERNAL SERVICES

有时，访问内部服务以调试或排查出现问题的服务非常重要。对于这种情况，您可以使用 `kubectl port-forward` 命令临时访问未通过 Ingress 资源在集群外部公开的服务。例如，要在不经过 API 网关的情况下访问 Agenda 服务，您可以使用以下命令：

```
kubectl port-forward svc/fmtok8s-agenda-rest 8080:80
```

您应该会看到以下输出，请确保不要终止该命令：

```
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

![](img/2ab98a5ff1ec073700d902ba95287fd6_51_0.png)

图 2.X `kubectl port-forward` 允许您为调试目的公开服务

然后使用您的浏览器、curl 在另一个标签页或任何其他工具指向 http://localhost:8080/info 来访问公开的 Agenda 服务。以下图像显示了如何使用 `curl` 访问 Agenda 服务 info 端点并借助 `jq` 打印漂亮/彩色的 JSON 负载，您需要单独安装 `jq`。

```
curl localhost:8080/info | jq --color-output
{
  "name": "Agenda Service (REST)",
  "version": "v0.1.0",
  "source": "https://github.com/salaboy/fmtok8s-agenda-rest/releases/tag/v0.1.0",
  "podId": "app-fmtok8s-agenda-rest-7cfc4b5b4c-llqr4",
  "podNamespase": "default",
  "podNodeName": "dev-worker"
}
```

图 2.X 使用 `port-forward` 通过 curl localhost:8080 访问 Agenda 服务

在本节中，您已检查了创建以在 Kubernetes 中运行应用程序容器的主要 Kubernetes 资源。通过查看这些资源及其关系，您将能够在问题出现时排查问题。

对于日常操作，`kubectl` 命令行工具可能不是最优的，可以使用不同的仪表板来探索和管理 Kubernetes 工作负载，例如：k9s、Kubernetes Dashboard 和 Octant。您可以在附录 X 中查看使用 Octant 探索同一应用程序。

## 2.3 Cloud-Native applications Challenges

与单体应用程序不同，如果出现问题，单体应用程序会完全宕机，而 Cloud-Native 应用程序在某个服务宕机时不应崩溃。Cloud-Native 应用程序为故障而设计，应在出现错误时继续提供有价值的功能。在修复问题的同时提供降级服务，比完全无法访问应用程序要好。在本节中，您将更改 Kubernetes 中的某些服务配置，以了解应用程序在不同情况下的行为。

在某些情况下，应用程序/服务开发人员需要确保他们构建的服务具有弹性，并且一些问题将由 Kubernetes 或基础设施解决。

本节涵盖与 Cloud Native 应用程序相关的最常见挑战之一。我认为提前了解会出什么问题很有用，而不是在构建和交付应用程序时才发现。这不是一个详尽的列表，它只是开始，以确保您不会被众所周知的问题困住。以下部分将以会议平台为例说明并突出这些挑战。

- **不允许停机**：如果您在 Kubernetes 之上构建和运行 Cloud-Native 应用程序，而仍然遭受应用程序停机的影响，那么您就没有充分利用所使用的技术栈的优势。
- **服务的内置弹性**：下游服务会宕机，您需要确保您的服务已为此做好准备。Kubernetes 有助于动态服务发现，但这不足以使您的应用程序具有弹性。
- **处理应用程序状态并非易事**：我们必须了解每个服务的<|message|> 基础设施要求，以便高效地允许 Kubernetes 扩展我们的服务。
- **数据不一致数据**：在分布式应用程序中工作时常见的问题是数据不存储在单一位置，而趋向于分布。应用程序需要准备好处理不同服务对世界状态有不同视图的情况。
- **了解应用程序的工作方式（监控、追踪和遥测）**：清楚了解应用程序的性能以及它是否按预期工作，对于在出现问题时快速找到问题至关重要。
- **应用程序安全和身份管理**：处理用户和安全问题总是事后才考虑。对于分布式应用程序，及早明确记录和实现这些方面将有助于您通过定义“谁可以在何时做什么”来完善应用程序需求。

让我们从第一个挑战开始。

### 2.3.1 Downtime is not allowed

在第 2.2.x 节中，您增加了 API 网关的副本数；当您的服务基于平台通过创建运行服务的容器副本来扩展服务的假设创建时，这是一个很棒的功能。那么，当服务无法处理复制，或某个服务没有可用的副本时会发生什么？

您还将 API 网关扩展为始终运行 2 个副本。这意味着如果其中一个副本因任何原因停止运行，Kubernetes 将尝试启动另一个，直到运行 2 个。

![](img/2ab98a5ff1ec073700d902ba95287fd6_53_0.png)

图 2.X API 网关 / 用户界面的两个副本

您可以通过杀死 API 网关的两个 Pod 中的一个来快速尝试 Kubernetes 的此自愈功能。您可以通过运行以下命令来执行此操作：

```
kubectl get pods
```

图 2.X 检查两个副本是否都在运行

现在，复制其中一个 Pod Id 并删除它：

```
kubectl delete pod <POD_ID>
```

然后再次列出 Pod：

```
kubectl get pods
```

图 2.X Kubernetes 一旦检测到一个 Pod 宕机，就会立即创建一个新的副本

您可以看到 Kubernetes（更具体地说是 ReplicaSet）一旦检测到只有一个正在运行，就会立即创建一个新的 Pod。在新 Pod 创建和启动期间，您有一个副本来响应请求，直到第二个副本启动并运行。此机制确保至少有两个副本来响应用户的请求。

![](img/2ab98a5ff1ec073700d902ba95287fd6_55_0.png)

图 2.X 一旦 Kubernetes 检测到一个 Pod 行为异常，就会将其杀死并创建一个新的

如果您只有一个副本，如果您杀死正在运行的 Pod，您将在新容器创建并准备好提供请求之前经历应用程序停机。您可以使用以下命令恢复到单个副本：

```
kubectl scale --replicas=1 deployments/<DEPLOYMENT_ID>
```

继续尝试，删除 API 网关 Pod 唯一可用的副本：

```
kubectl delete pod <POD_ID>
```

![](img/2ab98a5ff1ec073700d902ba95287fd6_56_0.png)

图 2.X 在单个副本重启时，没有备份来响应用户请求

在杀死 Pod 后，立即通过刷新浏览器（http://localhost）访问应用程序。您应该在浏览器中看到“503 Service Temporarily Unavailable”，因为 Ingress Controller（为简单起见，图中未显示）无法在 API 网关服务后面找到正在运行的副本。如果等待片刻，您将看到应用程序恢复。

