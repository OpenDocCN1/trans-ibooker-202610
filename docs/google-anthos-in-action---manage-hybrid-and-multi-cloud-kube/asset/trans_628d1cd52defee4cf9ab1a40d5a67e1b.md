

假设你已经有一个容器化的 Python 应用，该应用响应 *Hello Python Sample v1!* 并存储在 Docker Hub 中。 （请参阅 [https://github.com/knative/docs/tree/main/code-samples/eventing/helloworld/helloworld-python](https://github.com/knative/docs/tree/main/code-samples/eventing/helloworld/helloworld-python) 查看该应用的源代码。

+   1. 运行以下命令创建 Knative Service：

```
kubectl apply -f service.yaml
```

其中 Service 在 service.yaml 文件中定义：

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: test
  namespace: default
spec:
  template:
    spec:
      containers:
        - image: docker.io/<user>/<image_name>
          env:
            - name: TARGET
              value: "v1"
```

+   2. 部署后会为你创建多个对象。这包括 Pods、Knative Service、Configuration、Revision 和 Route。 你可以通过运行以下命令验证它们：

```
kubectl get pod,ksvc,configuration,revision,route
```

+   3. 要获取 IP 地址，你可以访问服务并获取 Istio ingress 网关的 IP 地址：

```
kubectl get ksvc helloworld-python --output=custom-columns=NAME:
```

这将返回 URL ：

```
NAME                 URL
helloworld-python     http://helloworld-python.default.1.2.3.4.xip.
```

4. 现在通过运行 curl 查询来测试应用程序：

```
curl http://helloworld-python.default.1.2.3.4.xip.io
```

请注意 xip.io 域是所谓的 Magic DNS，你可以在安装 Knative 时进行配置（参见： [https://knative.dev/docs/serving/using-a-custom-domain/](https://knative.dev/docs/serving/using-a-custom-domain/)）。

5. 你应该会看到以下输出

```
Hello Python Sample v1!
```

你已成功部署了第一个 Knative 应用程序！

为了获得 Knative 的实践经验，我们建议你按照下面的示例进行操作。它们涵盖了端到端的 Knative 应用开发和部署场景，并支持多种语言。我们特别推荐 Mete Atamel 关于 Knative 的教程，它会手把手地带你从非常简单的部署到非常复杂的部署，包括使用 Google Cloud 服务如 Pub/Sub、AI API、BigQuery 等。我们相信你会玩得很开心！

## 表 11.2. 部署到 Knative 的参考资料

| 标题 | URL |
|---|---|
| Knative Serving 代码示例 | [https://knative.dev/docs/serving/samples/](https://knative.dev/docs/serving/samples/) |
| Knative Eventing 代码示例 | [https://knative.dev/docs/eventing/samples/](https://knative.dev/docs/eventing/samples/) |
| Mete Atamel Knative 教程，包含多个示例 | [https://github.com/meteatamel/knative-tutorial](https://github.com/meteatamel/knative-tutorial) |

## Knative 总结

使用 Knative Service，你不再需要在 Kubernetes 的灵活性和 Function as a Service 的简易性之间做出选择。你可以获得两者的优点。你可以在任何地方运行你的无服务器工作负载。使用 Knative Eventing，你能够订阅并接收来自多个预定义源的事件，以及使用云原生架构定义自己的源。

## Cloud Run 与 Anthos 上的 Knative

Cloud Run 是一个完全托管的无服务器服务，Knative on Anthos 运行在你的 Anthos 集群之上。你可以以相同的方式与 Cloud Run 交互，无论你选择哪个版本。然而 Cloud Run 运行在 Google 基础设施上，你无需担心底层平台。

注意：为本书的目的，我们将把 Cloud Run（完全托管）称为 Cloud Run。

## 图 11.10. Cloud Run 和 Knative on Anthos 架构

![](img/fb3bebce7b5f593bca1a7f8a1371be06_356_0.png)

好吧，我们已经知道主要区别是什么，但你仍然可能会

| 执行环境 | Google Infra | GKE 集群 |
|---|---|---|

那么何时选择每个服务？这在很大程度上取决于你希望对应用程序执行拥有多少控制权，以及是否需要 GKE 节点可能提供的定制硬件。例如，你可能希望利用 GPU 来提升 ML 流水线的性能。在这种情况下，Knative on Anthos 将是最佳选择。

## 11.4 总结

在本章中，我们讨论了 Knative 如何融入 Anthos 套件，并且我们学习了以下内容：

+   - Knative 抽象掉了 Kubernetes 的复杂性。
+   - 工作负载可以移植到任何 Kubernetes 集群。
+   - Knative 有多个组件，可以解决多种用例。
+   - Eventing 是编排来自各种源的事件的组件。
+   - Service 是允许你部署基于容器的无服务器工作负载并将其提供给用户的组件。
+   - 无服务器 Kubernetes 工作负载可以使用 Knative on Anthos 进行部署和提供服务。
+   - 可以使用修订版本控制应用程序的版本。
+   - 可以使用修订参数管理应用程序的流量。
+   - 你可以使用丰富的开源工具生态系统来获取应用程序的洞察，用于监控、日志记录和追踪。

下一章将讨论 Anthos 网络的网络概念。

[1] Cloud Load Balancing: https://cloud.google.com/load-balancing

[2] Cloud Armor: https://cloud.google.com/armor

[3] Cloud CDN（内容分发网络）: https://cloud.google.com/cdn/docs/overview

[4] PaaS https://en.wikipedia.org/wiki/Platform_as_a_service

[5] CaaS: https://www.ibm.com/services/cloud/containers-as-a-service

[6] FaaS: https://en.wikipedia.org/wiki/Function_as_a_service

[7] Tekton Pipelines: https://github.com/tektoncd/pipeline

[8] App Engine: https://cloud.google.com/appengine

[9] Knative Service 在本章的下一节中进行了解释。

[10] Horizontal Pod Autoscaler（HPA）: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

[11] A/B 测试: https://martinfowler.com/articles/feature-toggles.html

[12] Canary 发布: https://martinfowler.com/bliki/CanaryRelease.html

[13] gRPC: https://grpc.io/docs/what-is-grpc/introduction/

[14] Cloud IoT Core: https://cloud.google.com/iot-core

## 12 Anthos - 网络环境

作者 Ameer Abbas

## 本章涵盖

+   - Anthos 云网络和多个云环境之间的混合连接。
+   - Anthos Kubernetes 和 GKE 网络，包括数据平面 v2。
+   - Anthos 多集群网络，包括服务发现和路由。
+   - 服务到服务和客户端到服务的连接。

Anthos 网络可以分为四个部分。每个部分在实体之间提供一层连接，例如环境（例如公有云和本地）、Anthos GKE 集群，以及服务到服务的通信。四个层如下：

+   1. **云网络和混合连接** - 本节涉及网络的最低层，涵盖不同基础设施环境如何互连。
+   2. **Anthos GKE 网络** - Anthos GKE 集群根据其部署的基础设施有多种实现。本节涵盖 Anthos GKE 集群网络，包括 Ingress 在各种环境中的工作方式。
+   3. **多集群网络** - 本节涉及各种 Anthos GKE 集群如何相互连接。Anthos GKE 集群可能部署在单个基础设施环境中（例如 GCP），也可能部署在多个基础设施环境中（GCP 和本地数据中心）。
+   4. **服务和客户端连接** - 本节涉及在 Anthos 上运行的应用程序如何相互连接。本节还涉及在 Anthos 外部运行的客户端和服务如何

连接到 Anthos 平台内部运行的服务。

## 图 12.1 Anthos 网络的四个层

![](img/fb3bebce7b5f593bca1a7f8a1371be06_361_0.png)

## 12.1 云网络和混合连接

本节涉及基础设施环境级别的网络连接各个方面。Anthos 是一个多云平台，可以运行在一个或多个公有和私有云环境中。

在基础设施层，有两种主要方式部署 Anthos。

+   1. 单一云环境中的 Anthos - 例如 GCP 或本地数据中心，甚至在另一个公有云中。

