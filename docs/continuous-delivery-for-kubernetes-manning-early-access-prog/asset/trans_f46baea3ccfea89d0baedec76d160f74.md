

![](img/2ab98a5ff1ec073700d902ba95287fd6_188_0.png)

基于路径的路由要求生产者或某种路由机制知道用于处理传入 CloudEvent 的函数的路径。

Figure 7.8 Multiple functions per `func` project using path-based routing

虽然某些框架已经开箱即用地提供了其中一个或两个功能，但你可以使用任何编程语言实现这些方法。

但无论我们在同一个容器内有一个或多个函数，我们如何连接一个函数与另一个函数呢？最好让函数对它们设计用于消费的事件做出反应，而不是手动直接向我们想要使用的函数发送 HTTP 请求。如果一个函数返回一个 CloudEvent，能否自动将其路由到正在消费该 CloudEvent 的函数呢？

## ROUTING EVENTS BETWEEN FUNCTIONS USING KNATIVE EVENTING

等等，我们已经见过如何使用 Knative Eventing 将事件的消费者和生产者连接起来，我们能否对函数也做同样的事情？答案是肯定的，所以让我们看看连接两个函数需要什么。

正如我们在之前的图表中所看到的，我们需要有不同的函数执行不同的操作，以测试在正确的时间调用了正确的函数。

让我们介绍我们的两个函数：Uppercase 和 Improve。

![](img/2ab98a5ff1ec073700d902ba95287fd6_188_1.png)

在处理函数时，让平台将事件路由到可以处理它们的函数是有意义的。生产事件的应用程序不会知道函数的具体位置以便直接请求它们。相反，它们会知道一个公共地址，在那里可以发送事件，而平台会负责路由。

正如你所看到的，作为输入，你可以提供一个 UppercaseRequestedEvent 或一个 UppercasedEvent，平台将知道如何将你的事件路由到正确的函数。如果你提供一个 UppercaseRequestedEvent，输出事件将自动转发到 Improve 函数，因为输出类型与 Improve 函数的输入类型匹配。

Figure 7.9 Uppercase and Improve function examples

为了让事情更有趣，我们可以用 Java 使用 Spring Boot 和 Spring Cloud functions 创建 *Uppercase* 函数，而用 Go 创建 *Improve* 函数。因为每个函数将使用不同的技术栈和编程语言，将两个函数放在同一个容器中是不可行的。因此我们将有两个函数，每个函数都在自己的容器中。对于创建这两个函数，我们可以使用 `func` 的另一个非常方便的功能，即使用来自自定义仓库的外部模板。我为这两个函数创建了模板，因此无需编码即可获得 Uppercase 和 Improve 函数的基础版本。

```
func create -l springboot -t uppercase --repository https://github.com/salaboy/func-templates
```

And

```
func create -l go -t improve --repository https://github.com/salaboy/func-templates
```

一旦我们有了两个函数，我们就需要弄清楚如何一起路由事件，为此我们将使用我们在第 6 章中学到的 Broker 和 Trigger 资源。

首先我们需要一个 Broker，它将作为来自我们应用程序的事件的主要入口点。关于我们可以有多少个 Broker 没有限制，但对于这个示例我们只需要一个。你可以通过将以下资源应用到你的集群来创建一个：

```
kubectl create -f - <<EOF
apiVersion: eventing.knative.dev/v1
kind: Broker
metadata:
  name: default #1
  namespace: default #2
EOF
```

#1 Broker 的名称是 default，你可以选择对你的应用程序有意义的任何名称
#2 这个 Broker 创建在 `default` 命名空间中，这对于理解如何从 Kubernetes 集群外部将事件路由到 Broker 很重要。

一旦我们有了 Broker，我们唯一需要做的就是创建 Triggers，以确保我们的函数接收到正确类型的事件。

我们将创建一个 Trigger 将事件转发到 `uppercase` 函数，并创建一个 Trigger 将事件转发到 `improve` 函数。

