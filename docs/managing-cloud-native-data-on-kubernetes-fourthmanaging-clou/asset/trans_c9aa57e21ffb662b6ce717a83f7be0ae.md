

```
tier: frontend
type: LoadBalancer
```

请注意该服务的类型为 LoadBalancer，这应该使你能够从本地机器相对轻松地访问它。执行命令 `kubectl get services` 以获取负载均衡器的 IP 地址，然后你就可以通过 URL *http://<ip>* 在浏览器中打开 WordPress 实例。尝试登录并创建一些页面。

## 从 Kubernetes 发行版访问服务

访问服务的具体细节将取决于你使用的 Kubernetes 发行版，以及你是在生产环境中部署应用，还是只是像我们这里这样快速测试一下。如果你使用的是桌面版 Kubernetes 发行版，为了简化操作，你可能希望改用 NodePort 服务而不是 LoadBalancer。你也可以查阅特定说明以了解如何访问服务，例如 Minikube 或 K3d 提供的文档。

当你完成对 WordPress 实例的实验后，可以使用以下命令清理你在本地目录中使用的配置文件中指定的资源，包括存储在 PersistentVolumeClaim 中的数据：

```
kubectl delete -k ./
```

此时，你可能会觉得这相对容易，尽管我们声称是在做“困难的方式”。从某种意义上说，你是对的。到目前为止，我们部署了一个简单的数据库的单个节点，使用了合理的默认值，我们不需要花费太多时间进行配置。创建单个节点当然是可以的，如果你的应用程序只会存储少量数据。那就是在 Kubernetes 上部署数据库的全部内容吗？当然不是！现在我们已经通过这个简单的数据库部署介绍了几个基本的 Kubernetes 资源，是时候稍微提高一下复杂度了。让我们开始吧！

## 在 Kubernetes 上运行 Apache Cassandra

在本节中，我们将研究如何在 Kubernetes 上运行多节点数据库，使用 Apache Cassandra。Cassandra 是一种 NoSQL 数据库，最初由 Facebook 开发，于 2010 年成为 Apache 软件基金会的顶级项目。Cassandra 是一种运营数据库，提供表格数据模型，其 Cassandra 查询语言 CQL 与 SQL 相似。

Cassandra 是一种为云设计的数据库，它通过添加节点实现水平扩展，每个节点都是对等节点。这种去中心化设计已被证明具有接近线性的可扩展性。Cassandra 通过存储数据的多个副本或副本来实现高可用性，包括将这些副本分布在多个数据中心和云区域的逻辑。Cassandra 建立在与 Kubernetes 相似的原则之上，即它旨在检测故障并在系统在后台恢复到预期状态时继续运行。所有这些特性使 Cassandra 非常适合在 Kubernetes 上部署。

为了讨论此部署的工作方式，了解 Cassandra 分布数据的两种不同视角是有帮助的：物理和逻辑。借用《Cassandra：权威指南》中的部分视觉效果，你可以在图 3-4 中看到这些视角。从物理角度来看，Cassandra 节点（不要与 Kubernetes 工作节点混淆）使用称为机架和数据中心的概念进行组织。虽然这些术语暴露了 Cassandra 的起源——在 2000 年代中期，本地数据中心是软件部署的主要方式——但它们可以灵活应用。在云部署中，机架通常代表可用区，而数据中心代表云区域。然而无论如何表示，重要的是它们代表了物理上分离的故障域。Cassandra 利用对该拓扑的认知，确保它在多个物理位置存储副本，以最大化在发生故障时数据的可用性，无论这些故障是单台机器、一组服务器、一个可用区还是整个区域。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_94_0.png)

图 3-4. Cassandra 分布式架构的物理和逻辑视图

逻辑视图帮助我们理解 Cassandra 如何确定将数据放置在每个节点上。Cassandra 中每行数据都由主键标识，主键由一个或多个用于在节点间分配数据的分区键列组成，以及可选的聚簇列，可用于在分区内组织多行数据以实现高效访问。Cassandra 中的每次写入（以及大多数读取）都通过提供分区键值来引用特定分区，Cassandra 将其哈希在一起生成一个称为 token 的值，该值介于 −2^63 和 2^63−1 之间。Cassandra 为其每个节点分配一个或多个 token 范围的责任（图 3-4 中为简化起见显示每个节点一个范围）。在分配 token 范围时，会考虑物理拓扑，以确保你的数据副本分布在机架和数据中心之间。

