

- **Cloud Armor**: https://cloud.google.com/armor/docs/
- **Google Cloud Security Foundations Guide**: https://services.google.com/fh/files/misc/google-cloud-security-foundations-guide.pdf
- **Best Practices Guides**: https://cloud.google.com/security/best-practices#section-1

## 16
Google Cloud 管理选项

本章描述如何利用各种可用的管理选项，以便您可以轻松管理 **Google Cloud Platform (GCP)** 服务。在之前的章节中，我们已经涉及了可用于管理 Google 服务的命令行工具。在本章中，我们将更详细地查看这些工具，其中包括 `gcloud`、`cbt`、`bq` 和 `gsutil`。

> **考试提示**
> 您必须了解用于管理特定服务的最高效命令行工具。复习并理解本章中的每个命令行工具，并确保您可以快速将其映射到 GCP 服务。例如，如果我们被要求使用命令行工具创建一个新的存储桶，我们应该立即知道需要使用 `gsutil`。
> 
> 我们还建议您更深入地查看 `gsutil` 命令行并理解它们，以便您可以设置生命周期策略。

随着本章对每个管理选项的介绍，您可以预期了解如何访问它并从示例中获得实践知识。您还可以预期看到可在现实世界中使用的基于代码的命令，但对于考试成功也很重要需要理解。我们将首先查看使用 **应用程序编程接口 (APIs)**，然后再查看剩余的工具。

在本章中，我们将介绍您可以使用的各种管理选项。具体来说，我们将涵盖以下主题：

- 使用 API 管理资源
- 使用 Google Cloud Shell
- GCP **软件开发工具包 (SDK)**
- 探索 Cloud Deployment Manager
- 定价计算器
- 其他需要考虑的事项

## 使用 API 管理资源

API 使用现在非常普遍。API 是应用程序的访问点，为开发人员在与之通信的方式上提供灵活性，极大地提高了他们的效率。

> **重要说明**
> GCP 为其服务的 API 提供了完整的文档。我们建议您查看 https://cloud.google.com/apis/ 并点击您希望了解更多信息的特定服务。

API 使用不同的身份验证服务，具体取决于 API 调用来自何处以及它们请求的资源：

- **API 密钥**：这些是可以用于 API 调用不需要访问用户数据时的加密字符串。这些非常适合让开发人员快速上手。密钥在 GCP 控制台下的 **APIs & Services | Credentials** 中创建。然后这些密钥在 API 请求中使用。
- **OAuth 客户端 ID**：这些基于范围，意味着不同的权限将授予不同的 ID。如果开发人员需要请求用户数据，则使用此方法。
- **服务账号**：这些属于 **虚拟机 (VM)** 实例的应用程序。请参阅 *第 15 章，安全与合规*，以获取有关服务账号的更深入信息。

让我们看一个使用 API 的示例。如前所述，我们可以浏览到 GCP 控制台中的 **API & Services**，然后选择 **ENABLE APIS AND SERVICES**。如果我们搜索 `Compute Engine`，如以下屏幕截图所示，我们可以选择 **TRY THIS API**：

![](img/de2c97ab017f36b3750a5bade111edd7_509_0.png)

图 16.1 – 启用 API

这将带我们进入 **Compute Engine API** 文档页面。这是 GCP 提供的一项不错的功能，可帮助我们在其所有服务中使用 API 调用。如果可以滚动浏览文档，我们将看到与 GCE 相关的所有 API 调用的列表。让我们寻找一种可用于创建新实例的方法；即在 `v1.instances.insert` 列表下：

Compute Engine > Documentation > Reference

### 方法：instances.insert

在指定的项目中使用请求中包含的数据创建实例资源。

图 16.2 – API 示例

如果我们点击此方法，它将带我们进入一个页面，允许我们通过 API 请求 VM 实例。我们首先需要注意的是右侧的菜单，在那里我们可以选择 OAuth 2.0 作为我们的身份验证方法。这允许我们使用登录 GCP 控制台的相同用户授权 API 调用。有些必填信息需要填写，用红色标出：

- **project**：这是您的 GCP 项目的 ID。
- **zone**：这是您希望 VM 驻留的区域。

以下屏幕截图显示了这些字段的示例填充：

![](img/de2c97ab017f36b3750a5bade111edd7_510_0.png)

图 16.3 – 试用此 API

