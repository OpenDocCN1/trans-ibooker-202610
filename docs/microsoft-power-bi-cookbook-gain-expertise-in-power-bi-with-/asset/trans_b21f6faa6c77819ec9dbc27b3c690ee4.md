
),

GroupedRows =
    Table.Group(
        UpperCase, {"EnglishProductName"},
        {{"Rows", each Table.RowCount(_), Int64.Type}}
    ),
Duplicates = Table.SelectRows(GroupedRows, each [Rows] > 1)
in
    Duplicates

EnglishProductName 列被选中、修剪、转换为大写、分组并进行筛选，以始终检索任何重复的键值。请禁用此查询的加载，因为此查询仅用于故障排除。

## 另请参阅

- Table.SelectColumns: http://bit.ly/38Qk7Lt
- Table.RemoveColumns: http://bit.ly/3cJju7p
- Table.TransformColumns: http://bit.ly/3tsdxm2
- Table.DuplicateColumn: http://bit.ly/3cIF63X
- Table.Distinct: http://bit.ly/38V8mmN
- Text.Trim: http://bit.ly/3eUmAZ0
- Text.Upper: http://bit.ly/3vFw2R6
- 面向文本的 M 函数参考: http://bit.ly/2nUYjnw

## 创建自定义列

业务用户通常通过附加列来扩展现有报表和数据模型的输出，以帮助他们分析和呈现数据。这些列的逻辑通常通过 Excel 公式或计算 DAX 列来实现。更出色的解决方案是通过 Power Query 编辑器和 M 语言创建这些列，尤其是在逻辑无法快速迁移到数据仓库或 IT 资源的情况下。

开发自定义列还可以显著提高数据模型及其支持的可视化效果的易用性和分析能力。在本方案中，我们将创建列以应用自定义命名格式，并通过现有列简化客户维度的分析。

## 准备工作

要准备本方案，请按以下步骤从 **AdventureWorksDW2019** 数据库中导入 **DimCustomer** 表：

- 1. 打开 Power BI Desktop，在 **开始** 选项卡的功能区中选择 **转换数据**，以打开 **Power Query 编辑器**。
- 2. 创建一个名为 AdWorksDW 的导入模式数据源查询。该查询应类似于以下内容：

```powerquery
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

- 3. 将此查询单独放入名为 **数据源** 的查询组中。
- 4. 右键单击 AdWorksDW 并选择 **引用**，在数据预览区域选择 DimCustomer 表，然后将此查询重命名为 DimCustomer。

有关执行这些步骤的其他详细信息，请参阅本章中的*管理查询和数据源*方案。

## 如何创建自定义列

要实现本方案，请执行以下步骤：

- 1. 使用 `Table.SelectColumns` 从 DimCustomer 表中检索所需的源列：FirstName、LastName、MiddleName、Title 和 BirthDate。

```powerquery
let
    Source = AdWorksDW,
    dbo_DimCustomer = Source{[Schema="dbo",Item="DimCustomer"]}[Data],
    SelectColumns =
        Table.SelectColumns(dbo_DimCustomer,
            {"FirstName", "LastName", "MiddleName", "Title",
             "BirthDate"}
        )
in
    SelectColumns
```

- 2. 编写一个 `Table.AddColumns` 函数，并使用 `if...then` 表达式来处理给定目标格式 Mr. John A. Doe 的不同场景：

```powerquery
NameFormatTbl =
    Table.AddColumn(
        SelectColumns,"Formatted Name", each
```

```
if [Title] = null and [MiddleName] = null
    then [FirstName] & " " & [LastName]
    else if [Title] = null
        then [FirstName] & " " & Text.Range([MiddleName],0,1) & ". " & [LastName]
    else
        [Title] & " " & [FirstName] & " " & Text.Range([MiddleName],0,1) & ". " & [LastName]
)
```

- 3. 添加变量，使表达式能够支持当前系统日期与 BirthDate 列之间的比较。

```
let
    CurrentDate = DateTime.Date(DateTime.LocalNow()),
    CurrentYear = Date.Year(CurrentDate),
    CurrentMonth = Date.Month(CurrentDate),
    CurrentDay = Date.Day(CurrentDate),

    Source = AdWorksDw,
