
4. 在 **“开始”** 选项卡的功能区中，单击 **“关闭并应用”**

```
Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019"),
    Navigation = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
    ExpandedDimSalesTerritory = Table.ExpandRecordColumn(
        Navigation, "DimSalesTerritory",
        {"SalesTerritoryCountry"}, {"SalesTerritoryCountry"})
in
    ExpandedDimSalesTerritory
```

5. 在 **“可视化”** 窗格中，单击省略号（...），然后选择 **“获取更多视觉对象”**

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_403_0.png)

图9.5：从 AppSource 导入自定义视觉对象

6. 使用 **“搜索”** 查找经过认证的（蓝色星号/复选标记图标）**“子弹图”** 视觉对象，并单击 **“添加”** 按钮

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_403_1.png)

图9.6：从 Power BI Desktop 添加自定义视觉对象

## 如何使用自定义视觉对象和快速见解丰富内容

要实现本操作指南，请执行以下步骤：

- 1. 创建以下度量值：

```
Sales 2011 = SUMX(FILTER('Internet Sales',YEAR([OrderDate])=2011),[Sales Amount])

Sales 2012 = SUMX(FILTER('Internet Sales',YEAR([OrderDate])=2012),[Sales Amount])

Sales 2012 Improve = [Sales 2011] * .7

Sales 2012 Sat = [Sales 2011] * .8
```

子弹图还支持在格式窗格中手动输入目标值和目标值的百分比。但建议使用由 DAX 度量值驱动的数据驱动方法，因为这样可以在其他视觉对象中复用这些计算，并且在目标和阈值逻辑发生变化时，可以轻松调整多个报表。

## 更多内容

Power BI 服务中的 **“快速见解”** 功能和分析引擎现已可用于 Power BI Desktop。若要了解其工作原理，请执行以下步骤：

- 1. 使用以下代码创建 Margin % 度量值：

```
Margin % =
    VAR __Price = SUM('Internet Sales'[SalesAmount])
    VAR __Cost = SUM('Internet Sales'[TotalProductCost])
RETURN DIVIDE(__Price - __Cost,__Price,0)
```

- 2. 创建一个折线图视觉对象，将“订单日期”（层次结构，而不是列）作为轴，将 Margin % 度量值作为值。

- 3. 在图表中右键单击 2013 年的数据点，然后选择“分析”，再选择“解释下降”。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_405_0.png)

图9.8：Power BI Desktop 中的“分析”功能

## 应用高级分析和自定义视觉对象

右键单击特定数据点时，会出现 **“分析”** 选项，从而可以生成特定于所选项目的其他视觉对象，例如折线图上的日期或条形图上的维度值。生成的视觉对象随后可以添加到 Power BI Desktop 文件中，并可像所有其他视觉对象一样进行编辑。**“分析”** 选项是 Power BI Desktop 中 **“快速见解”** 功能和分析引擎的体现。

**“快速见解”** 也可以对 Power BI 服务中的整个数据集执行。若要对数据集运行快速见解，请单击该数据集的省略号，然后选择 **“获取快速见解”**。通过同一上下文菜单中的 **“查看见解”** 选项，可以访问生成的见解。每条见解都包含一个 Power BI 视觉对象、所采用的见解（算法）标题（例如离群值和相关性）以及一段简短描述。来自 **“查看见解”** 的视觉对象也可以固定到新的和现有的仪表板。

**“快速见解”** 会对数据集应用复杂的算法，包括类别离群值、相关性、时间序列中的变化点、低方差、多数、时间序列中的季节性以及时间序列中的总体趋势。见解引擎在限定的时间内呈现结果。

**“快速见解”** 视觉对象可以像其他 Power BI 报表和仪表板磁贴一样，固定到新的和现有的仪表板。此外，可以针对之前由快速见解生成的视觉对象再次执行快速见解。

通过隐藏或取消隐藏列，可以改进 **“快速见解”** 的结果。**“快速见解”** 不会搜索隐藏的列，因此隐藏（或删除）不必要的列可以让见解算法只关注重要的列。同样，可以删除或隐藏任何重复的列，从而高效利用可用于运行快速见解的时间。

## 另请参阅

