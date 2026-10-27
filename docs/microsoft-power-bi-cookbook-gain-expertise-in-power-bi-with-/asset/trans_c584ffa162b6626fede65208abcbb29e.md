

## 另请参阅

- ► Power Query 查询折叠：https://bit.ly/3jB5wrx
- ► 展开表格列时优化 Power Query：https://bit.ly/3trdKFA

## 加强数据导入和集成流程

许多 Power BI 数据集必须在没有数据仓库甚至关系型数据库源系统的情况下创建。这些数据集通常会转换和合并较少结构化和受管控的数据源，例如文本和 Excel 文件，一般需要更复杂的 M 查询来为分析准备数据。更复杂的 M 查询与这些源中周期性的结构变化和数据质量问题相结合，可能导致刷新失败以及支持数据集方面的挑战。此外，由于 M 查询有时最初完全通过 Query Editor 界面创建，所生成的实际 M 代码可能包含意外的逻辑，从而导致结果不正确并对源数据产生不必要的依赖。

本配方包含增加数据导入流程可靠性和可管理性的实用示例，包括数据源合并、错误处理和注释，以及应对缺失或更改的源列。

## 准备工作

为本配方做准备，请执行以下步骤：

- 1. 识别用于评估数据刷新可靠性的数据集，例如从逗号分隔值 CSV 文件导入信息的数据集
- 2. 在 Power BI Desktop 中打开该数据集的 PBIX 文件

本示例的目标是从包含 30 列描述客户的文本文件中检索四列。

## 如何操作...

要实施本配方，请使用以下步骤：

- 1. 使用文本/CSV 连接器连接到文件并用从参数创建的查询替换硬编码路径：

```
let Source = Csv.Document(File.Contents(CustomerTextFile),[Delimiter=" ", Columns=30, Encoding=1252, QuoteStyle=QuoteStyle.None]),
PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars=true])
in PromotedHeaders
```

## 还有更多...

除了可靠性改进之外，还可以使用 Query Editor 中的 Tools 功能区中提供的 Query Diagnostics 来分析查询性能。还有改进 Power Query 代码整体可维护性的方法。其中两种方法是注释和数据源管理。

与所有编程语言一样，注释可用于提高 M 代码的整体可维护性，如以下示例所示：

```
/* This query joins the Product query to the Product Subcategory query.
    The product subcategory column 'EnglishProductSubcategoryName' is renamed
    'Product Subcategory' */
let ProductToProductSubCatJoin =
try
// Nested outer join based on Subcategory Surrogate Key
Table.NestedJoin(Product,{"ProductSubcategoryKey"},#"Product Subcategory",{"
ProductSubcategoryKey"},"ProductSubCatColumns",JoinKind.LeftOuter) otherwise
Product,
AddProductSubCatColumn =
try
// Will return nulls if EnglishProductSubcategoryName is renamed or missing in
Product Subcategory query
Table.ExpandTableColumn(ProductToProductSubCatJoin, "ProductSubCatColumns",{"Eng
lishProductSubcategoryName"}, {"Product Subcategory"}) otherwise Product
in AddProductSubCatColumn
```

注释以多行和单行格式使用，以帮助解释逻辑。多行注释以 /* 开始并以 */ 结束，而单行注释以 // 字符开头。变量名（即 AddProductSubCatColumn）采用正确的大小写且没有空格，以避免不必要的双引号并进一步描述流程。

提高 Power Query 代码可维护性的另一种方法是进行适当的数据源管理。考虑一个包含 10 个查询的 PBIX 文件，其中三个不同的数据源硬编码到每个查询中。在此示例中，10 个查询使用三个单独的源（SQL Server、Excel 文件和 MS Access 数据库文件）。对数据源路径或连接信息进行的任何更改都需要更改多达 10 个不同的查询。要了解如何解决此问题，请考虑以下内容：

- 1. 打开 Power BI Desktop 文件并识别所有查询访问的数据源
- 2. 报告视图中 Home 选项卡功能区 Transform data 下拉菜单中的 Data source settings 对话框将显示当前文件源
- 3. 为了获得更详细的信息，请打开 Power Query Editor 并从 View 选项卡的功能区中单击 Query Dependencies

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_529_0.png)

图 11.14：10 个查询的 Query Dependencies 视图

- 4. 在查询窗口中创建以下文件夹组：**Parameters**、**Data Source Queries**、**Dimensions** 和 **Facts**
- 5. 创建六个文本参数以从三个源中抽象文件名、文件路径、服务器和数据库名称
- 6. 从引用这些参数的单个空白查询中开发三个数据源查询：
  = Sql.Database("#SQL Server AdWorks Server", "#SQL Server AdWorks DB")
  = "#MS Access AdWorks Path" & "\" & "#MS Access AdWorks DB" & ".accdb"
  = "#MS Excel Ad Works Path" & "\" & "#MS Excel Ad Works File" & ".xlsx"
