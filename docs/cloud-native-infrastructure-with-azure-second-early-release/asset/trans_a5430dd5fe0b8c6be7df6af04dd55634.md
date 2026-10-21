

```
$ kubectl apply -f keda-config/jobs/secret.yaml
$ kubectl apply -f keda-config/jobs/keda.yaml
```

+   - 通过运行以下命令验证你的扩缩作业是否就绪：

```
$ kubectl get ScaledJob
$ kubectl get TriggerAuthentication
```

+   - 你可以通过运行测试脚本来测试你的 KEDA 部署。首先你需要设置 Python 环境：

```
$ python3 -m venv .
$ source bin/activate
$ pip3 install logger
$ pip3 install azure-servicebus
```

+   - 现在你需要创建你的程序来向 Azure Service Bus 队列发送消息。你需要修改 connection_string 和 queue_name：

```
import os
import sys
import time
import yaml
from logger import logger
from azure.servicebus import ServiceBusClient,
ServiceBusMessage
```

```
def send_a_list_of_messages(sender):
    messages = [ServiceBusMessage(f"Message in list {i}")
    for i in range(1000)]
    sender.send_messages(messages)
    logger.info("Sent a list of 1000 messages")
```

```
def main():
    with open("azure-service-bus.yaml", 'r') as stream:
        config = yaml.safe_load(stream)

    logger.info("Start sending messages")
    connection_string = <FIXME>
    queue_name = <FIXME>

    queue =
ServiceBusClient.from_connection_string(conn_str=connecti
on_string, queue_name=queue_name)

    with queue:
        sender =
queue.get_queue_sender(queue_name=queue_name)
        with sender:
            send_a_list_of_messages(sender)

    logger.info("Done sending messages")
    logger.info("------------------------")

if __name__ == "__main__":
    main()
```

+   - 运行 *python3 send_message_queue.py*。这将创建 1,000 条消息并发送到 Azure Service Bus 队列。

你可以在 Azure Service Bus 队列中看到消息被摄取然后处理（图 11-4）。

![](img/8d47d1b5dbca55e784ae602145a894c7_378_0.png)

图 11-4. Azure Service Bus 队列

在下一节中，我们将介绍 OpenFaaS，它在独立系统和 Kubernetes 集群上都提供简单、无[content]

```
$ kubectl apply -f keda-config/jobs/secret.yaml
$ kubectl apply -f keda-config/jobs/keda.yaml
```

+   - 通过运行以下命令验证你的扩缩作业是否就绪：

```
$ kubectl get ScaledJob
$ kubectl get TriggerAuthentication
```

+   - 你可以通过运行测试脚本来测试你的 KEDA 部署。首先你需要设置 Python 环境：

```
$ python3 -m venv .
$ source bin/activate
$ pip3 install logger
$ pip3 install azure-servicebus
```

+   - 现在你需要创建你的程序来向 Azure Service Bus 队列发送消息。你需要修改 connection_string 和 queue_name：

```
import os
import sys
import time
import yaml
from logger import logger
from azure.servicebus import ServiceBusClient,
ServiceBusMessage
```

```
def send_a_list_of_messages(sender):
    messages = [ServiceBusMessage(f"Message in list {i}")
    for i in range(1000)]
    sender.send_messages(messages)
    logger.info("Sent a list of 1000 messages")
```

```
def main():
    with open("azure-service-bus.yaml", 'r') as stream:
        config = yaml.safe_load(stream)

    logger.info("Start sending messages")
    connection_string = <FIXME>
    queue_name = <FIXME>

    queue =
ServiceBusClient.from_connection_string(conn_str=connecti
on_string, queue_name=queue_name)

    with queue:
        sender =
queue.get_queue_sender(queue_name=queue_name)
        with sender:
            send_a_list_of_messages(sender)

    logger.info("Done sending messages")
    logger.info("------------------------")

if __name__ == "__main__":
    main()
```

+   - 运行 *python3 send_message_queue.py*。这将创建 1,000 条消息并发送到 Azure Service Bus 队列。

你可以在 Azure Service Bus 队列中看到消息被摄取然后处理（图 11-4）。

![](img/8d47d1b5dbca55e784ae602145a894c7_378_0.png)

图 11-4. Azure Service Bus 队列

在下一节中，我们将介绍 OpenFaaS，它在独立系统和 Kubernetes 集群上都提供简单、无附加功能的函数。

## OpenFaaS

OpenFaaS 是 Kubernetes 的另一个事件驱动函数服务，允许你使用任何语言编写函数，然后将它们打包到 Docker 或 OCI 兼容的容器中。

### OpenFaaS 架构

OpenFaaS 参考架构（如图 11-5 所示）与 KEDA 相当相似。然而，OpenFaaS 确实依赖于 Prometheus 的指标作为自动扩缩信号。

![](img/8d47d1b5dbca55e784ae602145a894c7_378_1.png)

图 11-5. OpenFaaS 参考架构

## 安装 OpenFaaS

OpenFaaS 有三种推荐的安装方法：

- 使用 `arkade`
- 使用 `helm`
- 使用 Flux 或 ArgoCD 安装

为了与本书其余部分保持一致，我们将使用 Helm 部署 OpenFaaS。

- 创建命名空间（一个用于 OpenFaaS 核心服务，一个用于函数）：