- Power BI 自定义视觉对象：https://bit.ly/3xY9GAk
- 经过认证的 Power BI 视觉对象：https://bit.ly/2RETFPa
- 自动生成数据集的数据见解：https://bit.ly/33qJErT
- 优化数据以用于 Power BI 快速见解：https://bit.ly/3TrWOPz

## 使用 ArcGIS 地图创建地理空间映射

ESRI 的 ArcGIS 地图和空间分析软件是 **地理信息系统（GIS）** 市场的领导者，它内置于 Power BI Desktop 中，可从数据的空间分量中生成更深入的见解。熟悉的报表可视化字段井以及 Power BI 的交叉筛选功能，可以与 ArcGIS 的地理空间功能和数据集（如分类类型、图钉和参考图层）相结合，从而在 Power BI 解决方案中构建自定义的智能地理视觉对象。

在本操作指南中，会创建一个自定义地理列，以包含多个地理属性（即街道地址、城市和州），从而支持 ArcGIS 服务进行准确的地理编码。然后，使用 Power BI Desktop 中的 ArcGIS 视觉对象将客户地址绘制到聚类主题地图视觉对象中，并辅以图钉和信息图。

## 准备工作

要准备本操作指南，请执行以下步骤：

- 1. 在 Power BI 服务中，单击右上角的“设置”（齿轮图标），并在“常规”选项卡上启用 ArcGIS 地图。这需要具有 Power BI 服务的管理员访问权限。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_407_0.png)

图9.9：Power BI 服务中的“常规设置”对话框

- 2. 在 Power BI Desktop 中打开一个新文件。

- 3. 在“开始”选项卡的功能区中单击“转换数据”。

- 4. 在“Power Query 编辑器”中，使用以下代码创建一个名为 Customers 的新查询（编辑 Source 行）：

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV",
    "AdventureWorksDW2019"),
    dbo_DimCustomer = Source[{Schema="dbo",Item="DimCustomer"}][Data],
    ExpandGeography = Table.ExpandRecordColumn(dbo_DimCustomer,
    "DimGeography",
    {"City", "StateProvinceCode", "EnglishCountryRegionName",
    "PostalCode",
    "DimSalesTerritory"},
    {"City", "StateProvinceCode", "EnglishCountryRegionName",
    "PostalCode",
    "DimSalesTerritory"}),
    ExpandSalesTerritory = Table.ExpandRecordColumn(ExpandGeography,
    "DimSalesTerritory", {"SalesTerritoryRegion"},
```

```
{"{SalesTerritoryRegion}"}),
    DupCity = Table.DuplicateColumn(ExpandSalesTerritory, "City", "City1"),
    DupZip = Table.DuplicateColumn(DupCity, "PostalCode", "Zip"),
    Address = Table.CombineColumns(DupZip,
        {"AddressLine1", "City1", "StateProvinceCode"},
        Combiner.CombineTextByDelimiter(", ", QuoteStyle.None),"Address"),
    FullAddress = Table.CombineColumns(Address,{"Address", "Zip"},
        Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),"Full Address")
    in
    FullAddress
```

- 5. 在 **“开始”** 选项卡的功能区中单击 **“关闭并应用”**。
- 6. 在 Power BI Desktop 中，选择 **Customers** 表中的 **“城市”** 列，并分配 **“数据类别”** 为 **“城市”**。
- 7. 在 **Customers** 表中选择 **“完整地址”** 列，并分配 **“数据类别”** 为 **“地址”**。

## 如何使用 ArcGIS 地图创建地理空间映射

要实现本操作指南，请执行以下步骤：

- 1. 使用 **Customers** 表中的 **SalesTerritoryRegion** 列应用页面级筛选器，并且只选择 **“西南”**。
- 2. 在 **“可视化”** 窗格中，选择 **“ArcGIS Maps for Power BI”** 视觉对象，然后将视觉对象的大小扩展到整个页面。
- 3. 在 **Customers** 表中，将 **“完整地址”** 列添加为 **“位置”**，将 **CustomerKey** 列添加为 **“大小”**，并将 **YearlyIncome** 列添加为 **“颜色”**。
- 4. 在 **“格式”** 窗格中，展开 **“位置类型”**，然后选择 **“美国”**。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_408_0.png)

- 5. 在 ArcGIS 地图视觉对象的左上角，选择 **“展开地图工具”**。

