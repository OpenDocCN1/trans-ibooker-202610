

![](img/8d47d1b5dbca55e784ae602145a894c7_287_0.png)

图 8-6. 显示简单架构的 Hubble UI 截图

你可以使用 kubectl 安装 Hubble：

```
kubectl apply -f
https://raw.githubusercontent.com/cilium/cilium/v1.9/install/kubernetes/quick-hubble-install.yaml
```

## 将 Cilium 与云集成

正如我们之前提到的，Cilium 具有丰富的特性集可供利用。在本节中，我们将使用 Cilium 作为网络提供者和策略执行器（防火墙）。然后我们将查看 Cilium 的可观测性。

## 主机防火墙

与我们对 Calico 的深入探讨类似，这里我们将实现一组网络策略。Cilium 的优势在于它可以在 L3、L4 或 L7 上执行策略。网络策略执行有三种模式，如表 8-1 所示。

表 8-1. 网络策略执行

| 模式 | 描述 |
|------|-------------|
| Default | 如果规则选择了端点并且规则具有 ingress 或 egress 部分，则端点将根据策略拒绝。 |
| Always | 在所有端点上启用策略执行，即使没有规则选择特定端点。 |
| Never | 在所有端点上禁用策略执行，并且允许所有流量。 |

要为 Cilium 代理管理的全部端点在运行时配置策略执行模式，请使用以下命令并选择 default、always 或 never 之一的模式：

```
$ cilium config PolicyEnforcement={default,always,never}
```

策略是基于允许列表模型编写的，如果没有明确的允许规则，流量将被阻止。

L3 和 L4 规则可以使用以下方法指定：

### 标签

使用选择器对 Pod 进行分组。

### 服务

使用 Kubernetes 服务定义策略。

### 实体

Cilium 管理的特殊预定义作用域。

### IP/CIDR

一个 IP 或 IP 范围（CIDR）。

### DNS 名称

一个 DNS 名称。

例如，如果你想允许两个带标签的 Pod 之间的流量，可以创建以下策略：

```
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: "l3-rule"
spec:
  endpointSelector:
    matchLabels:
      role: backend
  ingress:
  - fromEndpoints:
  - matchLabels:
      role: frontend
```

你可以通过运行以下命令应用此策略：

```
$ cilium policy import l3-rule-example-1.yaml
```

此外，如果你想创建一个 L4 示例，其中你想将出站 egress DNS 限制为 Google 公共 DNS 服务器，你可以编写以下示例中显示的规则：

```
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: "allow-to-google-dns"
spec:
  endpointSelector:
    {}
```

```
egress:
- toCIDR:
  - 8.8.8.8/32
  - 8.8.8.4/32
  toPorts:
  - ports:
    - port: '53'
      protocol: UDP
  - ports:
    - port: '53'
      Protocol: TCP
```

你可以通过运行以下命令应用规则：

```
$ cilium policy import l4-rule-example-1.yaml
```

最后，Cilium 还允许对 HTTP(s)、Kafka 和 DNS 流量进行 L7 可见性。在下一个示例中，我们允许所有 Pod 对 80 端口上的 /admin 端点进行流量：

```
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: "rule1"
spec:
  description: "Allow HTTP GET /admin
endpointSelector:
  {}
ingress:
- fromEndpoints:
  toPorts:
  - ports:
    - port: "80"
      protocol: TCP
    rules:
      http:
      - method: "GET"
        path: "/admin"
```

你可以在 Layer 7 policy Cilium 文档中找到有关 L7 策略规则的更具体细节。

NOTE
Cilium 提供了一个交互式工具来建模网络策略并可视化它们。你可以在 Cilium Editor 页面上尝试使用它。

## 可观测性

正如我们之前简要提到的，Hubble 是 Cilium 操作员观察其基础设施中发生情况的地方。一旦你安装了 Hubble 和连接性测试（来自安装步骤），你将能够观察命名空间内的网络流。在本节中，我们将更进一步，利用 Cilium 的一项新功能，该功能提供 L3 和 L4 协议可见性，这将为每个端口上的流量提供更具体的详细信息。你需要实现 L7 策略才能获得 L7 可观测性功能。

