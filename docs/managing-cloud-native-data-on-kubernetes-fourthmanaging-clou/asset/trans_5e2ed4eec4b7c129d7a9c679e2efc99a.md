

![](img/cb55ccac39ceccbc2806f8db5c8fad69_213_0.png)

图 6-7. Kube-Prometheus Stack 的组件

通过几个快速步骤即可将 K8ssandra 与 Prometheus 连接起来。K8ssandra 文档中的说明会引导你使用 kube-prometheus 安装 Prometheus Operator，如果你尚未安装的话。由于 kube-prometheus 会在自己的命名空间中安装 Prometheus Operator，因此你需要确保该 Operator 拥有管理其他命名空间中资源所需的权限。

要将 K8ssandra 与 Prometheus 集成，只需在 K8ssandraCluster 资源上设置属性，以在 Cassandra 和 Stargate 节点上启用监控。例如，你可以执行以下操作来为集群中所有数据中心的节点启用监控：

```
apiVersion: k8ssandra.io/v1alpha1
kind: K8ssandraCluster
metadata:
  name: demo
spec:
  cassandra:
    datacenters:
      ...
    telemetry:
      prometheus:
        enabled: true
  stargate:
    telemetry:
      prometheus:
        enabled: true
```

也可以根据[文档](documentation)所述，选择性地为单个数据中心启用监控。

让我们看看集成是如何工作的。首先，让我们考虑 Cassandra 节点如何暴露指标。如上文 Cass Operator 部分所述，Cassandra 通过 JMX 暴露管理功能，其中包括指标报告。[Apache Cassandra 的指标收集器](Metrics Collector for Apache Cassandra)（MCAC）是一个开源项目，它通过 HTTP 暴露指标，以便 Prometheus 或其他使用 Prometheus 协议的后端可以访问。K8ssandra 和 Cass Operator 使用的 Cassandra Docker 镜像中包含了 MCAC 以及 Management API 作为在 Cassandra 容器中运行的额外进程。此配置显示在图 6-8 的左侧。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_215_0.png)

图 6-8. 使用 Kube-Prometheus Stack 监控 Cassandra

图 6-8 的右侧显示了 Prometheus 和 Grafana 如何配置为消费并暴露 Cassandra 指标。K8ssandra Operator 会为启用了监控的每个 CassandraDatacenter 创建 ServiceMonitor 资源。ServiceMonitor 是由 Prometheus Operator 定义的 CRD，其中包含有关如何从一组 Pod 收集指标的配置详细信息，包括以下内容：

- 引用用于标识 Pod 的标签名称的 `selector`
- 连接信息，例如用于从每个 Pod 收集指标的 `scheme`（协议）、`port` 和 `path`
- 拉取指标的 `interval`
- 可选的 `metricRelabelings`，即指示任何期望的指标重命名，或甚至指示应丢弃且不由 Prometheus 摄取的指标的指令。

K8ssandra 为 Cassandra 和 Stargate 节点创建单独的 ServiceMonitor 实例，因为暴露的指标略有不同。要观察部署在集群中的 ServiceMonitor，可以执行类似 `kubectl get servicemonitors -n monitoring` 的命令。

Prometheus 通过作为 Kubernetes 服务公开的 PromQL 端点向 Grafana 和其他工具提供对指标的访问。kube-prometheus 安装会配置一个 Grafana 实例，使用 Grafana DataSource CRD 的实例连接到 Prometheus。Grafana 接受使用 yaml 文件定义的仪表板，你可以将其作为 ConfigMap 提供。有关加载显示 Cassandra 和 Stargate 指标的仪表板定义的指导，请参阅 K8ssandra [文档](documentation)。你可能还希望创建仪表板，将应用程序指标与 K8ssandra 提供的<|message|> 数据层指标一起显示，以便获得应用程序性能的集成视图。