我们还需要用有关请求的信息填充请求正文。让我们查看以下示例中所需的内容。我们需要提供有关机器类型、机器名称以及磁盘和映像类型的信息：

```
{
  "machineType": "zones/us-central1-f/machineTypes/n1-standard-1",
  "name": "test001",
  "disks": [
    {
      "initializeParams": {
        "sourceImage": "projects/debian-cloud/global/images/family/debian-9"
      },
      "boot": true,
```

```
"interface": "SCSI"
}
],
"networkInterfaces": [
{
"network": "global/networks/default"
}
]
}
```

经过初步验证后，我们可以通过点击 **Authorize and execute** 执行请求。响应代码 **200 OK** 意味着我们的请求已成功执行：

![](img/de2c97ab017f36b3750a5bade111edd7_511_0.png)

图 16.4 – API 返回代码

仔细查看，我们可以看到有关用户、URI 和区域的信息：

```
{
"id": "5737119379050389182",
"name": "operation-1625514064727-5c6657c67cea7-42560a30-2bf3bace",
"zone": "https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/us-central1-f",
"operationType": "insert",
"targetLink": "https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/us-central1-f/instances/test001",
"targetId": "6541634553944923838",
"status": "PENDING",
"user": "gcparchitectbook@gmail.com",
"progress": 0,
"insertTime": "2021-07-05T12:41:06.131-07:00",
```

```
"selfLink": "https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/us-central1-f/operations/operation-1625514064727-5c6657c67cea7-42560a30-2bf3bace",
"kind": "compute#operation"
}
```

现在，让我们通过创建一个新磁盘来看另一个示例。我们可以简单地搜索 v1.instances.insert 方法。同样，我们需要填充项目和区域。请求正文也是必需的：

```
{
  "name": "newdisk001",
  "sizeGb": 10
}
```

点击 Authorize and execute 后，我们再次可以看到响应：

```
{
  "id": "4514886327875854282",
  "name": "operation-1625514277056-5c665890fb0fa-1bd73387-5f597370",
  "zone": "https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/us-central1-f",
  "operationType": "insert",
  "targetLink": "https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/us-central1-f/disks/newdisk001",
  "targetId": "3511243588531838922",
  "status": "RUNNING",
  "user": "gcparchitectbook@gmail.com",
  "progress": 0,
  "insertTime": "2021-07-05T12:44:37.706-07:00",
  "startTime": "2021-07-05T12:44:37.713-07:00",
  "selfLink": "https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/us-central1-f/operations/operation-1625514277056-5c665890fb0fa-1bd73387-5f597370",
  "kind": "compute#operation"
}
```

我们建议您探索 GCP API 的 Explorer 页面，以查看核心 GCP 服务中有哪些 API 可用。在接下来的部分中，我们将查看可供我们使用的更多命令行工具。

## 使用 Google Cloud Shell

GCP 套件中的主要管理工具是 Google Cloud Shell。这是一个免费的 g1-small Google Compute Engine (GCE) 实例，提供命令行访问，以便您可以通过 Linux shell 管理 GCP 基础设施。这里有趣的是，它可以直接从您的 GCP 控制台访问，只需点击右上角的 shell 访问按钮即可：

![](img/de2c97ab017f36b3750a5bade111edd7_513_0.png)

图 16.5 – 启动 Google Cloud Shell

激活后，控制台屏幕将分为两部分，Google Cloud Shell 将在浏览器中可见：

![](img/de2c97ab017f36b3750a5bade111edd7_513_1.png)

图 16.6 – Google Cloud Shell

不需要额外的授权步骤，因此我们可以安全地管理项目和资源，而无需安装任何其他管理工具集。此外，本章中将介绍的 SDK 命令行作为标准打包。此外，Google Cloud Shell 还预装了管理工具，如 MySQL 客户端、Docker 和 kubectl。最后，您的开发人员还可以访问不同的语言，包括 Go、Python、Node.js 和 Ruby。有关已安装功能的完整最新列表，我们建议您访问 https://cloud.google.com/shell/docs/features#tools。

Google Cloud Shell 配备 5 GB 的持久磁盘存储，挂载到您的 $HOME 目录。这意味着您可以在会话之间安全地存储脚本或用户配置并安装任何所需的软件包。请注意，$HOME 目录之外的任何保存内容将在会话终止时丢失；即在空闲一小时后。