```
kubectl apply -f
https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

- 添加 OpenFaaS Helm 图表：

```
helm repo add openfaas https://openfaas.github.io/faas-netes/
```

- 最后，部署 OpenFaaS：

```
helm repo update \
  && helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas \
    --set functionNamespace=openfaas-fn \
    --set generateBasicAuth=true
```

- 你可以通过运行以下命令检索管理员密码；该密码将用于访问 OpenFaaS 仪表板：

```
PASSWORD=$(kubectl -n openfaas get secret basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode)
&& \
echo "OpenFaaS admin password: $PASSWORD"
```

现在我们已经安装了 OpenFaaS，我们将编写一个 OpenFaaS 函数。

## 编写你的第一个 OpenFaaS 函数

在本节中，我们将利用 faas-cli 快速创建和部署一个无服务器函数。

- 通过在 OSX 上运行 `brew install faas-cli`，或在 Linux 上运行 `curl -sSL https://cli.openfaas.com | sudo sh` 来下载 faas-cli。
- 创建一个文件夹来放置你的新函数：
- 使用 faas-cli 创建一个新的 Python 函数项目：

```
mkdir -p ~/functions && cd ~/functions
```

```
faas-cli new --lang python helloworld
```

这将创建三个新文件：

```
$ ls
helloworld/handler.py
helloworld/requirements.txt
helloworld.yml
```

handler.py 文件是你编写函数的地方。requirements.txt 文件与标准的 Python *requirements.txt 文件*相同。*helloworld.yml* 文件定义了函数的一些样板。

- 在 *handler.py* 文件中，我们将定义我们的函数。此函数将打印 "Hello World! You said: " 加上发布到函数端点的数据：
  
  ```
  def handle(req):
      print("Hello World! You said: " + req)
  ```

- 最后，在你的 *helloworld.yml* 文件中，你将定义函数的相关元数据。http://127.0.0.1:31112 是 OpenFaaS 网关/API 服务器的默认地址：
  
  ```
  provider:
    name: openfaas
    gateway: http://127.0.0.1:31112
  
  functions:
    hello-python:
      lang: python
      handler: ./hello-world
      image: hello-world
  ```

- 构建无服务器镜像并推送到你在第 2 章中创建的 Harbor 仓库：
  
  ```
  $ faas-cli build -f ./hello-world.yml
  $ faas-cli push -f ./hello-world.yml
  ```

- 部署到 Kubernetes 集群：
  
  ```
  $ faas-cli deploy -f ./hello-world.yml
  Deploying: hello-python.
  No existing service to remove
  ```

```
Deployed.
200 OK
URL: http://127.0.0.1:31112/function/hello-python
```

- 上一步输出了你可以测试的 URL。使用 `curl` 来测试它：

```
$ curl -X POST 127.0.0.1:31112/function/hello-world -d
"My name is Michael"
Hello world! My name is Michael
```

默认情况下，OpenFaaS 使用每秒请求数（rps）作为执行自动扩缩的机制。你可以在 Autoscaling 文档页面上找到有关 OpenFaaS 自动扩缩配置的更多信息。

此时，你将在 Kubernetes 上拥有一个运行中的 OpenFaaS 函数。你也可以使用 faasd 在 Kubernetes 外部运行 OpenFaaS。有关更多信息，请参阅 OpenFaaS 部署页面。

最后，OpenFaaS 建立在 Kubernetes HPA 之上。你可以从 OpenFaaS Kubernetes 教程页面了解有关如何负载测试和自动扩缩你的 OpenFaaS 函数的更多信息。

## 总结

虽然无服务器绝对不是适合所有人的方案，但在正确的情况下使用可以提供显著的优势。无服务器空间仍在不断发展，但入门门槛极低，这使其成为令人兴奋的技术。在本章中，我们探讨了四种在 Azure 上运行无服务器函数的方式。首先我们介绍了 Azure 函数应用，然后介绍了三个领先的 CNCF 项目：Knative、KEDA 和 OpenFaaS。此时，我们已经概述了云原生基础设施的主要支柱。在第 12 章中，我们将回顾你在整个旅程中学到的内容。

## 第 12 章. 结论

## 早期发布读者的注意事项

使用 Early Release 电子书，你可以在这些标题的正式发布之前很久就利用这些技术——作者在撰写时提供的原始和未编辑内容，以便你可以在这些技术正式发布之前利用它们。

这将是最终书籍的第 12 章。请注意，GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何意见，或者如果你注意到本章中缺少材料，请联系编辑 rfernando@oreilly.com。

我们已经到了本书的结尾，让我们回顾一下我们的旅程。在本书中，我们的目标是提供一个单一资源，你可以用它在 Azure 中构建和运行云原生基础设施。虽然 Azure 是我们的重点，但你也可以将相同的知识应用于在不同云厂商平台上构建和运营你自己的云原生环境。让我们回顾一下你迄今为止学到的内容。

我们首先回顾了基础设施和服务世界随着云的引入而随时间演变的过程。为了充分利用云所提供的优势，我们需要开发原生于云的解决方案，而不是事后将其改编为云。我们理解云原生是充分利用云强大功能的方式。

在第 2 章中，我们介绍了使用 Azure 构建现代云原生环境的基石。我们设置了 Azure 账户并开始使用 Ansible、Terraform 和 Packer。通过将它们作为我们的关键工具，我们可以以自动化方式启用生产就绪的服务。我们

