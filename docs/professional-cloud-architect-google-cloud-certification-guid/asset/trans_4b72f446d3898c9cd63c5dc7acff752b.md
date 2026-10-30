

现在，让我们使用 Google 的开源 Dataflow 模板之一创建一个流式管道。

这里，我们有一个 Pub/Sub 订阅，我们希望将数据写入数据仓库服务以进行分析。在本示例中，我们将使用 BigQuery 来分析我们的数据。我们将在下一节讨论此服务。我们需要 Dataflow 将来自 Pub/Sub 的 JSON 格式消息转换为 BigQuery 元素。对于此示例，我们将使用一个 Google 公共 GCP 项目，该项目已设置 Pub/Sub 主题。我们还假设我们已准备好一个 BigQuery 表以供数据写入：

+   1. 从 **Navigation** 菜单浏览到 **BIG DATA | Dataflow**：

![](img/de2c97ab017f36b3750a5bade111edd7_402_0.png)

Figure 13.13 – Browsing to Dataflow

+   2. 点击 **CREATE JOB FROM TEMPLATE**：

![](img/de2c97ab017f36b3750a5bade111edd7_402_1.png)

Figure 13.14 – CREATE JOB FROM TEMPLATE

3. 现在，我们可以为 Dataflow 作业填充所需的参数：

+   - 选择部署 Dataflow 工作器并存储元数据的位置。我们将保持默认设置。
+   - 插入 Pub/Sub 输入主题。这是一个由 Google 作为模板一部分提供的公共项目。
+   - 插入数据将被写入的 BigQuery 输出表。
+   - 插入用于存储临时文件的 Cloud Storage 存储桶：

## Create job from template

Job name *
job001
Must be unique among running jobs

Regional endpoint *
europe-west2
Choose a Dataflow regional endpoint to deploy worker instances and store job metadata. You can optionally deploy worker instances to any available Google Cloud region or zone by using the worker region or worker zone parameters. Job metadata is always stored in the Dataflow regional endpoint. Learn more

Dataflow template *
Pub/Sub Topic to BigQuery

Streaming pipeline. Ingests JSON-encoded messages from a Pub/Sub topic, transforms them using a JavaScript user-defined function (UDF), and writes them to a pre-existing BigQuery table as BigQuery elements. OPEN TUTORIAL

Figure 13.15 – Create job from template

一旦选择了 dataflow 模板，我们需要填充其他必需参数。我们需要输入一个有效的 Pub/Sub 主题。在以下示例中，我们使用的是一个由公共 GCP 管理的主题。我们还需要输入一个 BigQuery 表以输出数据，然后输入临时文件写入的位置。在我们的示例中，我们使用名为 `cloudarchitectbookupdate` 的 Cloud Storage 存储桶：

Input Pub/Sub topic *
projects/pubsub-public-data/topics/taxirides-realtime
The Pub/Sub topic to read the input from. Ex: projects/your-project-id/topics/your-topic-name

BigQuery output table *
eighth-effect-319712:taxirides.realtime
The location of the BigQuery table to write the output to. If you reuse an existing table, it will be overwritten. The table's schema must match the input JSON objects. Ex: your-project:your-dataset.your-table

Temporary location *
gs://cloudarchitectbookupdate/temp
Path and filename prefix for writing temporary files. Ex: gs://your-bucket/temp

Encryption
Google-managed encryption key
No configuration required
Customer-managed encryption key (CMEK)
Manage via Google Cloud Key Management Service

SHOW OPTIONAL PARAMETERS

RUN JOB

Figure 13.16 – Additional options

4. 一旦我们运行了作业，我们将看到流程图作为输出以查看其状态。此图表显示了我们管道的阶段。它从 Cloud Pub/Sub 主题读取 JSON 格式消息，使用 JavaScript 用户定义函数对其进行转换，然后将其写入我们的 BigQuery 表。这是将 Cloud Pub/Sub 数据快速移动到 BigQuery 的一种方法：

![](img/de2c97ab017f36b3750a5bade111edd7_405_0.png)

Figure 13.17 – Graph view

5. 如果我们进入 BigQuery 表并运行 SQL 查询，我们将看到我们正在从 Pub/Sub 主题中提取数据（请注意，您的表名称将不同）：

![](img/de2c97ab017f36b3750a5bade111edd7_406_0.png)

Figure 13.18 – Query results

