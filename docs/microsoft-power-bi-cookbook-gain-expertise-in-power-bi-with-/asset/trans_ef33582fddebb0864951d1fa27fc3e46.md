

11. 在仪表板功能区中，选择**编辑**，然后选择**添加磁贴**。
12. 在**添加磁贴**窗格中，从**实时数据**区域选择**自定义流数据**。
13. 单击**下一步**按钮。
14. 在**你的数据集**下，选择**流传感器数据**，然后单击**下一步**。
15. 对于**可视化类型**，选择**折线图**。
16. 对于**轴**，使用 **timestamp**，对于**值**，使用 **ambient_temperature**。
17. 单击**下一步**按钮。
18. 为该磁贴输入**标题**名称**温度**，并单击**应用**按钮。
19. 向下滚动仪表板，找到**温度**磁贴。

## 工作原理

实际上，Power BI 中有三种类型的流数据集：

- 推送（Push）
- 流式（Streaming）
- PubNub

推送数据集由外部程序将数据推送到 Power BI；数据作为数据集存储在 Power BI 中。推送数据集将信息永久存储在 Power BI 中，之后可用于创建报表。使用推送数据集时，可通过报表生成视觉对象，然后将其固定到仪表板。推送数据集限制为每秒 1 个请求、每个请求 16 MB、每小时 100 万行。

流数据集将数据推送到 Power BI，但不将数据存储在 Power BI 数据集中。因此，流数据集不能用于报表，只能用于仪表板。流数据集限制为每秒 5 个请求、每个请求 15 KB，但对每小时处理的行数没有限制。

PubNub 数据集也不在 Power BI 中存储数据，因此只能用于仪表板。对于每秒发送的请求数或每个请求发送的数据量没有限制，因为 Power BI Web 客户端使用 PubNub SDK 读取现有的 PubNub 数据流，而不是将信息推送到 Power BI。

推送数据集和流数据集可以使用 Power BI REST API 创建，也可以在使用 Azure 流分析创建新的流数据集时，通过服务中的 **API** 选项创建。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_289_0.png)

图 5.34：新建流数据集的选项

在服务中使用 API 选项创建流数据集时，配置屏幕包含“历史数据分析”开关。此开关默认关闭，将创建流数据集——将“历史数据分析”切换到“开启”将创建推送数据集。创建 PUSH/STREAMING 数据集后，可以使用 Power BI REST API 的推送数据集功能将数据推送到数据集中。

Power BI 服务中的“Azure 流”选项在撰写本文时尚未完全实现，但实现后将提供一个界面，用于将 Azure 流分析数据集成到 Power BI 中。目前，必须从 Azure 流分析激活 Power BI 集成。Power BI 是 Azure 流分析数据的输出选项之一。将 Power BI 设置为 Azure 流分析的输出，会创建一个既可以利用推送也可以利用流式功能的 Power BI 数据集。默认情况下，此数据集存储 200,000 行数据，并采用先进先出（FIFO）方式使数据过期。

## 更多内容

对于实时或近实时数据，另一个选项是使用自动页面刷新。自动页面刷新用于报表页，可设置视觉对象的刷新间隔，且仅在查看报表时生效——该选项不适用于导入数据源。自动页面刷新可在 Power BI Desktop 或 Power BI 服务的页面格式选项中配置。刷新类型可以配置为固定间隔，也可以基于变更检测配置。变更检测支持因存储模式、数据源以及使用的是专用容量（Premium）还是共享容量而异。共享容量不支持任何存储模式或数据源的变更检测。最小固定刷新间隔也因存储模式、数据源和容量类型而异。对于专用容量，最小固定刷新间隔为 1 秒，但 Azure Analysis Services 除外，其间隔为 30 分钟。对于共享容量，所有存储模式和数据源的最小固定刷新间隔均为 30 分钟。

## 定义新表

选择数据源以定义数据流的表。你可以将数据映射到标准的通用数据模型（Common Data Model）表，也可以改为定义自定义表。了解更多

添加新表

图 5.35：定义新表

4. 在“所有类别”选项卡中，在“数据源”下选择 Excel。

