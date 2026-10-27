
有关执行这些步骤的更多详细信息，请参阅本章中的“管理查询和数据源”配方。

## 如何选择和重命名列

要实现此配方，请在高级编辑器中执行以下步骤：

- 1. 通过 Table.AddColumn 函数，从名字和姓氏创建一个名称列。

  ```powerquery
  CustomerNameAdd = Table.AddColumn(
      dbo_DimCustomer, "Customer Name",
      each [FirstName] & " " & [LastName],
      type text
  )
  ```

- 2. 使用 Table.SelectColumns 函数，从 DimCustomer 表现有的 30 列中选择 10 列。

  ```powerquery
  SelectCustCols = Table.SelectColumns(CustomerNameAdd,
      {
          "CustomerKey", "Customer Name", "Annual Income",
          "Customer Gender", "Customer Education",
          "MaritalStatus",
          "Customer Phone Number", "CommuteDistance",
          "AddressLine1",
          "TotalChildren"
      }, MissingField.UseNull
  )
  ```

请注意，指定的某些列名实际上并不存在。这是故意的，将在下一步中修复。但请注意，这些列不会生成错误，而是显示 null 值。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_83_0.png)

## 访问和检索数据

使用 Table.RenameColumns 函数为用户应用直观的名称，并有助于 Q&A 引擎进行自然语言查询。将此语句插入到 SelectCustCols 语句上方，并进行相应调整。完整查询现在应类似于以下内容：

```powerquery
let
    Source = AdWorksDW,
    dbo_DimCustomer = Source{[Schema="dbo",Item="DimCustomer"]}[Data],
    CustomerNameAdd =
        Table.AddColumn(
            dbo_DimCustomer, "Customer Name",
            each [FirstName] & " " & [LastName],
            type text
        ),
    #"Renamed Columns" =
        Table.RenameColumns(CustomerNameAdd,
            {
                {"YearlyIncome", "Annual Income"},
                {"Gender", "Customer Gender"},
                {"EnglishEducation", "Customer Education"},
                {"Phone", "Customer Phone Number"}
            }
        ),
    SelectCustCols =
        Table.SelectColumns(#"Renamed Columns",
            {
                "CustomerKey", "Customer Name", "Annual Income",
                "Customer Gender", "Customer Education",
                "MaritalStatus",
                "Customer Phone Number", "CommuteDistance",
                "AddressLine1",
                "TotalChildren"
            }, MissingField.UseNull
        )
in
    SelectCustCols
```

### 工作原理

Table.AddColumn 函数将 FirstName 和 LastName 列连接起来，并包含一个可选的最后一个参数，该参数将列类型指定为文本。

Table.SelectColumns 函数指定要从数据源检索的列。未指定的列将从检索中排除。

实现相同效果的另一种方法是使用 Table.RemoveColumns 函数。但是，在这种情况下，需要删除 20 列，而不是显式定义要保留的 10 列。为了避免在某个源列更改或缺失时查询失败，最好指定并命名 10 列而不是 20 列。通过使用 Table.SelectColumns 的可选参数 MissingField.UseNull，可以进一步提高查询的弹性。使用此参数时，如果所选列不可用，查询仍会成功，只需在所有行中为该列插入 null 值即可。

使用 Table.SelectColumns 函数的另一个优点是，可以在检索所选列时按指定顺序重新排序列。这有助于查询设计过程，并避免使用 Table.ReorderColumns 函数编写额外表达式的需要。在数据视图中，加载到数据模型的查询的初始列顺序会得到保留。但是，在 Power BI Desktop 的报表视图和数据视图中，“字段”窗格中显示的字段列表会自动按字母顺序排列。

对于导入数据模型，您可以考虑删除表示同一表中其他列的简单表达式的列。例如，如果 Extended Amount 列等于 Unit Price 和 Order Quantity 列的乘积，则可以选择仅导入后两列。DAX 度量值可以改为计算 Extended Amount 值。这样做可能是为了保持较小的模型大小。但是，不建议对 DirectQuery 模型使用此技术。

使用 Table.RenameColumns 函数重命名列，以删除任何源系统指示符，在非键列的单词之间添加空格，并应用特定于维度的名称，例如 Customer Gender 而不是 Gender。Table.RenameColumns 函数还提供了 MissingField.UseNull 选项。

### 更多内容...

导入模型在内部以列式压缩格式存储。每列的压缩数据都会增加文件的总磁盘大小。数据大小的主要因素是列的基数（cardinality）。具有许多唯一值的列无法很好地压缩，因此会占用更多空间。消除高基数列可以减小数据模型的大小，从而减小 PBIX 文件的整体大小。然而，对于导入模型，查询性能（除其他因素外）取决于查询所访问的各个列的大小。

### 另请参阅

- Table.SelectColumns: http://bit.ly/38Qk7Lt
- Table.RenameColumns: http://bit.ly/3rTVfd4
- Table.RemoveColumns: http://bit.ly/3cJju7p
- Table.ReorderColumns: http://bit.ly/3cEo0Jg
- Table.AddColumn: http://bit.ly/3vGJZ6b
- Power BI 文档：为 Q&A 准备数据: http://bit.ly/2nBLAGc

## 转换和清理数据

在 Power BI 的 M 查询中应用的转换用于保护数据模型的完整性，并支持增强的分析和可视化。要实施的具体转换因数据质量、集成需求和整体解决方案的目标而异。但是，开发人员至少应设法保护模型关系的完整性，并通过反规范化和标准化来简化用户体验。此外，开发人员应与数据源所有者核实，以确定某些必需的转换是否可以在源中实施，或者是否可以通过 SQL 视图对象提供，从而无需使用 Power Query (M) 表达式。

本配方演示如何保护模型免受源数据中重复值的影响，这些重复值可能会阻止在数据模型中形成正确的关系，甚至可能导致查询失败。虽然这里使用了一个简单场景，但本配方演示了在尝试合并多个数据源和消除重复项时可能遇到的情况。

### 准备工作

要开始准备，请通过执行以下操作从 AdventureWorksDW2019 数据库中导入 DimProduct 和 FactResellerSales 表：

- 1. 打开 Power BI Desktop，然后从“开始”选项卡的功能区中选择“转换数据”以打开 Power Query 编辑器。
- 2. 创建一个名为 AdWorksDW 的“导入”模式数据源查询。此查询应类似于以下内容：

  ```powerquery
  let
      Source = Sql.Database("localhost\MSSQLSERVERDEV",
      "AdventureWorksDW2019")
  in
      Source
  ```

- 3. 将此查询隔离在名为“数据源”的查询组中。
- 4. 右键单击 AdWorksDW 并选择“引用”，在数据预览区域中选择 DimProduct 表，然后将此查询重命名为 DimProduct。右键单击 EnglishProductName 列并选择“删除其他列”。
- 5. 重复上一步，但这次选择 FactResellerSales。展开 DimProduct 列，仅选择 EnglishProductName。将此列重命名为 EnglishProductName。