现在我们准备考虑 Cassandra 如何映射到 Kubernetes。考虑 Cassandra 架构的两个影响很重要：

### 有状态性

每个 Cassandra 节点都有其负责维护的状态。Cassandra 具有通过从其他副本流式传输数据到新节点来替换节点的功能，这意味着使用本地临时存储的配置是可能的，但代价是更长的启动时间。然而，更常见的做法是为每个 Cassandra 节点配置持久存储。在任何一种情况下，每个 Cassandra 节点都需要拥有自己的唯一 PersistentVolumeClaim。

### 标识

尽管在完全对等架构中，每个 Cassandra 节点在代码、配置和功能方面是相同的，但节点在实际角色方面是不同的。每个节点在数据中心和机架的拓扑中以及其分配的 token 范围方面都有其身份。

这些对标识的要求以及与特定 PersistentVolumeClaim 的关联为 Deployment 和 ReplicaSet 带来了挑战，而它们并不是为处理这些情况而设计的。Kubernetes 早期就意识到需要另一种机制来管理像 Cassandra 这样的有状态工作负载。

### StatefulSets

Kubernetes 开始通过 1.3 版本中 PetSets 的 alpha 版本提供管理有状态工作负载的资源。该功能随着时间的推移而成熟，现在被称为 StatefulSets（参见：边栏：你的有状态工作负载是宠物还是牲畜？）。StatefulSet 与 ReplicaSet 有一些相似之处，因为它负责管理一组 Pod 的生命周期，但其管理方式存在显著差异。为了满足 Cassandra 等有状态应用程序的需求，StatefulSets 表现出以下关键属性：

### Pod 的稳定标识

首先，StatefulSets 为 Pod 提供稳定的名称和网络标识。每个 Pod 都根据 StatefulSet 的名称加上序数编号进行命名。例如，一个名为 `cassandra` 的 StatefulSet 将拥有名为 `cassandra-1`、`cassandra-2`、`cassandra-3` 等的 Pod，如图 3-5 所示。这些是稳定的名称，因此如果 Pod 因某种原因丢失并需要替换，替换 Pod 将具有相同的名称，即使它在不同的工作节点上启动。每个 Pod 的名称被设置为其主机名，因此如果你创建了一个无头服务，你实际上可以根据需要寻址单个 Pod，例如：`cassandra-1.cqlservice.default.svc.cluster.local`。我们将在本章后面 *访问 Cassandra* 部分讨论更多关于为 Cassandra 运行 Kubernetes 服务的内容。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_96_0.png)

图 3-5. 使用 StatefulSets 在 Kubernetes 上部署 Cassandra 的示例

### 有序的生命周期管理

StatefulSets 为管理 Pod 的生命周期提供了可预测的行为。在扩展 StatefulSet 中的 Pod 数量时，新 Pod 会根据下一个可用编号添加。例如，扩展图 3-5 中的 StatefulSet 将导致创建诸如 `cassandra-4` 和 `cassandra-5` 的 Pod。缩减规模则具有相反的行为，因为具有最高序数编号的 Pod 会首先被删除。这种可预测性简化了管理，例如在减少集群规模之前，很明显应备份哪些节点。

### 持久磁盘

与创建所有 Pod 共享的单个 PersistentVolumeClaim 的 ReplicaSet 不同，StatefulSets 为每个 Pod 创建一个 PVC。如果 StatefulSet 中的 Pod 被替换，替换 Pod 将绑定到具有其正在替换的状态的 PVC。替换可能发生在 Pod 失败或调度程序选择在另一个节点上运行 Pod 以平衡负载时。对于像 Cassandra 这样的数据库，这使得在 Cassandra 节点丢失时能够快速恢复，因为替换节点可以立即从关联的 PersistentVolume 恢复其状态，而无需从其他副本流式传输数据。

## 管理数据复制

在规划应用程序部署时，请确保考虑数据是在数据层还是存储层进行复制。像 Cassandra 这样的分布式数据库自行管理复制，根据你请求的复制因子在多个节点上存储数据的副本，通常每个 Cassandra 数据中心为 3 个。你选择的存储提供商也可能提供复制。如果每个 Cassandra Pod 的 Kubernetes 卷有 3 个副本，你最终可能会存储 9 份数据。虽然这 certainly 促进了高数据存活率，但这可能比你预期的成本更高。

