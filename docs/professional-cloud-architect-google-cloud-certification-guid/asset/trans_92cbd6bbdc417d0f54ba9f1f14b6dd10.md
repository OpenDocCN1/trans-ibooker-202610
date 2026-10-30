

```
google3539162_student@cloudshell:~ (qwiklabs-gcp-979d5d649f59493b)$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
nginx-deployment-5c689d88bb-2d7pq   1/1     Running   0          2m33s
nginx-deployment-5c689d88bb-srtn8   1/1     Running   0          2m33s
nginx-deployment-5c689d88bb-z4mkp   1/1     Running   0          2m33s
```

Figure 6.37 – Pods

4. 现在，我们需要返回到控制台并检查 Workloads 菜单。我们会看到 nginx-deployment 存在。点击 Name 查看其详细信息，如以下截图所示：

![](img/de2c97ab017f36b3750a5bade111edd7_199_0.png)

Figure 6.38 – Workloads

5. 点击 Name 列中的 nginx-deployment 链接以进入详细视图。在详细视图顶部，我们会被询问是否要公开服务。点击 Expose 按钮，如以下截图所示：

![](img/de2c97ab017f36b3750a5bade111edd7_199_1.png)

Figure 6.39 – Exposing a Deployment

6. 现在，我们可以选择一个 **Port** 和一个 **Target port**、一个 **Protocol**，以及最重要的 Service 类型。我们将选择一个 **Load balancer**。如果点击 **View YAML**，你会看到服务的定义。你可以复制它并使用 `kubectl` 从 shell 部署它。相反，我们将点击 **Expose** 按钮来看看如何在 GUI 中完成此操作：

![](img/de2c97ab017f36b3750a5bade111edd7_200_0.png)

Figure 6.40 – Service settings

7. 部署服务需要几秒钟时间。部署完成后，它将在 **Kubernetes Engine** 菜单下的 **Services** 中可见。找到 `nginx-deployment-services` 服务并点击其名称以查看以下详细信息。找到 **Load balancer IP** 值并复制它：

| Cluster | high-cpu-cluster-1 |
| Namespace | default |
| Labels | app : nginx |
| Stackdriver logs | Deployment: nginx-deployment |
| Type | LoadBalancer |
| External endpoints | 35.194.21.47:80 |

# LoadBalancer

| Cluster IP | 10.39.252.240 |
| Load balancer IP | 35.194.21.47 |
| Load balancer | ac57afbda82f711e9b18142010a80008 |

# Deployments

| Name | Status | Pods |
| nginx-deployment | OK | 3/3 |

# Serving pods

| Name | Status | Restarts | Created on |
| nginx-deployment-5c689d88bb-2d7pq | Running | 0 | May 30, 2019, 6:20:12 PM |
| nginx-deployment-5c689d88bb-srtn8 | Running | 0 | May 30, 2019, 6:20:12 PM |
| nginx-deployment-5c689d88bb-z4mkp | Running | 0 | May 30, 2019, 6:20:12 PM |

# Ports

| Port | Node Port | Target Port | Protocol | |
| 80 | 31871 | 80 | TCP | Port forwarding |

Figure 6.41 – Service details

8. 将 IP 粘贴到你的网络浏览器中，或点击 External endpoints 键旁边的链接。你会看到以下 NGINX 欢迎信息：

![](img/de2c97ab017f36b3750a5bade111edd7_202_0.png)

Figure 6.42 – Accessing the application

至此，我们已成功将第一个应用程序部署到 GKE 集群。正如你所见，你可以使用 GUI 或命令行来完成此操作。Google Cloud Console 允许你在无需手动定义 YAML 文件的情况下部署 Kubernetes 对象。它还会可视化最有用的对象及其关系。使用 GUI，你还可以执行一些第二天操作。此时，我们鼓励你获得一些实践经验，并尝试使用这两种方法部署你的应用程序。

## Cluster second-day operations

