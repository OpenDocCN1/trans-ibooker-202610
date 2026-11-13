
# 镜像和 Dockerfile

镜像是您保存在仓库中的内容。它们由您的 `Dockerfile` 的“编译”版本组成，包括所有需要运行的文件。`Dockerfile` 是描述镜像应有形态的文件，类似于源代码。一个 `Dockerfile` 通常会生成一个或多个镜像。镜像之间也可以相互依赖和关联。

请注意，例如，Swift 的 `Dockerfile` 长这样：

```
FROM swift:5.1.1
```

这意味着我们正在使用另一个镜像作为新镜像的基础。上述版本为 5.1.1 的 Swift 镜像，实际上使用的是 Ubuntu 镜像。类似于编程中的类可以相互关联和继承，Docker 镜像也可以相互基于。好的一点是，新的 Docker 镜像只包含与继承镜像的差异部分。这意味着我们的最终镜像可以非常小。

# 容器

容器是在主机上运行的镜像。因此，当 Docker 启动我们的镜像时，它们就变成了容器。换句话说，容器就是被启动的镜像。每当 Docker 用于运行服务器应用程序时，它就被归类为运行该应用程序的容器。因此，容器类似于应用程序的操作系统。

需要 Debian Linux 的应用程序将拥有一个类似于 Debian Linux 的容器。需要 CentOS 的应用程序将拥有一个 CentOS 环境。然而，这些可以在完全不同的主机系统（如 macOS）中运行。

# 启动 Hello World

在本节中，我们将启动一个实际的 Docker 镜像。最后，您应该已经运行了一个 Docker 容器，并看到了它的样子。让我们启动并运行 Docker。打开终端并运行以下命令：

```
% docker run hello-world
```

如果您收到以下输出，应确保 Docker Desktop 已启动并正在运行：

```
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
```

如果一切正常，您应该会看到以下内容：

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:9572f7cdcee8591948c2963463447a53466950b3fc15a247fcad1917ca215a2f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

注意 Docker 如何在找不到镜像时自动开始拉取它？Docker 总是检查本地缓存，所以要确认镜像是否已经存在。请记住 Docker 镜像可以相互关联。拉取镜像时，我们实际上希望 Docker 加载相关的镜像。在此例中，`hello-world` 镜像依赖项不多，但其他 Docker 镜像（如 `swift`）依赖于其他几个镜像。然而，镜像管理对我们开发者来说相当透明，所以我们不必过多担心。

现在我们了解了 Docker 的运作方式，并成功启动了“Hello World”应用程序，让我们开始在 AWS 上使用 Docker。

# 在 AWS 上运行 Docker

在 AWS 中运行 Docker 可以通过几种方式实现。AWS 代表 Amazon Web Services，是目前最流行、最大的云服务提供商。无数网站运行在 AWS 上，包括 Netflix、Airbnb、Amazon 等。在本节中，我们将学习如何在 AWS 上运行 Docker。

最不方便但可定制性最强的方法是我们自己在云计算机上安装 Docker，然后以那种方式运行我们的后端。在深入探讨如何在 AWS 中运行 Docker 之前，让我们先定义几个概念：

*   **Amazon Elastic Compute Cloud (EC2)：** 这是虚拟服务器（`units/computing units`）运行的地方。我们说“服务器”时，通常指的就是这个。这些机器通常是虚拟的，我们完全可以自行决定如何使用它们。操作系统、设置、权限等全由我们决定。

*   **Fargate：** 这是专门为 Docker 设计的 EC2 托管版本。在此版本中，除 Docker 镜像外的一切都由 AWS 替我们管理。

*   **Amazon Elastic Container Services (ECS)：** ECS 是一项 AWS 服务，允许我们以托管方式运行服务。

*   **任务描述：** Docker 容器的描述和配置。在 ECS 中启动的服务将使用任务描述来运行这些容器。一个任务可以运行多个容器，尽管通常一个就足够了。

*   **Elastic Container Registry (ECR)：** AWS 提供可用于存储 Docker 镜像的仓库。

*   **负载均衡器：** AWS 提供负载均衡器，以便您连接到服务。

在本节中，我们将探索在 AWS 上部署 Docker 最原生的方式。AWS 提供 Elastic Container Services (ECS) 作为部署 Docker 容器最直接的方式。ECS 的工作方式如下图所示：

![](assets/414a3fd1-abef-4b56-ade9-38faccdc04d9.png)

流程从您推送（上传）Docker 镜像到 ECR 开始。ECS 的任务定义将使用该镜像部署服务。服务可以拥有任意数量的实例。每个实例本质上都是一个运行在您自己的 EC2 实例或 Fargate 上的 Docker 容器。

请确保您已设置并运行 AWS CLI。如果需要帮助，请访问 `https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html`。

那么，让我们通过以下步骤启动并运行我们的服务：

1.  输入以下命令以创建 Docker 仓库：

```
% aws ecr create-repository --repository-name examplevapor
```

您应该会看到以下输出：

```
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:140099160703:repository/examplevapor",
        "registryId": "140099160703",
        "repositoryName": "examplevapor",
        "repositoryUri": "140099160703.dkr.ecr.us-east-1.amazonaws.com/examplevapor",
        "createdAt": 1580499621.0,
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        }
    }
}
```

2.  现在，我们必须创建一个集群来运行我们的应用程序：

```
% aws ecs create-cluster --cluster-name example-cluster
```

输出应如下所示：

```
{
    "cluster": {
        "clusterArn": "arn:aws:ecs:us-east-1:140099160703:cluster/example-cluster",
        "clusterName": "example-cluster",
        "status": "ACTIVE",
        "registeredContainerInstancesCount": 0,
        "runningTasksCount": 0,
        "pendingTasksCount": 0,
        "activeServicesCount": 0,
        "statistics": [],
        "tags": [],
        "settings": [
            {
                "name": "containerInsights",
                "value": "disabled"
            }
        ]
    }
}
```

这里我们使用的是 ECS 集群。这是管理项目和服务的好方法。这意味着您可以将所有相关服务放入一个集群中。

接下来，我们需要创建任务描述。您可以在本章的源代码中找到它，但这里简要列出其内容：

```
{
  "family": "vapor-example-app",
  "executionRoleArn": "arn:aws:iam::140099160703:role/ecsTaskExecutionRole",
  "containerDefinitions": [
    {
      "name": "vapor-example-app",
      "portMappings": [
        {
          "hostPort": 8080,
          "protocol": "tcp",
          "containerPort": 8080
        }
      ],
      "essential": true,
      "environment": [
        {
          "name": "EXAMPLE_VAR",
          "value": "configValue"
        }
      ],
      "image": "140099160703.dkr.ecr.us-east-1.amazonaws.com/examplevapor:latest"
    }
  ],
  "memory": "512",
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "networkMode": "awsvpc",
  "cpu": "256"
}
```
