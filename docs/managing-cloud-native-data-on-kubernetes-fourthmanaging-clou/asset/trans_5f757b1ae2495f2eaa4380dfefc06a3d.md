

## STATEFULSETS：过去、现在与未来

与 RedHat 工程师兼 Kubernetes SIG Apps 成员 Maciej Szulik 共同撰写

Kubernetes 应用特别兴趣小组（SIG Apps）负责开发用于管理 Kubernetes 上应用程序工作负载的控制器。这包括诸如 Jobs 和 CronJobs 之类的批处理工作负载、诸如 Deployments 和 Replica Sets 之类的其他无状态工作负载，当然还有用于有状态工作负载的 StatefulSets。

StatefulSet 控制器的工作方式与这些其他控制器略有不同。当你考虑 Deployments 或 Jobs 时，控制器只需要管理 Pods。你不必担心底层数据，因为这些数据要么由持久卷处理，要么在销毁并重新创建 pod 时丢弃每个 pod 的数据是可以接受的。然而，当你尝试运行数据库或任何需要状态在运行之间得以保留的工作负载时，这种行为是不可接受的。这导致 StatefulSet 控制器中存在显著的额外复杂性。编写和成熟 Kubernetes 控制器的主要挑战一直是处理边缘情况。StatefulSets 在这方面类似，但对于 StatefulSets 来说，正确处理故障情况更为紧迫，这样我们就不会丢失数据。

我们遇到了一些有趣的 StatefulSet 用例，以及用户希望更改核心实现中设定的边界的情况。例如，我们收到了更改 StatefulSet 在更新期间处理 pod 的方式的拉取请求。在原始实现中，StatefulSet 控制器一次只更新一个 pod，如果在滚动更新期间出现问题，整个滚动更新将被暂停，并且 StatefulSet 需要人工干预以确保数据不会损坏或丢失。一些用户希望 StatefulSet 控制器忽略 pod 卡在挂起状态或无法运行的问题，并 simply 重启这些 pod。然而，需要记住的是，对于 StatefulSets 来说，保护底层数据是最重要的优先事项。我们可能会做出建议的更改，以便在数据保护不太重要的开发环境中允许更快地并行更新，但需要通过功能标志选择加入。

另一个经常被请求的功能是能够在删除 StatefulSet 时自动删除 StatefulSet 的 PersistentVolumeClaims。原始行为是保留 PVC，再次作为数据保护机制，但有一个 Kubernetes 增强提案（KEP）用于自动删除，目前正在考虑用于 Kubernetes 1.23 版本。

尽管 StatefulSets 管理 pod 的方式与其他控制器存在一些显著差异，但我们正在努力尽可能使不同控制器之间的行为更加相似。一个例子是 pod 模板中添加了 minReadySeconds 设置，这允许你说，我希望此应用程序在向其发送流量之前再不可用一段时间。这对于需要更多时间初始化自己的某些有状态工作负载很有帮助，例如预热缓存，并使 StatefulSets 与其他控制器保持一致。

另一个例子是正在进行的工作，以统一所有应用程序控制器中的状态报告。目前，如果你正在构建任何更高级别的编排或管理工具，你需要有不同的行为来处理 StatefulSets、Deployments、DaemonSets 等的状态，因为它们各自由不同的作者编写。每位作者对状态中应包含的内容、资源应如何表达其是否可用、是否正在进行滚动更新、是否不可用或正在发生什么等信息都有不同的要求。DaemonSets 在报告状态方面的差异尤其明显。

还有一项正在进行的功能，允许你为 StatefulSet 设置 maxUnavailable pod 数量。此数字将在 StatefulSet 的初始滚动更新期间应用，并允许更快地扩展副本数。这是另一项使 StatefulSets 与其他控制器的工作方式更加一致的功能。如果你想了解 SIG Apps 团队正在进行的工作，最好的方法是查看标记为 `sig/apps` 的 [Kubernetes 开放问题](Kubernetes open issues)。

