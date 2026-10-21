

kind: Cluster
metadata:
  name: rook-cassandra
  namespace: rook-cassandra
spec:
  version: 3.11.6
  repository: my-private-repo.io/cassandra
  mode: cassandra
  annotations:
  datacenter:
    name: us-east2
    racks:
      - name: us-east2
        members: 3 # 将此数字上调或下调

```
$ kubectl -n rook-cassandra describe clusters.cassandra.rook.io rook-cassandra
```

> **NOTE**
> 
> 你可以在 Cassandra CRD 文档中找到有关 Cassandra 可配置项的更多详细信息。

同样，Ceph 和 NFS 集群也可以使用 Rook 操作符轻松部署。

## TiKV 简介

TiKV（Titanium Key-Value）由 PingCAP Inc. 创建，是一款开源的分布式事务型键值存储。与许多键值和 NoSQL 系统不同，TiKV 提供了简单的（原始）API 以及提供原子性、一致性、隔离性和持久性（ACID）合规的事务 API。

## 为什么使用 TiKV？

TiKV 提供了以下功能：

- 地理复制
- 水平扩展
- 一致的分布式事务
- Coprocessor 支持
- 自动分片

由于其自动分片和地理复制功能，以及能够扩展到存储超过 100 TB 数据的能力，TiKV 是一个有吸引力的选择。它还通过使用 Raft 协议进行复制提供了强大的共识保证。

## TiKV 架构

如前所述，TiKV 具有可用于不同用例的两种 API：原始和事务。表 9-1 概述了这两种 API 之间的差异。

## 表 9-1 TiKV API：原始与事务

|  | 原始 | 事务 |
|---|---|---|
| 描述 | 一个较低级别的键值 API，用于直接与单个键值对交互 | 一个较高级别的键值 API，提供 ACID 语义 |
| 原子性 | 单个键 | 多个键 |
| 使用时机... | 你的应用程序不需要分布式事务或多版本并发控制（MVCC） | 你的应用程序需要分布式事务和/或 MVCC |

TiKV 在每个节点中使用 RocksDB 作为存储容器，并利用 Raft 组提供分布式事务，如图 9-2 所示。Placement Driver 充当集群管理器，并确保所有分片都满足其复制约束，并且数据已在池上进行负载均衡。最后，客户端使用 Google 远程过程调用（gRPC）协议连接到 TiKV 节点，该协议针对性能进行了优化。

![](img/8d47d1b5dbca55e784ae602145a894c7_324_0.png)

图 9-2. TiKV 架构

在 TiKV 节点内（如图 9-3 所示），RocksDB 提供底层存储机制，Raft 为事务提供共识。TiKV API 为客户端提供交互接口，而 coprocessor 处理类似 SQL 的查询并从底层存储中组装结果。

![](img/8d47d1b5dbca55e784ae602145a894c7_324_1.png)

图 9-3. TiKV 实例架构

## 在 Kubernetes 上部署 TiKV

TiKV 可通过多种自动化方法部署，包括 Ansible、Docker 和 Kubernetes。在以下示例中，我们将使用 Kubernetes 和 Helm 部署一个基本集群。

- 安装 TiKV 自定义资源定义：
  
  ```
  $ kubectl apply -f
  https://raw.githubusercontent.com/tikv/tikv-operator/master/manifests/crd.v1beta1.yaml
  ```
- 我们需要探索 Helm 操作符。首先，添加 PingCap 仓库：
  
  ```
  $ helm repo add pingcap https://charts.pingcap.org/
  ```
- 现在为 tikv-operator 创建一个命名空间：
  
  ```
  $ kubectl create ns tikv-operator
  ```
- 安装操作符：
  
  ```
  $ helm install --namespace tikv-operator tikv-operator
  pingcap/tikv-operator --version v0.1.0
  ```
- 部署集群：
  
  ```
  $ kubectl apply -f
  https://raw.githubusercontent.com/tikv/tikv-operator/master/examples/basic/tikv-cluster.yaml
  ```