- 7. 为这些查询分配名称，例如 **MS Access Ad Works Connection** 并禁用其加载到数据模型
- 8. 最后，修改每个 10 个查询以引用其中三个数据源查询，如下所示：

```
let Source = Access.Database(File.Contents("#MS Access Ad Works Connection"), [CreateNavigationProperties=true]),
Customer = Source[{"Schema="",Item="DimCustomer"}][Data]
in Customer
```

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_529_1.png)

图 11.15：Power Query Editor 中合并且参数化的数据源

## 增强和优化现有的 Power BI 解决方案

文件夹组、参数和数据源查询使理解和管理检索过程变得更容易。

## 另请参阅

- - *Power Query 中 10 个常见错误及如何避免陷阱* 作者 Gil Raviv：`http://bit.ly/2uW6c33`
- - *Query Diagnostics*：`https://bit.ly/2YBEmKv`

## 隔离和记录 DAX 表达式

建议将表达式隔离为独立且可互换的 DAX 度量或作为度量中的变量，以简化数据集的开发和所有权。独立度量可以从 **Fields** 列表中隐藏，但包含核心业务定义和高效的过滤逻辑，以驱动模型中许多其他度量的结果和性能。虽然作用域限定于每个度量，但 DAX 变量提供自记录的编码风格，并且与标量值度量不同，还支持表值，从而允许更大的模块化。

在本配方中，DAX 变量、度量和注释在不同示例中使用，演示了 DAX 表达式的创建和使用的最佳实践。

## 准备工作

为本配方做准备，请执行以下步骤：

- 1. 在本地打开 Power BI Desktop 文件并通过单击 **Home** 选项卡功能区中的 **Transform Data** 访问 **Power Query Editor**。
- 2. 创建一个名为 AdWorksDW 的查询，类似于以下内容：
  
      ```m
      let
          Source = Sql.Database("localhost\\MSSQL_SERVERDEV", "AdventureWorksDW2019")
      in
          Source
      ```
- 3. 禁用 **AdWorksDW** 查询的加载并将其放置在 **Data Sources** 查询组中。
- 4. 右键单击 **AdWorksDW** 查询并选择 **Reference**。
- 5. 重命名查询为 **Reseller Sales** 并选择 **FactResellerSales** 表。
- 6. 重复步骤 4 和 5 以创建选择 **DimReseller** 表的 **Resellers** 表和选择 **DimDate** 表的 **Dates** 查询。
- 7. 将 **Reseller Sales** 查询移动到 **Facts** 查询组，并将 **Resellers** 和 **Dates** 查询移动到 **Dimensions** 查询组
- 8. 在 **Other Queries** 查询组中，创建一个名为 **Calculations** 的空白查询
- 9. 从 **Home** 选项卡的功能区中选择 **Close & Apply**
- 10. 保存您的工作

## 如何操作...

要实施本配方，请使用以下步骤：

- 1. 在 **Calculations** 表中创建以下基础度量：
  `Reseller Gross Sales = SUM('Reseller Sales'[SalesAmount])`
  `Reseller Discount Amount = SUM('Reseller Sales'[DiscountAmount])`
- 2. 在 **Calculations** 表中创建 **Reseller Margin %** 度量：
  `Reseller Margin % =`
  `/*`
  `Net Sales = Gross sales net of discounts that have shipped`
  `Product Cost = Product standard cost of all ordered products (including not shipped). A Date of 12/31/2099 used for unshipped sales order lines since 1/1/2015`
  `*/`
  `VAR ShippedSales = CALCULATETABLE('Reseller Sales', 'Reseller Sales'[ShipDate] <> DATEVALUE("12/31/2099"))`
  `VAR NetSalesShipped = CALCULATE([Reseller Gross Sales] - [Reseller Discount Amount],ShippedSales)`
  `VAR ProductCost = SUMX('Reseller Sales', 'Reseller Sales'[OrderQuantity] * 'Reseller Sales'[ProductStandardCost])`
  `RETURN`
  `DIVIDE(NetSalesShipped - ProductCost,NetSalesShipped)`
- 3. 在 **Calculations** 表中创建 **Reseller Gross Sales (Custom)** 度量：
  `Reseller Gross Sales (Custom) =`
  `VAR CurrentDate = MAX('Dates'[FullDateAlternateKey])`
  `VAR ResellerTypes = CALCULATETABLE('Resellers', Resellers[BusinessType] = "Warehouse")`
  `VAR DateHistory = --Trailing 10 Days`
  `FILTER(ALL('Dates'),'Dates'[FullDateAlternateKey] <= CurrentDate && 'Dates'[FullDateAlternateKey] >= CurrentDate - 10)`
  `RETURN`
  `CALCULATE([Reseller Gross Sales],ResellerTypes,DateHistory)`