要启用 L3 和 L4 可见性，你需要在 Pod 上启用注解。以下是一个示例：

```
$ kubectl annotate pods --all io.cilium.proxy-visibility="<Egress/53/UDP/DNS>,<Ingress/80/TCP/HTTP>"
```

这将启用对 Pod 的出站（egress）DNS 流量以及入站（ingress）HTTP 流量的指标。

可见性信息由注解中的逗号分隔的元组列表表示：

```
<{Traffic Direction}/{L4 Port}/{L4 Protocol}/{L7 Protocol}>
```

启用后，Hubble 将显示协议特定的信息。图 8-7 显示了 DNS 可见性的示例。

![](img/8d47d1b5dbca55e784ae602145a894c7_293_0.png)

图 8-7. DNS 流量可观测性的示例

如果你想了解如何检查传输层安全（TLS）加密连接，请访问 Inspecting TLS Encrypted Connections with Cilium 页面。

## Flannel

Flannel 是 Kubernetes 的（OSI）第 3 层网络 fabric 系统，最初是 Red Hat CoreOS 项目的一部分。它是 Kubernetes 最简单的 CNI 之一。它依赖于一个名为 flanneld 的二进制代理在每个主机上运行，以配置覆盖 Pod 网络，然后将配置存储在 Kubernetes 数据存储中。Flannel 仅提供网络连接。它不支持任何类型的网络策略执行。

## 部署 Flannel

当你设置集群时，可以通过运行以下命令快速安装 Flannel：

```
$ kubectl apply -f
https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

应用 *kube-flannel.yml* 将定义以下内容：

- 一个用于基于角色的访问控制（RBAC）的 `ClusterRole` 和 `ClusterRoleBinding`。
- Flannel 使用的 Kubernetes 服务帐户。
- 一个包含 CNI 配置和 Flannel 配置的 `ConfigMap`。Flannel 配置中的网络应与 Pod 网络的无类域间路由（CIDR）方案匹配。 后端的选择也在此处进行，默认值为 VXLAN。
- 用于在每个节点上部署 Flannel Pod 的每个架构的 DaemonSet。Pod 有两个容器：Flannel 守护进程本身，以及用于将 CNI 配置部署到 kubelet 可读取的位置的 `initContainer`。

当你运行 Pod 时，它们将从 Pod 网络 CIDR 分配 IP 地址。无论这些 Pod 最终位于哪个节点，它们都能够相互通信。

> **NOTE**
> 
> 此清单将配置 Pod 网络默认为 10.244.0.0/16 并默认使用 VXLAN 后端。每个节点在 Pod 中将获得一个 10.244.X.0/24 掩码。

你可以通过修改写入 *kube-flannel.yaml* 文件中 *net-conf.json* 的数据来重新配置后端以使用不同的地址空间、更小的掩码或不同的后端设置。你可以在 Flannel backend 文档和 Flannel 配置页面上找到有关后端配置的更多信息。

现在 Kubernetes 工作节点将具有来自你的 Azure VNet 的 IP 地址，但任何部署的 Pod 将具有来自配置的 Pod 网络 IP 范围的 IP 地址。如果你想要多个 Pod 网络（用于网络流量分离），你将不得不运行单独的 `flanneld` 进程。

## Flannel 深入解析

Flannel 是一个更简单的 Kubernetes CNI，需要最少的配置。它具有可插拔的后端，可在网络上的机器之间创建覆盖网络（见图 8-8）。支持的传输后端包括：

- VXLAN
- Host-gw
- 用户数据报协议（UDP）
- Alivpc
- Alloc
- Amazon Virtual Private Cloud（VPC）
- Google Compute Engine（GCE）
- IPIP
- IPsec

![](img/8d47d1b5dbca55e784ae602145a894c7_296_0.png)

图 8-8. Flannel 架构示例

Flannel 项目团队建议将 VXLAN 或 host-gw 作为通信后端。如果你需要加密流量，IPsec 也是一个选项。你可以在 Flannel 文档中找到有关支持后端的更多信息。

Flannel 不允许你从一个守护进程运行多个网络；你需要使用不同的配置文件运行单独的守护进程。