- 你可以通过运行以下命令检查部署状态：
  
  ```
  $ kubectl wait --for=condition=Ready --timeout 10m
  tikvcluster/basic
  ```
  
  这将创建一个单主机存储集群，存储空间为 1 GB，并包含一个 Placement Driver（PD）实例。

你可以在集群定义中修改 `replicas` 和 `storage` 参数以匹配你的需求。我们建议你为冗余目的至少使用三个副本进行存储。例如，如果你运行 `kubectl edit tikv.org TikvCluster` 并将 `replicas` 修改为 4，`storage` 修改为 500Gi：

```
apiVersion: tikv.org/v1alpha1
kind: TikvCluster
metadata:
  name: basic
spec:
  version: v4.0.0
  pd:
    baseImage: pingcap/pd
    replicas: 4
    # if storageClassName is not set, the default Storage Class
    of the Kubernetes cluster will be used
    # storageClassName: local-storage
    requests:
      storage: "1Gi"
    config: {}
  tikv:
    baseImage: pingcap/tikv
    replicas: 4
    # if storageClassName is not set, the default Storage Class
    of the Kubernetes cluster will be used
    # storageClassName: local-storage
    requests:
      storage: "500Gi"
    config: {}
```

你将获得四个副本的 500 GB 存储分区。

## 关于 etcd 的更多内容

我们已在整本书中（隐秘地）使用了 etcd。在这里我们将花一些时间详细讨论它。

Etcd 是一个强一致性（Raft 共识）的分布式键值存储，旨在用于分布式系统。它设计为能够优雅地处理领导者选举以及网络中断和意外节点停机等故障。Etcd 具有简单的 API（图 9-4），因此你会发现我们在这本书中讨论的许多系统（例如 Kubernetes、TiKV、Calico 和 Istio）在幕后利用了它。

通常，etcd 用于以键值格式存储配置值、功能标志和服务发现信息。Etcd 允许客户端监视这些项目以获取更改，并在它们更改时重新配置自身。Etcd 也适用于领导者选举和锁定用途。Etcd 在 Kubernetes 中用作服务发现后端，也用于 Rook 中的编排。

![](img/8d47d1b5dbca55e784ae602145a894c7_327_0.png)

图 9-4. Etcd 运行架构

如果你利用 etcd 作为核心基础设施，你很快就会理解这个基础设施可靠运行有多重要。由于 etcd 集群是基础设施的关键部分，我们建议以下措施以确保 Azure 中 etcd 集群的稳定。

## 硬件平台

一致的键值存储性能在很大程度上依赖于底层硬件的性能。性能不佳的一致键值集群会严重影响分布式系统的性能。etcd 也是如此。如果你每秒进行数百次事务，你应该为虚拟机使用托管固态硬盘（SSD）（我们推荐高级或超高速 SSD）以防止性能下降。你至少需要四个核心来运行 etcd，并且吞吐量会随着 CPU 核心数量的增加而扩展。你可以在通用硬件页面和 AWS 指南上找到有关容量规划和云硬件配置的更多信息（etcd 目前未发布 Azure 专用指南）。

我们建议在以下 Azure SKU 上运行 etcd：

- Standard_D8ds_v4（八核 CPU）
- Standard_D16ds_v4（16 核 CPU）

这些 SKU 明确允许你附加高级或超高速 SSD，这将提高集群的性能。

> **NOTE**
> 与我们在这本书中讨论的任何生产用例一样，我们强烈建议你在所有 Azure 计算实例上启用加速网络。

## 自动扩展和自动修复

如果我们不讨论自动扩展和自动修复如何与 etcd 配合使用，那就说不过去了。自动扩展不建议用于 etcd，因为如果容量增加或减少对集群性能产生不利影响，可能会产生意想不到的后果。也就是说，启用对损坏的 etcd 实例的自动修复（自动恢复）被认为是可以的。你可以通过在 Kubernetes 集群上运行以下命令来禁用 etcd 的自动扩展：

```
$ kubectl delete hpa etcd
```

这将禁用水平 Pod 自动扩缩器（HPA），它用于在 Kubernetes 中自动将集群扩展或缩减。你可以在 Kubernetes 文档中找到有关 HPA 的更多信息。

