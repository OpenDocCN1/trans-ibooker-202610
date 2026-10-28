

## Kubernetes 内部指标

主节点负责运行 Kubernetes 控制平面，后者反过来监控集群、做出调度决策并确保集群处于期望状态。因此，收集关键指标并监控控制平面组件（如 API Server、调度器、控制器和 etcd）至关重要，并将它们可视化到一个地方。这些指标提供了集群性能的详细视图，也有助于排查问题。由于所有组件都作为容器运行，让我们先来看最重要的容器指标：

| Category | Container Metrics | Description |
| --- | --- | --- |
| Summary | Containers running | Total number of containers running |
| Usage | Host CPU usage | CPU usage of the host on which container runs |
|  | Host storage usage | Storage usage of the host on which container runs |
|  | Host memory usage | Memory usage of the host on which container runs |
| CPU | CPU usage by container | CPU usage of the container under observation |
|  | Total container and system CPU usage | CPU usage of the container and the system |
| Memory | Memory usage by container in bytes | Memory usage of the container under observation measured in bytes |
|  | Memory usage % | Memory usage % of the container under observation |
| Networking | Bytes received by container | Bytes received over the network by the container |
|  | Bytes transferred by container | Bytes transmitted over the network by the container |
|  | Networking errors by container | Network errors, connection resets by container |
| Storage | Total disk usage | Disk usage by the container |
|  | Disk I/O | Disk IOPS by the container |

| Category | Kubernetes Metrics | Description |
| --- | --- | --- |
| **Summary Usage** | Nodes | Total no. of nodes in a Kubernetes cluster |
|  | Pods | Total no. of pods in a Kubernetes cluster |
|  | Pod containers | Total no. of pod containers in a Kubernetes cluster |
|  | Namespaces | Total no. of namespaces in a Kubernetes cluster |
|  | Average node CPU utilization | Average CPU utilization % by node |
|  | Average node memory utilization | Average memory utilization % by node |
|  | Average node storage utilization | Average storage utilization % by node |
|  | Cluster memory usage | Memory usage of the Kubernetes cluster |
| **Namespaces** | Pod count by namespaces | Total no. of pods in a namespace |
|  | Pod container count by namespaces | Total no. of pod containers in a namespace |
|  | Memory usage by namespaces | Total memory usage in a namespace |
| **Node** | CPU utilization by node | CPU utilization % of a node |
|  | CPU usage by node | CPU usage by a node |
|  | Memory utilization by node | Memory utilization % of a node |
|  | Memory usage by node | Memory usage by a node |
|  | Bytes received rate by node | Rate of bytes received over the network by the node |
|  | Bytes transferred rate by node | Rate of bytes transferred over the network by the node |
|  | File system usage % | File system usage % by node |
|  | File system usage by node | File system used by node in bytes |
|  | File system available storage by node | File system available in node in bytes |
| Pod | CPU usage rate by pod | CPU utilization % of a pod |
|  | Memory usage by pod | Memory usage by a pod |
|  | Bytes received rate by pod | Rate of bytes received over the network by the pod |
|  | Bytes transferred rate by pod | Rate of bytes transferred over the network by the pod |
|  | File system usage by pod | File system usage % by pod |

## Kubernetes 环境中的运维

由于 Kubernetes 本身并不直接优化性能，开发人员必须在整个技术栈中识别并调整各个资源，如主机、集群、容器和应用程序。每个容器和 Pod 都是不同的；有些可能是 CPU 密集型，而另一些可能运行的是 I/O 密集型工作负载。为了理解 Kubernetes 环境中的性能参数，运维人员必须提出以下问题：

- CPU 和内存的利用率水平如何？是否有节点面临资源争用？
- Pod 经历了怎样的延迟？某些 Pod 是否比集群中的其他 Pod 延迟更高？
- 为什么在启动自动扩缩容时会出现性能下降？自动扩缩容是否导致性能滞后？多租户是否带来性能影响？
- 底层依赖是否会阻碍应用程序的性能？

开发团队和 SRE 团队都有整体责任去关注应用程序的性能并确保整体 SLO 得到满足：

- 开发人员使用页面加载时间、缓存行为和事务时间等指标来定义 SLO，以提高软件质量。
- SRE 负责对应用程序和服务至关重要的平台服务。他们使用 SLO 来支持高速软件发布生命周期的质量。

决策是基于将开发人员和 SRE 的 SLO 与业务 SLO 进行关联得出的。在对环境进行更改后，开发人员和 SRE 会将应用程序数据与 Kubernetes 控制平面数据关联起来，以了解这些更改对整体环境的影响。此过程揭示了 Kubernetes 组件与其运行的工作负载之间的依赖关系。

例如，如果 Kubernetes 集群中的某些 Pod 过载或配置错误，集群内的其他所有操作以及部署的应用程序也可能受到影响。虽然 SRE 和平台工程师需要操作 Kubernetes  orchestration 层，但开发人员必须了解 Kubernetes orchestration 层对其应用程序和微服务的影响。因此，他们的挑战不同，如下表所示。

| Developers’ Challenges | SREs’ Challenges |
| --- | --- |
| - Resolve mismatch between desired and actual state (applications/workloads, container images, number of replicas, network and disk resources) for better resource usage and application performance<br>- Operate workloads within specified resource limits for their applications or microservices (e.g., increased number of pods per node can lead to service latencies)<br>- Take care of upstream and downstream dependencies<br>- Prevent pod failures | - Ensure that the Kubernetes components operate as expected (e.g., over- or underallocated resources may cause nonstarting or crashing pods under high workloads)<br>- Run within cloud provider limits (e.g., if a container exceeds its memory limit, Kubernetes might terminate it)<br>- Understand etcd load per object because major etcd failure will cripple or take down the container infrastructure<br>- Avoid node hotspots (e.g., nodes with low memory or disk capacity) |

尽管他们的挑战不同，但开发人员和 SRE 都需要摄取、观察、警惕并理解 Kubernetes 中暴露的数据流和指标。他们还需要共同协作，才能在 Kubernetes 平台上有效运行容器化应用程序。

## CHAPTER 3

## 现代监控系统的架构

在本书的第一章中，我们学习了监控和云原生概念，重点关注设计监控系统所必需的基础术语和思想。在第二章中，我们讨论了可观测性以及为什么它对于理解基于微服务的现代应用程序的动态如此重要。我们审视了可观测性的支柱——指标、日志和追踪——然后通过同样的视角探索了观察 Kubernetes 环境的可能性。到目前为止，我们已经确立了端到端可观测性解决方案的必要性，我会口语化地称之为云原生应用程序的现代监控系统。

在本章中，我们将研究监控的各个组件，了解它们的功能以及它们如何融入整体现代监控系统。我们将设计一个端到端架构，能够识别影响系统最细微的问题，并在最短时间内发出警报。到本章结束，以及本书第一部分结束时，读者将对监控的一般概念、其组件和框架、监控系统应包含的设计以及相关的术语和概念有坚实的理解。

在着手设计该架构之前，我们首先需要确定构成此监控平台的各个组件。这些单独的组件是

- Data collection
- Data storage
- Data analysis/query engine
- Data visualization

