

## 将 Power BI 与其他应用程序集成

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_613_0.png)

请注意，租户可以被视为公寓楼中的一间公寓。Microsoft Azure 中的 Power BI 服务类似于大楼，提供基础、管道和其他公共设施。然而，Power BI 服务中的每个居民都有自己的私人生活区域（租户）。

3. 登录 Microsoft Azure 门户，https://portal.azure.com。
4. 选择 **创建资源**。
5. 搜索 **Analysis Services** 并选择 **创建**。
6. 通过填写必填字段创建 Azure Analysis Services 服务器：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_613_1.png)

为了获得最小的延迟，所选位置应与 *准备工作* 中 Power BI 租户的位置相匹配。需要标准或开发层 Azure Analysis Services 实例才能从 Power BI Desktop 导入。

7. 点击 **创建** 并等待服务器在 Azure 门户中可见（通常不到一分钟）。
8. 导航到 **Analysis Services** 资源并打开该实例。
9. 在 **模型** 下，选择 **管理**。
10. 选择 **新建模型**，选择 **示例数据**，然后点击 **添加** 按钮。

一个新的名为 **adventureworks** 的模型将被添加到你的 Analysis Services 实例中。

### 如何操作...

要实现此配方，请执行以下操作：

1. 在 Analysis Services 资源打开的情况下，点击 **概述** 并获取 **服务器名称**。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_614_0.png)

2. 如果服务器上有多个模型，请确认模型名称并选择要连接到的透视（可选）。Azure 门户中也会列出 AAS 服务器上的所有模型。
3. 打开一个新的 Power BI Desktop 文件，点击 **获取数据**，并选择 **Analysis Services**。
4. 输入或粘贴服务器名称和数据库（模型名称），然后按 **确定** 按钮：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_614_1.png)

5. 使用 **Microsoft 账户** 进行身份验证。
6. 创建 Power BI 报表并将其发布到 Power BI 服务中的工作区。

正如你所见，连接到 Analysis Services 数据库非常简单直接。

### 其工作原理...

**实时连接** 是默认选项，并且这应该代表绝大多数如果不是全部连接，因为数据已经导入到（或在 SSAS DirectQuery 模型的情况下从中连接）Azure Analysis Services 数据库。导入数据到 Power BI Desktop 将需要其自己的刷新过程，但在某些罕见场景中，DAX 查询可以从 AAS 数据库中检索，然后选择性地将此数据与其他数据源在 Power BI Desktop 中合并或集成。

就像 Power BI Desktop 报表与 Power BI 服务中数据集的实时连接一样，报表作者也可以为与 Analysis Services 的实时连接创建特定于给定报表的 DAX 度量。此功能使熟悉 DAX 的报表作者能够满足报表独特的指标需求。如果相同的报表级度量在多个报表中被重复创建，负责 Analysis Services 模型的 BI/IT 团队可以考虑将此逻辑实现到模型中。

Azure Analysis Services 实例按小时计费，根据 **QPUs（查询处理单元）** 和内存计费。一个虚拟核心大约等于 20 QPUs。例如，S4 实例拥有 400 QPUs，大约有 20 个虚拟核心和 100 GB RAM：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_615_0.png)

图 13.32：Azure Analysis Services 实例定价

Azure Analysis Services 服务器可以暂停，并且在服务器暂停期间不会产生费用。此外，服务器的定价层可以上下移动服务层，例如从 S1 到 S3 或反之亦然。服务器也可以从较低的服务层升级，例如从开发版到标准版，但服务器不能从较高的服务层降级。

### 还有更多...

鉴于 Power BI 和 Analysis Services Tabular 共享相同的数据库引擎，并且由于 Azure Analysis Services 消除了通过本地数据网关在 Power BI 服务与本地服务器之间通信的查询延迟和基础设施成本，组织可以考虑将 Power BI 和 SSAS 模型迁移到 Azure Analysis Services。作为一个示例，模型的源数据如 Teradata 可以保留在本地，但模型表和表分区的计划或触发刷新过程将通过本地数据网关更新 Azure 主持的模型。除了 Azure Analysis Services 平台即服务（PaaS）提供的其他成本和灵活性优势之外，Power BI Premium 容量可以使所有业务用户访问构建在 Azure Analysis Services 模型之上的 Power BI 报表和仪表板。一旦迁移到 Azure Analysis Services，表格模型可以作为 Power BI 报表的源进行访问。

### 另请参阅

- Azure Analysis Services: https://azure.microsoft.com/en-us/services/analysis-services/
- 连接到 Azure Analysis Services 的客户端库: http://bit.ly/2vzLAv0

### 与 Power Automate 和 Power Apps 集成

Power BI 的工具和服务旨在从数据中得出意义和洞察力，并使这些洞察力可供他人访问。虽然这两种功能都很重要，但 Power BI 本身并不打算对分析可视化的数据采取行动或执行业务决策。此外，信息工作者经常与许多应用程序或服务交互，因此，为了保持生产力，需要在 Power BI 与这些应用程序之间自动化工作流并嵌入逻辑，以简化业务流程。Power Apps 和 Power Automate 都是 Microsoft 365 应用程序，是与 Power BI 一起构成业务应用程序平台的一部分，通过使业务用户能够创建自定义业务应用程序和工作流流程来满足这些需求。

在本配方中，创建了一个 Power Automate 工作流来支持 Power BI 服务中的流式数据集。具体来说，Power Automate 工作流被配置为每两分钟从本地 SQL Server 表中读取，并将此数据推送到 Power BI，以提供接近实时的可见性并支持数据驱动的警报和通知。

### 准备工作

为准备此配方，请执行以下步骤：

1. 打开 **SQL Server Management Studio (SSMS)**，连接到包含 **AdventureWorksDW2019** 数据库的本地 SQL 数据库，并熟悉 **dbo.FactSurveyResponse** 表的 **列**：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_617_0.png)

2. 确保你有一个与 Power Automate 兼容的数据网关正在运行。有关安装说明，请参阅 *第 1 章，配置 Power BI 工具*。

在本配方中，使用本地数据网关通过 Power Automate 从本地 SQL Server 数据库表支持 Power BI 流式数据集。根据先前章节，支持 Power BI 刷新过程和实时连接或 DirectQuery 模型的同一网关也可以用于 Power Apps 和 Power Automate。根据这些不同活动和应用程序产生的负载，以及基于网关资源监控，可能有必要将 Power Apps 和 Power Automate 隔离到专用的本地网关，或在未来向高可用性网关集群添加服务器。

### 如何操作...

要实现此配方，请执行以下操作：

1. 在 Power BI 服务中打开一个工作区，并在顶部菜单栏中点击 **新建** 按钮。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_618_0.png)

图 13.34：Power BI 服务中工作区的新建选项

2. 选择 **流式数据集**，点击 **API** 图标，然后点击 **下一步** 按钮。
3. 配置流式数据集以与 **FactSurveyResponse** 表的列和数据类型保持一致。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_618_1.png)

图 13.35：流式数据集配置

