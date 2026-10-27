
测试新度量：如图 8.20 所示，对列应用筛选器以减少可用值或剩余值。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_390_0.png)

图 8.20：Regions Remaining 和 Subcategories Remaining 度量

SalesTerritoryRegion 和 Product Subcategory 列分别受到应用于 SalesTerritoryGroup 和 Product Category 列的筛选器的影响。鉴于 15 个产品子类别的文本字符串字符数较多，因此只显示剩余数量。

请注意，当筛选器直接应用在给定列上时，这些剩余表达式将返回与第一个示例相同的字符串值。例如，如果在销售区域地区切片器上选择了 Northwest 和 Northeast 地区，那么它们将是仅剩的两个地区。这两个示例中应用的技术可以进一步混合或丰富，例如将数值度量与分隔字符串返回的每个维度值关联起来。

## 另请参阅

- DAX FILTERS 函数：https://bit.ly/3aAVTFB
- DAX CONCATENATEX 函数：https://bit.ly/3sVmvHQ
- DAX ISCROSSFILTERED 函数：https://bit.ly/3vo13gr

## 避免用户手动点击

在 BI 项目中，常见场景是需要定制一组核心报表和仪表板，以更好地匹配更大团队或组织职能中特定角色或用户的职责与分析需求。理想情况下，特定业务用户应能立即、默认地看到相关数据，而无需与内容交互或修改内容，例如应用筛选器选择。

Power BI 丰富的自助服务功能有时可以满足这一需求（或部分满足），而其他由 IT 支持的、针对特定角色的报表和仪表板则是另一个实际可行的选择。

第三种选择（也是本技巧的主题）是将基于用户的动态筛选逻辑嵌入 DAX 度量中。通过这种方法，可以在组织的多个层级中利用单个或少数几个报表和仪表板，从而避免开发新报表的需要。

## 准备工作

要准备此技巧，请按照以下步骤操作：

- 1. 在本地打开 Power BI Desktop 文件，然后通过单击“开始”选项卡功能区中的“转换数据”来访问 Power Query 编辑器。
- 2. 创建一个名为 AdWorksDW 的查询，类似于以下内容：
- 3. 禁用 AdWorksDW 查询的加载，并将其放入 Data Sources 查询组。
- 4. 右键单击 AdWorksDW 查询，然后选择“引用”（Reference）。
- 5. 将查询重命名为 Internet Sales，然后选择 FactInternetSales 表。
- 6. 对名为 Sales Territories（指向 DimSalesTerritory 表）的查询和名为 Customers（指向 DimCustomers 表）的查询重复步骤 4 和 5。
- 7. 编辑 Customers 查询以展开 DimGeography 列：

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

```
let
    Source = AdWorksDW,
    dbo_DimCustomer = Source[{"Schema"="dbo",Item="DimCustomer"}][Data],
    ExpandGeography = Table.ExpandRecordColumn(
        dbo_DimCustomer, "DimGeography",
        {"City", "StateProvinceName", "EnglishCountryRegionName", "PostalCode"},
        {"City", "StateProvinceName", "EnglishCountryRegionName", "PostalCode"})
in
    ExpandGeography
```

## 在 Power BI 中实现基于用户的动态可见性

- 8. 将 **Internet Sales** 查询移动到名为 **Facts** 的查询组，并将 **Sales Territories** 和 **Customers** 查询移动到名为 **Dimensions** 的查询组。
- 9. 在 **Dimensions** 查询组中创建一个名为 **Sales User Roles** 的 **Enter Data** 查询，包含以下信息：

| EmailAddress | SalesTerritoryKey | UserRole |
|---|---|---|
| david8@adventure-works.com | 2 | Country |
| linda3@adventure-works.com | 4 | Region |
| pamela0@adventure-works.com | 6 | Group |

- 10. 在 **Other Queries** 组中，创建一个名为 **Calculations** 的 **Blank** 查询。
- 11. 在 **Home** 选项卡的功能区中选择 **Close & Apply**（关闭并应用）。
- 12. 在 **Sales Territories** 表和 **Internet Sales** 表的 **SalesTerritoryKey** 列之间创建关系。
- 13. 在 **Sales Territories** 表和 **Sales User Roles** 表的 **SalesTerritoryKey** 列之间创建关系。
- 14. 在 **Internet Sales** 表和 **Customers** 表的 **CustomerKey** 列之间创建关系。
- 15. 将 **Customers** 表中 **Postal Code** 列的 **Data category**（数据类别）设置为 **Postal Code**。
- 16. 保存你的工作。

现在的模型应与 *图 8.21* 中所示相同。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_392_0.png)

图 8.21：此技巧的模型

## 如何避免用户手动点击

要实现此技巧，请使用以下步骤：

- 1. 在 **Calculations** 表中创建以下 DAX 度量：

```
User Principal Name = "david8@adventure-works.com"
```

