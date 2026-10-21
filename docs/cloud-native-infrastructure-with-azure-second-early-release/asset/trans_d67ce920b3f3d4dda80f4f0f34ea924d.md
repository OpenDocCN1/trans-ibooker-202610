

创建函数应用时，还必须链接一个存储账户。这可用于临时存储。每个应用在创建时都会被分配一个公共的 HTTP(s) 端点，以便函数可以被访问。可以使用私有端点来保护函数应用。

## 创建函数应用

在本节中，我们将使用 Azure 门户创建一个函数应用。

- 我们将开始打开 Azure 门户并在 Function App 下点击 Create。
- 现在我们将填写函数应用的基本参数：
  - 我们将创建一个名为 serverless-test 的新资源组。
  - 我们应用程序的 DNS 名称将是 ourtestfunctionapp.azurewebsites.net。
  - 我们将运行代码（一个函数）而不是容器。
  - 语言将是 Python v3.9。
  - 函数应用将部署在 East US 区域。
- 在 Hosting 选项卡中：
  - 我们将创建一个名为 functionstorageaccount 的新存储账户。由于存储账户名称在全球范围内唯一，您需要选择自己的名称。
  - 因为我们运行的是 Python，所以必须运行 Linux。
  - 最后，为本示例的目的，我们将使用 Consumption 计划。
- 在 Monitoring 选项卡中：
  - 我们将关闭 Application Insights。
  - 然后我们将点击 Review + Create。

现在我们已经创建了函数应用，我们需要创建一个函数。Visual Studio Code 有一个扩展，Azure Functions，它使创建、测试和发布函数变得简单。

- 如果您尚未安装 Visual Studio Code，您可以从 Visual Studio Code 下载页面下载。
- 安装 Visual Studio Code 后，您需要安装 Azure Functions 扩展。
- 点击屏幕左侧活动栏上的 Azure 图标，这将打开 Azure Functions 窗口。点击“Create new project”。
- 将弹出一个对话框，您需要在其中选择一个本地目录来保存您的代码。
- 然后您将被提示在单独的 Visual Studio Code 弹出窗口中输入一些项目信息：
  - 选择函数项目的语言：选择 Python。
  - 选择一个 Python 别名来创建虚拟环境：选择您的 Python 解释器的位置。
  - 如果位置未显示，输入您的 Python 二进制文件的完整路径。
  - 选择项目的第一个函数的模板：选择“HTTP trigger”。
  - 提供函数名称：输入 HttpExample。
  - 选择授权级别：选择 Anonymous，这允许任何人调用您的函数端点。要了解授权级别，请参阅 Authorization keys 文档。

- 选择您希望如何打开项目：选择“Add to workspace”。
- 使用此信息，Visual Studio Code 生成一个带有 HTTP 触发器的 Azure Functions 项目。您可以在 Explorer 中查看本地项目文件。要了解更多关于创建的文件的信息，请参阅 Generated project files 文档。

- 此时，您将拥有一个项目的骨架。向 *HttpExample* 文件添加以下代码，这将返回向函数发出请求的客户端的 IP（类似于 *icanhazip.com*）：

```
import azure.functions as func

def main(req: func.HttpRequest ) -> func.HttpResponse:
    response = False
    if (req.headers.get('X-Forwarded-For')):
        response = req.headers.get('X-Forwarded-For').split(':')[0]
    else:
        response = "127.0.0.1"
    return func.HttpResponse(response)
```

- 通过按键盘上的 F5 然后访问 http://localhost:7071/api/HttpExample 来测试您的函数。
- 要部署到 Azure，您需要在 Azure Functions 窗口中点击“Sign in to Azure…”按钮。
- 点击 Upload 按钮，这将允许您将函数上传到之前创建的应用。

现在我们已经了解了 Azure 原生的产品，让我们讨论使用 Kubernetes 部署的函数应用。

## Knative

Knative 是一个构建在 Kubernetes 之上的无服务器平台，在 Kubernetes 发布时因其热度而成为行业早期热门选择。Knative 的优势之一是函数被打包并作为 Docker 容器执行，这使得将其集成到现有云环境中的工作量最小。

## Knative 架构

Knative 有两种运行模式：

- Knative Serving 使用 Kubernetes 部署无服务器函数或容器，并完全支持自动缩放（包括缩减到零 Pod）。它还额外支持 Gloo 和 Istio 等云网络解决方案。
- Knative Eventing 允许开发人员开发围绕事件驱动架构构建的应用程序。在事件化部署中，您可以指定事件生产者（源）以做出响应。

让我们看看如何在 Kubernetes 上安装 Knative。

## 在 Kubernetes 上安装和运行 Knative Serving

在本节中，我们将运行一个基本的 Knative serving 系统。

- 首先安装自定义资源：
- 然后安装 serving 组件：

```
kubectl apply -f https://github.com/knative/net-kourier/releases/download/v0.26.0/kourier.yaml
```

```
kubectl apply -f https://github.com/knative/serving/releases/download/v0.26.0/serving-core.yaml
```

如果您想使用任何网络集成（Kourier、Ambassador、Contour、Istio），您可以按照网络层说明进行操作。

- 现在您需要创建您的应用程序。我们将使用 Python 的 Flask 库创建一个简单的应用程序：
```
import logging
import os

from flask import Flask

app = Flask(__name__)

@app.route('/', methods=['POST'])
def hello_world():
    return f'Hello world. Data {request.data}\n'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=int(os.environ.get('PORT', 8080)))
```

- 现在您需要一个包含您的无服务器应用程序的 Docker 镜像：
```
FROM python:3.7-slim

RUN pip install Flask gunicorn

WORKDIR /app
COPY . .
```

```
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 app:app
```

- 使用以下命令构建并推送您的新无服务器镜像：

```
docker push https://myharborinstallation.com/helloworld:v1
```

- 现在在名为 *hello-world.yaml* 的文件中定义您的无服务器服务：

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: helloworld
  namespace: default
spec:
  template:
    metadata:
      name: helloworld
    spec:
      containers:
        - image: docker.io/test/helloworld:v1
```

- 使用以下命令部署函数：

```
kubectl apply -f hello-world.yaml
```

- 此时，您的无服务器函数将被部署。您可以通过运行以下命令找到函数的 URL：

```
$ kubectl get ksvc hello-world --output=custom-columns=NAME:.metadata.name,URL:.status.url
```

- 通过调用 URL 测试您的函数：

```
$ curl -X POST <URL found from above> -d "My Data"
Hello World: My Data
```

现在一个基本的 serving 函数正在运行，让我们看看 Knative eventing。

## 在 Kubernetes 上安装和运行 Knative Eventing

在本节中，我们将安装 Knative eventing 组件并编写一个简单的事件系统。

- 首先安装自定义资源：

```
kubectl apply -f https://github.com/knative/eventing/releases/download/v0.26.0/eventing-crds.yaml
```

- 现在安装 serving 组件：

```
kubectl apply -f https://github.com/knative/eventing/releases/download/v0.26.0/eventing-core.yaml
```

此时，您将拥有 eventing 组件已安装。我们将重用前面部分中的 Docker 镜像。然而，我们将配置 Knative，以便我们可以向 Knative 内的消息总线发送 HTTP 消息，而不是发布到 HTTP 应用程序服务器。当消息到达消息总线时，它将触发我们的 Python 应用程序服务器。在更复杂的示例中，您可以将 Kafka 主题设置为事件源。

- 创建一个 *knative-eventing.yaml* 文件：