```

- 4. 使用 Table.AddColumn 函数基于 BirthDate 列创建 Customer Year、Customer Month 和 Customer Day 列。

```
AddCustomerYear =
    Table.AddColumn(
        NameFormatTble, "Customer Year", each Date.Year([BirthDate]),
        Int64.Type
    ),
    AddCustomerMonth =
        Table.AddColumn(
            AddCustomerYear, "Customer Month", each Date.Month([BirthDate]),
            Int64.Type
        ),
    AddCustomerDay =
        Table.AddColumn(
            AddCustomerMonth, "Customer Day", each Date.Day([BirthDate]),
            Int64.Type
        )
```

NameFormatTble 表达式中的三个 if...then 条件涵盖了返回格式化姓名的所有场景，因为查询必须处理 Middle Name 和 Title 列中的 null 值，以及 Middle Name 列中的不同值。Text.Range 用于提取中间名的首字符。

对于 CurrentDate、CurrentYear、CurrentMonth 和 CurrentDay 变量，使用 DateTime.LocalNow 函数作为当前日期的来源；然后将其用于年、月、日。

对于 AddCustomerYear、AddCustomerMonth 和 AddCustomerDay 表达式，将 Int64.Type 值传递给 Table.AddColumn 的可选 type 参数，以将新列设置为整数。

对于 CustomerAge 和 CustomerSegment 表达式，使用了嵌套的 if...then 表达式。使用此方法是因为目前 M 中尚无可用于年、月、周等日期间隔的 DATEDIFF 函数（T-SQL、DAX）的等效实现。Duration.Days 函数可用于日间隔，其他 duration 函数可用于小时、分钟和秒间隔。

CustomerAge 表达式将 CurrentMonth 和 CurrentDay 变量与 AddCustomerMonth 和 AddCustomerDay 表达式创建的客户列的值进行比较，以便使用 CurrentYear 变量和 AddCustomerYear 表达式创建的列来计算客户的年龄。然后，由 CustomerAge 列创建的列用于 CustomerSegement 表达式，以得出年龄段分割列。新的自定义列可用于支持基于客户年龄和分段的多种可视化效果。

## 更多内容

Power Query 编辑器提供了用于添加列的图形用户界面。这些界面提供了一种添加列的方法，作为手动编写代码的替代方案。本质上，添加列的 M 代码是由这些界面作为输出生成的。

其中一种界面是“从示例创建列”（Column From Examples）功能，它允许用户直接输入所需列输出的示例。引擎会确定要添加到查询中的 M 函数和一系列步骤，以返回与提供的示例一致的结果。若要探索此功能，请按照以下步骤操作：

- 1. 创建引用 AdWorksDW 查询的新查询。
- 2. 选择 DimCustomer 表。
- 3. 选择 Title、FirstName、MiddleName、LastName 和 BirthDate 列，并删除所有其他列。
- 4. 选择“添加列”选项卡，然后在功能区中选择“从示例创建列”按钮。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_95_0.png)

图 2.33：从示例创建列功能

- 5. 在第一行中输入客户的 FirstName、MiddleName 和 LastName 值，然后按 Enter 键。请注意，其余行将根据第一行自动计算。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_95_1.png)

图 2.34：从示例创建列界面

- 6. 单击“确定”按钮以接受转换。

另一种添加列的界面是“条件列”（Condition Column）功能。此功能提供了一种创建条件列的方法，作为编写 if...then 表达式的替代方案。若要查看此功能的运作方式，请按照以下步骤操作：

- 1. 选择“添加列”选项卡，然后在功能区中选择“条件列”。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_95_2.png)

图 2.35：条件列功能

