

## 考试提示

我们不期望 PCA 考试中出现关于 Eventarc 的问题。但请留意考试指南，因为这可能会在没有通知的情况下发生变化。至少，你应该记住 Eventarc 允许你基于 Pub/Sub 和审计日志触发事件。后者极其强大，因为所有 Google Cloud 服务都可以生成审计日志。了解有关 Eventarc 的更多信息，请查看 https://cloud.google.com/eventarc/docs/creating-triggers。

## 部署到 Cloud Run

要能够使用 Cloud Run，你需要在 API 库中启用 Cloud Run API。如果只是想测试一个已经容器化的应用程序，你可以通过在 Cloud Shell 中运行以下命令将其部署到 Cloud Run：

```
gcloud run deploy [SERVICE] --image gcr.io/[PROJECT-ID]/[IMAGE]
```

这将使用存储在 gcr.io/[PROJECT-ID]/[IMAGE] 路径中的镜像部署名为 [SERVICE] 的 Cloud Run 服务。

## 重要说明

如果你尚未配置默认区域，将提示你进行配置。

对于考试，你应该了解可以为 Cloud Run 服务设置的高级设置。因此，让我们看看如何从 Cloud 控制台部署服务：

- 1. 从汉堡菜单中选择 Cloud Run。你将看到 Cloud Run 欢迎页面：

![](img/de2c97ab017f36b3750a5bade111edd7_216_0.png)

图 7.4 – Cloud Run 欢迎页面

2. 点击 CREATE SERVICE 链接。填写服务的名称并选择服务托管的区域。点击 NEXT 进入第 2 部分：

图 7.5 – 服务设置

3. 在第 2 部分，指定要用于部署的容器镜像路径并点击 NEXT：

图 7.6 – 配置服务的第一个版本

4. 或者，你也可以使用第二个选项；即 **从源代码库持续部署新版本**。下面，你会看到如果选择此选项，我们将需要提供代码所在的代码库。Cloud Build 将使用此代码为你构建镜像，将其存储在 Container Registry 中，并部署到 Cloud Run。在每次拉取请求时，流程都会再次执行。这样，你就可以为 Cloud Run 服务获得无缝的 GitOps 流程。点击 **ADVANCED OPTIONS** 下拉菜单：

## 使用 Cloud Build 设置

在 Cloud Build 驱动的持续部署下，对源代码库的更改会自动构建为 Container Registry 中的容器镜像并部署到 Cloud Run。

你的代码应监听 $PORT 上的 HTTP 请求。你的代码库必须包含 Dockerfile 或 Go、Node.js、Python、Java 或 .NET Core 的源代码才能构建为容器镜像。

## 1 源代码库

Repository Provider
GitHub

Repository *
konradclapa/hello-world-py
Can't find your repository? Manage connected repositories

I understand that GitHub content for the selected repositories will be transferred to this GCP project to provide the connected service. Members of this GCP project with sufficient permissions will be able to create and run triggers on these repositories, based on transferred GitHub content. I also understand that content from all GitHub app triggers in this GCP project may be transferred to GitHub in order to provide functionality like showing trigger names in GitHub build results. This will apply to all existing and future GitHub App triggers in this project. Learn more

ADVANCED OPTIONS

NEXT

## 2 Build Configuration

图 7.7 – 源代码库

5. 在 **Build Configuration** 步骤中，你可以配置将触发流程的分支，以及构建类型。你可以提供带有镜像清单的 Dockerfile，或使用 Google Cloud Buildpacks（https://cloud.google.com/blog/products/containers-kubernetes/google-cloud-now-supports-buildpacks）用于受支持的编程语言：

## Build Configuration

![](img/de2c97ab017f36b3750a5bade111edd7_219_0.png)

图 7.8 – Build Configuration

6. 在我们的场景中，我们将使用现有容器镜像继续部署。因此，我们必须返回到第 3 步的菜单并进入 **Advanced settings** 部分。此部分包含多个标签页。正如你将看到的，其中大多数设置是 Pod 或 Deployments Kubernetes 资源清单中也可以找到的参数。一些你可能不熟悉的其他设置来自 Knative Service（https://knative.dev/docs/serving）清单。

在 **CONTAINER** 标签页中，我们可以执行以下操作：

