

在持续开发能力之上，Skaffold 提供了 CI/CD 管道的构建模块。它支持使用 kubectl、Helm[8] 和 Kustomize[9] 进行部署。让我们来看一下下方的图表，该图表可视化了使用 Skaffold 进行开发的流程。

**图 14.4 Skaffold 功能**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_483_0.png)

在图 14.4 中，你可以看到一个简单的管道可视化。Skaffold 会监视指定文件夹中的源文件更改。当检测到更改时，Skaffold 会自动构建镜像并将其推送到注册表。容器构建完成后，Skaffold 会将容器镜像部署到预定义的 Kubernetes 端点。在接下来的部分中，我们将解释 Skaffold 如何与 Cloud Code[10] 集成以获得更好的开发者体验（DX）。

## 使用 Skaffold

用户通过 `Command Line Interface`（CLI）与 Skaffold 交互。Skaffold 的完整指南可以在这里找到：[https://skaffold.dev/docs/references/cli/](https://skaffold.dev/docs/references/cli/)。为了快速了解如何使用 Skaffold，让我们来看一下基本步骤。我们将看到如何部署用 Go 编写的“Hello world”应用程序。

### 安装 Skaffold

安装 Skaffold 取决于底层操作系统。Skaffold 可以作为 gcloud 的组件安装。它也可以作为容器镜像 `gcr.io/k8s-skaffold/skaffold:latest` 提供，可直接在云原生 CI/CD 工具中使用。所有安装选项都在官方网站 [11] 中有说明。对于本地预览，你可以使用 Minikube，它允许你在笔记本电脑上部署应用程序。

### Skaffold 配置文件

Skaffold 使用单个配置 YAML 文件 `skaffold.yaml` 来定义 CD 管道中的步骤。它类似于 Kubernetes 资源清单。让我们来看一个非常基础的示例配置文件：

```yaml
apiVersion: skaffold/v2beta8
kind: Config
build:
  artifacts:
    - image: skaffold-example
deploy:
  kubectl:
    manifests:
      - k8s-*
```

在上述管道中定义了两个阶段：**build** 和 **deploy**。在构建阶段，Skaffold 会查找 Dockerfile 定义并使用它构建名为 `skaffold-example` 的容器镜像。在部署阶段，Skaffold 使用 kubectl 部署以“k8s-”前缀开头的所有 YAML 文件中定义的对象。

关于配置文件结构的非常全面的解释可以在 Skaffold 网站 [12] 上找到。

### 启动 Skaffold

你可以通过运行以下命令自动生成 `skaffold.yaml` 配置：

```bash
skaffold init
```

这将检测当前文件夹中的源文件并创建一个包含构建和部署部分的非常简单的配置文件。让我们创建三个文件：

- `Dockerfile` - 容器镜像定义
- `main.go` - 简单的“Hello world”Go 应用程序
- `k8s-pod.yaml` - Kubernetes Pod 定义

https://skaffold.dev/docs/references/yaml/

**Dockerfile 内容：**

```dockerfile
FROM golang:1.12.9-alpine3.10 as builder
COPY main.go .
ARG SKAFFOLD_GO_GCFLAGS
RUN go build -x -gcflags="${SKAFFOLD_GO_GCFLAGS}" -o /app main.go
FROM alpine:3.10
ENV GOTRACEBACK=single
CMD ["./app"]
COPY --from=builder /app .
```

**`main.go` 内容：**

```go
package main
import (
    "fmt"
    "time"
)

func main() {
    for {
        fmt.Println("Hello world!")
        time.Sleep(time.Second * 1)
    }
}
```

**`k8s-pod.yaml` 内容：**

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: getting-started
spec:
    containers:
    - name: getting-started
      image: skaffold-example
```

这将生成一个我们在上一节中已经看到的 `skaffold.yaml` 文件，其中包含两个阶段：

```yaml
apiVersion: skaffold/v2beta3
kind: Config
metadata:
    name: getting-started
build:
    artifacts:
    - image: skaffold-example
deploy:
    kubectl:
        manifests:
        - k8s-pod.yaml
```

你可以从这里开始，根据需要使用 Skaffold 文档扩展该文件。

### 使用 Skaffold 开发

现在我们文件夹中已经有四个文件，包括 Skaffold 配置文件。现在我们可以开始持续开发，Skaffold 将监视源文件夹中的更改并执行 `skaffold.yaml` 文件中定义的所有步骤。

要开始开发，运行命令：

```bash
skaffold dev
```

Skaffold 会自动将已部署容器的日志尾部输出到控制台。

现在，如果你将源文件 `main.go` 中的打印内容从“hello world”改为“hello from Skaffold”，Skaffold 将自动检测更改，重建镜像，将其推送到注册表并部署。由于日志会尾部输出到控制台，你应该会看到消息“hello from Skaffold”。

### 使用 Skaffold 单次运行

虽然 `skaffold dev` 会持续监视源文件，你也可以通过运行 `skaffold run` 命令执行一次工作流。这在你只想运行一次执行而不希望每次代码修改时都触发流程时很有用。

### 支持的功能

到目前为止，我们应该对如何使用 Skaffold 开始开发有了基本了解，让我们来看一下其他有用的功能。

### 管道阶段

到目前为止，我们只查看了 Skaffold 的基本功能，然而该工具还有更多能力可以应对高级管道阶段。例如，开发人员可能不希望在每次代码更改后都重建镜像。在这种情况下，Skaffold 可以将文件，例如 `main.go` 源文件，同步到容器中。要实现这一点，你需要使用文件同步功能。下面可以找到当前所有可用阶段的列表：

**图 14.5 Skaffold 工作流**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_487_0.png)

虽然图 14.5 显示了工作流中的所有步骤，但下面的列表显示了可用于执行这些步骤的所有 Skaffold 管道阶段：

- **Init** 生成基本的 Skaffold 配置
- **Build** 使用选择的构建器构建镜像
- **Test** 使用 structure-tests[13] 测试镜像
- **Tag** 基于不同策略标记镜像
- **Deploy** 使用 kubectl、kustomize 或 helm 部署应用程序
- **File Sync** 将更改的文件直接同步到正在运行的容器
- **Log Tailing** 从容器尾部输出日志
- **Port Forwarding** 将服务端口转发到 localhost
- **Cleanup** 清理清单和镜像

正如我们所见，我们有一个完整的管道，可以让我们部署和预览 Anthos 应用程序。

### 支持的环境

Skaffold 支持本地和远程 Kubernetes 集群。你可以使用本地开发集群如 Minikube，或部署在远程位置的 Anthos/Kubernetes 集群。

要连接到远程 Kubernetes 集群，你需要在 `kubeconfig` 文件中设置上下文，就像连接到任何 Kubernetes 集群一样。默认上下文可以通过运行命令 `skaffold dev --kube-context <myrepo>` 或通过更新 `skaffold.yaml` 文件中的 `deploy.kubeContext` 属性来覆盖，例如：

```yaml
deploy:
  kubeContext: minikube
```

### 支持的构建工具

如果你需要使用其他工具构建容器镜像，`skaffold.yaml` 配置文件中的 `build` 部分可以为自定义构建器配置。要使用自定义构建器，在 `build` 部分定义使用该构建器的适当选项。有关自定义构建器的详细信息请参阅 [https://skaffold.dev/docs/pipeline-stages/builders/](https://skaffold.dev/docs/pipeline-stages/builders/)

目前支持以下工具：

- Docker
- Jib[^14]
- Bazel[^15]
- Buildpacks[^16]

你也可以使用符合 Skaffold 定义标准的自定义脚本。

### 在 CI/CD 管道中使用 Skaffold

Skaffold 可以用作 CI/CD 管道中的工具。已经存在一个社区维护的构建器 [^17]，可以直接与 Cloud Build 一起使用，Cloud Build 是原生的 Google CI 工具（我们将在下一节详细了解它）。在 CI/CD 工作流中，最有用的 Skaffold 命令包括：

- `skaffold build` - 构建并标记你的镜像
- `skaffold deploy` - 部署给定的镜像
- `skaffold delete` - 清理已部署的制品
- `skaffold render` - 构建并标记镜像，并输出模板化的 Kubernetes 清单

### Skaffold 总结

在本节中，我们学习了如何安装 Skaffold 并在持续开发工作流中使用它。有关使用 Skaffold 进行开发工作流的更多信息，请遵循 Skaffold 快速入门指南[^18]。

## 14.2.3 Cloud Code：使用本地 IDE 开发

到目前为止，我们已经学习了如何预览 Anthos 应用程序，但

