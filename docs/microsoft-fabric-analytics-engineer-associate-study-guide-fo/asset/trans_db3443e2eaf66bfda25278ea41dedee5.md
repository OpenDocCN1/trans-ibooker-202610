

在浏览器中。相反，你可以点击顶部的编辑按钮，这样你就可以编辑报表并使其符合你对可视化、交互以及其他业务需求的需求。

要向现有可视化添加新元素，只需从字段列表中将它们拖放到可视化中即可。要更改可视化，你可以进行对象内编辑，选择它后，你会看到屏幕右侧的属性窗格（见图 2-30）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_92_0.png)

图 2-30. Power BI 网页体验中的对象内编辑

然后，你就可以像在 Power BI Desktop 中一样编辑可视化。你可以从顶部菜单添加新的可视化，并将报表保存并分享给组织。如果你愿意，你也可以编辑报表的移动版布局，你可以在屏幕左下角找到它（见图 2-30）。默认情况下，选择的是桌面版布局，你可以选择移动版布局并设计报表以适应在手机或平板上使用时的需求（见图 2-31）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_93_0.png)

图 2-31. Power BI 网页体验中的移动版布局

你也可以使用桌面版布局中现有的可视化，并将它们拖放到移动版布局的画布上。当你需要在画布上更改可视化时，只需在画布上选择该项并使用屏幕右侧的属性窗格即可。

## KQL 查询到 Power BI

在处理 KQL 查询到 Power BI 时，我们需要使用一种特殊的数据获取实现方法。使用 Power BI Desktop 应用程序和 Power BI 网页体验的方法略有不同，我们将在下一节中介绍这两种方法。

## Power BI Desktop 中的 KQL 查询

在 Power BI Desktop 应用程序中，使用获取数据选项并在连接器列表中找到 Azure Data Explorer（Kusto）连接器（见图 2-32）。

## 获取数据

搜索

- 全部
- 文件
- 数据库
- Microsoft Fabric
- Power Platform
- Azure
- 在线服务
- 其他

## Azure

- Azure SQL 数据库
- Azure Synapse Analytics SQL
- Azure Analysis Services 数据库
- Azure Database for PostgreSQL
- Azure Blob 存储
- Azure Table 存储
- Azure Cosmos DB v1
- Azure Data Explorer（Kusto）
- Azure Data Lake Storage Gen2
- Azure HDInsight（HDFS）
- Azure HDInsight Spark
- HDInsight 交互式查询
- Azure 成本管理
- Azure 设备注册表（ADR）（Beta）
- Azure 资源图
- Azure Cosmos DB v2

已认证连接器

模板应用

连接

取消

图 2-32. Power BI Desktop 中的 Azure Data Explorer（Kusto）连接器

当你选择此连接器时，它会弹出一个对话框，你可以在其中输入集群名称和数据库名称，并且你有选项可以粘贴 KQL 查询并将其用作分析工作的源（见图 2-33）。

## Azure Data Explorer（Kusto）

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_95_0.png)

图 2-33. Azure Data Explorer（Kusto）连接对话框

接下来，向下滚动并选择 Direct Query 数据连接模式（见图 2-34）。

你需要选择 Direct Query 模式，因为 KQL 引擎在处理来自数据库的数据查询方面比 Power BI 处理相同数据量的能力要好得多。KQL 引擎专为非常大的数据集而构建，可以处理你想要的几乎任何查询，并且连接器在*查询折叠*（将我们在 Power BI 中所做的工作翻译成实际的原生 KQL 查询）方面做得非常好，供 KQL 引擎执行。因此，在此场景中请不要犹豫选择 Direct Query 选项。

现在你已完全配置了连接器，可以点击确定开始像平常一样在 Power BI 中使用数据。

## Azure Data Explorer（Kusto）

高级选项（可选）

限制查询结果记录数（可选）

示例：500000

限制查询结果数据大小（字节）（可选）

示例：67108864

禁用结果集截断（可选）

示例：false

附加设置语句（以分号分隔...）（可选）

示例：set query_datascope=hotcache;

数据连接模式 ①

导入

DirectQuery

确定

取消

图 2-34. 为 KQL 查询连接选择 Direct Query 作为数据连接模式

## Power BI 网页体验中的 KQL 查询

正如我们在 Power BI Desktop 方法中看到的，如果我们记得选择 Direct Query 选项，使用 KQL 查询就很容易。但在 Power BI 网页体验中，我们需要使用稍微不同的方法。

在这里，我们需要在 eventhouse 和 KQL 数据库级别开始。在 KQL 数据库中，我们有一个标准附加的 *KQL 查询集*，它只是一种将 KQL 查询作为数据库一部分保存的笔记本。

图 2-35 显示了 KQL 数据库和附加的 KQL 查询集，其中创建了一个 KQL 脚本。这是一个非常基础的 KQL 脚本，它获取一个表并根据列过滤行。

当 KQL 查询完成并且我们对结果集满意时，我们可以继续将其添加到新的 Power BI 报表中。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_98_0.png)

图 2-35. 带有简单 KQL 脚本的 KQL 查询集

# 注意

请注意图 2-35 中顶部菜单中倒数第二个菜单项是创建 Power BI 报表。只有在查询集中选择了特定的 KQL 查询时，此项才会被启用，如果没有选择 KQL 查询，我们就无法点击此菜单项。请注意，在图中，KQL 查询已被选中，因此我们可以点击创建 Power BI 报表并将数据获取到报表中。

当我们点击创建 Power BI 报表时，会出现一个屏幕，右侧显示 Kusto 查询结果和我们可以操作的列。然后我们可以开始创建和设计我们的报表（见图 2-36）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_99_0.png)

图 2-36. Power BI 网页体验中的 KQL 查询

在图中，我们添加了一些简单的可视化，并且可以继续保存报表并与组织分享，正如我们在本章前面解释的那样。

到目前为止，我们已经涵盖了将数据获取到 Power BI 并将数据摄取到 Fabric 的所有方面。但关于我们应该选择哪种服务的问题呢：湖仓、仓库还是事件中心？这也是 DP-600 考试的一部分，因此在下一节中，我们将深入探讨如何在三个选项中进行选择。

## 选择湖仓、仓库或事件中心

Microsoft Fabric 有三个用于存储分析数据的主要服务：湖仓、仓库和事件中心。但选择适合任务的服务可能很困难，因为它们都不是一刀切的解决方案。因此，在这里，我们将尝试帮助你了解不同的服务以及如何为你的使用场景选择合适的服务。

湖仓和仓库服务都建立在存储在 OneLake 中的 Delta Parquet 文件格式方法之上。尽管底层引擎不同，但它们在 OneLake 中存储数据时共享一些相似的功能。

事件中心也可以将数据存储在 OneLake 服务中，但它以选择性镜像选项的形式直接在 KQL 数据库或 KQL 数据库中的每个单独表中进行存储。

## 湖仓

此服务旨在支持来自不同来源的结构化、半结构化和非结构化数据。它还旨在使用笔记本方法最有效，使用 Spark、Scala、PySpark、Python SparkR 和 SparkSQL 脚本语言来摄取、操作和处理数据结构。它还在 OneLake 连接和 SQL 分析端点中提供数据。然而，它不支持多表事务，因为仓库服务支持，并且它将数据存储在引擎中按数据库和表分组的文件夹和文件中。

> 提示
> 你也可以使用 Data Factory 管道将数据摄取到湖仓，但这不在 DP-600 考试范围内。

## 仓库

此服务也在存储中支持半结构化和结构化数据，并且它使用 Transact-SQL（T-

