

2. 我们现在需要选择一个操作模式。**Native mode** 是官方的 Firestore 模式，是 Cloud Datastore 的下一个主要版本，并提供新功能，例如实时更新以及移动和网页客户端库。Datastore mode 使用传统的 Cloud Datastore 系统行为。在我们的示例中，我们将使用 **Datastore mode：**

|  | Native mode | Datastore mode |
|---|---|---|
|  | 启用 Cloud Firestore 的所有功能，支持离线和实时同步。SELECT NATIVE MODE | 在 Cloud Firestore 强大的存储层之上利用 Cloud Datastore 的系统行为。SELECT DATASTORE MODE |
| API | Firestore | Datastore |
| Scalability | 自动扩展到数百万并发客户端 | 自动扩展到每秒数百万次写入 |
| App engine support | 不支持 App Engine 标准 Python 2.7 和 PHP 5.5 运行时 | 所有运行时 |
| Max writes per second | 10,000 | 无限制 |
| Real-time updates | ✓ | ✕ |
| Mobile/web client libraries with offline data persistence | ✓ | ✕ |
| Query consistency | 强一致性 | 强一致性 |
| Data model | Documents / collections | Entities / kinds |
| Web console | Google Cloud Platform 和 Firebase 中的 Firestore 页面 | Google Cloud Platform 中的 Datastore 页面 |

Figure 11.16 – Choosing Datastore mode

3. 选择 Datastore 数据库的位置并点击 CREATE DATABASE：

Choose carefully: your location selection is permanent and will also apply to this project's App Engine app

Figure 11.17 – Selecting a location

4. 数据库准备就绪后，我们将被引导至 Entities 菜单。点击 CREATE ENTITY 来创建我们的第一个 Datastore 实体：

Figure 11.18 – Creating an entity

我们现在可以为实体指定一个命名空间、一个 Kind（在 SQL 术语中即表）以及一个键标识符，后者将自动生成。让我们将命名空间保持为默认值并创建一个名为 exam 的新 kind：

Figure 11.19 – Create an entity

5. 点击 **ADD PROPERTY** 指定名称——在本例中为 `title`。我们还可以从多种类型中选择，但在本示例中，我们希望使用 **String**。最后，我们将为其赋值为 `Professional Cloud Architect`。点击 **DONE** 添加该属性：

# Properties

![](img/de2c97ab017f36b3750a5bade111edd7_347_0.png)

Figure 11.20 – Adding entity properties

6. 我们可以重复 *Step 5* 来添加另一个属性。这次，我们将使用 `cost` 作为名称，**Integer** 作为类型，`200` 作为值。当我们在实体中添加完所有想要的属性后，点击 **CREATE**。让我们再创建两个具有 exam kind 的新实体，分别用于 `Associate Cloud Engineer` 考试和 `Professional Cloud Developer` 考试：

![](img/de2c97ab017f36b3750a5bade111edd7_347_1.png)

Figure 11.21 – Creating an entity

7. 现在，我们可以根据 cost 或 title 过滤实体查询。让我们查找有关 Professional Cloud Developer 考试的信息：

![](img/de2c97ab017f36b3750a5bade111edd7_348_0.png)

Figure 11.22 – Filter entities

8. 我们也可以使用 QUERY BY GQL 并输入类似 SQL 的查询来获得相同的结果：

![](img/de2c97ab017f36b3750a5bade111edd7_348_1.png)

```
SELECT * from exam where Title = 'Professional Cloud Developer'
```

Figure 11.23 – Executing the query

至此，我们已经了解了创建 Datastore 数据库以及查询所创建实体的便捷性。现在，让我们继续并查看 Datastore 与 Firestore 之间的一些差异。

## Summary of Datastore mode versus Native mode

Cloud Firestore 在 Native mode 中是完整重品牌的产品。它融合了 Cloud Datastore 和 Firebase 实时数据库的优点，提供与 Datastore 相同的概念性 NoSQL 数据库，但由于 Firebase 是为低延迟移动应用而构建的，因此还包括以下新功能：