当然，此查询将被改进，而不是我们选择所有内容。

需要额外注意的一点是，尽管前面的示例已从模板构建了管道作业，但这只是可用选项之一。此选项允许我们无需编码即可快速开始。但是，GCP 为我们提供了使用以下语言构建管道的选项：

+   - Java and Maven
+   - Python
+   - SQL

## IAM

对 Cloud Dataflow 的访问通过 IAM 保护。让我们看一下预定义角色的列表，以及每个角色的简短描述：

+   - **Dataflow Admin**：此角色有权创建和管理 Dataflow 作业。
+   - **Dataflow Developer**：此角色有权执行和操作 Dataflow 作业。
+   - **Dataflow Viewer**：此角色对所有 Dataflow 相关资源具有只读权限。
+   - **Dataflow Worker**：此角色为 GCE 服务账号提供执行 Dataflow 管道工作单元的权限。

## Quotas and limits

Google Cloud Dataflow 带有预定义的配额。这些默认配额可以通过 Navigation 菜单和 IAM & Admin | Quotas 进行更改。从此菜单中，我们可以查看当前配额并请求提高这些限制。我们建议您了解每个服务的限制，因为这可能会影响您的可扩展性。对于 Cloud Dataflow，我们应注意以下限制：

+   - Dataflow 使用 Compute Engine 执行管道代码，然后我们受 Compute Engine 配额的约束。
+   - 每个管道的工作器数量限制为 1,000 个。
+   - 作业创建请求的限制为 10 MB。
+   - 侧输入分片的限制为 20,000 个。

## Pricing

Dataflow 按每个作业按秒计费。价格因地区而异，具体取决于位置。

## BigQuery

BigQuery 是一种完全托管的无服务器分析服务。它可以扩展到 PB 级数据，适用于数据仓库工作负载。它是我们解决方案的分析阶段，一旦 Dataflow 处理了我们的数据，BigQuery 将通过在很短的时间内查询大量数据为我们的业务提供价值。查询以 SQL 语言执行，因此对许多人来说使用起来很容易。我们应强调 BigQuery 是企业级规模，可以极快地执行大型 SQL 查询——所有这些都不需要我们预配任何底层基础设施。

> **Exam Tip**
> BigQuery 非常适合数据仓库工作负载，因为它具有 PB 级存储容量。

## BigQuery features

在本节中，我们将查看数据集和表。这些是 BigQuery 的两个重要概念，我们需要理解它们才能在考试中取得成功。

## Datasets

数据集用于组织和控制对表和视图的访问。我们在将数据加载到 BigQuery 之前至少需要一个数据集。数据集名称在每个项目中是唯一的，我们需要指定一个位置。位置可以是区域或多区域，并且在数据集创建后我们无法更改位置。在选择位置时应考虑一些因素，具体取决于导出或摄取要求。例如，如果我们计划从 Cloud Storage 等外部数据源查询数据，我们查询的数据必须与我们的 BigQuery 数据集位于同一位置。

我们可以通过浏览汉堡菜单 | BIG DATA | BigQuery，然后 SQL Workplace 创建新数据集。最后，点击我们项目旁边的三个点并选择 Create Dataset：

![](img/de2c97ab017f36b3750a5bade111edd7_408_0.png)

Figure 13.19 – Selecting a dataset location

创建数据集时需要考虑的另一个因素是是否应用默认表过期时间。BigQuery 对数据存储和每次查询收费。我们应该考虑设置过期日期，如果我们的表是临时使用的。以下示例显示了在选择数据位置后与前一个截图相同的截图：

![](img/de2c97ab017f36b3750a5bade111edd7_409_0.png)

Figure 13.20 – Default table expiration

我们为数据集中的每个表设置了 14 天的过期值。

## Tables

一旦我们创建了数据集，我们就可以创建表并将数据加载到其中。表将包含组织成行的单个记录。每个表由一个模式定义，该模式将描述列名称和数据类型；例如，字符串或整数。模式在我们创建表时决定。我们还可以在创建表时为表设置过期日期。如果我们在创建数据集时设置了过期值，则该值将被创建表时设置的过期值覆盖。如果两者均未设置值，则表将永不过期。

分区表使管理和查询数据变得更容易。这些是被拆分为较小分区的表，这意味着它将提高查询性能并降低成本。这是因为查询读取的字节数也减少了。

