

例如，一个 Power BI 视觉对象，使用 **FactResellerSales** 表中的 **SalesAmount**（销售额）按 **DimReseller** 中的 **ResellerName**（经销商名称）分组，将生成一条 SQL 语句，该语句从两个表中选择列，并实现在模型中定义的联接。然而，由于传入的 SQL 语句将这些源视图作为派生表嵌入，关系引擎能够生成一个查询计划，该计划仅扫描支持联接和聚合所需的列。

## 更多内容...

DirectQuery 模型的性能和可扩展性主要取决于关系数据源。如果不需要近乎实时的可见性，建议使用具有引用完整性的反规范化星型架构，并且该系统应与 OLTP 工作负载隔离。此外，对于报告和分析查询，建议使用受支持的 DirectQuery 源提供的内存和列式功能。

默认情况下，DirectQuery 模型会生成外联接 SQL 查询，以确保即使没有相关的维度，度量值也能返回正确的值。但是，您可以将 DirectQuery 模型配置为发送内联接查询。这可以通过在建模视图中编辑表之间的关系，勾选**假设引用完整性**设置来完成（参见*图 2.19*）。与源系统资源一样，这是影响 DirectQuery 模型性能的首要因素之一。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_65_0.png)

当然，在启用此设置之前，您应确保源中存在引用完整性；否则，可能会返回不正确的结果。

当然，源关系模式的设计和该系统的硬件资源也会极大地影响 DirectQuery 模型的性能。

建议采用具有反规范化表的经典星型架构设计，以减少查询时所需的联接操作。使用列存储技术（例如 SQL Server 的聚集列存储索引）和表分区来优化关系事实表，也将显著有利于 DirectQuery 模型。

## 另请参阅

- Power BI Desktop DirectQuery 文档：http://bit.ly/2nUoLG
- Power BI 数据源文档提供了按支持的连接选项分类的详细数据源列表：http://bit.ly/30N5oFG

## 导入数据

导入是 Power BI Desktop 的默认数据连接模式。在 Power BI Desktop 中创建的导入模型使用与 Analysis Services 表格 2016+ 导入模型相同的内存、列式压缩存储引擎（VertiPaq）。导入模式模型支持集成不同的数据源（例如 SQL Server 和 DB2），并通过完全支持所有 DAX 函数，在开发指标和行级安全角色方面提供更大的灵活性。

然而，导入模式数据集存在一些限制。例如，Power BI Pro 许可证用户无法将大于 1GB 的 Power BI Desktop 文件发布到 Power BI 服务中的共享容量。Power BI Premium（专用、隔离的硬件）支持 10GB 及更大的数据集（启用大型数据集后，数据集大小受 Premium 容量大小或管理员设置的最大大小限制）。对于如此大的数据集，重要的是考虑采用增量刷新，只刷新和导入新的和更改的数据，而不是刷新整个数据集。

本食谱描述了使用 M 和查询编辑器为标准星型架构分析模型开发导入模式查询的过程。引入了一种暂存查询方法，作为有效增强模型维度的一种手段。此外，还包含了一些技巧，用于在刷新期间使用更少的资源，并避免因修订后的源数据而导致的刷新失败。这些方法的更多细节包含在本章的其他食谱中。

## 准备就绪

在此示例中，来自 **AdventureWorksDW2019** 数据库的 **DimProduct**、**DimProductSubcategory** 和 **DimProductCategory** 表被集成到一个导入查询中。此查询包括所有产品行，仅包含英语列，并使用用户友好的名称。源数据库中已定义了多对一关系。

要为此食谱做准备，请执行以下操作：

- 1. 打开 Power BI Desktop。
- 2. 创建一个名为 **AdWorksDW** 的导入模式数据源查询。此查询应类似于以下内容：
- 3. 将此查询隔离在名为 **Data Sources** 的查询组中。
- 4. 禁用此查询的加载。

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

有关执行这些步骤的更多详细信息，请参阅本章中的*管理查询和数据源*食谱。

## 如何导入数据

要实现此食谱，请执行以下步骤：

- 1. 右键单击 **AdWorksDW**，然后选择**引用**。这将创建一个以 **AdWorksDW** 查询作为其源的新查询。
- 2. 选择此新查询，在预览数据中，于 **Name** 列中找到 **DimProduct** 表。单击此行 **Data** 列中的 **Table** 链接。
- 3. 将此查询重命名为 **DimProduct**。
- 4. 对 **DimProductCategory** 和 **DimProductSubcategory** 表重复步骤 1 – 3。
- 5. 创建一个名为 **Staging Queries** 的新查询组。
- 6. 将 **DimProduct**、**DimProductCategory** 和 **DimProductSubcategory** 查询移动到 **Staging Queries** 组。

## 访问和检索数据

- 7. 禁用 **Staging Queries** 组中所有查询的加载。您完成的查询集应类似于*图 2.20*。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_68_0.png)

*图 2.20：暂存查询*

斜体表示这些查询不会加载到模型中。

- 8. 创建一个新的**空白查询**，并将其命名为 **Products**。
- 9. 打开 **Products** 查询的**高级编辑器**。
- 10. 在 **Products** 查询中，使用 `Table.NestedJoin` 函数联接 `DimProduct` 和 `DimProductSubcategory` 查询。这与在**主页**选项卡的功能区中选择**合并查询**选项时使用的函数相同。需要左外联接来保留所有 DimProduct 行，因为指向 DimProductCategory 的外键列包含空值。
- 11. 添加一个 `Table.ExpandColumns` 表达式，从 `DimProductSubcategory` 表中检索必要的列。**Products** 查询现在应具有以下代码：

```
```powerquery
let
    ProductSubCatJoin =
        Table.NestedJoin(
            DimProduct,"ProductSubcategoryKey",
            DimProductSubcategory,"ProductSubcategoryKey",
            "SubCatColumn",JoinKind.LeftOuter
        ),

    ProductSubCatColumns =
        Table.ExpandTableColumn(
            ProductSubCatJoin,"SubCatColumn",
            {"EnglishProductSubcategoryName","ProductCategoryKey"},
            {"Product Subcategory", "ProductCategoryKey"}
        )
in
    ProductSubCatColumns
```
```

NestedJoin 函数将联接结果以表值的形式插入到一个列（SubCatColumn）中。第二个表达式将这些表值转换为 DimProductSubcategory 查询中的必要列，并提供简单的 Product Subcategory 列名，如图 2.21 所示。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_69_0.png)

图 2.21：已添加产品子类别列

Power Query 编辑器中的查询预览将在预览数据的最右侧显示新列。

- 12. 在 ProductSubCatColumns 表达式下方添加另一个表达式，使用 Table.NestedJoin 函数将上一个表达式（产品与子类别的联接）与 DimProductCategory 查询联接起来。
- 13. 与步骤 8 一样，在一个新表达式中使用 Table.ExpandTableColumn 函数来展示所需的产品类别列。

```
),

ProductCatJoin =
    Table.NestedJoin(
        ProductSubCatColumns,"ProductCategoryKey",
        DimProductCategory,"ProductCategoryKey",
        "ProdCatColumn",JoinKind.LeftOuter
    ),

ProductCatColumns =
    Table.ExpandTableColumn(
        ProductCatJoin,"ProdCatColumn",
        {"EnglishProductCategoryName"}, {"Product Category"}
    )
in
    ProductCatColumns
```

