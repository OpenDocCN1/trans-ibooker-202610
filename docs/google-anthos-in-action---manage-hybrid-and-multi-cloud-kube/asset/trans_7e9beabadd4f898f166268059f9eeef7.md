
因此，您应该会看到类似下面的输出：

```
NAME                 TYPE         CLUSTER-IP       EXTERNAL-IP      PORT
app-source-vm        ClusterIP    None             <none>           <none>
kubernetes           ClusterIP    10.63.240.1      <none>           443
my-service           LoadBalancer 10.63.243.209    35.232.24.49     80:
```

在这里，外部 IP 地址是 35.232.24.49。现在我可以打开浏览器，确认一切正常（参见图 18.5）。

![](img/fb3bebce7b5f593bca1a7f8a1371be06_685_0.png)

恭喜！您已成功使用 CLI 将运行在 GCE 上的虚拟机迁移到运行在 GKE 上的容器中。如果您想了解另一个基本迁移示例，建议您参考 quicklabs[27] 上的实操实验“Migrate for Anthos: Qwik Start”。

既然您已经知道如何使用 CLI 执行迁移，我们将进入下一节，在那里我们将使用 Cloud 控制台 UI 来执行迁移。

## 18.4.3 Google Cloud Console UI 迁移示例

在本节中，我们使用 M4A 将运行在 Google Compute Engine 上的虚拟机应用迁移到 Google Kubernetes Engine。GKE 处理集群可以位于云端或本地。迁移将通过 Google Cloud Console 提供的图形用户界面（GUI）运行。请注意，CLI 和 GUI 的迁移过程是一致的。

GKE 集群将用作“处理集群”来控制迁移。过程中生成的工件将存储在 Google Cloud Storage（GCS）上，最终的容器映像会推送到 Google Container Registry。在底层，这与 CLI 驱动的迁移完全相同。

第一步是通过以下地址从控制台访问 Anthos Migrate（参见图 6）：

[https://console.cloud.google.com/anthos/migrate](https://console.cloud.google.com/anthos/migrate)

### 图 18.6：访问 Anthos Migrate 以迁移到容器

![](img/fb3bebce7b5f593bca1a7f8a1371be06_686_0.png)

为简单起见，我们将从 Marketplace 部署一台预装了 Tomcat Server 的虚拟机。我们将使用此工作负载将 Tomcat 从虚拟机迁移到容器。

首先，让我们访问带有 Tomcat 的 Google Click-to-Deploy 仓库（参见图 18.7）。URL 为：

https://console.cloud.google.com/marketplace/details/click-to-deploy-images/tomcat

### 图 18.7：将 Tomcat 应用部署到虚拟机

![](img/fb3bebce7b5f593bca1a7f8a1371be06_687_0.png)

然后，让我们选择要部署的区域（参见图 18.8）：

### 图 18.8：已部署的 Tomcat 应用

![](img/fb3bebce7b5f593bca1a7f8a1371be06_688_0.png)

一旦部署了带有 Tomcat Server 的虚拟机，就可以通过外部 IP 访问该网站（参见图 9）。

### 图 18.9：使用 Google Click-to-Deploy 部署的 Tomcat 解决方案

![](img/fb3bebce7b5f593bca1a7f8a1371be06_689_0.png)

### Tomcat

由 Google Click to Deploy 提供的解决方案

| 站点地址 | http://35.238.48.196/ |
| 管理员用户 | tomcat |
| 管理员密码（临时） | EKDm4yrQ |
| 实例 | tomcat-vm-gulli-vm |
| 实例区域 | us-central1-c |
| 实例机器类型 | n1-standard-1 |

在此部署中，IP 地址是 http://35.238.48.196/，因此如果您访问该网站，应该会看到如图 18.10 所示的输出：

### 图 18.10：从 Web 访问 Tomcat

# It works !

下一步是启动正式的迁移过程。首先要做的是创建一个“处理集群”，该集群将用于控制源虚拟机的迁移。您可以通过访问 Anthos 上的“Migrate to containers”菜单并使用“添加处理集群”选项来完成此任务（参见图 18.11）：

### 图 18.11：开始迁移过程

### 添加处理集群

最好遵循 GUI 中提供的建议，创建一个专门用于处理的新集群（参见图 18.12）：

### 图 18.12：为“处理集群”选择名称

![](img/fb3bebce7b5f593bca1a7f8a1371be06_690_0.png)

集群就绪后，您应该能够通过 Google Cloud Console 的 GKE 部分看到它（参见图 18.13）。

### 图 18.13：处理集群已准备好使用

### 2 定义源名称和类型

为新的源定义一个名称。

名称 *
migrate-vm-tomcat-gulli

选择要从中迁移工作负载的源环境。

Compute Engine

了解如何定义当前通过工具 CLI 支持的其他源类型（VMWare、AWS 和 Azure）。了解更多

选择名称后，您可以选择源虚拟机所在的项目（参见图 18.19）。

### 图 18.19：选择源虚拟机所在的项目

### 设置迁移

迁移名称 *
tomcat-migr

选择源 *
migrate-vm-tomcat-gulli processing-cluster-migrate

要了解如何添加更多源，请点击此处。

虚拟机操作系统类型 *
Linux

选择待迁移虚拟机的操作系统类型（Windows 或 Linux）。

虚拟机 ID *
tomcat-vm-gulli-vm

指定要迁移的虚拟机的 ID。

迁移意图 *
Image

然后，M4A 将开始生成迁移计划（参见图 18.22）：

### 图 18.22：生成迁移计划

| 迁移名称 ↑ | VM ID | 状态 |
|---|---|---|
| tomcat-migr | tomcat-v... | 正在生成迁移计划 |

在迁移过程中，我们可以使用以下命令检查进度，该命令将输出大量调试信息：

```
migctl migration status my-migration -v
```

一旦生成了迁移计划（参见图 18.23），就可以使用 GUI 检查结果：

**图 18.23：M4A 生成的迁移计划概览**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_696_0.png)

