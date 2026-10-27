

separately. 如果你不想自行管理 Knative 安装，目前已有多个托管服务可供选择：Google Cloud Run 和 Google Cloud Run for Anthos、Openshift Serverless、Managed Knative for IBM Cloud Kubernetes Service 等，其中 Knative 和底层的 Kubernetes 均由提供商管理。相关服务的列表可在 https://knative.dev/docs/install/knative-offerings/ 找到。

## 11.3.4 Knative Kubernetes 资源类型

Knative 包含一组控制器和自定义资源定义（CRDs），用于扩展原生的 Kubernetes API。因此，与 Knative 的集成与与 Kubernetes API 本身的交互非常相似。我们将在下一节中查看 Knative 资源。

如果考虑一个简单的 Kubernetes 应用，你应该有诸如 Pods、Deployments 和 Services 之类的对象。如果加入 Service Mesh，你将有额外的资源来处理流量管理，如 VirtualServices、DestinationRules 等。而使用 Knative，你只需用一个资源即可控制部署，即 Knative Service，它允许你同时部署工作负载并处理流量。所有必需的 Kubernetes 和 Service Mesh 资源都将为你创建。

## 11.3.5 Knative Serving

Knative Serving 允许你轻松部署基于容器的无服务器工作负载，并通过 HTTP(s) 请求（近期已宣布支持 gRPC）将其提供给用户。例如，你可以使用 Knative Serving 提供整个电子商务网站的前端。它会根据需求自动扩展你的工作负载（从 0 到 N），并将流量路由或拆分到你选择的版本（revision）。要使用原生 Kubernetes 实现此功能，你需要使用额外的 Kubernetes 资源，如水平 Pod 自动扩缩器 HPA[10]。Knative Serving 通过新的自定义资源定义（CRDs）扩展了 Kubernetes API，如 Knative Serving Service、Configuration、Route 和 Revision。下图展示了 Knative 资源之间的依赖关系。

Figure 11.4. Knative Serving resources

![](img/fb3bebce7b5f593bca1a7f8a1371be06_340_0.png)

让我们逐一查看这些 CRDs：

- **Service**（API 路径 service.serving.knative.dev）：这是 Knative Serving 中最重要的资源。它会自动创建整个工作负载生命周期所需的其他 Knative 资源。随着服务的更新，会创建新的 Revision。在 Knative Service 中，你可以同时定义容器版本以及

流量规则。

**Note:**

这与原生的 Kubernetes Service 对象不同，初次使用时可能会让新用户感到困惑。

下面我们可以看到一个 Service 定义的示例，它将一个简单的 hello world 工作负载部署到 Knative Serving。这将自动创建其他资源：Revision、Configurations 和 Route。

# Listing 11.1

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
    name: helloworld
    namespace: default
spec:
    template:
        spec:
            containers:
                - image: docker.io/{username}/helloworld
                  env:
                    - name: TARGET
                      value: "Python Sample v1"
