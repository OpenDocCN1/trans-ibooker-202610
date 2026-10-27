
它可以处理少量事件的小负载，并能够扩展以处理事件流。

目前已有大约 20 个预定义的源可供使用，而且这个列表还在不断增长。你也可以开发自己的源。Knative Eventing 也具有很强的可插拔性，因此你可以选择在处理事件时如何存储事件。你可以选择使用内存存储或持久化存储。Knative 使用开放的 CNCF 标准 CloudEvents 来解析原始事件。事件的目标既可以是 Knative 服务，也可以是 Kubernetes 服务。Eventing 管道可以非常简单，比如将简单事件发送到单个服务，但也可以变得非常复杂。为了理解 Cloud Run，我们先从基础知识入手。

## 11.3.8 Knative Eventing 资源

Knative 事件最基本的组件是 Broker 和 Trigger。如果我们看下面的示意图，可以看到事件由外部源生成，并由 Broker 捕获。在那里，一个或多个 Trigger 接收事件，对事件进行过滤，然后将事件传递给 Service。

![](img/fb3bebce7b5f593bca1a7f8a1371be06_346_0.png)

Knative 的架构带来了清晰的关注点分离。Knative Eventing Broker 是一个事件网格（Event Mesh），它拉取或接收事件，而 Knative Trigger 负责过滤事件并将事件路由到目标。事件源是 Knative Eventing 的控制平面，确保事件被发送到 Broker。

现在让我们更仔细地了解一下这些资源。

- **Broker API 路径 `broker.eventing.knative.dev`** 本质上是一个可寻址的事件交付系统，你可以将其安装在命名空间中。这可以通过在命名空间上设置标签来完成。类似你使用 Istio 时希望对 Pod 进行 sidecar 注入的做法。Broker 接收事件，然后将事件发送给订阅者。消息存储在由 Broker 管理的通道中。

  该通道可以是一个简单的内存通道，也可以为了可靠性而使用持久化存储。这方面的例子有 Pub/Sub 和 Kafka。通道的配置存储在 ConfigMap 中。你可以将 Broker 安装到多个命名空间中。这样做的使用场景是，如果你想要不同类型的消息。你还可以过滤 Broker 接收哪些事件。

  下面我们来看一个 Knative Broker 的示例定义：

  ```yaml
  apiVersion: eventing.knative.dev/v1
  kind: Broker
  metadata:
    annotations:
      eventing.knative.dev/broker.class: MTChannelBasedBroker
    name: default
    namespace: default
  spec:
    config:
      apiVersion: v1
      kind: ConfigMap
      name: config-br-default-channel
      namespace: knative-eventing
  ```

- **Triggers API 路径 `trigger.eventing.knative.dev`** 将事件与 Service 匹配。因此，它被定义为针对特定类型的事件（例如 Cloud Storage 对象创建时将事件发送到该服务）。Trigger 可以基于一个或多个属性过滤事件。如果有多个属性，则所有属性值都必须匹配。它还可以从接收的事件中生成新的事件类型。这对于过滤包含敏感数据的事件来说是一个很好的用例。

  下面我们来看一个 Knative Trigger 的示例定义：

  ```yaml
  apiVersion: eventing.knative.dev/v1alpha1
  kind: Trigger
  metadata:
      name: helloworld-python
      namespace: knative-samples
  spec:
      broker: default
      filter:
          attributes:
              type: dev.knative.samples.helloworld
              source: dev.knative.samples/helloworldsource
      subscriber:
          ref:
              apiVersion: v1
              kind: Service
              name: helloworld-python
  ```

- **Source API 路径（`<source_name>.eventing.knative.dev`）** 被定义为 CRD。这个列表还在不断增长，包括：AWS SQS、Google Cloud Pub/Sub、Google Cloud Scheduler、Google Cloud Storage、GitHub、GitLab 等。要查看完整的事件列表，请参阅：https://knative.dev/docs/eventing/sources/。你可以使用现有的 Source，也可以创建自己的 Source。下面的示例展示了如何配置 *CloudPubSubSource* 事件源。每当有消息发布到名为“testing”的 Pub/Sub 主题时，就会生成该事件。

  下面我们来看一个 Knative CloudPubSubSource 的示例定义：

  ```yaml
  apiVersion: events.cloud.google.com/v1
  kind: CloudPubSubSource
  metadata:
      name: cloudpubsubsource-test
  spec:
      topic: testing
      sink:
          ref:
              apiVersion: v1
              kind: Service
              name: event-display
  ```

