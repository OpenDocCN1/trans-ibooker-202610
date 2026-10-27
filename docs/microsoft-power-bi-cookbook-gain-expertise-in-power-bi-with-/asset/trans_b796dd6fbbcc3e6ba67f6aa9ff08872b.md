

## 访问和检索数据

请确保在 ProductCatColumns 表达式后添加一个逗号。此外，请确保将 `in` 关键字下方的行更改为 ProductCatColumns。

ProductCatJoin 表达式将连接的结果添加到 DimProductCategory（右表）并将其添加到新列（ProductCatColumn）。下一个表达式 ProductCatColumns 添加所需的 **产品类别** 列，并将 EnglishProductCategoryName 列修订为产品类别。在此连接操作中也需要左外部连接，因为 DimProductSubcategory 上的产品类别外键列允许空值。

14. 在 ProductCatColumns 表达式之后添加一个表达式，使用 Table.SelectColumns 函数选择加载到数据模型所需的列。

15. 此外，添加一个最终表达式，通过 Table.RenameColumns 重命名这些列，以消除对英语的引用并在单词之间提供空格。

```
),
SelectProductColumns =
    Table.SelectColumns(ProductCatColumns,
        {
            "ProductKey", "EnglishDescription",
            "EnglishProductName", "Product Subcategory",
            "Product Category"
        }
    ),
RenameProductColumns =
    Table.RenameColumns(SelectProductColumns,
        {
            {"EnglishDescription", "Product Description"},
            {"EnglishProductName", "Product Name"}
        }
    )
in
    RenameProductColumns
```

请确保在 ProductCatColumns 表达式后添加一个逗号。此外，将 `in` 关键字下方的行更改为 RenameProductColumns。

**Power Query 编辑器** 中 **产品** 查询的预览现在应与 *图 2.22* 所示相似。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_71_0.png)

图 2.22：产品查询结果

由于 ProductKey 列将在报表层中隐藏，因此无需重命名该列。实际上，产品维度将包含更多列。关闭并应用更改后，只有 Products 表将被加载到模型中。

反规范化的 Products 表现在支持 Power BI Desktop 模型中的三级层次结构，从而显著改善报表和分析。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_71_1.png)

图 2.23：产品层次结构

### 其工作原理

Table.NestedJoin 的默认连接类型是左外部连接。然而，由于支持其他连接类型（例如，内部、反向和完全外部），因此建议在表达式中明确指定此参数。Products 表示例中需要左外部连接，因为 DimProduct 和 DimProductSubcategory 上的外键列都允许空值。否则，建议出于性能原因使用 Table.NestedJoin 或 Table.Join 函数实现的内部连接。本章的 Combining and Merging Queries 配方中还涵盖了有关连接函数的更多详细信息以及作为分段查询替代方案设计内联查询的提示。

当查询通过 Table.NestedJoin 或 Table.Join 函数连接两个表时，会向第一个表添加一列，其中包含一个 Table 对象，其中包含来自第二个表的已连接行。此列必须使用 Table.ExpandTableColumn 函数进行扩展，该函数会根据连接操作生成额外的行。

一旦通过连接和列扩展操作生成了所有行，就可以使用 Table.SelectColumns 操作指定最终结果中所需的特定列；然后可以使用 Table.RenameColumns 函数按需重命名这些列。

### 还有更多...

使用导入模式，我们可以做很多事情来增强查询以帮助报表开发和显示。其中一个例子是，我们可以添加额外的列来提供报表视觉对象中属性的自动排序。具体来说，假设我们希望美国区域组织默认在可视化中彼此相邻显示。默认情况下，由于 AdventureWorksDW2019 中 DimOrganization 表中的 Organization 列是文本列，中央分部（美国的一部分）会根据文本列的默认字母排序出现在加拿大和法国之间。我们可以修改一个从 DimOrganization 表提取的简单查询，以添加一个数字排序列。要了解其工作原理，请按照以下步骤操作：

1. 使用本配方中使用的同一 Power BI 文件，打开 Power Query 编辑器，右键单击 AdWorksDW 查询并选择引用。
2. 选择 DimOrganization 表并将查询重命名为 DimOrganization。
3. 打开 DimOrganization 查询的高级编辑器窗口。
4. 向 DimOrganization 维度的导入查询添加 Table.Sort 表达式。排序的列应位于层次结构的父级或更高级别。
5. 添加一个使用 Table.AddIndexColumn 函数的表达式，该函数将根据上一步中应用的表排序添加基于顺序的整数。完成的查询应如下所示：

```
let
    Source = AdWorksDW,
    dbo_DimOrganization = Source{[Schema="dbo",Item="DimOrganization"]}[Data],
    OrgSorted = Table.Sort(
        dbo_DimOrganization,
        {
            {"ParentOrganizationKey", Order.Ascending},
            {"CurrencyKey", Order.Ascending}
        }
    ),
    OrgSortIndex = Table.AddIndexColumn(OrgSorted,"OrgSortIndex",1,1)
in
    OrgSortIndex
```

6. 最后，按住 Ctrl 键，通过单击其列标题选择 OrganizationKey、OrganizationName 和 OrgSortIndex 列。右键单击 OrgSortIndex 列并选择删除其他列。预览数据现在应显示为图 2.24 所示。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_73_0.png)

图 2.24：修改后的组织维度查询

使用此表达式，表首先按 ParentOrganizationKey 列排序，然后按 CurrencyKey 列排序。新的索引列从此排序表的第一行开始，按每行递增一个进行增长。净效果是所有美国部门都分组在表的末尾。

我们现在可以使用此新的索引列来调整 OrganizationName 列的默认字母数字排序行为。要了解其工作原理，请执行以下步骤：

1. 选择关闭并应用以退出 Power Query 编辑器以加载 DimOrganization 表。
2. 在数据视图中，选择 OrganizationName 列。
3. 从列工具选项卡中，将排序依据列下拉菜单设置为 OrgSortIndex 列。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_73_1.png)

图 2.25：数据视图中的排序依据

4. 最后，右键单击 **OrgSortIndex** 列并选择 **隐藏在报表视图中**。

使用 OrganizationName 列的视觉对象现在将按其父组织对值进行排序，使得美国组织一起出现（但不按字母顺序）。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_74_0.png)

图 2.26：组织自动排序

### 另请参阅

- Power BI 服务中的数据集模式：http://bit.ly/30P2HKF
- 导入建模的数据缩减技术：http://bit.ly/30RsMZI

### 应用多个筛选器

精确且通常复杂的筛选条件的应用一直是商业智能的核心，Power BI Desktop 在其查询、数据模型和可视化组件中支持丰富的筛选功能。在许多场景中，通过查询编辑器和 M 函数在查询级别进行筛选是最佳选择，因为这可以减少导入和 DirectQuery 数据模型的负载，并消除在多个报表或可视化中重新应用相同筛选逻辑的需要。

虽然查询编辑器图形界面可用于配置筛选条件，但本配方演示了 M 的核心筛选函数以及 M 在常见多条件筛选场景中的使用。本配方中构建的 M 表达式查询旨在突出最常见的筛选用例。

