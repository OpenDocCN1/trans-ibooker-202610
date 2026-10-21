

这里有几个细节需要注意；首先，有一个 URL 可以复制到浏览器中访问该服务。如果你是在云服务提供商上运行并且在安装 Knative 时配置了 DNS，那么该 URL 应该可以立即访问。LASTCREATED 列显示的是该 Service 最新 Knative Revision 的名称。Knative Revision 是指向我们 Service 特定配置的指针，这意味着我们可以将流量路由到它们。

你可以使用 `curl` 或将浏览器指向 http://email-service.default.X.X.X.X.sslip.io/info 来测试 Knative Service URL

你应该会看到以下输出：

```
{"name":"Email Service (REST)","version":"v0.1.0","source":"https://github.com/salaboy/fmtok8s-email-rest/releases/tag/v0.1.0","podId":"","podNamespace":"","podNodeName":""}
```

与任何其他 Kubernetes 资源一样，你也可以使用 `kubectl describe ksvc email-service` 来获取该资源的更详细描述。

如果你列出其他知名资源，如 Deployment、Services 和 Pods，你会发现 Knative Serving 会为你创建并管理它们。因为这些现在是受管理的资源，通常不建议手动更改它们。如果你想更改应用程序配置，应该编辑 Knative Service 资源。

我们之前在集群上应用的 Knative Service，默认情况下的行为与手动创建 Service、Deployment 和 Ingress 不同。

Knative Service 默认情况下：

- **它是可访问的**：在公共 URL 下自行公开，以便你可以从集群外部访问它。它不会创建 Ingress 资源，因为它使用之前安装的 Knative Networking 堆栈。因为 Knative 对网络堆栈有更多控制权并管理 Deployment 和 Services，所以它知道服务何时准备好提供请求，减少了 Services 和 Deployments 之间配置错误。
- **管理 Kubernetes 资源**：它创建两个 Services 和一个 Deployment：Knative Serving 允许我们同时运行同一服务的多个版本。因此它会为每个版本创建一个新的 Kubernetes Service（在 Knative Serving 中称为 Revisions）
- **收集服务使用情况**：它会创建一个指定 `user-container` 的 Pod 和一个名为 `queue-proxy` 的 sidecar 容器。
- **根据需求伸缩**：如果没有请求命中服务，自动将自身缩放到零（默认在 90 秒后）
  - 它通过 `queue-proxy` 收集的数据将 Deployment 副本缩放到 0 来实现
  - 如果请求到达且没有可用的副本，它会在排队请求的同时进行扩容，以免请求丢失。
- 配置更改历史由 Knative Serving 管理：如果更改 Knative Service 配置，将会创建新的 Revision。默认情况下，所有流量都将路由到最新 revision。

当然，这些是默认值，但你可以微调每个 Knative Service 以满足你的目的，例如实现之前描述的发布策略。

在下一节中，我们将了解如何使用 Knative Serving 的高级流量处理功能来实现金丝雀发布、蓝绿部署、A/B 测试和基于 Header 的路由。

## 5.2.3 高级流量拆分功能

让我们先看看如何为应用程序的其中一个服务使用 Knative Service 实现金丝雀发布。

本节首先介绍如何使用基于百分比的流量拆分进行金丝雀发布。然后介绍通过基于标签和基于 Header 的流量拆分进行 A/B 测试。

### 使用基于百分比的流量拆分进行金丝雀发布

如果你获取 Knative Service 资源（使用 `kubectl get ksvc email-service -oyaml`），你会注意到 `spec` 部分现在还包含一个默认创建的 `spec.traffic` 部分，因为我们没有指定任何内容。

```
traffic:
- latestRevision: true
  percent: 100
```

默认情况下，100% 的流量被路由到服务的最新 Knative Revision。

现在假设你在服务中做了更改以改进邮件发送方式，但你的团队不确定人们会如何接受，并且我们希望避免因网站问题导致人们不愿登录会议而产生的反弹。因此我们可以让两个版本并行运行并控制有多少流量被路由到每个版本（在 Knative 术语中称为 Revision）。

