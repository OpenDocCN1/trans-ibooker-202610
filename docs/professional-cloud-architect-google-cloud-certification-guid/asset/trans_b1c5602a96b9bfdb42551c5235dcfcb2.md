

备份是一种在损坏或数据丢失的情况下从特定时间点恢复我们数据的方式。Cloud SQL 为我们提供了将数据恢复到原始实例或不同实例的选项。这两种方法都会覆盖目标实例上的所有当前数据。

让我们看看如何从控制台恢复：

+   1. 从我们的 MySQL 部署中，我们可以点击 BACKUPS 选项卡，然后滚动到页面的底部以找到我们想要恢复的特定备份：

![](img/de2c97ab017f36b3750a5bade111edd7_365_0.png)

图 11.36 – 选择备份

+   2. 如果我们没有任何副本或已删除它们，那么我们可以继续。你会注意到我们可以直接恢复到源实例，也可以选择另一个实例：

![](img/de2c97ab017f36b3750a5bade111edd7_365_1.png)

图 11.37 – 恢复备份

在下一节中，我们将了解如何迁移数据。

## 数据库迁移服务

GCP 现在提供了一项服务，允许我们轻松地将数据库从本地、Compute Engine 或甚至其他云迁移到 Cloud SQL。从本质上讲，这就是 MySQL 和 PostgreSQL 工作负载在 Cloud SQL 中的 *lift and shift*。对于希望将现有服务迁移到公共云的组织来说，这是一个理想的初始步骤，因为它意味着需要管理的基础设施更少，同时还能享受 Google Cloud 的 HA、DR 和性能优势。

## 实例克隆

我们之前提到过我们可以克隆实例。这将创建源实例的副本，但它将完全独立。这并不意味着我们已建立集群或只读副本。一旦克隆完成，对源实例的任何更改都不会反映在新的克隆实例中，反之亦然。实例 IP 地址和副本不会复制到新的克隆实例，必须在克隆实例上重新配置。同样，从源实例获取的任何备份都不会复制到克隆实例。

这可以从我们的 GCP 控制台或使用 gcloud 命令行工具完成：

+   1. 让我们看看执行此操作时我们将使用的语法：
+   ```
+   gcloud sql instances clone <instance source name> <target instance name>
+   ```
+   在这里，`<instance source name>` 是你想要克隆的实例的名称，而 `<target instance name>` 是你希望分配给克隆实例的名称。
+   2. 让我们看看如何克隆我们的 `cloudarchitect001` 实例：
+   ```
+   gcloud sql instances clone cloudarchitect001 cloudarchitect001cloned
+   ```

我们将克隆的实例命名为 `cloudarchitect001cloned`。

## IAM

访问 Google Cloud SQL 是通过 IAM 保护的。让我们看一下预定义角色的列表及其详细信息：

+   - **Owner**: 对所有 GCP 资源具有完全访问和控制权限
+   - **Editor**: 对所有 SQL 资源具有读写访问权限
+   - **Viewer:** 对所有 Google Cloud 资源具有只读访问权限，包括 Cloud SQL 资源
+   - **Cloud SQL Admin:** 对所有 Cloud SQL 资源具有完全控制权
+   - **Cloud SQL Editor:** 有权管理特定实例；无法查看或修改权限或修改 SSL 证书的用户；无法导入数据或从备份恢复，也无法克隆、删除或提升实例；并且无法停止或启动副本或删除数据库、副本或备份
+   - **Cloud SQL Viewer:** 对所有 Cloud SQL 资源具有只读权限
+   - **Cloud SQL Client:** 可通过 App Engine 和 Cloud SQL Proxy 访问云 SQL 实例
+   - **Cloud SQL Instance User:** 允许访问 Cloud SQL 资源

## 配额和限制

Google Cloud SQL 带有预定义的配额。这些默认配额可以通过 **Navigation** 菜单下的 **IAM & Admin | Quotas** 部分进行更改。从此菜单中，我们可以查看当前配额并请求提高这些限制。我们建议你熟悉每个服务的限制，因为这可能会影响你的可扩展性。对于 Cloud SQL，我们应该注意一些重要的限制。

> **Exam Tip**
> MySQL（第二代）和 PostgreSQL 的存储限制为 10 TB。

需要了解的其他限制包括以下内容：

+   - 每个项目最多有 100 个实例的限制。
+   - 最大存储限制为 64 TB。

## 定价

Cloud SQL 的定价基于 CPU 和内存定价、存储和网络定价以及实例定价。

对于 CPU 和内存，它取决于位置、CPU 数量以及你计划使用的内存量。只读副本的收费与独立实例相同。

