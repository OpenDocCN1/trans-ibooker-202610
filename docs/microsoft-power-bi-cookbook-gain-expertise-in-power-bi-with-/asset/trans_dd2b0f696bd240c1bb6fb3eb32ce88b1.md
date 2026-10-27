

+   3. 创建第二个参数 **ExcelFile**，类型为 **Text**，并将 **Current Value** 设置为 `Employees.xlsx`。
+   4. 将 **ExcelFolder** 和 **ExcelFile** 参数移动到 **Parameters** 查询组。
+   5. 在 **Other Queries** 组中创建一个新的 **Blank Query**，命名为 **ExcelFilePath** 并禁用加载：

```
let
    ExcelPath = ExcelFolder & "\\" & ExcelFile
in
    ExcelPath
```

+   6. 编辑 `EmployeeKeysAdHoc` 查询的 Source 行以使用 `ExcelFilePath` 查询：

```
Source = Excel.Workbook(File.Contents(ExcelFilePath), null, true),
```

Excel 文件的路径和文件名现在由 ExcelFolder 和 ExcelFile 参数控制。

## See also

+   ➤ Writing Custom Functions in Power Query M: https://bit.ly/3xnPuYc

## Capturing user selections with parameter tables

为 Power BI 报表用户提供参数功能的一种替代方法是使用专用的参数表。在这种方法中，参数表的值要么在数据集刷新过程中计算，要么作为一次性手动操作加载。DAX 度量引用此参数表以及模型中的其他表和表达式，以丰富自助分析体验并支持 Power BI 报表开发。这种方法最适合参数不经常更改的情况，因为 Power BI 中的硬编码参数表意味着数据集所有者必须进行此类更改。如果参数需要频繁更改，最好使用数据库表或外部文件作为参数表，这样更容易编辑和更改。

本食谱中的示例涉及为基线年度销售计划提供四种替代场景的简单可见性——高于和低于基线计划的 10% 和 20%。一组内联场景值嵌入在数据模型中，并使用 DAX 度量来捕获筛选上下文——例如业务用户选择——并计算相应的场景逻辑。

## Getting ready

为本食谱做准备，请按以下步骤操作：

+   1. 在本地打开一个 Power BI Desktop 文件，通过单击 **Home** 选项卡功能区中的 **Transform Data** 访问 **Power Query Editor**。
+   2. 创建一个名为 AdWorksDW 的查询，类似于以下内容：
   
   ```powerquery
   let
       Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
   in
       Source
   ```
+   3. 禁用 AdWorksDW 查询的加载并将其放在 Data Sources 查询组中。
+   4. 右键单击 AdWorksDW 查询并选择 **Reference**。
+   5. 使用以下代码创建 Products 查询：
   
   ```powerquery
   let
       Source = AdWorksDW,
       Navigation = Source{[Schema="dbo",Item="DimProduct"]}[Data],
       Subcategory =
           Table.ExpandRecordColumn(
               Navigation, "DimProductSubcategory",
               {"EnglishProductSubcategoryName", "DimProductCategory"},
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
+   6. 使用以下代码创建 CategoryPlan 查询：
   
   ```powerquery
   let
       Source = Products,
       Sales =
           Table.ExpandTableColumn(
               Source, "FactInternetSales",
               {"SalesAmount", "OrderDate"}, {"SalesAmount", "OrderDate"}
           ),
       RemoveNulls = Table.SelectRows(Sales, each [SalesAmount] <> null),
       Year =
   ```

```
Table.AddColumn(
    RemoveNulls, "Calendar Year", each Date.Year([OrderDate])
),
Month =
    Table.AddColumn(
        Year, "Calendar Month", each Date.MonthName([OrderDate])
    ),
MonthNum =
    Table.AddColumn(
        Month, "Month Number", each Date.Month([OrderDate])
    ),
Group =
    Table.Group(MonthNum, {"Calendar Year", "Calendar Month", "Product Category", "Month Number"}, {{"SalesAmount", each List.Sum([SalesAmount]), type nullable number}}),
