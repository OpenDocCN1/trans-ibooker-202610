

如果我们查看控制台内部，可以验证它是否已正确创建：

## Overview

| Instance ID | Instance name | Type | Storage |
|---|---|---|---|
| instance001 | myfirstinstance | Production | SSD |

### cluster001

| CPU utilization | Rows | Throughput |
|---|---|---|
| Average: — %<br>Hottest node: — % | Read: —/s<br>Write: —/s | Read: —/s<br>Write: —/s |

| Cluster ID | Zone | Nodes | Storage utilization |
|---|---|---|---|
| cluster001 | europe-west2-a | 3 | — / — |

Figure 16.12 – Overview

> **Important Note**
> 你也可以使用 `gcloud` 命令来创建 Bigtable 实例。`gcloud` 让我们可以创建生产或开发环境，其中开发环境性能有限且没有 SLA。如果需要创建开发 Bigtable 实例，则使用 `gcloud`。

## Exploring Cloud Deployment Manager

在本节中，我们将查看最有用的管理工具之一 – Google Cloud Deployment Manager。了解 Cloud Deployment Manager 的概念对于考试成功非常重要。

Deployment Manager 允许我们自动化创建和配置各种 GCP 资源的过程。我们可以使用 YAML 文件以声明式格式编写应用程序所需的所有资源，这可确保部署可重复且结果一致。YAML 是一种人类可以轻松阅读的数据序列化语言。总的来说，Deployment Manager 可以被称为基础设施即代码（IaC）。你可能对 Ansible 或 Terraform 等工具有所了解，并且会认识到你可以通过缩进来嵌套元素，而不是使用某些其他语言中使用的任何大括号或方括号。

> **Exam Tip**
> Deployment Manager 是一个非常大的主题，并且有深入课程可供你获得进一步的知识。对于考试，我们将解释成功所需的核心知识。

Deployment Manager 的一些组件是基础的。配置文件包含我们想要部署的所有资源及其属性。

配置文件以资源字符串开头，后面跟着一系列条目，每个条目具有以下字段：

- **Name:** 我们正在部署的资源名称；例如，`vm001`。
- **Type:** 这指定了此资源的基类型。有许多可用的资源类型，它们与我们希望部署的资源类型相关。例如，新 VM 实例的资源类型将为 `compute.v1.instance`。请注意，`v1` 代表生产就绪。
- **Properties:** 这指定了创建资源所需的属性。例如，如果我们希望从 GUI 部署新的 VM 实例，我们需要填充诸如操作系统类型、磁盘类型以及映像和网络接口等必填字段。这些将在配置文件的 `properties` 部分中填充。

> **Important Note**
> 资源类型的完整列表可以在此处找到：https://cloud.google.com/deployment-manager/docs/configuration/supported-resource-types。

让我们看看如何构建我们的第一个示例配置文件。在本章的 Using Google Cloud Shell 部分中，我们提到了 Cloud Shell 编辑器。让我们打开它并创建一个新文件。为此，只需右键单击你的用户 ID 并选择 New File：

![](img/de2c97ab017f36b3750a5bade111edd7_526_0.png)

Figure 16.13 – Cloud Shell – New File

为文件命名 – 例如，vm001.yaml – 并单击 OK 以创建它。我们现在准备编辑。在此示例中，代码将执行以下操作：

- 在 europe-west4-a 区域中创建一台 VM。
- 将其大小设置为 g1-small 机器类型。
- 使用 centos-6 作为我们的映像。
- 创建一个持久启动磁盘。
- 将 VM 添加到默认网络。
- 添加访问配置作为外部 NAT。

完成的配置文件如下所示：

```
resources:
- name: vm001
  type: compute.v1.instance
  properties:
```

```
zone: europe-west4-a
  machineType: https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/europe-west4-a/machineTypes/g1-small
  disks:
  - deviceName: boot
    type: PERSISTENT
    boot: true
    autoDelete: true
    initializeParams:
      sourceImage: https://www.googleapis.com/compute/v1/projects/centos-cloud/global/images/centos-stream-8-v20210721
  networkInterfaces:
  - network: https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/global/networks/default
  accessConfigs:
  - name: External NAT
    type: ONE_TO_ONE_NAT
```

现在，让我们保存它并使用此配置文件部署一台 VM。同样，我们将使用 Cloud Shell 工具使用 Deployment Manager 命令行语法执行此操作：

```
gcloud deployment-manager deployments create <deployment name> --config <config file>
```

这里，<deployment name> 是你整体部署的名称。请记住，VM 实例名称在我们的配置文件中指定，而 <config file> 是我们之前创建的配置文件的名称。让我们看一个使用我们的 vm001.yaml 配置文件创建名为 deployment01 的新部署的示例：

```
gcloud deployment-manager deployments create deployment01 --config vm001.yaml
```

如果我们浏览到 GCP 控制台中的 **Deployment Manager**，我们可以确认它已部署：

![](img/de2c97ab017f36b3750a5bade111edd7_528_0.png)

Figure 16.14 – Deployment Manager

我们还可以浏览到 GCP 控制台中的 Compute Engine 并确认 VM 实例存在：

![](img/de2c97ab017f36b3750a5bade111edd7_528_1.png)

Figure 16.15 – VM instance

因此，虽然我们花时间创建了一个配置文件，但我们只部署了一个 VM 实例。Deployment Manager 允许我们部署完整的环境。为此，我们应该使用模板。模板允许我们将配置文件的一部分抽象为可在部署之间重用的独立构建块。

模板是与配置文件分开的文件。它也是用不同的语言编写的 – Jinja 或 Python – 并作为类型导入并在配置文件中使用。

模板允许我们从配置文件传递变量，这意味着它们可以保持相当静态，我们只需从配置文件传递编辑后的变量。我们需要修改配置文件以反映我们想要调用模板文件。这可以通过在配置文件的顶部添加模板的路径来完成。我们还可以编辑配置文件以反映我们想要传递的变量。

让我们看一个示例。我们希望为创建 debian-9 VM 保留一个静态模板，但我们希望传递主机名、区域和机器类型等变量。让我们按如下方式创建一个新的配置 YAML 文件。让我们将其命名为 createdbytemplate.yaml。请注意，我们已将 vm-template.jinja 添加为类型，并且我们的变量已设置为属性：

```
imports:

  - path: vm-template.jinja


resources:

  - name: newinstance

    type: vm-template.jinja

    properties:

      name: createdbytemplate

      zone: europe-west4-a
      type: g1-small
```

现在，我们需要配置我们的模板文件，其中将包含要传递的变量的值。请注意，我们的主机名、机器类型和区域在以下格式的括号内设置：

```
{{ properties["variable name"] }}
```

这里，variable name 是从配置文件传递的字符串。以下代码是我们的 Jinja 模板文件：

```
resources:
- name: {{ properties["name"] }}
  type: compute.v1.instance
  properties:
    zone: {{ properties["zone"] }}
    machineType: zones/{{ properties["zone"] }}/machineTypes/{{ properties["type"] }}
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/debian-cloud/global/images/family/debian-9
    networkInterfaces:
    - network: global/networks/default
```

我们可以保存它并将其命名为 vm-template.jinja。现在，我们可以使用我们的 gcloud 命令调用我们的 createdbytemplate.yaml 文件：

```
gcloud deployment-manager deployments create templatedeployment --config createdbytemplate.yaml
```

现在，我们可以检查控制台以验证我们是否有一个名为 createdbytemplate 的新 VM：

