

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_134_0.png)

*图 3-12. LEFT OUTER JOIN 的结果，其中第二个表中不存在一个值*

结果是一个可用于报表或进一步转换的合并表。得益于直观的视觉界面，你无需编写任何代码即可执行这些连接。这使得处理相关数据对初学者以及对 SQL 或 Spark 不太熟悉的人来说变得容易。你可能还会注意到合并选项中称为 LEFT ANTI 和 RIGHT ANTI 的连接类型（请参考图 3-11）。这些连接类型在你想要查找一个表中没有在另一个表中对应匹配的行时很有用；这有助于你快速识别未匹配或缺失的数据。

无论你是在 Fabric 仓库中使用 SQL，还是在 Fabric Lakehouse 中使用笔记本和 Dataflow Gen2，你都需要理解连接和合并。它可以帮助你将来自不同来源的相关数据汇集在一起，并创建支持清晰且富有洞察力的报表和分析的表。

## 汇总数据

在你通过连接合并数据后，下一个重要步骤是对其进行汇总。

## 用于汇总数据的聚合

*聚合* 帮助你对数据进行分组并计算有意义的总计、平均值和计数，还可以帮助你找到最小值和最大值。常见的聚合函数如下：

SUM

这会将组中的所有值相加。

COUNT

这会计算行数或非 NULL 值的数量。

AVG

这会计算平均值。

MIN

这会找到最小值。

MAX

这会找到最大值。

让我们来看两个实际示例，按产品汇总总销售额和按地区汇总客户数量，以及如何在 Fabric 仓库中使用 SQL 以及在 Fabric lakehouse 中使用笔记本和 Dataflow Gen2 对其进行聚合。

## 在 Fabric 仓库中使用 SQL 进行聚合

使用 SQL，我们通过 GROUP BY 子句对数据进行分组和聚合。该子句告诉 SQL 按哪些列对行进行分组，而 SUM 和 COUNT 等聚合函数则对分组后的行进行操作以计算汇总值。例如，如果我们想查看每个产品的总销售额，我们按 ProductName 列对数据进行分组。SUM(SalesAmount) 部分计算每个组内的总销售额：

```
SELECT
    ProductName,
    SUM(SalesAmount) AS TotalSales
FROM
    sales
GROUP BY
    ProductName;
```

此查询按产品（ProductName）对销售数据进行分组，并使用 SUM 计算每个产品的总销售额。结果是一个表格，显示每个产品及其总销售额。

类似地，要按地区统计客户数量，我们按 Region 列分组，并在 CustomerID 列中使用 COUNT 函数来查找每个地区的客户数量：

```
SELECT
    Region,
    COUNT(CustomerID) AS NumberOfCustomers
FROM
    customers
GROUP BY
    Region;
```

在此，查询按 Region 对行进行分组，并统计每个组中的客户 ID 数量。

> **注意**
> 这些 SQL 聚合查询在 Fabric lakehouse 中使用 Spark SQL 时也适用。本章中，我们为清晰起见重点介绍仓库示例，但在两种环境中都可以执行相同的操作。

## 在 Fabric Lakehouse 的笔记本中进行聚合

在 Fabric lakehouse 的笔记本中，我们使用 PySpark 并使用 DataFrames。要执行聚合，我们使用 .groupBy() 方法定义要按其分组的列(s)，然后跟上 .sum()、.count() 或 .agg() 等聚合函数。假设我们想计算每个产品的总销售额。以下是 PySpark 中的做法：

```
sales_df.groupBy("ProductName").sum("SalesAmount").show()
```

.groupBy("ProductName") 方法按产品名称对数据进行分组，.sum("SalesAmount") 计算每个组的总销售额，.show() 在笔记本中显示结果。结果显示每个产品及其销售额之和（见图 3-13）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_137_0.png)

图 3-13. 在笔记本中按产品对销售进行分组

要统计每个地区的客户数量，我们可以做类似的操作：

```
customers_df.groupBy("Region").count().show()
```

在此示例中，.count() 统计每个地区中的行数（客户）。由于我们在 .groupBy() 之后直接使用 .count()，它返回每个组中的总行数——在本例中即每个地区。

## 在 Fabric Lakehouse 的 Dataflow Gen2 中进行聚合

