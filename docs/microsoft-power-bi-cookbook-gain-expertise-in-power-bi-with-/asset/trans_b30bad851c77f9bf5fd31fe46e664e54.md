

```
{"{UnitPrice_Sum}", each List.Sum([UnitPrice]), type number}),
ChangeType = Table.TransformColumnTypes(Group,
    {{"SalesAmount_Sum", Currency.Type},
    {"UnitPrice_Sum", Currency.Type}} )
in
ChangeType
```

+   6. 将 Internet Sales 查询移动到名为 Facts 的查询组中，将 Customers 和 Dates 查询移动到名为 Dimensions 的查询组中，并将 Sales Agg 查询移动到 Other Queries 查询组。
+   7. 在“开始”选项卡的功能区上单击“关闭并应用”以将表加载到数据模型中。
+   8. 在模型中创建以下单向、一对多关系，如图 3.50 所示。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_189_0.png)

请注意，您可能需要为数据源使用数据库凭据而不是 Windows 凭据。在测试中，我们在尝试使用 Sales Agg 表和维度表创建关系时，使用 Windows 凭据遇到了问题。

# 如何操作

要实现此配方，请使用以下步骤：

+   1. 在 **Model** 视图中，右键单击 **Sales Agg** 表并选择 **Manage aggregations**。
+   2. 为 **Sales Agg** 表创建下面所示的聚合，然后单击 **Apply all** 按钮。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_190_0.png)

图 3.51：Manage aggregations 对话框

+   3. 在 **Model** 视图中，选择 **Customers** 表，并在 **Properties** 窗格的 **Advanced** 区域中，将 **Storage mode** 设置为 **Dual**。
+   4. 对 **Dates** 表重复 *step 3*。
+   5. 从 SQL Server Management Studio 的 **Tools** 菜单打开 **SQL Server Profiler** 并运行跟踪。
+   6. 创建一个 **Table** 可视化，使用 **Customers** 表中的 **EnglishEducation** 列和 **Internet Sales** 表中的 **SalesAmount** 列。请注意，跟踪中没有任何内容被记录。
+   7. 创建一个 **Clustered column chart**，以 **Dates** 表中的 **CalendarYear** 列作为 **Axis**，以 **Internet Sales** 表中的 **SalesAmount** 列作为 **Values**。同样，没有任何内容被记录。

# 其工作原理

核心事实表 **Internet Sales** 必须设置为 **DirectQuery** 存储模式至关重要，因为聚合表仅适用于设置为 **DirectQuery** 存储模式的明细表。聚合表本身 **Sales Agg** 必须设置为 **Import** 存储模式。与 **Internet Sales** 表和 **Sales Agg** 表都有关系的两个维度表 **Customers** 和 **Dates**，应设置为 **Dual** 存储模式，以便这些表能够在任一模式下最佳运行。配置表的聚合会自动在数据模型中隐藏该表，因为聚合表必须被隐藏。

维度表与聚合表之间必须存在关系。这是因为聚合表在可能的情况下用作详细 **DirectQuery** 模式事实表的替代，因此聚合表应与事实表对维度表具有相同的关系。

在配置聚合时，还必须牢记列数据类型。这是因为 **Manage aggregations** 对话框强制执行 **Detail Column** 必须与 **Aggregation Column** 具有相同数据类型的规则。唯一例外是 **Count** 和 **Count table rows** 的 **Summarizations**。**Count** 和 **Count table rows** 需要整数聚合列，但不要求 **Detail Column** 的数据类型匹配。

其他限制包括链式聚合、由 DAX USERELATIONSHIP 函数支持的非活动关系的使用，以及使用相同的 **Summarization**、**Detail Table** 和 **Detail Column** 的重复聚合。均不支持这些场景。

# 还有更多...

为了正常运行，RLS 表达式应同时过滤明细表和聚合表。事实上，一旦为表设置了聚合，仅过滤聚合表而不过滤关联 **Detail Table** 的 RLS 表达式将不被 **Manage roles** 对话框允许。

