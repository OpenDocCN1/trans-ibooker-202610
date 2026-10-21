

+   - 在任务之间共享数据的输入和输出映射
+   - 事件触发器，允许你监听将触发 Pipeline 或 Task 的事件
+   - 一个命令行工具，可轻松从终端与 Task 和 Pipeline 交互
+   - 一个简单的仪表板，用于监控你的 pipeline 和任务执行情况。

![](img/2ab98a5ff1ec073700d902ba95287fd6_90_0.png)

Figure 3.X Tekton Dashboard a user interface to monitor your pipelines

图 3.x 显示了社区驱动的 Tekton Dashboard，你可以用它来可视化你的 pipeline 执行情况。请记住，由于 Tekton 是构建在 Kubernetes 之上运行的，你可以使用 `kubectl` 来监控你的 pipeline，就像监控任何其他 Kubernetes 资源一样，但对于技术性较低的用户来说，没有什么能比得上用户界面的。

但现在，如果你想使用 Tekton 实现一个 Service Pipeline，你将花费大量时间定义 Task、Pipeline、如何映射输入和输出、为你的 Git 仓库定义正确的事件监听器，然后再深入到低层级定义每个 Task 将使用哪些 docker 镜像。创建和维护这些 pipeline 及其相关资源可能会成为一份全职工作，为此 Tekton 启动了一个计划来定义一个 Catalog，其中可以共享 Task（Pipeline 和 Resources 计划在未来版本中发布），即 Tekton Catalog：https://github.com/tektoncd/catalog

在 Tekton Catalog 的帮助下，我们可以创建引用在 catalog 中定义的 Task 的 pipeline，因此我们无需担心定义它们。你还可以访问 https://hub.tekton.dev，它允许你搜索 Task 定义并为你在 pipeline 中安装和使用这些任务提供详细的文档。

![](img/2ab98a5ff1ec073700d902ba95287fd6_91_0.png)

Figure 3.X Tekton Hub a portal to share and reuse Tasks and Pipeline definitions

一个很好的例子是 Helm Upgrade from Source Task，它允许你基于 Helm Chart 源升级 Helm release。你可以在这里查看任务文档：https://hub.tekton.dev/tekton/task/helm-upgrade-from-source 像这样的 Task 对于实现环境 Pipeline 非常有帮助，我们用 Helm chart 定义环境的 content，然后每当我们更改 chart 的 content 时，就只需用 Tekton 升级 Helm Release。

Tekton Hub 和 Tekton Catalog 允许你重用由大量用户和公司创建的 Task 和 Pipeline。但你很快就会意识到 Tekton 对于你用构建的 pipeline 做什么并没有太多主见，因此在创建 Service 或 Environment pipeline 时，你需要做出大量不同的选项和选择。难道不是很好，如果所有被大量其他人使用的默认值、约定和最佳实践都能在一个工具中共享吗？在下一节中，我们将看看 Jenkins X 以及它如何采取整体方法来解决 Kubernetes 的 CI/CD。

## 3.3.2 Jenkins X: A one-stop-shop for CI/CD in Kubernetes

