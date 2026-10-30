

要从控制台创建快照，我们应浏览到导航菜单，然后进入 **Compute Engine | Snapshots**。然后，我们必须为快照命名并选择要使用的源磁盘。最后，我们可以选择要存储快照的位置，如以下屏幕截图所示：

![](img/de2c97ab017f36b3750a5bade111edd7_93_0.png)

图 4.18 – 创建快照

下图展示了多个快照的工作流程。第一个快照将是对磁盘上所有数据的完整快照，后续快照将是增量的：

![](img/de2c97ab017f36b3750a5bade111edd7_94_0.png)

图 4.19 – 多个快照

> **重要说明**
> 
> 快照是增量的，因此将包含与上一个快照不同的块。因此，如果你定期创建快照，其成本低于定期创建磁盘的完整映像。每个快照在 GCP 内的多个位置存储以实现冗余。只要有正确的权限，快照可以跨项目存储。

考试不会对快照进行详细介绍，但可以在此处找到更多信息：https://cloud.google.com/compute/docs/disks/create-snapshots。

## 现有磁盘

现有磁盘也可以附加到新实例。要创建新磁盘，我们可以浏览到导航菜单并进入 **Compute Engine | Disk**。以下屏幕截图显示我们正在创建新的空白磁盘：

![](img/de2c97ab017f36b3750a5bade111edd7_95_0.png)

图 4.20 – 一个新磁盘

创建成功后，我们可以在部署新的 Compute Engine 实例时使用此新磁盘。以下屏幕截图显示了当我们点击“现有磁盘”选项卡时其外观。我们可以看到所有可用的磁盘：

![](img/de2c97ab017f36b3750a5bade111edd7_96_0.png)

图 4.21 – 现有磁盘

我们也可以从前面的屏幕截图中看到，我们可以使用磁盘的快照作为启动盘。我们在本章前面讨论了如何拍摄快照。

## 标签

还有许多其他可修改的选项。为了对相关资源进行分组，对 VM 实例进行标记是有益的。这有助于组织和筛选计费与报告。标签使用键值对工作，可用于例如区分项目、环境等。只需点击添加标签，你将被提示添加键值对，它将被保存并添加到主创建实例屏幕中，如下所示：

![](img/de2c97ab017f36b3750a5bade111edd7_96_1.png)

图 4.22 – 标签

前面的屏幕截图是此示例，其中我们可以将实例标记为生产环境。

## 机密 VM 服务

启用此选项可在使用时加密数据。这将在处理期间保持数据在内存中加密，且 Google 无法访问加密密钥：

![](img/de2c97ab017f36b3750a5bade111edd7_97_0.png)

图 4.23 – 机密 VM 服务

## 容器

启用此选项可使用 Container-Optimized OS (COS) 部署和启动 Docker 容器。请注意，每个 VM 实例只能部署一个容器。这与 Google Kubernetes Engine (GKE) 不同，我们将在第 6 章《使用 Google Kubernetes Engine 管理 Kubernetes 集群》中讨论该主题：

![](img/de2c97ab017f36b3750a5bade111edd7_97_1.png)

图 4.24 – 容器

前面的图显示了部署容器映像的简单复选框。

## 删除保护

启用此选项可防止 GCE 实例被意外删除。勾选此框将在实例资源上设置 deletionProtection 属性。通常，只有需要无限期运行的关键 VM 实例才会勾选此选项。即使勾选后，实例仍可被重置、停止，甚至在项目终止后被移除。只有具有 compute.instance.create 权限的用户才能修改此标志以允许受保护的 VM 被删除。

## 预留

可以在特定区域为 VM 实例创建预留，使用自定义或预定义机器类型。使用预留可确保在需要时为你的工作负载提供资源；然而，一旦创建预留，你将立即开始为其付费，直到预留被删除。使用预留前应考虑需求增长。此选项允许你选择现有预留、创建新预留或不使用。

## 元数据

向 GCE 实例添加元数据允许未来的查询提取有关实例的信息。元数据存储在元数据服务器上，可以通过 Compute Engine API 或实例本身进行查询。我们可以提取诸如主机名、实例 ID 或我们应用的任何自定义元数据等信息。我们还可以使用元数据向应用程序提供配置信息，从而无需将此数据存储在应用程序本身中。

我们还使用元数据引用在实例部署期间执行的启动脚本。假设我们在名为 deployments 的 Cloud Storage 存储桶上托管名为 startup.sh 的启动脚本。为了确保其执行，我们需要添加存储桶和启动脚本的路径：

![](img/de2c97ab017f36b3750a5bade111edd7_98_0.png)

图 4.25 – 元数据

前面的屏幕截图显示了在控制台中其外观。

## 启动脚本

在填充 GCE 部署的变量时，我们也可以设置启动脚本。这是实例启动或重启时运行的脚本。常见示例包括安装特定软件或确保服务已启动。这些脚本通过我们之前提到的同一元数据服务器指定，并使用启动脚本元数据键。同样，这些脚本也可以通过 gcloud 命令以及在控制台中添加。让我们看一下来自 CLI 的示例命令。这是在安装 apache2 并填充主页。请注意，运行此代码后，GCP 会要求我们确认区域：

```
gcloud compute instances create cloudarchitect --tags http-server \
--metadata startup-script='#! /bin/bash
# Installs apache and a custom homepage
```

```bash
sudo su -
apt-get update
apt-get install -y apache2
cat <<EOF > /var/www/html/index.html
<html><body><h1>Hello World</h1>
<p>This page was created from a simple start up script!</p>
</body></html>
EOF'
```

也可以指定位于 Google Cloud Storage 上的脚本。同样，从 gcloud CLI，我们可以指定存储桶位置。在此示例中，我们的存储桶名为 `cloudarchitectbucket`：

```bash
gcloud compute instances create example-instance --scopes
storage-ro \
    --metadata startup-script-url=gs://cloudarchitectbucket/
startupscript.sh
```

> **考试提示**
> 
> 如果我们想指定关机脚本，则会浏览到元数据部分并添加 `shutdown-script` 作为键，并将脚本内容作为值添加。

我们应注意，启动脚本仅在网络可用时执行。

## 可抢占性

可抢占性会降低实例成本，但通常最多只能持续 24 小时，然后将被终止。某些操作，例如停止和启动实例，将重置此计数器。部署生命周期如此有限的 VM 听起来可能很奇怪，但如果我们在应用层具有容错能力，则利用可抢占实例可以带来显著的成本节约。让我们以批处理作业为例。

这些可以在可抢占实例上运行，如果某些实例被终止，处理会变慢但不会完全停止。这允许批处理在不给现有实例增加额外工作负载的情况下进行。我们应注意，然而，由于可抢占实例的性质，没有 GCP **服务等级协议 (SLA)** 适用，且它们可在仅提前 30 秒通知的情况下被提供商移除。