**Manage roles** 对话框包括设置 **Precedence** 的功能。这意味着您可以拥有不同粒度级别的多个聚合表，并设置这些聚合表的 **Precedence**，以便它们可以被单个子查询考虑。例如，如果 **Products** 表包含在此模型中，您可能会为 **Product Category** 创建一个聚合表，并为 **Product Subcategory** 创建一个单独的聚合表。您将把 **Product Category** 聚合表的粒度设置为 10，并将 **Product Subcategory** 聚合表的粒度设置为 0。子查询将首先考虑粒度最高的聚合表，如果该粒度级别不足，则会考虑粒度较低的其他聚合表。

# Authoring Power  
BI Reports

Power BI 报表是仪表板、数据探索以及 Power BI 中内容协作和分发的构建基块。Power BI Desktop 提供了丰富的数据可视化功能和选项，使能够在各种设备上构建高度针对性、用户友好的报表成为可能。由于每个 Power BI Desktop 报表可以包含多个页面，每个页面包含多个视觉对象，因此单个 Power BI 报表可以支持多种用例、受众和业务需求。例如，KPI 视觉对象可以固定到 Power BI 服务中的仪表板，而报表页面可以支持详细、特定领域的分析。这些功能可直接与竞争对手分析平台的可视化产品竞争，并且可以通过自定义视觉对象和报表主题进一步扩展。

报表设计中 Power BI 可视化功能的选型和配置对于从 *Chapter 2, Accessing and Retrieving Data* 和 *Chapter 3, Building a Power BI Data Model* 中涵盖的数据检索和建模过程中获取价值至关重要。在本章中，我们开发并描述了最基本的报表可视化和设计概念。此外，还提供了指导，以增强和控制用户在 Windows 和移动设备上与 Power BI 报表交互和使用时的用户体验。

在本章中，我们将介绍以下配方：

+   - Building Rich and Intuitive Reports
+   - Filtering at Different Scopes
+   - Integrating Card Visualizations
+   - Using Slicers
+   - Controlling Visual Interactions

# Getting ready

利益相关者矩阵可以帮助围绕访问模型和报表的不同各方需求来构建报表设计规划过程。利益相关者或业务部门，例如 Merchandising，出现在列轴上，并取代了第 3 章《Building a Power BI Data Model》中数据仓库总线矩阵中使用的规范化维度（即 Product 或 Vendor）。

|  | Stakeholders |  |  |  |  |  |  |  |
|---|---|---|---|---|---|---|---|---|
| BUSINESS PROCESSES | Executive | Sales | Finance | Marketing | Merchandising | eCommerce | Customer Service | Supply Chain |
| Internet Sales | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |  |  |
| Internet Sales Plan | ✓ | ✓ |  | ✓ |  | ✓ | ✓ | ✓ |
| Reseller Sales | ✓ | ✓ | ✓ |  | ✓ |  |  |  |
| General Ledger | ✓ |  | ✓ |  |  |  |  |  |
| Inventory |  |  | ✓ | ✓ | ✓ | ✓ |  |  |
| Customer Surveys | ✓ | ✓ | ✓ | ✓ |  | ✓ | ✓ | ✓ |
| Customer Service Calls |  |  | ✓ |  |  |  | ✓ |  |
| Shipping |  |  | ✓ |  | ✓ |  |  | ✓ |

图 4.1：利益相关者矩阵

在此示例中，数据模型包含四个突出显示的事实表：Internet Sales、Internet Sales Plan、Reseller Sales 和 General Ledger。当给定业务职能内可能存在多个具有各自独特需求和用例的利益相关者时，这些利益相关者可以作为列添加到利益相关者矩阵中。在 Power BI 中，有许多选项可使用相同的数据模型和底层检索和数据架构来满足不同利益相关者的独特需求。

# How to Build Reports

虽然报表开发应是一个敏捷过程，在报表设计和开发过程中应定期征求利益相关者的反馈，但在报表开始前进行扎实的规划和设计过程对于了解其范围和要求非常有益。单个报表不应试图解决不相关的业务问题或满足高度多样化用户（如企业财务分析师或商店经理）的需求。多范围报表可能导致复杂的用户体验和报表级定制，这难以维护和扩展。报表规划和设计过程应回答以下五个问题：

+   1. 谁将访问此报表？
+   2. 此报表的首要优先事项在业务问题方面是什么？
+   3. 报表将如何访问和使用？

