
contact with nodes that don’t match its cluster name, it will ignore them.

- CASSANDRA_DC 和 CASSANDRA_RACK 标识每个节点所属的数据中心和机架。这凸显了 StatefulSets 暴露 Pod 规范方式中的一个有趣细节。由于模板应用于每个 Pod 和容器，因此无法在不同的 Cassandra Pod 之间改变配置的数据中心和机架名称。因此，通常使用每个机架一个 StatefulSet 的方式在 Kubernetes 中部署 Cassandra。
- CASSANDRA_SEEDS 定义了 Cassandra 集群中节点众所周知的<|message|>位置，新节点可用于将其自身引导到集群中。最佳实践是指定多个种子，以防其中一个在新节点加入时恰好宕机或离线。然而，对于此初始示例，指定初始 Cassandra 副本作为种子就足够了，其 DNS 名称为 cassandra-0.cassandra.default.svc.cluster.local。我们将在第 4 章中使用服务来查看更稳健的指定种子方法，如图 3-5 所示的“Seed Service”所暗示的那样。

容器规范中的最后一项是 volumeMount，它请求将 PersistentVolume 挂载到 /var/lib/cassandra 目录，该目录是 Cassandra 镜像配置为存储其数据文件的位置。由于每个 Pod 都需要自己的 PersistentVolumeClaim，因此名称 cassandra-data 是对下面定义的 PersistentVolumeClaim 模板的引用。

## Volume claim templates

StatefulSet 规范的最后一部分是 volumeClaimTemplates。规范必须为上面容器规范中引用的每个名称包含一个模板定义。在本例中，cassandra-data 模板引用了我们在这些示例中一直使用的标准存储类。Kubernetes 将使用此模板在该 StatefulSet 中启动新 Pod 时创建所请求大小为 1GB 的 PersistentVolumeClaim。

## StatefulSet lifecycle management

现在我们已经讨论了 StatefulSet 规范的组件，你可以继续应用源文件：

```
kubectl apply -f cassandra-statefulset.yaml
```

在应用过程中，你可以执行以下命令来观察 StatefulSet 启动 Cassandra Pod 的情况：

```
kubectl get pods -w
```

让我们描述一下可以从此命令的输出中观察到的一些行为。首先，你会看到单个 Pod cassandra-0。一旦该 Pod 进入 Ready 状态，然后你会看到 cassandra-1 Pod，cassandra-1 准备就绪后会跟随 cassandra-2。这种行为由 StatefulSet 的 podManagementPolicy 选择指定。让我们探索可用的选项以及帮助定义 StatefulSet 中 Pod 管理方式的其他设置。

podManagementPolicy 决定 StatefulSet 中 Pod 的添加或删除时机。我们在 Cassandra 示例中应用的 OrderedReady 策略是默认值。当此策略生效且 Pod 被添加时，无论是在初始创建还是扩容时，Kubernetes 都会一次扩展一个 Pod。随着每个 Pod 的添加，Kubernetes 会等待 Pod 报告 Ready 状态后才添加后续 Pod。如果 Pod 规范包含 readinessProbe，如本示例中所做的那样，Kubernetes 会迭代执行提供的命令以确定 Pod 是否准备好接收流量。当探针成功完成时，即返回代码为零时，它会继续创建下一个 Pod。对于 Cassandra，准备就绪通常通过 CQL 端口 9042 的可用性来衡量，这意味着节点能够响应 CQL 查询。