Jenkins X (http://jenkins-x.io) 为在 Kubernetes 之上进行 CI/CD 提供了一整套方案。这从安装你需要的组件开始，例如 Pipeline Engine（本例中为 Tekton）到监控 pipeline 的用户界面、制品仓库以及应用于基于约定的 pipeline 的最佳实践。理论上，使用 Jenkins X，在你安装之后，只需通过一条命令行导入你的服务，Service 和 Environment Pipeline 就会为你的项目设置好。Jenkins X 采取整体方法来解决 CI/CD，因此起初它是一个复杂的项目，很难掌握。本节旨在解释 Jenkins X 如何解决并简化 Kubernetes 项目的整个 CI/CD 流程，我的建议始终是尝试在自己的集群上使用该项目，但要解释所有组件如何协同工作则需要一整本书。

## JENKINS X ARCHITECTURE

如果你有兴趣尝试 Jenkins X，你需要明白 Jenkins X 分为两个主要组件：

+   - **一个名为 `jx` 的命令行工具**：你需要从 http://jenkins-x.io 网站下载 `jx` 命令行，或使用 brew for Mac OSX 或包管理器在本地安装命令行。

+   - **安装到 Kubernetes 集群中的一组服务和组件**：通过使用 `jx` 命令行工具，你可以在现有的 Kubernetes 集群中安装 Jenkins X。你可能会在部署应用程序的单独集群中安装 Jenkins X。

下图显示了安装 Jenkins X 时默认安装的组件，无论你决定在哪个云提供商上安装它。

![](img/2ab98a5ff1ec073700d902ba95287fd6_92_0.png)

在集群中安装 Jenkins X 时，它会安装构建你的云原生服务和管理你的环境所需的所有基础设施。

Figure 3.X Jenkins X Components installed on a Kubernetes Cluster

使用名为 `jx` 的命令行工具，你可以将 Jenkins X 安装到你的集群中。为了运行 Jenkins X，你需要一个 Kubernetes 集群作为起点，Jenkins X 可以为你创建。Jenkins X 使用名为 Terraform (https://www.terraform.io) 的工具来创建集群并在不同的云提供商中安装所需的组件。它还使用 Kubernetes 和云提供商特定的服务来确保你的服务和环境 pipeline 拥有所需的一切。例如，如果你在 Google Cloud Platform 上安装 Jenkins X，它会配置 GCR（Google Container Registry）用于存储由你的服务 pipeline 生产的 docker 镜像。在本节中，我们将看看在集群中安装 Jenkins X 时会安装什么，以及从零到为你的服务实现完整 CI/CD 的基本命令。

Jenkins X 安装并配置了服务和环境 pipeline 所需的所有组件，确保组件可以注册到 git 仓库以接收更改，并且在推送更改时 pipeline 会自动触发。Jenkins X 还安装了 pipeline 引擎（Tekton）以及用于存储作为 pipeline 流程创建的制品的仓库。请记住，你需要一个地方来存储你的二进制制品（已安装 Nexus）、你的 docker 镜像（取决于你安装 Jenkins X 的云提供商，将使用不同的容器注册表，如果你在 Google Cloud，则会使用 GCR.io）以及你的 Kubernetes 清单。Jenkins X 依赖 Helm 来打包和分发 Kubernetes 清单（YAML 文件），因此它会在集群中安装 Chart Museum。

当你在 Kubernetes 集群中安装 Jenkins X 时，除了安装它所需的所有组件外，它还会自动创建两个环境：Staging 和 Production，以及各自的 Environment Pipeline 和包含默认配置的 Git 仓库。如以下图所示，你的应用程序服务将从 `dev` 环境开始，自动提升到 `staging` 环境，最终到达 `production` 环境，用户在该环境中消费我们的服务。

![](img/2ab98a5ff1ec073700d902ba95287fd6_93_0.png)

Figure 3.X Jenkins X pre-defined environments

两个 pipeline 都将与两个单独的命名空间关联，每个环境的 workload 都将在其中运行。默认情况下，不会创建任何 service pipeline，因为尚未定义应用程序服务。

作为安装过程的一部分，Jenkins X 创建了两个默认环境，名为 "staging" 和 "production"，为 staging 环境配置了自动提升，这意味着每当我们的服务有新版本时，Environment Pipeline 将负责将服务“提升”（添加或升级服务）到环境中。生产环境需要手动提升，这意味着用户将负责决定何时可以将服务的新版本安装到生产环境中。

到目前为止，仅安装 Jenkins X 后，我们就有了两个环境和默认的环境 pipeline 可以使用。你现在准备好开始添加服务了。

## YOUR PROJECTS AND JENKINS X

一旦所有 pipeline 所需的组件都安装并配置为协同工作，你就可以开始将你的项目“导入”到 Jenkins X。

让我们看看这对于我们的 Conference Platform 应用程序会是什么样子，该应用程序由四个服务组成。如果我们有四个服务，我们就需要四个 service pipeline。每个 pipeline 都将独立发布每个服务。在 Jenkins X 中，我们可以通过

