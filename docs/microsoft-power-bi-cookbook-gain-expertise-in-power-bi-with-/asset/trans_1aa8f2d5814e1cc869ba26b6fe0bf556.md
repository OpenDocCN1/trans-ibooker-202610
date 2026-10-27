

还有一个 USEROBJECTID DAX 函数，当在 Power BI 服务或 Azure Analysis Services 的上下文中使用时，它会返回当前用户来自 AAD 的对象 ID。当与本地 SQL Server Analysis Services 一起使用时，将返回用户的 Security Identifier (SID)。

## See also

- Power BI security documentation and whitepaper: http://bit.ly/22NH zRS
- DAX USERPRINCIPALNAME function: https://bit.ly/3dMDhVc
- DAX USERNAME function: https://bit.ly/2RRFuWT
- DAX USEROBJECTID function: https://bit.ly/3aCEXPo

## Defining RLS Roles and Filter Expressions

数据安全应始终是首要考虑事项，BI 团队和开发人员应努力确保用户永远不会获得超出执行合法业务功能所必需的更大访问权限或权限。

在 Power BI 项目的上下文中，“谁应该可以看到什么？”这个问题应在发布任何报表或授予数据集的读取访问权限之前得到明确定义并记录下来。此外，应有一个明确定义的流程来创建和管理安全实现中使用的安全组。

Power BI 模型的数据安全由模型内定义的安全角色组成，每个角色包含一个或多个唯一的 DAX 过滤表达式。角色及其关联的过滤表达式在 Power BI Desktop 中创建，用户或组在 Power BI 服务中映射到安全角色。单个 DAX 过滤表达式可以应用于给定安全角色中模型的每个表，用户可以选择映射到多个安全角色。应用于安全角色中表的过滤表达式也会通过模型中定义的关联来过滤模型中的其他表，就像应用于 Power BI 报表的过滤器一样，并且会应用于安全角色成员提交的所有查询。

本食谱包含配置、部署和验证 RLS 角色的端到端示例，适用于导入和 DirectQuery 数据模型。还包括有关合并安全角色表以提高更改安全要求可管理性的额外指导。

## Getting ready

为准备本食谱，请执行以下步骤：

- 1. 在本地打开一个 Power BI Desktop 文件，通过单击“开始”选项卡功能区中的“转换数据”访问 Power Query 编辑器
- 2. 创建一个名为 AdWorksDW 的查询，类似于以下内容：
  ```
  let
      Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019")
  in
      Source
  ```
- 3. 禁用 AdWorksDW 查询的加载并将其放入数据来源查询组中
- 4. 右键单击 AdWorksDW 查询并选择引用
- 5. 重命名查询为 Internet Sales 并选择 FactInternetSales 表
- 6. 重复步骤 4 和 5，创建指向 DimResellers 表的 Resellers 查询、指向 FactResellerSales 查询的 Reseller Sales 查询、指向 DimSalesTerritory 表的 Sales Territories 查询以及指向 DimDate 表的 Dates 查询
- 7. 使用以下代码创建 Customers 查询：
  ```
  let
      Source = AdWorksDW,
      dbo_DimCustomer = Source[{Schema="dbo",Item="DimCustomer"}][Data],
      DimGeography = Table.ExpandRecordColumn(
          dbo_DimCustomer, "DimGeography",
          {"DimSalesTerritory"}, {"DimSalesTerritory"}),
      SalesTerritory = Table.ExpandRecordColumn(
          DimGeography, "DimSalesTerritory",
          {"SalesTerritoryGroup"}, {"SalesTerritoryGroup"})
  in
      SalesTerritory
  ```
- 8. 使用以下代码创建 Products 查询：
  ```
  let
      Source = AdWorksDW,
      Navigation = Source[{Schema="dbo",Item="DimProduct"}][Data],
      Subcategory = Table.ExpandRecordColumn(
          Navigation, "DimProductSubcategory",
  ```
  ```
  {
      "EnglishProductSubcategoryName", "DimProductCategory'",
      {"Product Subcategory", "DimProductCategory"}
  ),
  Category =
      Table.ExpandRecordColumn(
          Subcategory, "DimProductCategory",
          {"EnglishProductCategoryName"}, {"Product Category"}
  )
  in
      Category
  ```