同样，当 StatefulSet 被删除或缩容时，Pod 会一次删除一个。随着 Pod 被删除，其容器的任何提供的 `preStop` 命令都会被执行，以便它们有机会优雅地关闭。在我们当前的示例中，`nodetool drain` 命令被执行以帮助 Cassandra 节点干净地退出集群，将其 token 范围的职责分配给其他节点。当 Kubernetes 等待一个 Pod 完全终止后，才会删除下一个 Pod。`livenessProbe` 中指定的命令用于确定 Pod 是否存活，当它不再无错误完成时，Kubernetes 就可以继续删除下一个 Pod。有关配置就绪探针和存活探针的更多信息，请参阅 [Kubernetes 文档](https://kubernetes.io/docs)。

另一个 Pod 管理策略是 `Parallel`。当此策略生效时，Kubernetes 会同时启动或终止多个 Pod 以进行扩容或缩容。这会更快地将 StatefulSet 扩展到期望的副本数，但也可能导致某些有状态工作负载更长时间才能稳定。例如，像 Cassandra 这样的数据库在集群大小变化时会将数据在节点之间重新分发以平衡负载，并且在节点一次添加或删除时往往会更快稳定。

无论采用哪种策略，Kubernetes 都会根据序数号管理 Pod，扩容时始终使用下一个未使用的序数号添加 Pod，缩容时删除序数号最高的 Pod。

## Pod Management Policies

## Update Strategies

`updateStrategy` 描述了如果 Pod 模板规范发生变化，例如更改容器镜像，StatefulSet 中的 Pod 将如何更新。默认策略是 `RollingUpdate`，本示例中已选择该策略。另一种选项 `OnDelete` 要求你手动删除 Pod 才能应用新的 Pod 模板。

在滚动更新中，Kubernetes 将删除并重新创建 StatefulSet 中的每个 Pod，从序数号最大的 Pod 开始，逐步向最小的 Pod 进行。Pod 一次更新一个，你可以指定一个称为 partition 的 Pod 数量以执行分阶段发布或金丝雀发布。请注意，如果你在发布过程中发现了错误的 Pod 配置，你需要将 Pod 模板规范更新为已知良好的状态，然后手动删除使用错误规范创建的任何 Pod。由于这些 Pod 永远无法达到 Ready 状态，Kubernetes 不会决定用良好配置替换它们。

请注意，Kubernetes 为 Deployments、ReplicaSets 和 DaemonSets 提供了类似的生命周期管理选项，包括修订历史记录。

## MORE SOPHISTICATED LIFECYCLE MANAGEMENT FOR STATEFULSETS

关于 StatefulSets 的额外生命周期选项的一个有趣观点集合来自 OpenKruise，这是一个 CNCF 沙箱项目，它提供了 Advanced StatefulSet。Advanced StatefulSet 增加了包括以下功能在内的能力：

- 并行更新，最大不可用 Pod 数
- 具有基于提供的优先级策略的替代替换顺序的滚动更新
- 通过根据更新的 Pod 模板规范重新启动其容器来“就地”更新 Pod

此 Kubernetes 资源也命名为 StatefulSet，以方便在对现有配置影响最小的情况下使用。你只需将 apiVersion 从 apps/v1 改为 apps.kruise.io/v1beta1 即可。

我们建议获得更多管理 StatefulSets 的实践经验以巩固你的知识。例如，你可以监控 StatefulSet 扩容时 PersistentVolumeClaims 的创建。另一个可以尝试的操作：删除一个 StatefulSet 并重新创建它，验证新 Pod 是否能从原始 StatefulSet 恢复先前存储的数据。更多想法，你可能会发现以下指导教程有帮助：来自 Kubernetes 文档的 [StatefulSet Basics](StatefulSet Basics)，以及来自 Kubernetes 博客的 [StatefulSet: Run and Scale Stateful Applications Easily in Kubernetes](StatefulSet: Run and Scale Stateful Applications Easily in Kubernetes)。

StatefulSets 对于在 Kubernetes 上管理有状态工作负载非常有用，而且这甚至不包括我们未涉及的一些功能，例如 Pod 亲和性、反节点亲和性、管理内存和 CPU 的资源请求，以及 PodDisruptionBudgets 等可用性约束。另一方面，你可能希望拥有的一些功能 StatefulSets 并未提供，例如持久卷的备份/恢复，或访问凭据的安全配置。我们将在第 4 章及以后讨论如何在 Kubernetes 之上利用或构建这些功能。