```
kubectl create -f - <<EOF
apiVersion: eventing.knative.dev/v1
kind: Trigger
metadata:
  name: uppercase-function-trigger
  namespace: default
spec:
  broker: default #1
  filter:
    attributes:
      type: UppercaseRequestedEvent #2
  subscriber:
    ref:
      apiVersion: serving.knative.dev/v1
      kind: Service
      name: fmtok8s-java-uppercase-function #3
---
apiVersion: eventing.knative.dev/v1
kind: Trigger
metadata:
  name: improve-function-trigger
  namespace: default
spec:
  broker: default
  filter:
    attributes:
      type: UppercaseRequestedEvent #4
  subscriber:
    ref:
      apiVersion: serving.knative.dev/v1
      kind: Service
      name: fmtok8s-go-improve-function
EOF
```

#1 两个 Triggers（`improve-function-trigger` 和 `uppercase-function-trigger`）都是为 `default` Broker 创建的。`default` 只是我们创建的 broker 的名称，它没有任何特殊含义。你可以拥有任意数量的 Broker，但触发器始终引用单个 broker。

#2 对于每个 Trigger，你可以定义过滤器。在撰写本书时，这些过滤器是精确匹配，这意味着只有匹配定义的过滤器的事件才会被转发给订阅者。你可以根据多个属性进行过滤，包括 CloudEvents 扩展，但所有属性都必须匹配。社区正在进行持续的努力，以为 Trigger 启用更高级和动态的过滤。

#3 正如我们在第 6 章中所看到的，我们可以通过指定可寻址的 Kubernetes 资源来引用订阅者，这允许 Trigger 验证订阅者是否存在。如果你的订阅者不是可寻址资源，你始终可以使用 `uri` 方法。有关这些选项的更多详细信息，请参阅官方 Knative 文档。

#4 通过对不同的事件类型进行过滤，我们可以串联函数或创建不同的拓扑来处理不同的事件。请注意，创建 Triggers 不需要开发人员来完成，并且函数如何连接可以在运行时完成，支持不同环境中的不同配置。

![](img/2ab98a5ff1ec073700d902ba95287fd6_191_0.png)

# Figure 7.10 Broker and Triggers to wire our functions together

回顾一下，我们创建了两个函数，每个函数期望不同种类的事件。`uppercase` 函数期望一个 `UppercaseRequestedEvent` 并返回一个 `UpperCasedEvent`，而 `improve` 函数期望一个 `UpperCasedEvent` 作为输入并返回一个 `ImprovedEvent` 事件，目前没有人消费它。

我期望 `func` 项目在未来包含一些工具，用于根据函数签名自动将触发器注册到 Broker，从而无需手动定义 Broker 和 Triggers。

要测试此设置，你只需要向 Broker URL 发送一个事件（你可以通过运行 `kubectl get broker` 找到 Broker URL，然后使用 `kubectl port-forward` 将 Broker Ingress Service 暴露到集群外部。如果你从集群内部发送事件，使用 Broker URL 是正确的方法。

## MONITORING ALL EVENTS GOING TO A BROKER WITH SOCKEYE

因为所有事件都进入 Broker，并且每个函数都将事件返回到 Broker，我们可以截取并将所有事件转发到另一个服务以查看发生了什么。为此我们可以使用 Sockeye，我们只需要再创建一个没有过滤器的 Trigger，换句话说，它会将所有事件发送到 Sockeye。

我们可以通过运行以下命令将 Sockeye 部署到我们的集群：

```
kubectl apply -f https://github.com/n3wscott/sockeye/releases/download/v0.7.0/release.yaml
```

并使用以下命令为 Sockeye 创建一个新的触发器：

```
kubectl create -f - <<EOF
apiVersion: eventing.knative.dev/v1
kind: Trigger
metadata:
  name: sockeye-trigger
  namespace: default
spec:
  broker: default #1
  subscriber:
    ref:
      apiVersion: serving.knative.dev/v1
      kind: Service
      name: sockeye #2
EOF
```

#1 再次，我们针对 `default` broker。请注意，此 Trigger 定义中没有 `filter` 部分，这意味着所有事件都将被转发到订阅者。

#2 对于此示例，我们使用 Sockeye，它也部署为 Knative Service。这个简单的 Web 应用程序将允许我们监控发送到 Broker 的所有事件。