```

- **Revision**（API 路径 *revision.serving.knative.dev*）：这本质上是容器版本及其配置的不可变快照。它定义了实际提供给用户的内容。。
- **Configuration**（API 路径 *configuration.serving.knative.dev*）：这是应用程序的配置部分，用于强制执行工作负载的期望状态。它允许你将代码（容器）与配置部分分离。修改配置会导致创建新的 revision。
- **Route**（API 路径 *route.serving.knative.dev*）：它将端点映射到一个或多个 revisions。

当你使用 Knative 时，就不再需要担心原生的 Kubernetes 和 Service Mesh 资源，如：*Deployments*、*Services*、

*VirtualServices* 等。你只需将应用程序定义为 Knative Service，所有“后端”资源都会为你创建。

## Traffic Management

当你想用新镜像更新 Knative Service 时，会创建新的 revision，并且默认情况下流量会被导向新的 revision。你可以执行 A/B[^11] 测试和 Canary[^12] 发布，并通过定义 *metadata.spec.traffic* 属性来控制应将多少流量导向特定 revision。你也可以为特定 revision 设置标签，使其可通过专用 URL 访问。

Figure 11.5. Knative Service traffic flows

![](img/fb3bebce7b5f593bca1a7f8a1371be06_343_0.png)

要实现上图所示的路由，你需要在 Knative Service 中按如下方式设置 metadata.spec.traffic 属性：

```
```yaml
traffic:
  - tag: current
    revisionName: helloworld-v1
    percent: 95%
  - tag: candidate
    revisionName: helloworld-v2
    percent: 5%
  - tag: latest
    latestRevision: true
    percent: 0
```
```

正如你所见，开发者只需使用一个 Kubernetes 对象即可控制整个工作负载的提供方式。无需深入 Kubernetes 后端。

## 11.3.6 Knative Serving Control Plane

现在让我们看看使这一切成为可能的 Knative Serving 控制平面。正如我们之前所说，Kubernetes 利用 Istio 或其他受支持的服务网格进行流量管理。它还包含一系列服务来负责运行和扩展工作负载。

要获取你安装 Knative 时的 knative-serving 命名空间中的服务列表，请使用：

```
kubectl get services -n knative-serving
```

它将显示以下服务：

| NAME | TYPE | CLUSTER-IP | EXTERNAL-IP | PO |
|---|---|---|---|---|
| activator-service | ClusterIP | 10.96.61.11 | <none> | 80 |
| autoscaler | ClusterIP | 10.104.217.223 | <none> | 80 |
| controller | ClusterIP | 10.101.39.220 | <none> | 90 |
| webhook | ClusterIP | 10.107.144.50 | <none> | 44 |

正如你所见，对于这样一个复杂的服务，支持服务并不多，让我们逐一查看。

- **Activator** - 接收并缓冲针对非活动 revision 的请求，并向 autoscaler 报告指标。它还在 autoscaler 根据报告的指标扩展 revision 后重试对 revision 的请求。
- **Autoscaler** - 根据定义的参数设置处理负载所需的 Pod 数量。
- **Controller** - 监控并协调 CRDs 中定义的 Knative 对象。当创建新的 Knative Service 时，它会创建 Configuration 和 Route。它会创建 revision 以及对应的 Deployment 和 Knative Pod Autoscalers（KPAs）。
- **Webhook** - 拦截、验证和修改 Kubernetes API 调用，包括 CRD 插入和更新。设置默认值并拒绝

不一致和无效的对象。

如果你获取 Knative 安装命名空间中的部署列表

```
kubectl get deployments -n knative-serving
```

你会看到以下部署：

| NAME | DESIRED | CURRENT | UP-TO-DATE | AVAILAB |
|---|---|---|---|---|
| activator | 1 | 1 | 1 | 1 |
| autoscaler | 1 | 1 | 1 | 1 |
| controller | 1 | 1 | 1 | 1 |
| networking-certmanager | 1 | 1 | 1 | 1 |
| networking-istio | 1 | 1 | 1 | 1 |
| webhook | 1 | 1 | 1 | 1 |

我们已经讨论了其中四个服务，但还有两个尚未见过的部署。它们的作用如下：

- **Networking-certmanager:** 将集群 ingress 协调为 cert manager 对象。
- **Networking-istio:** 将集群的 ingress 协调为 Istio virtual service。

## 11.3.7 Knative Eventing

Eventing 是 Knative 的组件，用于编排来自 Kubernetes 集群内部或外部各种源的事件。这是事件驱动架构中非常重要的元素，它允许你使用现有事件源中的选择来触发你的服务，并为尚未可用的自定义源场景构建新的事件源。这与 Function as a service 非常不同，后者在函数触发方面仅使用 HTTP 请求或其他预定义触发器，如 Google Cloud Storage 事件。

所有 Knative Eventing 对象均定义为 CRDs。它确保事件按照 Knatvie 对象中定义的规则使用控制器进行处理。可扩展性会自动处理，因为事件会触发对容器的调用。它为你提供了与 Knative Serving 相似的可扩展性，因此你开始

