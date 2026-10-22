

底层 Kubernetes 提供程序。例如，我们在第 2 章中看到的许多容器存储接口（CSI）实现都使用 DaemonSets 在每个工作节点上运行存储驱动程序。另一个常见用途是在工作节点上运行执行监控任务的 Pod，例如日志和指标收集器。

## 摘要

在本章中，我们通过实践示例学习了如何在 Kubernetes 上部署单节点和多节点分布式数据库。在此过程中，你熟悉了 Kubernetes 资源，例如 Deployments、ReplicaSets、StatefulSets 和 DaemonSets，并了解了每种资源的最佳用例：

- 使用 Deployments/ReplicaSets 管理无状态工作负载或简单的有状态工作负载，如可依赖临时存储的单节点数据库或缓存
- 使用 StatefulSets 管理涉及多个节点并需要与特定存储位置关联的有状态工作负载
- 使用 DaemonSets 管理利用特定工作节点功能的工作负载

你还了解了每种资源所能提供的局限性。现在你已经在 Kubernetes 上部署有状态工作负载方面积累了经验，下一步是学习如何自动化所说的“第 2 天”操作，以保持此数据基础设施的运行。

## 第 4 章. 使用 Helm 在 Kubernetes 上自动化数据库部署

### 早期发布读者的说明

使用 Early Release 电子书，你可以在官方发布这些标题之前很久就利用这些技术——作者在写作时提供的原始未编辑内容。

这将是最终书籍的第 4 章。请注意，本书的代码示例可在 https://github.com/data-on-k8s-book 获取。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系编辑 jleonard@oreilly.com。

在上一章中，你学习了如何通过手工方式在 Kubernetes 上部署单节点和多节点数据库，一次创建一个元素。我们故意选择“困难的方式”，以帮助最大化你对如何利用 Kubernetes 原语来设置数据库所需的计算、网络和存储资源的理解。当然，这并不代表在 Kubernetes 上运行数据库的生产体验，原因有几个。

首先，团队通常不会手工部署数据库，一次一个 yaml 文件。那会相当乏味。即使将配置合并到单个文件中，对于更复杂的部署也可能变得相当复杂。考虑第 3 章中 Cassandra 作为多节点数据库所需的配置量增加，与单节点 MySQL 部署相比。本章中，我们将介绍有助于标准化数据库和其他应用程序部署的工具，减少你需要编写的配置代码量。

其次，虽然部署数据库很棒，但如何让它随时间保持运行呢？你需要我们的数据基础设施长期保持可靠和高效，而数据基础设施以需要大量维护而闻名。换句话说，运行系统的任务通常分为“第 1 天”——将应用程序部署到生产环境的欢乐日子，以及“第 2 天”——代表第一天之后每一天，你需要在保持高可用性的同时操作和演进你的应用程序。我们将在接下来的两章中开始解决数据基础设施操作，并在本书的其余部分贯穿这一主题。

## 使用 Helm charts 部署应用程序

首先，让我们看一个帮助你管理配置复杂性的工具：Helm。Helm 是 Kubernetes 的包管理器，是开源的并且是 CNCF 成熟项目。包管理器的概念在多种编程语言中很常见，例如 Python 的 pip、JavaScript 的 Node Package Manager（NPM）以及 Ruby 的 Gems 功能。还有针对特定操作系统的包管理器，例如 Linux 的 Apt 或 MacOS 的 Homebrew。如图 4-1 所示，包管理系统的基本要素是包、存储包的注册表，以及帮助用户在其本地系统上定位、安装和更新包的包管理器应用程序（或“客户端”）。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_116_0.png)

图 4-1. Helm，Kubernetes 的包管理器

Helm 将包管理概念扩展到 Kubernetes，并有一些有趣的差异。如果你使用过上面列出的包管理器之一，你会熟悉包由二进制文件（可执行代码）以及描述二进制文件的元数据组成，例如其功能、API 和安装说明。在 Helm 中，包称为 charts。然而，与直接包含二进制文件不同，charts 提供了如何使用前几章中介绍的 Kubernetes 计算、网络和存储资源，一步一步构建 Kubernetes 应用程序的描述。二进制文件是容器镜像，被引用在 charts 内的计算工作负载定义中，这些镜像位于公共或私有容器注册表中。

Helm 允许 charts 引用其他 charts 作为依赖项，这提供了一种通过创建 charts 组合来组合应用程序的绝佳方式。例如，你可以通过定义一个引用你希望重用的 MySQL 部署的 chart 的 Wordpress 部署 chart，来定义一个应用程序，例如上一章中的 Wordpress / MySQL 示例。或者，你甚至可以找到一个 Helm chart 定义了包含数据库的整个 Wordpress 应用程序。

## KUBERNETES 环境先决条件

本章中的示例假设你有权访问具有以下几个特征的 Kubernetes 集群：

- 集群应至少有 3 个工作节点，以便演示 Kubernetes 提供的机制，让你请求 Pod 在集群中分布。你可以在桌面上使用名为 Kind 的开源发行版创建一个简单集群。参见 Kind 快速入门指南，了解安装 Kind 并创建多节点集群的说明。此示例的代码还包含一个配置文件，你可能觉得有用，可以创建一个简单的三节点 Kind 集群（源代码）。
- 你还需要一个支持动态配置的 StorageClass。你可能希望按照 StorageClasses 类部分中的说明安装一个简单的 StorageClass 和配置程序，以公开本地存储（源代码）。

## 使用 Helm 部署 MySQL

为了让事情更具体一些，让我们使用 Helm 部署你在第 3 章中使用的数据库。首先，你需要使用 Helm 网站上的文档在你的系统上安装 Helm。接下来，添加 Bitnami Helm 仓库：

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Bitnami Helm 仓库包含各种 Helm charts，可帮助你部署基础设施，例如数据库、分析引擎和日志管理系统，以及包括电子商务、客户关系管理（CRM）和你猜对了：Wordpress 在内的应用程序。你可以在 GitHub 上的 Bitnami Charts 仓库中找到 charts 的源代码。此仓库的 README 提供了在各种 Kubernetes 发行版中使用 charts 的有帮助说明。

现在，让我们使用 bitnami 仓库提供的 Helm chart 部署 MySQL。在 Helm 的术语中，每个部署都称为 release。使用此 chart 可以创建的最简单的 release 看起来像这样：

```
# don’t execute me yet!
helm install mysql bitnami/mysql
```

如果你执行此命令，它将使用默认设置，使用 Bitnami MySQL Helm chart 创建一个名为 `mysql` 的 release。结果你将拥有一个单节点 MySQL。由于你已在第 3 章中手动部署了单节点 MySQL，让我们这次做点更有趣的事情并创建一个 MySQL 集群。为此，你将创建一个 `values.yaml` 文件，其内容如下（或重用源代码中提供的 [sample]( )）：

```
architecture: replication
secondary:
  replicaCount: 2
```

此 `values.yaml` 文件中的设置让 Helm 知道你希望使用 Bitnami MySQL Helm chart 中的选项，以复制架构部署 MySQL，其中有一个主节点和两个从节点。