让我们编辑（`kubectl edit ksvc email-service`）Knative Service 并应用更改：

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: email-service
spec:
  template:
    spec:
      containers:
      - image: salaboy/fmtok8s-email-rest:0.1.0-improved #A
  traffic: #B
  - percent: 80
    revisionName: email-service-00001
  - latestRevision: true
    percent: 20
```

#A 你已将服务将使用的 docker 镜像更新为 `fmtok8s-email-rest:0.1.0-improved`

#B 你已创建 80% / 20% 的流量拆分，其中 80% 的流量将继续流向你的稳定版本，20% 流向你刚刚更新的新版本

如果你现在尝试使用 `curl`，你应该能够看到流量拆分的效果：

```
salaboy> curl http://email-service.default.x.x.x.x.sslip.io/info
{"name":"Email Service (REST)","version":"v0.1.0",
"source":"https://github.com/salaboy/fmtok8s-email-rest/releases/tag/v0.1.0"}
salaboy> curl http://email-service.default.x.x.x.x.sslip.io/info
{"name":"Email Service (REST)","version":"v0.1.0",
"source":"https://github.com/salaboy/fmtok8s-email-rest/releases/tag/v0.1.0"}
salaboy> curl http://email-service.default.x.x.x.x.sslip.io/info
{"name":"Email Service (REST)","version":"v0.1.0",
"source":"https://github.com/salaboy/fmtok8s-email-rest/releases/tag/v0.1.0"}
salaboy> curl http://email-service.default.x.x.x.x.sslip.io/info
{"name":"Email Service (REST)","version":"v0.1.0",
"source":"https://github.com/salaboy/fmtok8s-email-rest/releases/tag/v0.1.0"}
salaboy> curl http://email-service.default.x.x.x.x.sslip.io/info
{"name":"Email Service (REST) - IMPROVED!!","version":"v0.1.0", #A
"source":"https://github.com/salaboy/fmtok8s-email-rest/releases/tag/v0.1.0"}
```

#A 每五个请求中有一个会进入新的 "IMPROVED!!" 版本。请注意，这可能需要一段时间，直到新的 Knative Revision 启动并运行。

一旦你验证了服务的新版本工作正常，你就可以开始发送更多流量，直到你有信心将 100% 的流量转移到它。如果出现问题，你只需将流量拆分恢复到稳定版本即可。

请注意，你不仅限于两个服务 revision；只要流量百分比总和为 100%，你可以创建任意多个。Knative 会确保遵循这些规则并扩缩所需 revision 以提供请求。你不需要创建任何新的 Kubernetes 资源，因为 Knative 会为你创建，从而降低了同时修改多个资源时可能出现的错误风险，希望一切保持一致。

使用百分比，你无法控制后续请求会落在何处。Knative 只会确保根据你指定的百分比保持公平的分布。如果例如你有一个用户界面而不是简单的 REST 端点，这可能会成为问题。

![](img/2ab98a5ff1ec073700d902ba95287fd6_144_0.png)

![](img/2ab98a5ff1ec073700d902ba95287fd6_144_1.png)

### 图 5.10 基于百分比的流量拆分场景和挑战

用户界面很复杂，因为浏览器会执行多个 GET 请求来渲染页面 HTML、CSS 和图像等。你很快就会遇到每个请求命中应用程序不同版本的情况。让我们看看另一种可能更适合测试用户界面或需要确保多个请求最终进入应用程序正确版本的情况的方法。

### 使用基于标签的路由进行 A/B 测试

如果你想对用户界面进行 A/B 测试，你需要给 Knative 一个区分请求发送位置的方法。你有两个选择。首先，你可以指向要尝试的服务的特殊 URL，其次是使用请求 header 来区分请求发送位置。让我们看看这两种替代方案的实际效果。

但让我们现在使用会议平台的用户界面：

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: ui-service #A
spec:
  template:
    spec:
      containers:
        - image: salaboy/fmtok8s-api-gateway:0.1.0 #B
```

#A 你需要为此 Service 指定一个名称

#B 你现在定义 API Gateway / 用户界面容器镜像，因为我们将测试多个请求进入同一版本

