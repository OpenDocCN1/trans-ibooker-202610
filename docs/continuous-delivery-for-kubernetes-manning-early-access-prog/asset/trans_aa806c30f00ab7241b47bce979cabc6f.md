

Kubernetes YAML 文件在服务仓库中为每个服务提供，然后环境仓库会复制这些文件，它们可能会不同步。想象一下，如果你有多个环境，你将需要维护所有副本的一致性，这可能会变得非常具有挑战性。

第二个选项（Helm 布局）稍微复杂一些，现在我们使用 Helm 来定义集群的状态。你可以使用 Helm 依赖项创建一个父 chart，该 chart 将包含应存在于环境中的所有服务的依赖项。如果这样做，环境流水线可以使用 `helm update` 将 chart 应用到集群中。我不喜欢这种方法的一点是，你为每次更改创建一个 Helm release，并且没有为每个服务单独的 release。这种方法的先决条件是每个服务都作为 Helm chart 打包，以便环境可以获取。

第三个选项是使用一个名为 `helmfile` 的项目（https://github.com/robol1/helmfile），它正是为此特定目的而设计的，用于定义环境配置。`helmfile` 允许你以声明方式定义需要在我们的集群中存在的 Helm release。这些 Helm release 将在我们运行 `helmfile sync` 时创建，前提是已定义了一个包含我们希望在集群中拥有的 helm release 的 `helmfile`。

无论你使用这些方法中的任何一种还是其他工具来执行此操作，期望都是明确的。你有一个包含配置的仓库（通常每个环境一个仓库），流水线将负责获取配置并使用工具将其应用到集群。

通常会有多个环境（staging、qa、production），甚至允许团队按需创建自己的环境以运行测试或日常开发任务。

如果你使用“每个环境一个命名空间”方法，通常为每个环境维护一个单独的 git 仓库，因为这有助于保持对环境的访问隔离和安全。这种方法很简单，但它在 Kubernetes 集群上提供的隔离不足，因为 Kubernetes 命名空间是为集群的逻辑分区而设计的，在这种情况下，Staging 环境将与 Production 环境共享集群资源。

![](img/2ab98a5ff1ec073700d902ba95287fd6_82_0.png)

一种策略是使用命名空间为不同的环境服务。虽然这简化了流水线将服务部署到不同环境所需的配置，但命名空间没有强大的隔离保证。

Figure 3.X One Environment per Kubernetes Namespace

一种替代方法可以是为每个环境使用一个全新的集群。主要区别是隔离和访问控制。通过为每个环境拥有一个集群，你可以更严格地定义谁以及哪些组件可以在这些环境中部署和升级事物，并为每个集群配置不同的硬件，例如多区域设置和其他

可扩展性问题，这些问题可能不适合在你的 Staging 和 Testing 环境中拥有。通过使用不同的集群，你还可以实现多云设置，其中不同的环境可以在不同的云提供商中托管。

Figure 3.x shows how you can use the namespace approach for Development Environments which will be created by different teams and then having separated clusters for Staging and Production. The idea here is to have the Staging and Production cluster configured as similarly as possible, so applications deployed behave in a similar way.

![](img/2ab98a5ff1ec073700d902ba95287fd6_83_0.png)

一种更现实的方法可以是使用同一个集群供不同团队日常工作，并为 Staging 和 Production 环境使用单独的集群。可以通过简单地复制另一个团队的仓库并创建一个新命名空间来添加新团队。对于服务要提升到 Staging 或 Production 环境，可以向特定的环境 git 仓库发送 Pull Requests/Merge Requests。

Figure 3.X One Environment per Kubernetes Cluster

## 3.2.3 Service Pipelines + Environment Pipelines

最后，让我们看看 Service Pipelines 和 Environment Pipeline 如何连接。这两个流水线之间的连接通过对仓库的 Pull Request 实现，因为当更改提交并合并时，流水线将被触发：

![](img/2ab98a5ff1ec073700d902ba95287fd6_84_0.png)

Figure 3.X A Service Pipeline can trigger an Environment Pipeline via a Pull Request

Service Pipeline 在所有工件发布并发布后，可以向需要更新或部署服务的环境仓库发送自动 Pull Request，从而触发 Environment Pipeline。这种简单但有效的机制允许在每次想要将某些内容升级或部署到我们的环境中时发送自动或手动 Pull Request。对于某些环境，例如 staging 或 development 环境，你可以自动化包含新服务版本的 Pull Request 的合并，从而使服务仓库中的更改能够自动传播到这些低风险环境。

在下一节中，我们将研究使用 Cloud-Native 方法实现这些流水线。

## 3.3 Implementing Cloud-Native Pipelines

正如我们之前讨论的，如果你想实现这些（Service 和 Environment）或其他流水线，你将需要一个 Pipeline Engine 来处理自动化所有这些任务，这样你实际上不需要实现任何代码，例如从版本控制系统克隆源代码或将工件发布到中央仓库。到目前为止，我们一直在概念层面上讨论流水线，但现在是时候看看实际实现。

过去已经构建了几个 Pipeline Engine，例如 Hudson/Jenkins，但在本书中，我们正在研究 Cloud-Native 解决方案，这些解决方案是专门为利用 Kubernetes 而构建的。如果我们具体谈论 Kubernetes，我们将寻找具有以下特征的解决方案：

- 流水线引擎可以根据工作量进行扩展，因此如果我们有更多流水线需要执行，流水线引擎应该能够仅请求更多硬件并动态扩展
- 流水线引擎应使用 Kubernetes 机制来运行流水线实例
- 流水线引擎应与 Kubernetes 生态系统集成

与 Service 和 Environment 流水线相关的任务大多很常见，并且已经被世界各地的团队作为开源代码实现并共享。如果你有与私有系统集成的特定需求，你可能需要实现一些集成，对于流水线中的大多数通用步骤，大多数技术栈都有现成解决方案。

由于本书使用 Kubernetes 作为目标平台，下一个部分是对一个流行的 Cloud-Native Pipeline Engine Tekton 的概述。

### 3.3.1 Tekton Cloud-Native Pipelines

Tekton 最初作为 Knative 项目的一部分创建（http://knative.dev），由 Google 创建，最初称为 Knative Build，后来从 Knative 中分离出来成为一个独立项目。你可以访问项目网站：http://tekton.dev。Tekton 的主要特点是它是一个为 Kubernetes 设计的 Cloud-Native Pipeline Engine。在本节中，我们将研究如何使用 Tekton 来定义 Service 和 Environment Pipelines。

### Tekton in Action

在 Tekton 中你有两个主要概念：Tasks 和 Pipelines。Tekton，Pipeline Engine 由一组组件组成，这些组件将理解如何执行我们定义的 Tasks 和 Pipelines。Tekton，与本书中涵盖的大多数 Kubernetes 项目一样，可以通过运行 `kubectl` 或使用 Helm Charts 安装到你的 Kubernetes 集群中。

当你安装 Tekton 时，你正在安装一组 Custom Resource Definitions，它们是 Kubernetes API 的扩展，在 Tekton 的情况下定义了 Tasks 和 Pipelines 是什么。Tekton 还安装了 Pipeline Engine 本身，它知道如何在我们创建它们时处理 Tasks 和 Pipelines 资源。

你可以使用 Helm 安装 Tekton，这要归功于与 Continuous Delivery foundation 的合作：https://github.com/cdfoundation/tekton-helm-chart

与每个 Helm Chart 一样，你首先需要添加 Helm Chart 仓库，然后安装 chart：

