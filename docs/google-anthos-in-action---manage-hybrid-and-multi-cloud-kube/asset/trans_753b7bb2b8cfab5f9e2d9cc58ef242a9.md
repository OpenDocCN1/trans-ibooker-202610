

## 设置

要启用此功能，用户必须执行以下 3 个操作：

### 1 - 启用多网络接口（multi-NIC）

通过向集群自定义资源的 `clusterNetwork` 部分添加 `multipleNetworkInterfaces` 字段并将其设置为 `true`，为 Pod 启用多网络接口。

```
...
  clusterNetwork:
    multipleNetworkInterfaces: true
    pods:
      cidrBlocks:
      - 192.168.0.0/16
    services:
      cidrBlocks:
      - 10.96.0.0/12
...
```

### 2 - 指定网络接口

使用 `NetworkAttachmentDefinition` 自定义资源来指定额外的网络接口。这些自定义资源对应于可供 Pod 使用的网络。

可以在创建集群时在集群配置清单中指定这些自定义资源，也可以直接将其添加到现有的目标集群中。

```
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
  name: gke-network-1
spec:
  config: '{
"type": "ipvlan",
"master": "ens224",
"mode": "l2",
"ipam": {
"type": "whereabouts",
"range": "172.120.0.0/24"
}
}'
```

### 3 - 将网络接口分配给 Pod

可以通过 `k8s.v1.cni.cncf.io/networks` 注解在 Pod 或 Deployment 清单中启用多个网卡，使用与特定 `NetworkAttachmentDefinition` 自定义资源及其命名空间对应的值。如下例所示，网络接口由目标集群默认命名空间中的两个 `NetworkAttachmentDefinition` 自定义资源 `gke-network-1` 和 `gke-network-2` 的名称指定。

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-deployment
spec:
...
  template:
    metadata:
      annotations:
        k8s.v1.cni.cncf.io/networks: default/gke-network-1, default/gke-network-2
      labels:
        app: sample-deployment
...
```

## 将网络接口限制到节点池（NodePool）

使用 `k8s.v1.cni.cncf.io/nodeSelector` 注解来指定 `NetworkAttachmentDefinition` 自定义资源有效的节点池。Anthos clusters on bare metal（裸金属上的 Anthos 集群）会强制所有引用此自定义资源的 Pod 部署到这些特定节点上。在以下示例中，Anthos clusters on bare metal 强制将分配了 `gke-network-1` 网络接口的所有 Pod 部署到 `multinicNP` 节点池。Anthos clusters on bare metal 会相应地为节点池打上 `baremetal.cluster.gke.io/node-pool` 标签。

```
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
    annotations:
        k8s.v1.cni.cncf.io/nodeSelector: baremetal.cluster.gke.io/node-pool
    name: gke-network-1
spec:
...
```

## 关于 SR-IOV 插件的说明

SR-IOV 是一种规范，本质上实现了物理 PCIe 设备的虚拟化。它允许将符合规范的网络设备（在主机节点上识别为物理功能 PF，通常代表单个 NIC 端口）划分为多个虚拟功能 VF，虚拟化工作负载可直接访问这些 VF。SR-IOV 对网络硬件的直接访问提供了增强的性能，因此被广泛用于基于 VM 的 VNF 中。

SR-IOV CNI 插件（https://github.com/intel/sriov-cni）使 Kubernetes Pod 能够直接连接到 SR-IOV VF，并将 VF 绑定到 DPDK 驱动程序。这为云原生网络功能（CNF）提供了增强的网络性能，就像以前为 VNF 所做的那样。

下面您可以看到一个示例图，展示了多个 CNF 通过多网卡 Pod 连接多个网段，串联在一起提供不同的服务（防火墙、深度包检测、SD-WAN）。

图 10.6：使用多个网卡连接不同网段的 CNF

![](img/fb3bebce7b5f593bca1a7f8a1371be06_318_0.png)

## 在裸金属上的 Anthos 上运行基于 VM 的工作负载

很可能并非所有网络功能都会部署为 CNF，有些仍将作为 VNF 保留，因此在一段时间内需要两种部署模型共存。

为了满足这种情况以及将部分工作负载作为 VM 而非容器运行的其他需求，Anthos on Bare Metal 提供了通过基于 KubeVirt 的 Anthos VM Runtime 运行基于 VM 的工作负载的可能性。

下面是对在 Anthos on Bare Metal 上运行 VM 所需执行任务的简要描述：

### 启用 Anthos VM Runtime

要启用 Anthos VM Runtime，您需要：

1. 更新 `VMRuntime` 自定义资源，将 `enabled` 设置为 `true`，如下例所示：

```yaml
apiVersion: vm.cluster.gke.io/v1
kind: VMRuntime
metadata:
  name: vmruntime
