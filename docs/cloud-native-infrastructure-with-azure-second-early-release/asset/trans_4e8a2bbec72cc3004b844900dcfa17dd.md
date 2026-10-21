

现在我们已经配置好网络，可以开始应用一些安全策略了。Calico 网络策略的功能比 Kubernetes 更强大，包括：

- 策略排序
- 拒绝规则
- 匹配规则的更大灵活性

Calico 网络策略支持使用 OSI 第 5 层到第 7 层标准以及加密身份来保护应用程序。你可以在 Calico 网络策略配置页面找到示例。

网络策略可以通过以下几种方式强制执行：

- 全局（适用于所有命名空间中的所有 Pod），称为 GlobalNetworkPolicy
- 按网络或主机，称为 HostEndpoint
- 按命名空间，称为 NetworkPolicy

## GlobalNetworkPolicy：允许 ICMP 流量

我们创建的第一个策略是一个全局网络策略，允许集群内所有主机之间的 ICMP 流量用于 ping 和 traceroute。按如下方式创建文件 global-policy.yaml：

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: allow-ping-in-cluster
spec:
  selector: all()
  types:
  - Ingress
  ingress:
  - action: Allow
    protocol: ICMP
    source:
      selector: all()
    icmp:
```

```
type: 8 # Ping request
- action: Allow
  protocol: ICMPv6
  source:
    selector: all()
  icmp:
    type: 128 # Ping request
```

通过运行以下命令应用策略：

```
$ calicoctl create global-policy.yaml
```

## NetworkPolicy：允许命名空间内两个标签之间的流量

在此示例中，我们将允许来自 production 命名空间且选择器为 green 的流量访问同一命名空间中选择器为 blue 的 Pod，端口为 TCP/1234。创建一个名为 network-policy.yaml 的新文件：

```
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: allow-tcp-1234
  namespace: production
spec:
  selector: color == 'blue'
  ingress:
  - action: Allow
    protocol: TCP
    source:
      selector: color == 'green'
    destination:
      ports:
      - 1234
```

通过运行以下命令应用策略：

```
$ calicoctl create network-policy.yaml
```

应用此策略后，流量将能够流向任何 green Pod 的 TCP 1234 端口。

## Cilium

过去五年中引入的最具创新性的网络工具之一是 Cilium 项目，它大量利用 eBPF 来提供面向云原生基础设施的网络、可观测性和安全功能套件。

Cilium 利用 eBPF Linux 内核技术，可为内核及其上运行的应用程序提供低开销、高性能的可见性和控制逻辑。eBPF 的使用使 Cilium 成为基础设施架构师的热门选择，因为该平台围绕三个关键支柱提供功能：

### Networking

Cilium 实现了自己的 CNI，利用 Linux eBPF。这使得 Cilium 性能极高，并允许其执行负载均衡，而无需依赖 kube-proxy。它还允许多集群连接，这对于大型安装非常重要。

### Observability

Cilium 对 eBPF 的使用使其成为独特的观测软件。Cilium 可以对网络连接和数据的所有层执行深度包检测。

### Security

Cilium 的最佳功能之一是它可以通过 IPSec 在主机之间执行透明加密。这意味着你可以通过高效的机制获得端到端加密，而无需更改应用程序。虽然 Cilium 提供了自己的 label+CIDR 匹配安全策略，但它也执行（OSI）第 7 层过滤（例如，过滤传出 DNS 查询或过滤传入 HTTP 请求）。

最后，Cilium 允许你对策略制定决策执行第 7 层内省（有关更多信息，请参阅此博客文章）。

Cilium 将 eBPF 作为其核心使用，使软件能够深入检查和操作（在需要时）所有网络流量，而不会影响系统性能。

Cilium 代理（cilium-agent）在每个节点上运行（见图 8-5）并通过 Kubernetes API 接受配置。代理会将该配置转换为在容器的网络接口、主机的网络或主机之一的网卡上运行的 eBPF 程序。

## Orchestration systems

### Cilium layer

Cilium CLI

Policy repository

Plug-ins

Cilium monitor

Cilium daemon
Code generation

Bytecode injection

BPF program

BPF program

BPF program

BPF program

eth0

Container

Container

Container

### Kernel

Figure 8-5. The Cilium architecture

## Deploying Cilium

Cilium 可以安装在自管理的 Kubernetes 集群或托管的 AKS 集群上。在这两种情况下，Cilium 都可以通过 Helm 部署，这使得安装变得简单。此外，Cilium 还提供了一个容器来执行连接检查，以验证集群中 Pod 之间的网络流量是否可以自由流动。

有关更多信息，请参阅部署 Cilium 到 Azure 的专用文档。

### Self-managed Cilium installation

我们将使用其 quick-install 文件安装 Cilium：

```
$ kubectl apply -f
https://raw.githubusercontent.com/cilium/cilium/v1.9/install/kubernetes/quick-install.yaml
```

以下代码将安装主机之间的连接检查：

```
$ kubectl apply -f
https://raw.githubusercontent.com/cilium/cilium/v1.9/examples/kubernetes/connectivity-check/connectivity-check.yaml
```

这将部署一系列 Pod，它们将使用各种网络路径相互连接以验证连接性。连接路径包括有无服务负载均衡以及各种网络策略组合。

### AKS Cilium installation

要在 AKS 上设置 Cilium，我们将使用 Helm chart。

- 首先我们将创建一个新的 AKS 集群：

```
$ export RESOURCE_GROUP_NAME=aks-test
$ export CLUSTER_NAME=aks-test
$ export LOCATION=westus
```

```
$ az group create --name $RESOURCE_GROUP_NAME --location $LOCATION
$ az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --location $LOCATION \
    --node-count 2 \
    --network-plugin azure
