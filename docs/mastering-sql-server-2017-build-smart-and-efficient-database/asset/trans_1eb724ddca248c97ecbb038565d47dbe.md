
首先让我介绍我将使用的记号。我将处理两个区间，分别记为 *i₁* 和 *i₂*。第一个区间的起始时间点是 *b₁*，结束是 *e₁*；第二个区间的起始时间点是 *b₂*，结束是 *e₂*。艾伦的**布尔运算符**定义如下表所示：

| 名称 | 符号 | 定义 |
|---|---|---|
| 等于 | (*i₁ = i₂*) | (*b₁ = b₂*) 且 (*e₁ = e₂*) |
| 之前 | (*i₁ before i₂*) | (*e₁ < b₂*) |
| 之后 | (*i₁ after i₂*) | (*i₂ before i₁*) |
| 包含 | (*i₁ ⊇ i₂*) | (*b₁ ≤ b₂*) 且 (*e₁ ≥ e₂*) |
| 真包含 | (*i₁ ⊃ i₂*) | (*i₁ ⊃ i₂*) 且 (*i₁ ≠ i₂*) |
| 相遇 | (*i₁ meets i₂*) | (*b₂ = e₁ + 1*) 或 (*b₁ = e₂ + 1*) |
| 重叠 | (*i₁ overlaps i₂*) | (*b₁ ≤ e₂*) 且 (*b₂ ≤ e₁*) |
| 合并 | (*i₁ merges i₂*) | (*i₁ overlaps i₂*) 或 (*i₁ meets i₂*) |
| 开始 | (*i₁ begins i₂*) | (*b₁ = b₂*) 且 (*e₁ ≤ e₂*) |
| 结束 | (*i₁ ends i₂*) | (*e₁ = e₂*) 且 (*b₁ ≥ b₂*) |

除了布尔运算符之外，艾伦的三个运算符接受区间作为输入参数并返回一个区间。这些运算符构成了简单的**区间代数**。请注意，这些运算符与您可能已经熟悉的关系运算符同名：`Union`、`Intersect` 和 `Minus`。然而，它们的行为与关系运算符并不完全一样。

一般来说，使用三个区间运算符中的任何一个，如果操作结果将导致时间点集合为空，或导致无法用一个区间描述的集合，则该运算符应返回 `NULL`。仅当两个区间相遇或重叠时，两个区间的并集才有意义。仅当区间重叠时，交集才有意义。`Minus` 区间运算符仅在某些情况下才有意义。例如，(3:10) Minus (5:7) 返回 `NULL`，因为结果无法用一个区间描述。

下表总结了区间代数中运算符的定义：

| 名称 | 符号 | 定义 |
|---|---|---|
| 并集 | $(i_1 \text{ union } i_2)$ | $(Min(b_1, b_2) : Max(e_1, e_2))$，当 $(i_1 \text{ merges } i_2)$；否则为 NULL |
| 交集 | $(i_1 \text{ intersect } i_2)$ | $(Max(b_1, b_2) : Min(e_1, e_2))$，当 $(i_1 \text{ overlaps } i_2)$；否则为 NULL |
| 差集 | $(i_1 \text{ minus } i_2)$ | $(b_1 : Min(b_2 - 1, e_1))$，当 $(b_1 < b_2)$ 且 $(e_1 \le e_2)$；$(Max(e_2 + 1, b_1) : e_1)$，当 $(b_1 \ge b_2)$ 且 $(e_1 > e_2)$；否则为 NULL |

下图以图形方式展示了区间代数运算符：

![](img/07d1225e179d5df9f2b2d26b026386c6_180_0.png)

## 时间约束

根据您正在解决的业务问题，您可能需要实现许多时间约束。请记住，对于应用程序时间，SQL Server 并没有提供太多帮助。您需要在代码中实现这些约束，并在可能的情况下使用 SQL Server 的声明性约束。但是，大多数约束需要通过自定义代码来实现，要么在触发器或存储过程中，要么甚至在应用程序代码中。

想象一下 Suppliers 表示例。一个供应商可以在表中出现多次，因为同一个供应商可能在多个时间段内分别存在合同。例如，在带有简化表头 Suppliers (supplierid, companyname, from, to) 的关系中，您可能会有如下两个元组：

```text
{2, Supplier VHQZD, d05, d07}
{2, Supplier VHQZD, d12, d27}
```

以下是您可能需要实现的一些可能约束：

- *结束* 不应小于 *开始*
- 同一供应商的两个合同不应具有重叠的时间间隔
- 同一供应商的两个合同不应具有相邻的时间间隔
- 任何供应商在任何时间点都不能同时拥有两个不同的合同
- 在供应商不在合同期内的时间点，不应有该供应商的供应

您可能还会发现更多约束。无论如何，SQL Server 2016 仅支持系统版本控制表。要维护应用程序的有效时间，您需要自行开发代码。

## 2016 年之前 SQL Server 中的时态数据