- 设置自定义容器端口。
- 定义应由容器执行的带参数的命令。
- 分配内存和 CPU。
- 设置请求超时。
- 配置 **每个容器的最大请求数。**
- 设置关于最小和最大实例数的自动扩缩：

## Advanced settings

CONTAINER
VARIABLES & SECRETS
CONNECTIONS
SECURITY

## General

Container port
8080
Requests will be sent to the container on this port. We recommend listening on $PORT instead of this specific number.

Container command
Leave blank to use the entry point command defined in the container image.

Container arguments
Arguments passed to the entry point command.

## Capacity

Memory allocated
512 MiB
Memory to allocate to each container instance.

CPU allocated
1
Number of vCPUs allocated to each container instance.

Request timeout
300 seconds
Time within which a response must be returned (maximum 3600 seconds).

Maximum requests per container
80
The maximum number of concurrent requests that can reach each container instance. What is concurrency?

## Autoscaling ?

Minimum number of instances *
0

Maximum number of instances
100

7. 在 **VARIABLES & SECRETS** 标签页中，你可以添加环境变量和密钥。这些环境变量可由你的应用程序用于存储配置。密钥对于向应用程序传递凭据很有用。了解有关使用密钥的更多信息，请访问 https://cloud.google.com/run/docs/configuring/secrets：

![](img/de2c97ab017f36b3750a5bade111edd7_221_0.png)

图 7.10 – VARIABLES & SECRETS

8. 在 **CONNECTIONS** 标签页中，你可以为服务启用 **HTTP/2 连接**（https://cloud.google.com/run/docs/configuring/http2）。请注意，默认情况下 Cloud Run 使用 HTTP/1。你还可以创建到 Cloud SQL 的连接以持久化数据。正如你可能回忆的那样，保存在容器中的所有数据在容器停止时都会消失。将数据存储在 Cloud SQL 中将允许你创建有状态应用程序。最后，你可以创建 VPC 连接器以与连接到 VPC 的任何工作负载进行通信：

![](img/de2c97ab017f36b3750a5bade111edd7_222_0.png)

图 7.11 – CONNECTIONS

9. 在最后一个名为 SECURITY 的标签页中，你可以配置服务将使用的服务账号。你还可以配置 Binary Authorization（你将在第 8 章《使用 Anthos 管理云原生工作负载》中了解更多关于 Binary Authorization 的信息）和客户管理的加密密钥。我们将保持默认设置不变并点击 NEXT：

## Configure the service's first revision

A service can have multiple revisions. The configurations of each revision are immutable.

Deploy one revision from an existing container image

Container image URL *
us-docker.pkg.dev/cloudrun/container/hello
SELECT
E.g. us-docker.pkg.dev/cloudrun/container/hello
Should listen for HTTP requests on $PORT and not rely on local state. How to build a container?

Continuously deploy new revisions from a source repository

## Advanced settings

CONTAINER   VARIABLES & SECRETS   CONNECTIONS   SECURITY

Service account
Default compute service account
Identity to be used by the created revision.

Verify container deployment with Binary Authorization ? PREVIEW
Clicking the checkbox will enable the Binary Authorization API.

Use a customer-managed encryption key (CMEK) PREVIEW

NEXT

图 7.12 – SECURITY

10. 在第 3 阶段，我们定义服务将如何被触发。在 **Ingress** 部分，我们定义允许哪些流量。我们可以允许所有流量、允许内部流量和来自 Cloud Load Balancing 的流量，或仅允许内部流量。我们还必须选择身份验证方法。我们可以允许未经过身份验证的调用以实现公共访问，或使用 Cloud IAM 进行身份验证。通过点击 **+ ADD EVENTARC TRIGGER**，我们可以为服务定义触发器：

## 3 Configure how this service is triggered

A service can be invoked directly or via events. Click “Add Eventarc Trigger” to create a new event-based trigger. [Learn more](Learn more)

Ingress ?
● Allow all traffic
○ Allow internal traffic and traffic from Cloud Load Balancing
○ Allow internal traffic only

Authentication * ?
○ Allow unauthenticated invocations
Check this if you are creating a public API or website.
○ Require authentication
Manage authorized users with Cloud IAM.

+ ADD EVENTARC TRIGGER

图 7.13 – Configure how this service is triggered

