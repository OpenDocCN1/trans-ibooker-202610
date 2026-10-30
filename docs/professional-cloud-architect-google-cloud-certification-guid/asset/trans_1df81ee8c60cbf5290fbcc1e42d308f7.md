

3. 可选地，我们可以向需要以此服务账号执行操作的<|message|>用户或组授予访问权限：

### 授予用户访问此服务账号的权限（可选）

授予需要以此服务账号执行操作的<|message|>用户或组访问权限。了解更多

![](img/de2c97ab017f36b3750a5bade111edd7_482_0.png)

图 15.14 – 分配角色

如果我们重新审视创建虚拟机实例的过程，我们可以在 Identity and API access 选项下分配此服务账号：

![](img/de2c97ab017f36b3750a5bade111edd7_482_1.png)

图 15.15 – Identity and API access

这将允许服务账号承担服务账号身份以进行 API 请求的身份验证。正如我们之前提到的，我们可以看到我们有将我们创建的账号分配给服务账号的选项。

如果选择默认服务账号，你会注意到有选择访问范围的选项。访问范围决定了 API 调用对服务的访问级别。最佳实践是为每个服务单独选择每个 API 的访问权限以提高安全性。以下是当前可用的完整 API 列表：

Access scopes
Allow default access
Allow full access to all Cloud APIs
Set access for each API

BigQuery
None

Bigtable Admin
None

Bigtable Data
None

Cloud Datastore
None

Cloud Debugger
None

Cloud Pub/Sub
None

Cloud Source Repositories
None

Cloud SQL
None

Compute Engine
None

Service Control
Enabled

Service Management
Read Only

Stackdriver Logging API
Write Only

Stackdriver Monitoring API
Write Only

Stackdriver Trace
Write Only

Storage
Read Only

Task queue
None

User info
None

图 15.16 – API 列表

服务账号使用服务账号密钥进行管理。有两种类型的服务账号密钥：

- **Google Cloud-managed**：Google Cloud-managed 密钥由 Compute Engine 和 App Engine 等服务使用，无法下载。
- **User-managed**：User-managed 密钥由 GCP 用户创建，可以下载和管理。你对 user-managed 密钥负全责。

我们将在本章的 *Encryption* 部分讨论密钥管理。

在结束对服务账号的讨论之前，最后一点需要注意的是，服务账号仍应遵循我们关于 IAM 所学到的相同原则。服务账号应仅获得该服务所需的最小权限集。

> **Exam Tip**
> 对于考试而言，这一主题是关键，以便你理解身份、角色和资源。我们在前几章中简要涉及了 IAM，但现在，我们希望更详细地研究它。在阅读进一步内容之前，需要注意的一些原则将在本章中更详细地解释如下：IAM 角色是可以分配给用户、组或服务账号的权限组；有不同类型的角色；我们应遵循最小权限原则；并且在可能的情况下，使用组来控制访问，而不是授予单个用户的访问权限。

在本节中，我们介绍了服务账号。我们解释了它们是一种特殊的账号类型，可以将用户从直接参与中移除，并且它们是 Google IAM 的重要组成部分。在下一节中，我们将查看访问控制列表，它是 IAM 的替代方案。

## Cloud Storage 访问管理

除了使用 IAM 权限来限制对 Cloud Storage 的访问外，还可以使用 **Access Control Lists (ACLs)** 来保护它。ACLs 应在你希望对对象而不是整个存储桶设置权限时使用，例如，要访问特定存储桶中的单个对象。这是因为 Cloud IAM 会将权限应用于存储桶中的所有对象。了解这一点至关重要，因为从可管理性角度来看，单独管理这些对象并不理想，如果存储桶中所有对象的权限有共同之处，则应使用 IAM 来控制访问。

ACL 由定义可以执行的操作的权限以及定义谁可以执行该操作的范围组成。当对存储桶发出请求时，ACL 将在适用时授予用户权限；否则，请求将导致 403 Forbidden 错误。存储桶中的对象只能被授予 Owner 或 Reader 权限，如以下屏幕截图所示：

### 编辑 notes.txt 权限

如果你不依赖单个对象级权限，可以开始在存储桶级别统一管理所有权限。转到存储桶的 Permissions 选项卡开始。了解更多

![](img/de2c97ab017f36b3750a5bade111edd7_485_0.png)

图 15.17 – GCP 存储桶文件权限

> **Important Note**
> 除了 ACL 之外，GCP 还提供 Storage Legacy bucket Owner、Storage Legacy bucket Reader 和 Storage Legacy bucket Writer 的传统角色。

与 IAM 一样，我们应遵循最小权限原则。如果你需要使用 ACL，则不要在不需要时应用所有者角色。你应注意确保存储桶不会不必要地公开（通过分配 allUser 角色）。为存储桶分配错误的权限是现实世界中非常典型的安全漏洞，在应用 ACL 之前应仔细考虑。

在考试前你应该了解的最后一点信息是使用签名 URL 的用法。此选项允许你授予访客访问权限，以便他们无需 Google 账号即可从存储中上传或下载。访问可以是时间限制的，并且可以为拥有该 URL 的任何人授予读取或写入访问权限。我们可以通过运行以下命令使用 gsutil 创建签名 URL：

```
gsutil signurl -d 10m Desktop/private-key.json gs://example-bucket/cat.jpeg
```

在此示例中，我们使用 private-key.json 作为我们的服务账号私钥来公开 example-bucket 中的 cat.jpeg 存储桶对象。

> Exam Tip
> 请记住，我们绝不应授予用户或服务超出所需范围的权限。始终使用最小权限原则。

接下来，我们将查看 Organization Policy Service。

## Organization Policy Service

Organization Policy Service 为我们提供对组织云资源的编程控制。这允许集中控制以配置组织资源的配置方式。此外，它有助于开发团队保持在合规边界内，并帮助项目所有者快速移动而无需担心或破坏合规性。

重要的是要强调 Organization Policies 与我们之前讨论的 IAM 不同。虽然 IAM 专注于允许管理员根据权限授权谁可以对特定资源执行操作，但 Organization Policies 专注于允许管理员对特定资源设置限制以确定它们如何配置。组织策略是一组限制，作为管理员，我们定义策略以强制对组织、文件夹或项目实施限制。

为了定义此类策略，我们会选择一种特定类型的限制，称为 **constraint**。约束应用于 Google Cloud 服务或服务列表。本质上，这是定义受控行为的蓝图。约束具有类型，要么是 list 要么是 Boolean。正如你所猜测的，list 约束将评估管理员设置的一组允许或拒绝的值。例如，这可以是可以连接到虚拟机的允许 IP 地址列表。Boolean 约束将要么强制执行要么不强制执行。

当策略设置在资源上时，该资源的所有后代默认会继承该策略。具有 **Organization Policy Administrator** 角色的用户可以在需要例外时修改此继承。

违规是指服务以违反组织策略限制的状态行事的情况。这可能发生在新策略对服务已处于的状态或操作设置限制时。

要创建 Organization Policy，请导航到 **IAM & Admin | Organization Policies**。这将列出可用的策略：

![](img/de2c97ab017f36b3750a5bade111edd7_487_0.png)

图 15.18 – Organization Policies