如前所述，在 2016 之前的 SQL Server 版本中，您需要自行处理时态数据。即使在 SQL Server 2016 中，您仍然需要处理人工时间或应用程序时间。以下代码演示了如何创建一个表，该表使用 b 和 e 列表示有效性区间，其中区间的开始和结束表示为整数。该表填充了来自 WideWorldImporters.Sales.OrderLines 表的演示数据：

```sql
USE tempdb;
GO
SELECT OrderLineID AS id,
```

```text
StockItemID * (OrderLineID % 5 + 1) AS b,
LastEditedBy + StockItemID * (OrderLineID % 5 + 1) AS e
INTO dbo.Intervals
FROM WideWorldImporters.Sales.OrderLines;
-- 231412 rows
GO
ALTER TABLE dbo.Intervals ADD CONSTRAINT PK_Intervals PRIMARY KEY(id);
CREATE INDEX idx_b ON dbo.Intervals(b) INCLUDE(e);
CREATE INDEX idx_e ON dbo.Intervals(e) INCLUDE(b);
GO
```

另请注意所创建的索引。这两个索引非常适合按区间的开始或结束进行搜索。您可以使用以下代码检查所有区间的最小开始和最大结束：

```sql
SELECT MIN(b), MAX(e)
FROM dbo.Intervals;
```

您可以在结果中看到，最小开始时间点为 1，最大结束时间点为 1155。现在您需要为这些区间赋予一些时间上下文。在本例中，一个时间点代表一天。以下代码创建了一个日期查找表并向其中填充数据。请注意，起始日期是 2014 年 7 月 1 日：

```sql
CREATE TABLE dbo.DateNums
(n INT NOT NULL PRIMARY KEY,
 d DATE NOT NULL);
GO
DECLARE @i AS INT = 1,
 @d AS DATE = '20140701';
WHILE @i <= 1200
BEGIN
INSERT INTO dbo.DateNums
(n, d)
SELECT @i, @d;
SET @i += 1;
SET @d = DATEADD(day,1,@d);
END;
GO
```

现在您可以将 `dbo.Intervals` 表与 `dbo.DateNums` 表连接两次，以便为表示区间开始和结束的整数提供上下文：

```sql
SELECT i.id,
    i.b, d1.d AS dateB,
    i.e, d2.d AS dateE
FROM dbo.Intervals AS i
    INNER JOIN dbo.DateNums AS d1
        ON i.b = d1.n
    INNER JOIN dbo.DateNums AS d2
        ON i.e = d2.n
ORDER BY i.id;
```

上一次查询的简要结果如下：

| id | b   | dateB      | e   | dateE      |
|----|-----|------------|-----|------------|
| 1  | 328 | 2015-05-24 | 332 | 2015-05-28 |
| 2  | 201 | 2015-01-17 | 204 | 2015-01-20 |
| 3  | 200 | 2015-01-16 | 203 | 2015-01-19 |

现在您可以看到哪个整数代表哪一天。

## 优化时态查询

时态查询的问题在于，从表中读取数据时，SQL Server 只能使用一个索引，成功地从一侧仅消除不可能是结果的行，然后扫描其余数据。例如，您需要查找表中与给定区间重叠的所有区间。请记住，当第一个区间的开始小于或等于第二个区间的结束，且第二个区间的开始小于或等于第一个区间的结束时，两个区间重叠，用数学表达式表示就是 $(b_1 \le e_2)\ AND\ (b_2 \le e_1)$。

以下查询搜索与区间 $(10, 30)$ 重叠的所有区间。注意，第二个条件 $(b_2 \le e_1)$ 被改写为 $(e_1 \ge b_2)$ 以便于阅读（表中区间的开始和结束始终位于条件的左侧）。给定或搜索的区间位于表中所有区间时间线的起始处：

```sql
SET STATISTICS IO ON;
DECLARE @b AS INT = 10,
    @e AS INT = 30;
SELECT id, b, e
FROM dbo.Intervals
```

有一种解决方案可以使用单个索引从搜索区间的两侧消除行。下图展示了这一逻辑：

![](img/07d1225e179d5df9f2b2d26b026386c6_185_0.png)

优化时态查询

图中的区间按下边界排序，表示 SQL Server 对 idx_b 索引的使用情况。从给定（搜索）区间的右侧消除区间很简单：只需消除所有开始处比给定区间结束至少大一个单位（更靠右）的区间。您可以在图中用最右侧的虚线标出的边界看到这一点。然而，从左侧消除则更为复杂。为了使用相同的索引（即用于从左侧消除的 idx_b 索引），我需要在查询的 WHERE 子句中使用表中区间的开始。我必须向左侧移动，远离给定（搜索）区间的开始，至少移动表中最长区间的长度，图中用标注标示。在左侧黄色线之前开始的区间不可能与给定的（蓝色/深灰色）区间重叠。

既然我已经知道最长区间的长度是 20，我可以用一种相当简单的方式编写增强查询：

```sql
DECLARE @b AS INT = 570,
    @e AS INT = 590;
DECLARE @max AS INT = 20;
SELECT id, b, e
```