```

- 现在我们将创建一个服务主体来与 Azure API 交互并填充环境变量以传递给 Helm：

```
$ az ad sp create-for-rbac --name cilium-operator > azure-sp.json

$ AZURE_SUBSCRIPTION_ID="$(az account show | jq -r .id)"
$ AZURE_CLIENT_ID="$(jq -r .appId < azure-sp.json)"
$ AZURE_CLIENT_SECRET="$(jq -r .password < azure-sp.json)"
$ AZURE_TENANT_ID="$(jq -r .tenant < azure-sp.json)"
$ AZURE_NODE_RESOURCE_GROUP="$(az aks show --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME | jq -r .nodeResourceGroup)"
```

- 我们将本地安装 Cilium 仓库，然后在 Kubernetes 集群上安装 Cilium：

```
$ helm repo add cilium https://helm.cilium.io/
$ helm install cilium cilium/cilium --version 1.9.9 \
    --namespace kube-system \
    --set azure.enabled=true \
    --set azure.resourceGroup=$AZURE_NODE_RESOURCE_GROUP \
    --set azure.subscriptionID=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantID=$AZURE_TENANT_ID \
```

```
--set azure.clientID=$AZURE_CLIENT_ID \
--set azure.clientSecret=$AZURE_CLIENT_SECRET \
--set tunnelDisabled \
--set ipam.mode=azure \
--set masquerade=false \
--set nodeinit.enabled=true
--set kubeProxyReplacement=strict \
--set hostFirewall=true \
--set loadBalancer.algorithm=maglev \
--set k8sServiceHost=REPLACE_WITH_API_SERVER_IP \
--set k8sServicePort=REPLACE_WITH_API_SERVER_PORT
```

- 我们可以通过检查是否已创建四个初始 Pod，并且 `cilium` 和 `cilium-operator` Pod 正在运行来验证安装：

```
$ kubectl -n kube-system get pods --watch
cilium-operator-ad4375ds5-2x2q3        1/1     Running
0               4m18s
cilium-s5x8xk                    1/1     Running
0               4m19s
```

## Installing Hubble

你还想安装 Hubble，这是一个 UI，允许你查看服务图，该图描绘了服务之间的依赖关系以及网络使用和性能的统计数据。Hubble 的实时更新、类似图形的界面（见图 8-6）允许你展示有关容器之间数据布局和流向的大量信息。此外，它还提供了一个建模 Cilium 策略的界面。