到目前为止，我们已经创建了一个集群并部署了第一个应用程序。一切都在运行。现在，让我们来看看第二天操作，例如升级和扩缩容。正如你所知，如果手动部署集群，这些操作可能非常耗时。当你使用 GKE 时，它会为你自动执行这些任务。你只需启动它，甚至可以允许 GKE 决定何时触发操作。让我们从升级开始。

## Upgrading the cluster

Google Kubernetes 允许我们无需手动安装任何东西即可简单地升级集群组件。master 和节点是分别升级的。请记住，master 只能与比其版本旧两个次要版本以内的节点一起工作。让我们开始：

- 1. 在运行任何命令之前，通过运行以下命令设置默认区域：

```
gcloud config set compute/zone us-central1-a
```

- 2. 要检查 master 和节点的当前版本，请使用以下命令：

```
gcloud container clusters describe $CLUSTER_NAME
```

- 3. Google 会自动升级 master，但你也可以通过运行以下命令手动触发 master 升级：

```
gcloud container clusters upgrade $CLUSTER_NAME --master
```

- 4. master 升级后，你可以通过运行以下命令升级节点：

```
gcloud container clusters upgrade $CLUSTER_NAME
```

GKE 会尝试将正在升级的节点上的 Pod 重新调度。节点将被替换为具有所需版本的新节点。当节点向 master 注册时，它们会被标记为可调度。

> **Important Note**
> 虽然你可以降级节点，但 Google 不推荐这样做。
> 你可以将节点降级到比 master 低一个补丁版本。

### Auto-upgrades

对于新创建的集群，自动升级默认启用。节点会被升级以跟上 master 的版本。升级在选定的维护窗口期间进行。一次只升级一个节点，并且只在集群的一个节点池内进行。在升级期间，驻留在正在升级的节点上的 Pod 会被重新调度。如果无法重新调度，Pod 会进入 pending 模式。要启用自动升级，请在创建或更新集群时使用 --enable-autoupgrade 标志。

### Auto-repair

自动修复选项允许你保持集群节点的健康。它监控节点的健康状态，并在需要时重新创建它们。当节点报告 NotReady 或大约 10 分钟内未报告时，会触发重新创建。如果节点大约 30 分钟内磁盘空间不足，也会触发。在重新创建之前，节点会被 drained。如果节点无响应，此操作可能不成功。要启用自动修复，请在创建或更新集群时使用 --enable-autorepair 标志。

### Resizing the cluster

GKE 集群可以通过增加或减少节点池中的节点数量来调整大小。要执行调整大小，请使用以下命令：

```
gcloud container clusters resize $CLUSTER_NAME --node-pool $POOL_NAME --num-nodes $NUM_NODES
```

这里，我们有以下选项：

- - $CLUSTER_NAME: 集群的名称
- - $POOL_NAME: 要调整大小的节点池的名称
- - $NUM_NODES: 池中运行的节点数

当你增加集群大小时，新的 Pod 可能会被调度到新节点上，但旧的 Pod 不会被迁移。当你减少集群大小时，托管在被移除节点上的 Pod 将被删除。它们只有在由复制控制器管理时才会在其他节点上重新创建。要在移除前 drained 节点，你可以使用 beta 命令：

```
gcloud beta container clusters resize $CLUSTER_NAME --node-pool $POOL_NAME --num-nodes $NUM_NODES
```

### Autoscaling a cluster

你也可以将集群设置为自动扩缩容。此扩缩容事件可由工作负载和资源使用情况的变化触发。自动扩缩容监控你的节点并检查是否应增加或减少。集群中运行的节点的最大数和最小数在以下命令中定义：

```
gcloud container clusters create $CLUSTER_NAME --num-nodes $NUM --enable-autoscaling --min-nodes $MIN_NODES --max-nodes $MAX_NODES --zone $COMPUTE_ZONE
```

这里，我们有以下选项：

- - $CLUSTER_NAME: 集群的名称
- - $NUM: 初始节点数
- - $MIN_NODES: 最小节点数
- - $MAX_NODES: 最大节点数