正如你所见，kube-prometheus 为 Kubernetes 集群提供了一个全面且可扩展的监控栈，就像 K8ssandra 为数据管理提供栈一样。K8ssandra 与 kube-prometheus 的集成是你可以组装 Kubernetes 资源集成栈以形成更强大的应用程序的绝佳示例。

## 使用 Cassandra-Reaper 执行修复

作为 NoSQL 数据库，Cassandra 默认强调高性能（尤其是写入）和高可用性。如果你熟悉 CAP 定理，你会理解这意味着有时 Cassandra 会暂时牺牲节点间数据的 consistency 以提供这种大规模的高性能和高可用性，这种方法称为 *最终一致性*。Cassandra 确实提供了通过指定复制策略和每个查询所需的一致性级别的选项来根据需要调整一致性程度的能力。用户和管理员应了解这些选项及其行为，以便有效地使用 Cassandra。

Cassandra 具有多种内置的“反熵”机制，例如 hinted handoff 和 repair，有助于随着时间的推移在节点之间保持数据一致性。Repair 是一种后台进程，节点通过它将自己拥有的部分数据与也负责该数据的其他节点的最新内容进行比较。虽然这些检查可以通过使用校验和来 somewhat 优化，但 repair 仍然是一个性能密集型过程，最好在集群负载较低或非高峰时段执行。结合可用的多种选项，包括完整和增量修复，执行修复传统上需要为每个集群进行一些定制。它也一直是一个手动过程，遗憾的是经常被一些 Cassandra 集群管理员忽视。

### 关于 CASSANDRA 中修复的更多细节

有关修复的更深入介绍，请参阅 Cassandra: The Definitive Guide，其中在第 6 章和第 12 章分别描述了修复概念和可用选项。

Cassandra Reaper 的创建是为了消除在 Cassandra 集群上执行修复的难度，并优化修复性能，以最大限度地减少在繁忙集群上运行修复的影响。Reaper 由 Spotify 创建，并由 The Last Pickle 增强，后者目前在 GitHub 上管理该项目。Reaper 暴露了一个 RESTful API，用于为一个或多个 Cassandra 集群配置修复计划，并提供命令行工具和 Web 界面，指导管理员创建计划。

K8ssandra 提供了将 Cassandra Reaper 实例作为 K8ssandraCluster 的一部分合并的选项。K8ssandra Operator 包含一个 Reaper Controller，负责通过其关联的 Reaper CRD 管理本地 Cassandra

Reaper 管理器进程。默认情况下，在 K8ssandraCluster 中启用 Reaper 会导致在安装中表示的每个 Kubernetes 集群中安装一个 Reaper 实例，但你也可以使用单个 Reaper 实例在多个数据中心甚至多个 Cassandra 集群之间管理修复，前提是它们可以通过网络访问。

### 成为 Kubernetes 原生的重要性如何？

K8ssandra 对 Reaper 的使用是构建更复杂的数据基础设施栈时涉及的权衡的一个例子。例如，Reaper Manager 的更 Kubernetes 原生设计可能涉及将每个修复任务分解为 Kubernetes CronJob，可以与关联的 CassandraDatacenter 一起调度，从而更多地利用 Kubernetes 内置资源。虽然这可能是 Reaper 未来发展的有效方向，但 K8ssandra 项目目前选择将 Reaper 集成作为一个能很好完成任务的工作基础设施，同时为未来更 Kubernetes 原生的方法留下空间。

我们在第 5 章中看到了这种“封装 vs. 重写”类型决策的另一个例子，其中 Vitess Operator 按原样重用 Vitess 控制守护进程 vctld 及其 vctlclient。在这两个示例中，项目开发人员都做出了务实的选择，进行初始部署以“足够”地将现有基础设施移植到 Kubernetes 运行，同时为未来更 Kubernetes 原生的方法留下空间。在第 7 章中，我们将研究从头开始在新基础设施项目上采用 Kubernetes 原生方法的情况。

## 使用 Cassandra Medusa 备份和恢复数据

管理备份是维护任何存储数据的系统的<|message|> 高可用性和灾难恢复计划的重要组成部分。Cassandra