对于存储和网络，它取决于位置、每月每 GB 使用的存储量以及从 Cloud SQL 的网络出站流量。

实例定价也取决于实例所在的位置和共享核心机器类型。

## 总结

在本章中，我们通过查看哪些选项最适合常见需求来了解可供我们使用的存储选项。我们还了解了 Google Cloud Storage、Cloud Firestore 和 Cloud SQL。

对于 Google Cloud Storage，我们查看了用例、存储类别以及服务的一些主要功能。我们还涵盖了一些在传输数据时需要考虑的事项。很明显，Cloud Storage 提供了很多灵活性，但在我们需要存储更结构化数据时，我们需要考虑替代方案。

对于 Cloud Datastore，我们了解到该服务是一个 NoSQL 数据库，如果你的应用程序依赖于高度可用且结构化的数据，则非常理想。此外，Cloud Datastore 可以轻松地从零扩展到数 TB 的数据，并且非常适合 ACID 事务。我们还了解到它提供最终一致性或强一致性；然而，如果我们有不同的需求并且需要一个具有完整 SQL 支持的 **在线事务处理 (OLTP)** 的关系数据库，那么我们应该考虑 Cloud SQL。

对于 Cloud SQL，我们了解到我们有不同的产品——MySQL、SQL Server 和 PostgreSQL。我们回顾了如何创建和管理我们的实例，如何创建副本以及如何恢复我们的实例。

在下一章中，我们将继续通过介绍 Cloud Spanner 和 Bigtable 来了解 GCP 存储选项。

## 进一步阅读

阅读以下文章以获取有关本章所涵盖主题的更多信息：

+   - **Cloud Storage**: https://cloud.google.com/storage/docs/
+   - **Cloud Datastore**: https://cloud.google.com/datastore/docs/
+   - **Cloud SQL**: https://cloud.google.com/sql/docs/
+   - **Cloud SQL pricing:** https://cloud.google.com/sql/pricing
+   - **Importing data into Cloud SQL:** https://cloud.google.com/sql/docs/mysql/import-export/importing
+   - **Exporting data from Cloud SQL:** https://cloud.google.com/sql/docs/mysql/import-export/exporting

## 12
探索 GCP 中的存储和数据库选项 – 第 2 部分

在我们存储和数据库探索的第一部分中，我们查看了多个服务。在本章中，我们将重点关注剩余的核心数据库选项。

这些服务的概念可能听起来与我们在上一章中讨论的相似。例如，Cloud Spanner 与 Cloud SQL 相似，而 Bigtable 可能让你想到 Cloud Datastore。在本章中，我们不仅会介绍 Cloud Spanner 和 Bigtable 服务，还会向你展示它们之间的区别以及为什么你可能需要选择它们而不是其他产品。

本章涵盖以下主题：

+   - Cloud Spanner
+   - Bigtable

## Cloud Spanner

可能存在你需要水平扩展而 Cloud SQL 无法满足这些需求的情况。引入 Cloud Spanner。Cloud Spanner 是一种云原生、完全托管的解决方案，旨在将关系数据库功能，例如 ACID 事务和 SQL 查询的支持，与非关系数据库的水平扩展相结合。我们应该在需要每秒高查询量或跨多个区域交付时使用 Cloud Spanner。与大多数数据库不同，Cloud Spanner 是全球分布式的，并提供强一致的数据库服务和高性能。

> **Important Note**
> 再次强调，Cloud Spanner 是强一致的。

它还提供 >=99.999% 的可用性 SLA，当你使用多区域实例时，并且能够提供高达每秒 10,000 次读取查询或每秒 2,000 次写入查询。重要的是要理解 Cloud Spanner 类似于 SQL 并具有模式；然而，由于其高可用性，它非常适合关键任务工作负载。其主要使用案例来自金融和零售行业。

> **Exam Tip**
> 具有水平可扩展性，它还支持跨多个区域托管的应用程序。请记住，Cloud Spanner 非常适合需要强一致性的工作负载。

让我们看看 Cloud Spanner 的配置。我们必须在我们的 GCP 项目内创建一个 Cloud Spanner 实例来执行此操作。

## 实例配置

当我们创建新实例时，我们应该选择实例配置。这将决定我们的实例驻留的位置以及实例中数据库的复制。我们可以在以下之间进行选择：

+   - **Multi-regional:** 使用此实例，我们将获得 99.999% 的更高 SLA——或每年大约 5 分钟的停机时间——但成本会更高。多区域允许数据库的数据在多个区域的多个区域中复制，使我们能够从位置读取数据并具有低延迟。然而，由于副本分布在多个区域以上，我们的应用程序会看到写入延迟略有增加。