特别是，选项菜单允许我们编辑生成的迁移计划（参见图 18.24）：

**图 18.24：查看并编辑生成的迁移计划**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_696_1.png)

让我们通过编辑迁移计划来看看生成了什么（参见图 18.25）：

**图 18.25：编辑生成的迁移计划**

### 编辑迁移计划

```
```yaml
apiVersion: anthos-migrate.cloud.google.com/v1beta2
kind: GenerateArtifactsFlow
metadata:
  creationTimestamp: null
  labels:
    migration: kuwflyjr
    migration-name: tomcat-migr
    migration-namespace: v2k-system
  name: generate-artifacts-flow-f3d851ee-2b23-48ad-88a8-6f21356d8918
  namespace: v2k-system
  ownerReferences:
  - apiVersion: anthos-migrate.cloud.google.com/v1beta2
    blockOwnerDeletion: true
    controller: true
    kind: Migration
    name: tomcat-migr
    uid: f3d851ee-2b23-48ad-88a8-6f21356d8918
spec:
# Review and set which artifacts to generate.
```
```

通常，您不需要更改迁移计划。但是，如果您需要剔除不需要的虚拟机组件，或者需要添加一些附加配置，这将会很有用。检查并（如有必要）编辑迁移计划后，我们就可以开始生成工件（参见图 18.26）：

**图 18.26：使用编辑后的迁移计划生成工件**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_698_0.png)

生成迁移计划后，可以通过 Google Cloud Console 中的“工件”标签页进行检查。这些工件包括 Dockerfile、用于部署的容器映像（可直接部署）、容器映像基础层（不可运行的映像层）、部署规范 YAML、迁移计划 YAML 和工件链接 YAML（参见图 18.27）：

### 图 18.27：生成的迁移工件

![](img/fb3bebce7b5f593bca1a7f8a1371be06_698_1.png)

生成的工件存储在 Google Cloud Storage（GCS）中，基础层和映像层分别使用不同的存储桶（参见图 18.28）：

### 图 18.28：存储在 GCS 中的工件

![](img/fb3bebce7b5f593bca1a7f8a1371be06_699_0.png)

让我们查看一下包含 Dockerfile、部署规范、清单和迁移 YAML 的系统映像（参见图 18.29）：

**图 18.29：Dockerfile、deployment_spec、manifest 和迁移文件**

| 名称 | 大小 | 类型 |
|---|---|---|
| Dockerfile | 700 B | text/plain; charset=utf-8 |
| deployment_spec.yaml | 1.7 KB | text/plain; charset=utf-8 |
| manifest.yaml | 735 B | text/plain; charset=utf-8 |
| migration.yaml | 3.1 KB | text/plain; charset=utf-8 |

此外，为迁移生成的映像会自动推送到 Google Container Registry（GCR），您可以通过控制台浏览这些映像（参见图 18.30）：

**图 18.30：为迁移生成的映像并推送到 GCR**

