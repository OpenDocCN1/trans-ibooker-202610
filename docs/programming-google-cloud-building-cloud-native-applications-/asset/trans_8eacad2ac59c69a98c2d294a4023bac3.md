

```
gcloud kms decrypt \
  --location "global" \
  --keyring "pigeonkeyring" \
  --key "pigeonkey" \
  --ciphertext-file ./pigeonsecret.txt.encrypted \
  --plaintext-file ./pigeonsecret.txt.decrypted
```

你已成功加密并解密了示例文件的内容！

## Audit Logs

框架的一个重要组成部分是日志记录。在安全上下文中，我们需要知道谁做了什么以及何时做。Google Cloud Audit Logs 记录每一次管理活动。日志存储在受保护的存储区域中，从而形成不可变的日志和高度持久的审计跟踪。

Cloud Audit Logs 为每个 Google Cloud 项目、文件夹和组织维护三种审计日志：

### Admin Activity

Admin Activity 审计日志包含修改云资源配置或元数据的 API 调用或其他管理操作的日志条目。

### Data Access

Data Access 审计日志包含读取资源配置或元数据的 API 调用，它还捕获用户驱动的 API 调用，这些调用会创建、修改或读取用户提供的资源数据。

### System Event

System Event 审计日志包含修改资源配置的管理操作的日志条目。

## Identity Access Management

Identity Access Management（IAM）是一个框架，用于实施确保用户和服务对资源具有适当访问权限的策略。Google Cloud 为我们提供了一个服务来管理这些 Identity Access Management，名为 Cloud IAM，名称简单明了。Cloud IAM 有三个主要组件：

### *Member*

成员可以是 Google 账号用户、服务账号、Google 群组，或可以访问资源的 G Suite 或 Cloud Identity 域。

### *Role*

角色是一组权限，权限决定了在资源上允许的操作。

### *Policy*

策略将成员绑定到角色。

让我们谈谈权限，以便你对 IAM 为什么重要有一个初步的了解。（我们将在后续章节详细讨论框架的安全性）。在 Google Cloud IAM 世界中，权限以 **service.resource.verb** 的形式表示。当我们使用最小权限构建应用程序时，这可能会变得非常复杂和令人困惑。为了简化分配权限的过程，我们使用角色的概念，角色是一组权限。Google Cloud 为用户提供了很大的灵活性，你可以使用自定义角色或预定义角色。为了再次简化权限管理流程，我们尽量使用 Google Cloud 为我们提供的预定义角色。例如，预定义角色 Pub/Sub Publisher（roles/pubsub.publisher）仅提供向 Pub/Sub 主题发布消息的访问权限。

除了 IAM，关于用户身份验证的内容将在未来的章节中介绍，在那里我们将利用 IAM、用户身份验证、日志和加密来构建安全的云原生应用程序。

## Summary

到目前为止，你应该对用例、我们的框架以及我们如何利用云原生方法来满足业务需求有了一个良好的理解。我们在本章中涵盖了许多主题，我保证随着每一章的展开会变得更容易，因为每一章都会在前一章的基础上构建。

我还想指出，本书中使用的用例是为了将学习应用到实际应用中。然而，你可以将所学内容应用于任何云原生应用程序，因为所有应用程序都需要本章中讨论的类似方法。随着我们继续推进，每个章节末尾都会放置一个云原生检查点，以验证我们是否满足要求。

## Chapter 2. Getting Data into Google Cloud

### A NOTE FOR EARLY RELEASE READERS

使用 Early Release 电子书，你可以在这些标题的正式发布之前很久就获得书籍的早期形式——作者在撰写时的原始未编辑内容——从而利用这些技术。

这是最终书籍的第 2 章。请注意，GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何意见，或者如果你注意到本章中缺少材料，请联系编辑 mcronin@oreilly.com。

本章旨在关注将数据导入 Google Cloud 以供我们的云原生应用程序使用。然而，在我们应用程序的上下文中介绍如何将数据导入 Google Cloud 将对每个人都有益。我们的數據，如你从用例中回忆的那样，将在 Google Cloud 外部，是我们呼叫中心原始的音频文件。你需要在接近实时地处理这些音频文件，以便我们的用户可以访问从我们的管道收集的指标。Google Cloud 提供了以下存储选项：

- Zonal standard persistent disk and zonal SSD persistent disk
- Regional standard persistent disk and regional SSD persistent disk
- Local SSD for high-performance local block storage
- Cloud Storage buckets: Object storage.
- Filestore: High-performance file storage

存储选择有几种，但你会为存储音频文件选择哪一种？让我们回顾一下需求：