spec:
  enabled: true
  # useEmulation default to false if not set.
  useEmulation: true
  # vmImageFormat default to "qcow2" if not set.
  vmImageFormat: qcow2
```

2. 如果您的节点不支持硬件虚拟化，或者您不确定，请将 `useEmulation` 设置为 `true`。如果可用，硬件虚拟化比软件模拟提供更好的性能。如果未指定，`useEmulation` 字段默认为 `false`。

3. 您可以通过设置 `vmImageFormat` 字段来更改所创建 VM 使用的镜像格式，该字段支持两种磁盘镜像格式值：`raw` 和 `qcow2`。如果未设置 `vmImageFormat`，Anthos VM Runtime 将使用 `raw` 磁盘镜像格式创建 VM。`raw` 格式可能比 `qcow2`（一种写时复制格式）提供更好的性能，但可能占用更多磁盘空间。

4. 保存配置并验证 `VMRuntime` 自定义资源已启用：您可以执行 `kubectl describe vmruntime vmruntime` 并检查描述显示 `VMRuntime.Status.Ready` 设置为 `true`。

### 创建 VM

在创建 VM 之前，建议配置一个 cloud-init 文件，以确保在创建 VM 后您可以获得其控制台访问权限。有两种方法可以创建自定义 cloud-init 文件。最简单的方法是在创建 VM 时指定 `--os=<OPERATING_SYSTEM>` 标志。此方法会自动配置一个简单的 cloud-init 文件，并适用于以下操作系统：

- Ubuntu
- CentOS
- Debian
- Fedora

VM 创建完成后，您可以使用以下凭据首次访问它，然后更改密码：

```
user: root
password: changeme
```

如果您的镜像包含不同的基于 Linux 的操作系统，或者您需要更高级的配置，您可以手动创建自定义 cloud-init 文件，并通过指定 `--cloud-init-file=<path/to/file>` 标志来指定该文件的路径。在最基本的形式中，cloud-init 文件是一个 YAML 文件，包含以下内容：

```
#cloud-config
user: root
password: changeme
lock_passwd: false
chpasswd: {expire: false}
disable_root: false
ssh_authorized_keys:
- <ssh-key>
```

要使用 kubectl 创建 VM，您需要：

1. 使用以下命令安装 virtctl 插件：`sudo -E ./bmctl install virtctl`
2. 执行命令 `kubectl virt create vm`，下面是一个带参数的示例：

```
kubectl virt create vm VM_NAME \
  --boot-disk-access-mode=MODE \
  --boot-disk-size=DISK_SIZE \
  --boot-disk-storage-class="DISK_CLASS" \
  --cloud-init-file=FILE_PATH \
  --cpu=CPU_NUMBER \
  --image=IMAGE_NAME \
  --memory=MEMORY_SIZE
```

参数说明如下：

- **VM_NAME**：您要创建的 VM 的名称。
- **MODE**：启动磁盘的访问模式。可能的值是 `ReadWriteOnce`（默认）或 `ReadWriteMany`。
- **DISK_SIZE**：您想要的启动磁盘大小。默认值为 `20Gi`。
- **DISK_CLASS**：启动磁盘的存储类。默认值为 `local-shared`。
- **FILE_PATH**：自定义 cloud-init 文件的完整路径。根据镜像的不同，创建后可能需要此路径才能获得 VM 的控制台访问权限。
- **CPU_NUMBER**：您要为 VM 配置的 CPU 数量。默认值为 1。
- **IMAGE_NAME**：VM 镜像，可以是 `ubuntu20.04`（默认）、`centos8` 或镜像的 URL。
- **MEMORY_SIZE**：VM 的内存大小。默认值为 `4Gi`。

如果未指定参数，则使用默认值。

或者，可以应用定义 `VirtualMachine` 自定义资源的清单，这也支持流行的 GitOps 部署方法（声明式与异步管理）。

## 大型计算集群的编排与自动化

边缘和无线接入网络（RAN）CNF 部署的影响之一是，需要一个中心控制平面来管理更广泛、更细粒度分布的计算集群，并且要部署的应用程序实例数量可能比传统数据中心或公有云中通常部署的数量高出几个数量级。

在某些情况下，需要在分布在边缘位置的数十、数百甚至数千个计算集群上部署同一应用程序的实例。通常，需要根据特定标准有选择地定义实例的部署位置，并且还要在非常短的时间内动态调整，以适应网络、被监控对象或应用程序用户不断变化的需求。

由以下部分提供的集中配置和策略管理能力：

