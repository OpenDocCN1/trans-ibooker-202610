

还有一个最终的文件我们也应该讨论。模式文件用作指南，向用户展示如何与模板交互并使用模板。例如，让我们看一个配置文件的示例，该文件将要求将 zone 作为变量传递给模板。我们可以为用户提供有关其使用方式的信息，以及需要设置哪些变量：

```
info:
  title: VM Instance Template
  author: Cloud Architect
  description: Creates a new instance
  version: 0.1

imports:
  - path: vm-template.jinja

required:
  - zone

properties:
  zone:
    type: string
    description: zone where VM will reside
```

> **重要提示**
> Google 提供了一些很棒的 Deployment Manager 示例，我们可以将其下载并在我们的免费层环境中使用。我们建议您下载并查看一个更复杂、更进一步的 Cloud Deployment Manager 模板，网址为 https://github.com/GoogleCloudPlatform/deploymentmanager-samples。

在下一节中，我们将介绍 Cloud Deployment Manager 的替代方案：Terraform。

# Terraform

Terraform 是一个开源工具，可让我们以声明方式配置 Google Cloud 资源。我们应该注意，它并不独属于 Google Cloud。Terraform 可以部署 AWS、Azure 和 VMware 等厂商的资源，仅举几例。与 Cloud Deployment Manager 一样，Terraform 也使用 IaC 方法，并记录下我们希望环境呈现的期望状态。不应对后端进行任何修改，Terraform 应该是事实来源。这与 Deployment Manager 类似，后者创建存储已部署资源信息的清单文件。然而，Terraform 使用其结构化语言 HashiCorp Configuration Language（HCL），这是我们上一节中看到的 YAML 文件方法的替代方案。对于首次接触 IaC 的人来说，这可能会带来一点更陡峭的学习曲线。Terraform 通过提供程序与 GCP 集成，该提供程序负责理解与底层基础设施的 API 交互。Terraform 的 Google 提供程序用于配置我们的 GCP 基础设施，由 Google 的 Terraform 团队和 HashiCorp 的 Terraform 团队共同维护，尽管他们也接受社区贡献！

使用 Google Cloud Shell，Terraform 会自动进行身份验证，我们可以直接从 Terraform 的 Google Provider 文档页面开始尝试。许多示例都有 Open in Google Cloud Shell 按钮，允许我们启动一个交互式 Cloud Shell 会话，其中加载了示例并且 Terraform 已准备就绪。

现在，让我们看一个部署 Cloud Storage 存储桶的示例。我们不会详细介绍 Terraform HCL 代码，因为它超出了本书的范围，但了解其大致外观是很有帮助的。Terraform 有一些主要命令来部署资源。首先，我们必须运行 terraform init，它会查找我们需要的特定提供程序并下载它们。Terraform 的一个好处是其漂移检测，即运行下一个 Terraform 命令 terraform plan 时，会显示我们想要应用的配置与已部署内容之间的差异。运行 plan 时不会部署任何内容。要执行计划，我们必须运行 terraform apply 命令。

对于我们的示例，我们将查看如何创建 Google Storage 存储桶。让我们通过从 https://github.com/terraform-google-modules/terraform-google-cloud-storage 仓库执行 git clone 命令来克隆一些 GitHub 上的 Terraform 模块代码。

接下来，我们必须更改目录到 /terraform-google-cloud-storage/examples/simple_bucket 并运行 terraform init，它会查找我们需要的特定提供程序并下载它们：

```
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/google versions matching ">= 3.43.0, < 4.0.0"...
- Installing hashicorp/google v3.74.0...
- Installed hashicorp/google v3.74.0 (self-signed, key ID 34365D9472D7468F)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has created a lock file .terraform.lock.hcl to record the provider selections it made above. Include this file in your version control repository so that Terraform can guarantee to make the same selections by default when you run "terraform init" in the future.

Terraform has been successfully initialized!
```

图 16.22 – terraform init 输出

当我们运行 terraform plan 命令时，会提示我们输入存储桶名称和项目的 ID。它不会部署任何内容。在此示例中，我们可以看到我们正在创建名为 cloudarchitectupdate001 的新多区域存储桶，位置在 EU：

```
Terraform will perform the following actions:

# google_storage_bucket.bucket will be created
+ resource "google_storage_bucket" "bucket" {
    + bucket_policy_only        = (known after apply)
    + force_destroy             = false
    + id                        = (known after apply)
    + location                  = "EU"
    + name                      = "cloudarchitectupdate001"
    + project                   = "primeval-nectar-318916"
    + self_link                 = (known after apply)
    + storage_class             = "STANDARD"
    + uniform_bucket_level_access = true
    + url                       = (known after apply)
    + versioning {
      + enabled = true
    }
  }

Plan: 1 to add, 0 to change, 0 to destroy.
```

图 16.23 – terraform plan 输出

要执行计划，我们必须运行 terraform apply 命令。输出将类似于以下内容：

```
google_storage_bucket.bucket: Creating...
google_storage_bucket.bucket: Creation complete after 1s [id=cloudarchitectupdate001]
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

图 16.24 – terraform apply 输出

最后，我们可以浏览到控制台中的 Cloud Storage 并确认我们的新存储桶已在正确位置创建：

![](img/de2c97ab017f36b3750a5bade111edd7_542_0.png)

图 16.25 – Google Cloud Storage 存储桶

当然，这是一个非常简单的示例，但您可以使用多个 VM、GKE 集群、负载均衡器等部署更复杂的资源集。如果您对 Terraform 模板的语法复杂性感到畏惧，请不要担心！Google 提供了一套现成的模板供您使用和学习。

# Cloud Foundation Toolkit

Cloud Foundation Toolkit（CFT）（https://cloud.google.com/foundation-toolkit）为 Deployment Manager 和 Terraform 提供即用型模板。之前的 Terraform 示例中用于创建 Cloud Storage 存储桶的代码直接来自这些现成模板。它们的设计目的是让我们能够在 Cloud 平台内快速构建可重复的企业级基础。这些模板是节省时间的工具，我们可以在其基础上进行构建以进行定制，从而为业务增添价值。它们还能让我们专注于应用程序，而不是繁琐的基础设施任务。

在下一节中，我们将介绍定价计算器。

# Pricing Calculator

Google 在计算在 GCP 运行应用程序的成本方面提供了帮助。无需在每个服务的文档中检查价格，您可以使用 GCP 定价计算器。该计算器可通过 https://cloud.google.com/products/calculator 在线获取。它允许您选择用于应用程序的服务和配置。您还可以添加估计的存储和网络需求。总价将为您计算：

![](img/de2c97ab017f36b3750a5bade111edd7_543_0.png)

图 16.26 – 定价计算器

在这里，您可以看到 GKE 集群上五个节点、500 GB 磁盘空间的成本示例。要添加估计的出站网络流量，只需选择 NETWORKING 图标并定义流量的去向即可：

![](img/de2c97ab017f36b3750a5bade111edd7_544_0.png)

图 16.27 – 定价计算器 – 网络

让我们再看一个示例，这次是针对 Cloud Storage。在此示例中，我们向您展示在新加坡使用 1 TB Cloud Storage 的费用。我们估计每月将有 500,000 次 A 类操作和 500,000 次 B 类操作。请注意，当您在计算器中添加操作时，它期望的数字单位为百万。在我们的示例中，我们输入了 0.5 百万：