构建能够满足所有有状态工作负载需求的 StatefulSets 是一项困难的任务；我们尝试以一种能够以一致的方式处理最常见需求的方式构建它们。我们显然可以为更多边缘情况添加支持，但这往往会使用户难以掌握功能变得显著复杂。总会有用户不满意，因为他们的用例未被覆盖，并且在不影响功能和性能的情况下我们能放入多少内容之间始终存在平衡。

在大多数用户需要更具体行为的情况下，例如处理边缘情况，通常是因为他们试图管理像 Postgres 或 Cassandra 这样复杂的应用程序。这就是创建自己的控制器甚至 operator 来处理这些特定情况的很好理由。即使听起来非常可怕，编写自己的控制器其实并不困难。你可以使用一些简单示例，包括 Kubernetes 代码库的一部分并由项目维护的 [示例控制器](sample controller)，在合理的时间内快速开始并在几天内运行一个基本控制器。O’Reilly 书籍 [Programming Kubernetes](Programming Kubernetes) 也有一章关于编写控制器的内容。不要仅仅假设你只能使用开箱即用的行为。Kubernetes 旨在开放且可扩展，无论是网络、控制器、CSI、插件等。如果你需要自定义 Kubernetes，就去做吧！

## 访问 Cassandra

一旦你应用了上面列出的配置，你就可以使用 Cassandra 的 CQL shell cqlsh 来执行 CQL 命令。如果你碰巧是 Cassandra 用户并且在本地机器上安装了 cqlsh 的副本，你就可以像客户端应用程序一样访问 Cassandra，使用与 StatefulSet 关联的 CQL 服务。然而，由于每个 Cassandra 节点都包含 cqlsh，这给了我们一个机会来演示在 Kubernetes 中以不同方式与基础设施交互的方法，通过直接连接到 StatefulSet 中的单个 pod：

```
kubectl exec -it cassandra-0 -- cqlsh
```

这应该会启动 cqlsh 提示符，然后你就可以使用 DESCRIBE KEYSPACES 探索 Cassandra 内置表的内容，然后使用 USE 选择特定的 keyspace 并运行 DESCRIBE TABLES。网上有许多 Cassandra 教程可以指导你完成更多创建自己的表、插入和查询数据的示例等。当你完成对 cqlsh 的实验后，可以输入 exit 来退出 shell。

删除 StatefulSet 与删除任何其他 Kubernetes 资源相同 - 你可以通过名称删除它，例如：

```
kubectl delete sts cassandra
```

你也可以通过用于创建它的文件引用删除 StatefulSet：

```
kubectl delete sts cassandra
```

当你删除一个策略为 Retain 的 StatefulSet 时，如本示例中所示，它创建的 PersistentVolumeClaims 不会被删除。如果你重新创建 StatefulSet，它将绑定到相同的 PVC 并重用现有数据。当你不再需要这些声明时，你需要手动删除它们。从本次练习中你想要执行的最后清理是删除 CQL 服务：kubectl delete service cassandra。

## 那 DAEMONSETS 呢？

如果你熟悉 Kubernetes 用于管理工作负载的资源，你可能已经注意到我们尚未提到 [DaemonSets](DaemonSets)。DaemonSets 允许你请求在 Kubernetes 集群中的每个 worker 节点上运行一个 pod，如图 3-6 所示。与指定副本数量不同，DaemonSet 会随着 worker 节点被添加到集群或从集群中移除而扩展或缩小。默认情况下，DaemonSet 将在每个 worker 节点上运行你的 pod，但你可以使用 [污点和容忍](taints and tolerations) 来覆盖此行为，例如限制某些 worker 节点，或选择也在 Kubernetes 主节点上运行 pod。DaemonSets 以与 StatefulSets 类似的方式支持滚动更新。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_112_0.png)

图 3-6. Daemon Sets 在选定的 worker 节点上运行单个 pod

表面上，DaemonSets 听起来可能对运行数据库或其他数据基础设施很有用，但这似乎并不是一种广泛的做法。相反，DaemonSets 最常用于与 worker 节点及其与