- 我们的呼叫中心每天处理 5,000 个电话
- 每个电话平均长 30 分钟
- 每个文件平均大小为 57 MB
  - 5,000 * 57 = 285 GB

你每月大约需要 8.5 TB 的存储空间，每年大约需要 102 TB。你可以排除持久磁盘，因为它们有会限制可扩展性的限制，参见表 2-1。

| Storage Type | Zonal standard persistent disks | Regional standard persistent disks | Zonal SSD persistent disks | Regional SSD persistent disks | Local SSDs | Cloud Storage buckets |
|---|---|---|---|---|---|---|
| Minimum capacity per disk | 10 GB | 200 GB | 10 GB | 10 GB | 375 GB | n/a |
| Maximum capacity per disk | 64 TB | 64 TB | 64 TB | 64 TB | 375 GB | n/a |
| Capacity increment | 1 GB | 1 GB | 1 GB | 1 GB | 375 GB | n/a |
| Maximum capacity per instance | 257 TB | 257 TB | 257 TB | 257 TB | 3 TB | Almost infinite |

从列表中可以看出，我们对持久磁盘的限制相当大。即使我们选择使用区域标准持久磁盘并创建一个实例来存储音频文件，我们的成本也会相当高。同时我们开始偏离云原生方法，因为现在我们需要管理计算实例、附加的持久磁盘，并且我们需要一种方法来监控存储中的事件，例如文件创建以触发云函数。在撰写本文时，成本分析如下：

- Zonal Standard Persistent Disks
  - Disk One 64 TB: Total Estimated Cost: USD $2,621.44 per 1 month
  - Disk Two 64 TB: Total Estimated Cost: USD $2,621.44 per 1 month
  - Approximate Total per Month for 128 TB, USD $5,242 per 1 month
- Cloud Storage Bucket
  - Approximate Total per Month for 128 TB, USD $2,621.44 per 1 month

此成本分析未考虑云存储每月的存储操作或挂载标准持久磁盘的计算实例成本。这些存储数字设定为一年，但你需要考虑第二年、第三年等会发生什么。我们的持久磁盘解决方案变得复杂且昂贵，因此最明智的做法是选择云存储桶。

你可能会想知道 Cloud Storage 中的存储操作。操作是更改存储桶和对象或检索其信息的动作。Cloud Storage 操作分为三类：Class A、Class B 和免费。表 2-2 引自 Google Cloud Storage 文档 https://cloud.google.com/storage/pricing#price-tables；我建议访问文档以确认自本书出版以来 Class A、B 和免费操作是否发生变化：

| API or Feature | Class A Operations | Class B Operations | Free Operations |
|---|---|---|---|
| JSON API | storage.*.insert storage.*.patch storage.*.update storage.*.setIamPolicy storage.buckets.list storage.buckets.lockRetentionPolicy storage.notifications.delete storage.objects.compose storage.objects.copy storage.objects.list storage.objects.rewrite storage.objects.watchAll storage.projects.hmacKeys.create storage.projects.hmacKeys.list storage.*AccessControls.delete | storage.*.get storage.*.getIamPolicy storage.*.testIamPermissions storage.*AccessControls.list storage.notifications.list Each object notification | storage.channels.stop storage.buckets.delete storage.objects.delete storage.projects.hmacKeys.delete |
| XML API | GET Service GET Bucket PUT POST | GET Bucket GET Object HEAD | DELETE |

让我们退一步，介绍文件存储、块存储和对象存储。

## File Storage

文件存储以分层结构存储数据。数据保存在文件和文件夹中，无论是从提供视图的系统还是从检索数据视图的系统呈现，都是一致的。文件存储传统上通过 Network File System 协议或 Server Message Block 协议访问。

## Block Storage

块存储以存储区域网络或本地附加磁盘的形式呈现。块存储通常安装在操作系统上，就像块设备是本地附加存储一样。一旦本地挂载，用户就可以创建文件系统供操作系统使用：应用程序或数据库。

## Object-Based Storage

对象存储以编程方式访问，而块存储只能通过操作系统访问。对象存储也是平面的；它不具有文件系统的分层结构。Cloud Storage 没有文件夹的概念，文件夹是一个恰好在其名称中包含 "/" 字符的对象。每个对象包含数据以及相关的元数据，这些元数据分布在多个硬件系统上以提供可扩展性和可靠性。

## Options for Getting Data to Cloud Storage

> NOTE
> 你现在或稍后可以使用 Qwiklabs 进行练习。本书的 Qwiklabs Quest 即将推出。