- 一个新的强一致性存储层
- 一个集合和文档数据模型
- 实时更新
- 移动和网页客户端库

Cloud Firestore 在 Datastore mode 中使用相同的 Datastore 系统行为，但将访问 Firestore 存储层，这将消除一些限制，例如以下内容：

- 不再局限于最终一致性，因为所有查询都变为强一致性
- 不再局限于 25 个实体组限制
- 不再局限于每个实体组每秒 1 次写入

## IAM

对 Google Cloud Datastore 的访问通过 IAM 保护。让我们查看预定义角色的列表及其详情：

- **Datastore owner with an AppEngine app admin**: 拥有对 Datastore mode 的完全访问权限
- **Datastore owner without an AppEngine app admin**: 拥有对 Datastore mode 的访问权限，但无法启用管理员访问、检查 Datastore mode 管理员是否已启用、禁用 Datastore mode 写入或检查 Datastore mode 写入是否已禁用
- **Datastore user**: 拥有对 Datastore mode 数据库中数据的读写访问权限。主要用于开发人员或服务账号
- **Datastore viewer**: 拥有读取所有 Datastore mode 资源 的权限
- **Datastore import-export admin**: 拥有管理导入和导出的完全访问权限
- **Datastore index admin**: 拥有管理索引定义的完全访问权限

## Quotas and limits

Google Cloud Datastore 带有预定义的配额。这些默认配额可以通过进入 **Navigation** 菜单，在 **IAM & Admin | Quotas** 部分进行更改。从此菜单中，我们可以查看当前配额并请求提高这些限制。我们建议您熟悉每个服务的限制，因为这会影响您的可扩展性。Cloud Firestore 提供免费配额，让我们零成本开始：

- 存储数据的限制为 1 GiB。
- 每日读取限制为 50,000 次。
- 每日写入限制为 20,000 次。
- 每日删除限制为 20,000 次。
- 每月网络出站限制为 10 GiB。

免费层级下有以下标准限制：

- 子集合的最大深度为 100。
- 文档的最大大小为 6 KiB。
- 文档的最大大小为 1 MiB。
- 每个数据库每秒最大写入次数为 10,000。
- 最大 API 请求大小为 10 MiB。
- 最多可将 500 次写入传递给一次提交操作或在一次事务中执行。
- 单个文档 Commit 操作或事务中最多可执行 500 个字段转换。

## Pricing

Google Cloud Firestore 的定价提供免费配额供我们开始使用，如前所述。

如果超出这些限制，将会产生费用，具体取决于数据所在位置而波动。一般定价按每 100,00 文档的读取、写入和删除计费。存储数据按每 GB 每月计费。

## Understanding Cloud SQL

从名称上看，Cloud SQL 是一个数据库服务，可轻松设置、维护和管理您在 Google Cloud 上的关系型 PostgreSQL、MySQL 或 SQL Server 数据库。

尽管我们正在配置底层实例，但它是一个完全托管的服务，能够处理多达 64 TB 的存储。Cloud SQL 数据库是关系型的，这意味着它们组织成表、行和列。作为替代方案，我们可以将 SQL Server 应用程序镜像安装到 Compute Engine 实例上，但 Cloud SQL 提供了许多由 Google 完全托管带来的好处——例如，可扩展性、修补和更新会自动应用，提供自动备份，并且开箱即用提供 HA。再次强调，虽然这些好处看起来很棒，但我们也应注意存在一些不支持的功能，并在承诺使用 Cloud SQL 之前加以考虑。

> **Exam Tip**
> Cloud SQL 可容纳多达 64 TB 的存储。如果您需要处理更大量的数据，则应考虑替代服务，例如 Cloud Spanner。

现在，让我们查看 Cloud SQL 提供的一些功能。