Index = Table.AddIndexColumn(Group, "Index", 1, 1, Int64.Type),
Plan =
    Table.AddColumn(
        Index, "Plan Amt",
        each if Number.IsEven([Index])
            then 1/Number.RandomBetween(5,10) * [SalesAmount] + [SalesAmount]
            else (-1 * 1/Number.RandomBetween(5,10) * [SalesAmount]) + [SalesAmount]
    ),
YearMonthSort =
    Table.AddColumn(
        Plan, "YearMonthSort", each [Calendar Year] * 100 + [Month Number]
    ),
YearMonth =
    Table.CombineColumns(
        Table.TransformColumnTypes(
            YearMonthSort, {{"Calendar Year", type text}}, "en-US"
        ),
        {"Calendar Year", "Calendar Month"},
        Combiner.CombineTextByDelimiter("-", QuoteStyle.None), "Year-Month"
    ),
Currency = Table.TransformColumnTypes(YearMonth, {{"Plan Amt", Currency.Type}})
in
    Currency
```

+   7. 将 **CategoryPlan** 查询移动到名为 **Facts** 的查询组，并将 **Products** 查询移动到名为 **Dimensions** 的查询组。

## Parameterizing Power BI Solutions

+   8. 禁用 **Products** 查询的加载。
+   9. 从 **Home** 选项卡功能区中选择 **Close & Apply**。
+   10. 在 CategoryPlan 表中，将 **Year-Month** 列的 **Sort By** 列设置为 YearMonthSort 列。
+   11. 保存你的工作。

## How to capture user selections

要实现本食谱，请执行以下步骤：

+   1. 在 Power BI Desktop 中，在 **Report** 视图中，单击 **Modeling** 选项卡功能区中的 **New Table**。
+   2. 使用 DAX DATATABLE 函数创建一个计算表，其中包含场景名称、场景值和排序键：

```
Plan Scenarios =
    DATATABLE(
        "Plan Scenario", STRING,
        "Var to Plan", DOUBLE,
        "Scenario Sort", INTEGER,
        {
            {"Plan",1,3},{"10% Above Plan",1.1,2},
            {"20% Above Plan",1.2,1},
            {"10% Below Plan",.9,4},
            {"20% Below Plan",.8,5}
        }
    )
```

+   3. 在 **Data** 视图中选择新表 (**Plan Scenarios**)，并使用功能区 **Column tools** 选项卡中的 **Sort by column** 功能将 **Plan Scenario** 列设置为按 **Scenario Sort** 列排序。
+   4. 右键单击 **Scenario Sort** 和 **Var to Plan** 列并选择 **Hide in Report View**。
+   5. 返回 **Report** 视图并创建一个度量以检索 **Plan Scenario** 列的筛选上下文：
   
   ```
   Sales Plan Scenario Filter Branch =
       SWITCH(TRUE(),
           NOT(ISFILTERED('Plan Scenarios'[Plan Scenario])),
           "No Selection",
           NOT(HASONEFILTER('Plan Scenarios'[Plan Scenario])),
           "Multiple Selections",
           "Single Selection"
       )
   ```
+   6. 创建一个 **Plan Amount** 度量：
   
   ```
   Plan Amount = SUM('SubCatPlan'[Plan Amt])
   ```
+   7. 创建一个 **Internet Sales Plan Scenario** 度量，该度量根据筛选上下文动态计算预算/计划金额：
   
   ```
   Internet Sales Plan Scenario =
       VAR FilterContext = [Sales Plan Scenario Filter Branch]
   RETURN
       SWITCH(TRUE(),
           FilterContext = "Single Selection",
               MIN('Plan Scenarios'[Var to Plan]) * [Plan Amount],
           FilterContext = "No Selection",
               [Plan Amount],
           FilterContext = "Multiple Selections",BLANK()
       )
   ```
+   8. 将货币格式应用于 **Plan Amount** 和 **Internet Sales Plan Scenario** 度量。
+   9. 创建可视化以显示不同的计划场景；示例显示在图 7.7 中：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_352_0.png)

图 7.7：计划场景的可视化

## How it works…

在使用 DAX DATATABLE 函数在 Plan Scenarios 表中时，列名和类型会被声明，并且每一行都用大括号括起来——M 查询中的 List 值。理想情况下，新的场景表可以持久化在数据仓库中，并且 Power BI 解决方案可以对场景名称和值的更改具有弹性。

