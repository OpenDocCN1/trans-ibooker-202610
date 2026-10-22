

另一个关注的重点是加快启动和关闭速度。网络限制，例如 TCP/IP 超时，限制了检测故障的速度，但 MySQL 的启动和关闭尚未达到该下限。我们在 PlanetScale 构建的第一个 operator 启动一个集群需要 10 到 20 分钟。这部分是由于 Operator SDK 的效率低下，部分原因是我们编写了一个带有巨大 reconcile 循环的单一控制器。我们重写了 operator，使其使用更新版本的 Operator SDK，并为每个资源提供单独的控制器。这使我们的启动和关闭时间快了 20 倍，这对于提供云服务来说是一个硬性要求。客户端期望这些操作花费 10 或 15 秒，而不是两三分钟。我们必须更聪明地实现这一点。

我们还需要 Kubernetes 提供更多原语，以便继续成熟数据库 operator。虽然 Kubernetes 提供了 Deployments、ReplicaSets 和 StatefulSets，但它尚未支持 MySQL 所需的主从概念。想象一下，如果你可以配置 Kubernetes 来指定一个主节点，并指定在主节点重启时执行的操作。Vitess 中包含的大量错误处理代码实际上就不需要了。虽然 Kubernetes 有一个 leader election 模块，但对于已经具有主从概念的 operator 来说，没有明确的方法来利用它。这导致了更多重复代码。

应用程序开发者希望对数据的存储位置有更多控制，并轻松地摄取或加载数据。每个在 Kubernetes 上提供数据库解决方案的组织都应考虑将其作为服务提供，以便开发者更容易使用。开发者不应该担心数据本地性。今天，如果开发者在 AWS 上运行应用程序，而某个特定数据服务在那里不可用，他们必须考虑使用另一个云或自行构建该功能。不管你在哪里运行它，都应该很容易为应用程序创建和填充数据源。

基础设施配置变得越来越容易，并且这一趋势持续下去就太好了。尽管如此，还有很多工作要做。我们这些靠为开源工作而获得报酬的人非常幸运，因为有很多开发者没有为他们的开源贡献获得报酬。希望雇主会继续看到在开源软件上工作的好处，我们可以继续作为一个社区成长。

## 一个不断增长的 Operator 生态系统

operator 模式在 Kubernetes 社区中变得非常流行，导致了 Operator Framework 的开发，这是一个用于创建和分发 operator 的生态系统。在本节中，我们将研究 Operator Framework 和相关的开源项目。

## 选择 Operators

虽然我们在本章中以 Vitess 作为示例数据库 operator 为重点，但 operator 显然与你数据栈的各个元素都相关。在云原生数据的各个方面，我们看到越来越多的成熟的开源 operator 可用于你的部署，我们将在后续章节中研究如何在 Kubernetes 上运行不同类型的数据基础设施时查看更多 operator。

在选择 operator 时，你应该考虑多个方面——它的功能是什么？它自动化了多少？它的支持程度如何？它是专有的还是开源的？Operator Framework 提供了一个很好的资源，你在寻找 operator 时应将其作为首选：Operator Hub。Operator Hub 是一个组织良好的各种 operator 列表，涵盖了云原生软件的各个方面。它依赖维护者提交他们的 operator 以供列出，这意味着许多现有 operator 可能未被列出。

Operator Framework 还包含 Operator Lifecycle Manager，这是一个用于在你的集群中安装和管理其他 operator 的 operator。你可以策划自己的 operator 自定义目录以允许在你的环境中使用，或使用他人提供的目录。例如，Operator Hub 本身可以被视为一个目录。

Operator Hub 提供的策划的一部分是根据 Operator Capability Model 对每个 operator 的能力进行评级。该能力模型中的级别在表 5-1 中进行了总结，使用了 Operator Framework 网站上的文本，并添加了我们自己的评论，以突出数据库 operator 的注意事项。示例并非规定性，但表明每个级别预期的能力类型。

表 5-1. Operator 能力级别

| 能力级别 | 特征 | 数据库 Operator 示例 | 工具 |
|---|---|---|---|
| Level 1 - Basic Install | Kubernetes 和工作负载的安装和配置 | operator 使用自定义资源为数据库集群提供集中的配置点。operator 通过创建 Deployment、ServiceAccounts、RoleBindings、PersistentVolumeClaims 和 Secrets 等资源来部署数据库，并帮助初始化数据库模式。 | Helm, Ansible, Go |
| Level 2 - Seamless Upgrades | 管理工作负载和 operator 的升级 | operator 可以在不丢失数据（或希望，不停机）的情况下将现有数据库更新到更新版本。operator 可以被其更新版本替换。 |  |
| Level 3 - Full Lifecycle | 能够创建备份并从备份恢复，能够故障转移或替换集群应用程序的部分，能够扩展应用程序 | operator 提供了在多个数据节点之间创建一致备份的方法，并能够使用这些备份来恢复或替换失败的数据库节点。operator 可以响应配置更改以添加或删除数据库节点，甚至可能是数据中心。 | Ansible, Go |
| Level 4 - Deep Insights | 提供包括警报、监控、事件或计量的功能 | operator 监控数据库软件的指标和日志输出，并使用这些信息来实现健康和就绪检查。operator 将指标和警报推送到其他基础设施。 |  |
| Level 5 - Auto-pilot | 提供包括自动扩展、自动修复、自动调优的功能 | operator 根据性能要求自动上下扩展集群中的数据库节点数量。operator 还可能动态调整 PV 大小或更改用于各种数据库节点的存储类。operator 自动执行数据库维护，例如重建索引以改善缓慢的响应时间。operator 检测异常工作负载模式并采取行动，例如重新分片以平衡工作负载。 |  |

这些级别对于评估数据库和为 operator 开发者提供目标都很有用。它们还对 Helm-based operator 能完成什么提供了有见地的看法，将其限制在 Level 2。对于完整的生命周期管理和自动化，需要更直接地参与 Kubernetes 控制平面。对于 Level 5 operator，目标是完全免维护的部署。

让我们快速看看一些流行的开源数据库的可用 operator：

## Cass-operator for Apache Cassandra

在 2021 年，Cassandra 社区中开发了自己的 operator 的几家公司聚集在一起支持 cass-operator。Cass-operator 受到了社区 operator 的最佳功能和 DataStax 运行 Astra 的经验的启发，Astra 是一个基于 Cassandra 的 DBaaS。该 operator 已捐赠给 K8ssandra 项目，在那里它是部署 Cassandra 到 Kubernetes 的更大生态系统的一部分。我们将在第 7 章更深入地研究 K8ssandra 和 cass-operator。

## PostgreSQL Operators

鉴于 PostgreSQL 是 MySQL 之后第二受欢迎的开源数据库，可用的 PostgreSQL operator 有几个。其中两个最受欢迎的 operator 是 Zalando Postgres Operator，以及来自 CrunchyData 的 PGO（也代表 Postgres Operator）。阅读博客 Comparing Kubernetes operators for PostgreSQL 以便对这些和其他 operator 进行有帮助的比较。