```
User Sales Territory Key = LOOKUPVALUE('Sales User Roles'[SalesTerritoryKey],
    'Sales User Roles'[EmailAddress],[User Principal Name])
```

```
User Sales Role =
    VAR RoleLookup = LOOKUPVALUE('Sales User Roles'[UserRole],
        'Sales User Roles'[EmailAddress],[User Principal Name])
RETURN IF(ISBLANK(RoleLookup),"Role Not Found",RoleLookup)
```

```
User Sales Group = IF([User Sales Role] = "Role Not Found",
    "Role Not Found", LOOKUPVALUE('Sales Territories'[SalesTerritoryGroup],
        'Sales Territories'[SalesTerritoryKey],[User Sales Territory Key]))
```

```
User Sales Country = IF([User Sales Role] = "Role Not Found",
    "Role Not Found", LOOKUPVALUE('Sales Territories'[SalesTerritoryCountry],
        'Sales Territories'[SalesTerritoryKey],[User Sales Territory Key]))
```

```
User Sales Region = IF([User Sales Role] = "Role Not Found",
    "Role Not Found", LOOKUPVALUE('Sales Territories'[SalesTerritoryRegion],
        'Sales Territories'[SalesTerritoryKey],[User Sales Territory Key]))
```

- 2. 在 **Calculations** 表中创建两个 DAX 度量，以检测 **Sales Territories** 表的筛选上下文，并相应地筛选 **Internet Sales** 表：

```
Sales Territory Detection = IF(ISCROSSFILTERED('Sales Territories'),
    "Filters Applied", "No Filters")
```

```
Internet Sales Amount = SWITCH(TRUE(),
    [Sales Territory Detection] = "Filters Applied" ||
    [User Sales Role] = "Role Not Found",SUM('Internet Sales'[SalesAmount]),
    [User Sales Role] = "Group",
```

```
CALCULATE(SUM('Internet Sales'[SalesAmount]),
FILTER(ALL('Sales Territories'),
'Sales Territories'[SalesTerritoryGroup] = [User Sales Group])),
[User Sales Role] = "Country",
CALCULATE(SUM('Internet Sales'[SalesAmount]),
FILTER(ALL('Sales Territories'),
'Sales Territories'[SalesTerritoryCountry] =
[User Sales Country])),
[User Sales Role] = "Region",
CALCULATE(SUM('Internet Sales'[SalesAmount]),
FILTER(ALL('Sales Territories'),
'Sales Territories'[SalesTerritoryRegion] =
[User Sales Region])) )
```

- 3. 创建一个包含新度量和 Sales Territory 表的标准报表，以测试或演示该逻辑。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_394_0.png)

图 8.22：用户 Pamela（北美地区的组角色成员）的默认筛选上下文

- 4. 更改 User Principal Name 度量以测试三种不同的角色。

## 工作原理

在生产环境中，User Principal Name 度量不会硬编码为电子邮件地址，而应使用以下公式：

```
User Principal Name = USERPRINCIPALNAME()
```

此外，生产级实现会将 **Sales User Roles** 表存储在 SQL 数据仓库中，而不是使用 **Enter Data** 查询。

此技巧中的度量相互依赖，最终形成 **Internet Sales Amount** 度量。这些度量的目的是为度量（**Internet Sales Amount**）提供特定的默认筛选上下文。例如，国家/地区角色成员在打开报表时，默认应看到按自己所在国家/地区筛选的数据。但是，条件逻辑也可以允许用户应用自己的筛选选择，从而提供额外的可见性选项。

**Sales Territory Detection** 度量是此方法的基础。如果 **Sales Territories** 表上的任何列都未被筛选（例如通过切片器），则销售度量应默认为基于用户的特定筛选上下文。如果已对 **Sales Territories** 列进行了筛选选择，则度量应使用这些选择。

如果当前用户在 **Sales User Roles** 表中找不到，**Internet Sales Amount** 度量默认会对 **Internet Sales** 表中的 **SalesAmount** 列求和。如果确定了用户的角色，并且未对 **Sales Territories** 表应用筛选器，则会应用用户角色级别（**Group**、**Country** 或 **Region**）的筛选器以及特定维度成员。

## 更多内容

示例报表左上角卡片视觉对象中显示的五个 DAX 度量定义如下：

```
User Role Status = "My Sales Role: " & [User Sales Role]
```

```
Sales Group Status = "My Group: " & [User Sales Group]
```

```
Sales Country Status = "My Country: " & [User Sales Country]
```

```
Sales Region Status = "My Region: " & [User Sales Region]
```

```
Filter Status = VAR Prefix = "Sales Territory Filter Status: "
RETURN IF([Sales Territory Detection] = "No Filters",
    Prefix & "Role Based",Prefix & "User Defined")
```

## 另请参阅

- DAX LOOKUPVALUE 函数：https://bit.ly/3gdjZKl