- 9. 将 **Internet Sales** 和 **Reseller Sales** 查询移动到 **Facts** 查询组，并将 **Sales Territories**、**Dates**、**Customers**、**Resellers** 和 **Products** 查询移动到 **Dimensions** 查询组
- 10. 在 **Other Queries** 组中，创建一个名为 **Calculations** 的 **Blank** 查询
- 11. 从“开始”选项卡的功能区中选择“关闭并应用”
- 12. 切换到“模型”视图并在 **Internet Sales** 和 **Sales Territories** 表之间创建 **SalesTerritoryKey** 列的关系
- 13. 在 **Reseller Sales** 和 **Sales Territories** 表之间创建 **SalesTerritoryKey** 列的关系
- 14. 在 **Reseller Sales** 和 **Products** 表之间创建 **ProductKey** 列的关系
- 15. 在 **Internet Sales** 和 **Products** 表之间创建 **ProductKey** 列的关系
- 16. 在 **Dates** 表的 **DateKey** 列与 **Internet Sales** 表的 **OrderDateKey** 列之间创建关系
- 17. 在 **Dates** 表的 **DateKey** 列与 **Reseller Sales** 表的 **OrderDateKey** 列之间创建关系
- 18. 在 **Internet Sales** 和 **Customers** 表之间创建 **CustomerKey** 列的关系
- 19. 在 **Reseller Sales** 和 **Resellers** 表之间创建 **ResellerKey** 列的关系
- 20. 保存你的工作

定义并记录要实现的安全角色要求以及这些角色的成员或组。使用 *第 3 章《构建 Power BI 数据模型》* 中描述的公交矩阵图（*图 3.1* 和 *图 3.2*）来帮助传达模型中当前存储的数据。鉴于数据的风险或敏感性，验证角色安全性是否确实是必需的（而不是报表或模型过滤器）。

不要将安全角色过滤器与 Power BI 中的各种其他形式的过滤器混淆，例如报表、页面和视觉级别的过滤器，以及 DAX 度量中的过滤逻辑。RLS 角色过滤器应用于安全角色成员的所有查询，有效地在查询时为给定角色生成数据模型的一个虚拟子集。鉴于将安全角色过滤器与报表查询过滤器叠加的性能影响，所有用户体验和分析过滤器都应在安全角色过滤器之外实现。安全过滤器应专门用于保护敏感数据。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_372_0.png)

图 8.4：RLS 角色和过滤表达式的模型

## How to Define RLS Roles and Filter Expressions

要实现本食谱，请使用以下步骤：

- 1. 在报表视图中，在 Calculations 表中创建以下度量：
  
  Customers Row Count = COUNTROWS('Customers')
  
  Internet Sales Row Count = COUNTROWS('Internet Sales')
  
  Products Row Count = COUNTROWS('Products')
  
  Reseller Sales Row Count = COUNTROWS('Reseller Sales')

```
Resellers Row Count = COUNTROWS('Resellers')
```

```
Sales Territories Row Count = COUNTROWS('Sales Territories')
Dates Row Count = COUNTROWS('Dates')
```

- 2. 使用上一步中创建的度量创建一个简单的表格可视化。示例显示在图 8.5 中。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_373_0.png)

图 8.5：Power BI 数据模型表格视觉中的行计数度量

- 3. 隐藏 Calculations 表中的 Calculations 列。

- 4. 单击“建模”选项卡功能区中的“管理角色”。

- 5. 单击“创建”按钮并将新角色命名为 United States Online Bike Sales。

- 6. 在 Customers、Products、Resellers 和 Sales Territories 表上分别创建以下四个表过滤 DAX 表达式：

```
[SalesTerritoryGroup] = "North America"
[Product Category] = "Bikes"
FALSE()
[SalesTerritoryCountry] = "United States"
```

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_373_1.png)

图 8.6：United States Online Bike Sales 的角色安全定义

- 7. 单击“创建”并将新角色命名为 Europe Reseller Mountain and Touring。

- 8. 在 Customers、Resellers 和 Sales Territories 表上分别创建以下三个表过滤 DAX 表达式：

```
FALSE()
[ProductLine] IN {"Mountain","Touring"}
[SalesTerritoryGroup] = "Europe"
```

- 9. 单击“保存”按钮。