| 所有类别 | 数据源 |
| --- | --- |
| 文件 | Excel |
| 数据库 | 文件 |
| Power Platform | 文件夹 |
| Azure | 文件 |
| 联机服务 | SharePoint 文件夹 |
| 其他 | 文件 |
|  | Text/CSV |
|  | 文件 |

图 5.36：数据源

5. 对于连接设置，单击“浏览 OneDrive”按钮，选择 CH5_R7_AuthoringDataflows.xlsx 文件，并单击“打开”。

6. 单击右下角的“下一步”按钮。

7. 选择 DimProduct、DimProductCategory 和 DimProductSubcategory。

搜索
显示选项
Excel [3]
DimProduct
DimProductCategory
DimProductSubcategory

图 5.37：选择数据

18. 将 **EnglishProductName** 列重命名为 **Product Name**。

19. 在**开始**选项卡的功能区中单击**合并查询**。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_293_0.png)

图 5.39：合并对话框

20. 选择 **DimProductSubcategory** 作为**用于合并的右表**，选择两个 **ProductSubcategoryKey** 列，并单击**确定**以接受默认的**左外部联接类型**。

21. 单击 DimProductSubcategory 列标题中的列展开图标，取消选择所有列，选择“Product Subcategory”和“ProductCategoryKey”列，然后取消选中“使用原始列名作为前缀”复选框；单击“确定”。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_294_0.png)

图 5.40：展开列

22. 单击“开始”选项卡功能区中的“合并查询”。

23. 选择 DimProductCategory 作为“用于合并的右表”，选择两个 ProductCategoryKey 列，并单击“确定”以接受默认的“左外部联接类型”。

24. 单击 DimProductCategory 列标题中的列展开图标，取消选择所有列，选择“Product Category”列，取消选中“使用原始列名作为前缀”复选框，并单击“确定”。

25. 选择 ProductCategoryKey 列，然后在按住 Ctrl 键的同时选择 ProductSubcategoryKey 列。

26. 右键单击 ProductSubcategoryKey 列标题，然后选择“删除列”。

27. 在“查询”窗格中右键单击 DimProductCategory 查询，然后取消选中“启用加载”。

28. 同样禁用 DimProductSubcategory 查询上的加载。

29. 在右下角，单击“保存并关闭”按钮。

30. 将数据流命名为 Products，并单击“保存”按钮。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_294_1.png)

图 5.41：保存数据流

31. 单击返回工作区，注意 **Products** 数据流已经出现。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_295_0.png)

图 5.42：Products 数据流

32. 将鼠标悬停在 **Products** 数据流上，并单击**立即刷新**图标。

## 工作原理

数据流允许在 Power BI 服务中构建 Power Query 查询。此外，与在 Power BI Desktop 中构建的 Power Query 查询不同，数据流可跨多个数据集（模型）重复使用。由于数据流和在 Power BI Desktop 中构建的 Power Query 查询使用相同的底层 M 代码技术，因此实际上可以将在 Power BI Desktop 中构建的查询轻松迁移到数据流，方法是使用**高级编辑器**功能在桌面端和数据流之间复制粘贴代码，或者在创建数据流时使用**导入模型**功能。

数据流将来自源的数据引入到存储在 Azure Data Lake Storage Gen2 中的符合通用数据模型（Common Data Model，Dataverse）的文件夹中。数据流还允许你将你自己的 Azure Data Lake Gen2 存储中的数据公开给其他 Azure 服务。创建后，数据流必须至少刷新一次，以初始化 Azure Data Lake Gen2 存储中的数据。数据流支持与数据集类似的设置，即你可以配置计划刷新并在刷新数据时使用网关。

专用容量（例如 Premium）支持额外的数据流功能，例如：

- 增强的计算引擎，使数据流能够利用 `DirectQuery`
- 刷新其他数据流中的链接表
- **自动机器学习（AutoML）**和认知服务

## 更多内容

创建后，数据流可以在 Power BI Desktop 中用作数据源。要了解其工作原理，请执行以下操作：

1. 打开新的 Power BI Desktop 文件。
2. 从**开始**选项卡的功能区中选择**获取数据**，然后选择 **Power BI 数据流**。
3. 展开创建 **Products** 数据流的工作区。
4. 展开 **Products** 数据流。
5. 选择 DimProduct 表，然后单击**加载**按钮。