在 Dataflow Gen2 中，聚合是通过 Power Query 编辑器中的转换以可视化方式完成的。在我们的 lakehouse 中，我们可以创建一个新的 Dataflow Gen2 并加载我们的表，一旦拥有表后，我们可以点击 Transform 以显示 Group by 按钮（见图 3-14）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_138_0.png)

图 3-14. 点击 Transform 会显示 Group by 按钮

然后，我们可以点击“Group by”来定义如何分组。例如，我们可以按 ProductName 分组，然后说明应执行什么操作，例如汇总 SalesAmount（见图 3-15）。

# Group by

指定要按其分组的列和所需的输出。

Basic Advanced

Group by *
ProductName

New column name *
Sales
Operation *
Sum
Column *
SalesAmount

Use fuzzy grouping

Fuzzy group options

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_139_0.png)

图 3-15. 按产品名称汇总销售额

结果将是 Product 在一列中，SalesAmount 在另一列中。

在通过连接合并数据后，聚合是将详细数据转换为有意义汇总的下一步关键步骤。聚合允许你按一个或多个列对行进行分组，并应用 SUM、COUNT、AVG、MIN 和 MAX 等函数来计算每个组内的总计、平均值、计数或极值。在 Fabric 仓库中，SQL 的 GROUP BY 子句对数据进行分组，而聚合函数则计算每个组的汇总值。在 Fabric lakehouse 中，笔记本利用 .groupBy() 方法与 .sum() 和 .count() 等聚合函数来实现相同结果。Dataflow Gen2 通过 Power Query 的“Group by”转换实现可视化分组和聚合，使汇总无需代码即可实现。

无论你是在 Fabric 仓库还是 lakehouse 工作，向数据添加新列或表都有助于你跟上不断变化的业务需求。

## 过滤器和聚焦于相关数据

处理数据时另一个重要步骤是对其进行过滤，使其尽可能窄且相关。我们过滤表以仅包含满足特定条件的行，因为这可以提高性能并使分析更直接。根据处理数据的地点，我们可以使用不同的过滤方法：

- SQL 中的 WHERE 子句，指定行必须满足的条件才能包含
- 在笔记本中过滤行，使用 Spark/PySpark DataFrames 与条件表达式
- Dataflow Gen2 中的可视化过滤器，使用 Power Query 转换

## 过滤数据

让我们看看如何在 Fabric 仓库中使用 SQL 过滤，在 Fabric lakehouse 中使用笔记本过滤，以及在 Dataflow Gen2 中可视化过滤。

## 在 Fabric 仓库中使用 SQL 过滤

在 SQL 中，你使用 WHERE 子句进行过滤，它允许你指定要选择的行的条件。例如，如果你只想查看最近 30 天的销售订单，你可以根据订单日期进行过滤：

```
SELECT *
FROM sales
WHERE
OrderDate >= DATEADD(day, -30, CURRENT_DATE);
```

此查询返回 OrderDate 在今天之前 30 天内的所有行。要过滤销售额大于 $100 的销售，查询如下：

```
SELECT  *
FROM sales
WHERE SalesAmount > 100;
```

WHERE 子句告诉 SQL 仅包含条件为真的行。

## 在 Fabric Lakehouse 的笔记本中过滤

在 PySpark 笔记本中，你通过在 DataFrames 上应用条件表达式使用 .filter() 或 .where() 方法进行过滤。 .filter() 和 .where() 的工作方式相同，并且可以与其他转换链接。例如，要过滤最近 30 天的订单，请执行以下操作：

```
from pyspark.sql.functions import current_date, date_sub

recent_sales_df = sales_df.where(sales_df.OrderDate >= date_sub(current_date(), 30))
recent_sales_df.show()
```

这里，.filter() 仅保留 OrderDate 在最近 30 天内的行。

> **注意**
> 
> 在 Microsoft Fabric 笔记本中，你可以使用 SparkSQL 和 PySpark 来过滤和转换数据。虽然 SparkSQL 使用类似 SQL 的语法，而 PySpark 使用基于 Python 的 DataFrame 操作，但两者在大多数任务中可以互换。在本章中，我们将展示使用 PySpark 的示例。

