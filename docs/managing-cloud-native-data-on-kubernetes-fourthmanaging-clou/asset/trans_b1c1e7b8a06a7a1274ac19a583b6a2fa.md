

```
Status:      Bound
Volume:      my-volume
Labels:      <none>
Annotations: pv.kubernetes.io/bind-completed: yes
             pv.kubernetes.io/bound-by-controller: yes
Finalizers:  [kubernetes.io/pvc-protection]
Capacity:    3Gi
Access Modes: RWO
VolumeMode:  Filesystem
Mounted By:  <none>
Events:      <none>
```

PVC 有两种状态值之一：Bound，表示它已绑定到卷（正如上面所示），或 Pending，表示它尚未绑定到卷。通常 Pending 状态意味着没有与该声明匹配的 PV。

下面是幕后发生的情况。Kubernetes 使用在 Pod 中作为卷引用的 PVC，并在调度 Pod 时将它们考虑在内。Kubernetes 识别与声明关联的属性相匹配的 PersistentVolumes，并将最小的可用卷绑定到该声明。这些属性可能包括标签或节点亲和性，正如我们在上面针对本地卷所看到的那样。

在启动 Pod 时，Kubernetes 控制平面确保 PersistentVolumes 已挂载到工作节点。然后，每个请求的存储卷都会在指定的挂载点挂载到 Pod 中。

## StorageClasses

上面显示的示例演示了 Kubernetes 如何将 PVC 绑定到已存在的 PersistentVolumes。这种在 Kubernetes 集群中显式创建 PersistentVolumes 的模型称为静态配置。Kubernetes 持久卷子系统还支持使用 StorageClasses 进行卷的动态配置（通常缩写为 SC）。StorageClass 负责根据集群中运行的应用程序的需求配置（和取消配置）PersistentVolumes，如图 2-11 所示。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_59_0.png)

图 2-11. StorageClasses 支持卷的动态配置

根据您使用的 Kubernetes 集群，很可能已经至少有一个 StorageClass 可用。您可以使用命令 kubectl get sc 来验证这一点。如果您在本地机器上运行简单的 Kubernetes 发行版且看不到任何 StorageClasses，您可以使用以下命令从 Rancher 安装一个开源本地存储提供程序：

```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

该存储提供程序已预装在 K3s 中，K3s 是 Rancher 提供的桌面发行版。如果您查看该语句中引用的 YAML 配置，您将看到以下 StorageClass 定义（源代码）：

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-path
provisioner: rancher.io/local-path
volumeBindingMode: WaitForFirstConsumer
reclaimPolicy: Delete
```

正如定义所示，StorageClass 由几个关键属性定义：

- `provisioner` 与底层存储提供程序（如公共云或存储系统）接口以分配实际存储。provisioner 可以是 Kubernetes 内置 provisioner 之一（称为“in-tree”，因为它们是 Kubernetes 源代码的一部分），也可以是符合 Container Storage Interface（CSI）的 provisioner，我们将在下面进行检查。
- `parameters` 是传递给 provisioner 的存储提供程序特定配置选项。常见选项包括文件系统类型、加密设置以及以 IOPS 为单位的吞吐量。有关更多详细信息，请查看存储提供程序的文档。
- `reclaimPolicy` 描述了在删除 PersistentVolume 时是否回收存储。默认值为 Delete，但可以覆盖为 Retain，在这种情况下，存储管理员将负责使用存储提供程序管理该存储的未来状态。
- 尽管上面的示例中未显示，但还有一个可选的 `allowVolumeExpansion` 标志。这表示 StorageClass 是否支持卷扩展的能力。如果为 true，则可以通过增加 PersistentVolumeClaim 的 storage.request 字段的大小来扩展卷。此值默认为 false。
- `volumeBindingMode` 控制存储何时配置和绑定。如果值为 `Immediate`，则只要创建引用 StorageClass 的 PersistentVolumeClaim，就会立即配置 PersistentVolume，并且声明会绑定到 PersistentVolume，而不管声明是否在 Pod 中被引用。许多存储插件还支持第二种模式，称为 `WaitForFirstConsumer`，在这种情况下，直到创建引用该声明的 Pod 之前，不会配置任何 PersistentVolume。这种行为被认为更可取，因为它为 Kubernetes 调度程序提供了更大的灵活性。

## LIMITS ON DYNAMIC PROVISIONING

Local 不能由 StorageClass 动态配置，因此您必须手动创建它们。

应用程序开发人员可以通过在定义中添加 storageClass 属性来在创建 PVC 时引用特定的 StorageClass。例如，这里是引用 local-path StorageClass 的 PVC 的 YAML 配置（源代码）：

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-local-path-claim
spec:
  storageClassName: local-path
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

如果在声明中未指定 storageClass，则使用默认 StorageClass。默认 StorageClass 可以由集群管理员设置。正如我们在上面的 Persistent Volumes 部分所示，您可以通过使用空字符串来选择不使用 StorageClasses，这表示您使用的是静态配置的存储。

StorageClasses 提供了一个有用的抽象，集群管理员和应用程序开发人员可以用作契约：管理员定义 StorageClasses，开发人员通过名称引用 StorageClasses。底层 StorageClass 实现的细节可能因 Kubernetes 平台提供商而异，促进应用程序的可移植性。

这种灵活性允许管理员创建代表各种不同存储选项的 StorageClasses，例如，以区分吞吐量或延迟方面的不同服务质量保证。这个概念在其他存储系统中被称为“profiles”。请参阅 *Developers are Driving the Future of Kubernetes Storage*（侧边栏），了解有关如何以创新方式利用 StorageClasses 的更多想法。

## Kubernetes Storage Architecture

在前面的章节中，我们讨论了 Kubernetes 通过其 API 支持的各种存储资源。在本章的剩余部分，我们将看看这些解决方案是如何构建的，这可以为我们提供有关如何构建云原生数据解决方案的一些宝贵见解。

## DEFINING CLOUD-NATIVE STORAGE

我们在本章中讨论的大多数存储技术都被捕获为 Cloud Native Computing Foundation（CNCF）景观中列出的“云原生存储”解决方案的一部分。CNCF 存储白皮书是一份有用的资源，定义了云原生存储的关键术语和概念。这两个资源都会定期更新。

## Flexvolume

最初，Kubernetes 代码库包含多个“in-tree”存储插件，即包含在与 Kubernetes 其余代码相同的 GitHub 仓库中。其优点是它有助于标准化连接不同存储平台的代码，但也有一些缺点。首先，许多 Kubernetes 开发人员对包含的广泛存储提供程序的专业知识有限。更重要的是，升级存储插件的能力与 Kubernetes 发布周期绑定，这意味着如果您需要存储插件的修复或增强，您必须等待它被接受到 Kubernetes 发布中。这减慢了 K8s 存储技术的成熟速度，因此采用速度也放缓了。

Kubernetes 社区创建了 Flexvolume 规范，以允许开发独立于 Kubernetes 源代码树开发的插件，即在不绑定到 Kubernetes 发布周期的情况下独立开发。与此同时，存储插件标准正在