这可以使用 Kubernetes Operators、Container sources 或现有的 Services 来完成。要了解如何开发自己的 Source，请参阅 Knative Eventing 文档（https://knative.dev/docs/eventing/custom-event-source/custom-event-source/）

## 11.3.9 Knative 用例

借助 Knative，你可以覆盖从简单的单一服务到非常复杂的多微服务应用的众多用例。使用 Knative Serving，你可以创建 HTTP 和 gRPC [13] 服务、webhook 和 API。你可以管理发布（rollout）和回滚（rollback）。你可以轻松控制应用程序的流量。借助 Cloud Eventing，你可以创建简单或非常复杂的事件管道。将这两种服务结合起来，你就可以交付完全云原生的、事件驱动的应用程序。

让我们看一个将运行中的服务绑定到 Cloud IoT Core [14] 的简单示例。来自 IoT 设备的消息被发送到 Google Cloud IoT Core，然后发送到 Pub/Sub。Cloud Eventing 服务使用 Pub/Sub Source 从主题获取消息。消息被发送到 Broker，并转换为 CloudEvents。Trigger 确保事件被发送到适当的服务，由该服务进一步处理、记录或向用户显示。

**图 11.8.**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_351_0.png)

如果你想亲自尝试 Pub/Sub 示例，我们建议你按照以下分步教程进行操作：https://github.com/google/knative-gcp/tree/main/docs/examples/cloudpubsubsource

## 11.3.10 可观测性

Knative 自带日志记录和追踪功能。支持以下开源软件：

- **Prometheus** 和 **Grafana** 用于指标
- **ELK** 技术栈（**Elasticsearch**、**Logstash** 和 **Kibana**）用于日志
- **Jaeger** 或 **Zipkin** 用于分布式追踪

要了解更多关于指标和追踪的信息，请参阅第 5 章“Anthos Service Mesh：大规模安全性和可观测性”。

你还可以使用 *Fluent Bit* 代理集成 **Google Cloud Logging**（原 Stackdriver Logging）来收集日志。每个组件的安装过程在以下文章中都有详细说明：
https://knative.dev/docs/serving/observability/logging/collecting-logs/

**图 11.9. Knative 可观测性生态系统**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_352_0.png)

由于这些组件像任何其他 Kubernetes 应用程序一样部署，你可以通过暴露 Kubernetes Service 来访问它们。下面是部署后集群中运行的 Pod 示例：

| 名称 | 就绪 | 状态 | 重启次数 |
|------|------|------|----------|
| grafana-798cf569ff-v4q74 | 1/1 | 运行中 | 0 |
| kibana-logging-7d474fbb45-6qb8x | 1/1 | 运行中 | 0 |
| kube-state-metrics-75bd4f5b8b-8t2h2 | 4/4 | 运行中 | 0 |
| node-exporter-cr6bh | 2/2 | 运行中 | 0 |
| node-exporter-mf6k7 | 2/2 | 运行中 | 0 |
| node-exporter-rhzr7 | 2/2 | 运行中 | 0 |
| prometheus-system-0 | 1/1 | 运行中 | 0 |
| prometheus-system-1 | 1/1 | 运行中 | 0 |

## 11.3.11 安装 Knative

你可以在多个云平台或本地环境中安装 Knative，只要运行的是 Kubernetes 集群。这包括但不限于：

- Amazon EKS
- Google GKE
- IBM IKS
- Red Hat OpenShift Cloud Platform
- Minikube

归根结底，Knative 只不过是一个 Kubernetes 应用程序。安装可以通过 YAML 文件或 Operator 来完成。要了解更多关于 Operator 的信息，请参阅：https://kubernetes.io/docs/concepts/extend-kubernetes/operator/。**Knative Serving** 组件的安装过程包括：

- 安装自定义资源定义（即 CRD）：
- 安装 Serving 的核心组件
- 安装网络层
- 配置 DNS
- 安装可选的 Serving 扩展

**Knative Eventing** 组件的安装过程包括：

- 安装自定义资源定义（即 CRD）：
- 安装 Eventing 的核心组件
- 安装默认的 Channel（消息）层
- 安装 Broker（事件）层
- 可选的 Eventing 扩展（源）

完成 Serving 和 Eventing 的安装后，你可以安装上一章描述的可观测性组件。有关端到端安装的详细分步说明，请参阅：https://knative.dev/docs/getting-started/quickstart-install/

## 11.3.12 部署到 Knative

你可以按照一个简单指南将你的第一个应用程序部署到 Knative。实际上，只需应用一个 Knative Service 对象即可。这就是

