

以下是结果：

```
Msg 13609, Level 16, State 4, Line 15
JSON text is not properly formatted. Unexpected character 'N' is found at position 417
```

| key | value | type |
|---|---|---|
| Guitar | David Gilmour | 1 |
| Bass Guitar | Roger Waters | 1 |
| Keyboard | Richard Wright | 1 |

你可以看到一条错误消息，但同时也返回了表。该表包含三行，因为复杂 Member 属性的前三个属性符合 JSON 规范。取而代之的是第四行生成了一条错误消息，并且第五行也没有显示，尽管它格式良好。

如果 JSON 路径表达式指向一个标量值或不存在的属性会怎样？在默认的 JSON 路径模式（lax）下，查询将返回一个空表；并且当你指定 strict 模式时，除了空表之外，还会显示一条错误消息（会引发批处理级异常），如下例所示：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish you Were Here",
"Year":1975,
"IsVinyl":true,
"Songs":[{"Title":"Shine On You Crazy Diamond","Writers":"Gilmour, Waters, Wright"},
{"Title":"Have a Cigar","Writers":"Waters"},
{"Title":"Welcome to the Machine","Writers":"Waters"},
{"Title":"Wish You Were Here","Writers":"Gilmour, Waters"}],
"Members":{"Guitar":"David Gilmour","Bass Guitar":"Roger Waters", "Keyboard":"Richard Wright", "Drums":"Nick Mason"}
}';
SELECT * FROM OPENJSON(@json, N'$.Members.Guitar');
SELECT * FROM OPENJSON(@json, N'$.Movies');
```

两个查询都返回一个空表：

| key | value | type |

使用 strict 选项的相同调用会产生错误消息：

```
SELECT * FROM OPENJSON(@json, N'strict $.Members.Guitar');
```

前一个查询的结果是第一个错误消息：

```
Msg 13611, Level 16, State 1, Line 12
Value referenced by JSON path is not an array or object and cannot be opened with OPENJSON
```

前一个示例中的第二个查询：

```
SELECT * FROM OPENJSON(@json, N'strict $.Movies');
```

前一个查询的结果是第二个错误消息：

```
Msg 13608, Level 16, State 3, Line 13
Property cannot be found on the specified JSON path.
```

你可以使用 OPENJSON，不仅可以将 JSON 数据转换为表格格式，还可以实现一些非 JSON 相关任务。

## 处理逗号分隔列表的值

以下代码示例演示了如何使用 OPENJSON 行集函数返回以逗号分隔列表提供的 ID 的订单详细信息：

```
USE WideWorldImporters;
DECLARE @orderIds AS VARCHAR(100) = '1,3,7,8,9,11';
SELECT o.OrderID, o.CustomerID, o.OrderDate
FROM Sales.Orders o
INNER JOIN (SELECT value FROM OPENJSON('[' + @orderIds + ']') ) x ON x.value= o.OrderID;
```

以下是前一个查询生成的订单列表：

| OrderID | CustomerID | OrderDate |
|---------|------------|-----------|
| 1 | 832 | 2013-01-01 |
| 3 | 105 | 2013-01-01 |
| 7 | 575 | 2013-01-01 |
| 8 | 964 | 2013-01-01 |
| 9 | 77 | 2013-01-01 |
| 11 | 586 | 2013-01-01 |

在此示例中，输入参数用方括号括起来以创建正确的 JSON 文本，并且在不带路径参数的情况下调用 OPENJSON 函数。OPENJSON 为 JSON 数组中的每个元素创建一行，这正是你所需要的。

## 返回两个表行之间的差异

另一个可以使用 `OPENJSON` 的示例是返回表中两个行之间的差异。例如，当你将不同环境的应用程序设置放在数据库表中时，你可能需要知道两个环境之间的设置有何不同。你可以通过比较每列中的值来实现此任务，但如果表有很多列，这可能会很麻烦且容易出错。

以下示例返回 SQL Server 2017 实例中 **master** 和 **model** 数据库的数据库设置差异：

```
SELECT
    mst.[key],
    mst.[value] AS mst_val,
    mdl.[value] AS mdl_val
FROM OPENJSON ((SELECT * FROM sys.databases WHERE database_id = 1 FOR JSON
AUTO, WITHOUT_ARRAY_WRAPPER)) mst
INNER JOIN OPENJSON((SELECT * FROM sys.databases WHERE database_id = 3 FOR
JSON AUTO, WITHOUT_ARRAY_WRAPPER)) mdl
ON mst.[key] = mdl.[key] AND mst.[value] <> mdl.[value]
```

以下是这两个数据库的值不同的列列表。

| key | mst_val | mdl_val |
|---|---|---|
| name | master | model |
| database_id | 1 | 3 |
| snapshot_isolation_state | 1 | 0 |
| snapshot_isolation_state_desc | ON | OFF |
| recovery_model | 3 | 1 |
| recovery_model_desc | SIMPLE | FULL |
| is_db_chaining_on | true | false |
| target_recovery_time_in_seconds | 0 | 60 |

这非常方便且高效；你不需要知道或编写很多带有列名的 OR 语句。例如，在此示例中使用的系统视图（`sys.databases`）中有 78 列，在关系型 Transact-SQL 语句中，你需要在 `WHERE` 子句中全部包含它们。

## 带显式模式的 OPENJSON

如果需要在默认提供的格式上获得更多控制，你可以显式指定自己的模式。该函数仍然会返回一个表，但列由你定义。要指定结果表模式，请使用 `OPENJSON` 函数的 `WITH` 子句。以下是带显式模式的 `OPENJSON` 函数的语法：

```
```sql
OPENJSON( jsonExpression [ , path ] )
[
    WITH (
        column_name data_type [ column_path ] [ AS JSON ]
        [ , column_name data_type [ column_path ] [ AS JSON ] ]
        [ , . . . n ]
    )
]
```
```

使用 `WITH` 子句时，你需要指定至少一列。对于每列，你可以指定以下属性：

- `column_name`：这是输出列的名称。
- `data_type`：这是输出列的数据类型。
- `column_path`：这是使用 JSON 路径表达式指定的输出列的值（它可以是 JSON 属性或数组元素的 value）；此参数是可选的。
- `AS JSON`：使用此选项指定列路径中引用的属性表示对象或数组；此参数是可选的。

理解函数工作原理的最佳方法是查看示例。以下代码显示了如何将 JSON 属性提取为列，并将其值作为 JSON 原始数据类型的行：

```
```sql
DECLARE @json NVARCHAR(MAX) = N'{
  "Album":"Wish You Were Here",
  "Year":1975,
  "IsVinyl":true,
  "Songs":[{"Title":"Shine On You Crazy Diamond","Writers":"Gilmour, Waters, Wright"},
    {"Title":"Have a Cigar","Writers":"Waters"},
    {"Title":"Welcome to the Machine","Writers":"Waters"},
    {"Title":"Wish You Were Here","Writers":"Gilmour, Waters"}],
  "Members":{"Guitar":"David Gilmour","Bass Guitar":"Roger Waters","Keyboard":"Richard Wright","Drums":"Nick Mason"}
}';
SELECT * FROM OPENJSON(@json)
WITH
```

```
(
    AlbumName NVARCHAR(50) '$.Album',
    AlbumYear SMALLINT '$.Year',
    IsVinyl    BIT '$.IsVinyl'
);
```

前一个操作的结果是一个由 WITH 语句定义的表：

| AlbumName | AlbumYear | IsVinyl |
|---|---|---|
| Wish You Were Here | 1975 | 1 |

你可以添加第四列来显示乐队成员。以下是代码：

```
SELECT * FROM OPENJSON(@json)
WITH
(
    AlbumName NVARCHAR(50) '$.Album',
    AlbumYear SMALLINT '$.Year',
    IsVinyl   BIT '$.IsVinyl',
    Members   VARCHAR(200) '$.Members'
);
```

以下是结果表：

| AlbumName | AlbumYear | IsVinyl | Members |
|---|---|---|---|
| Wish You Were Here | 1975 | 1 | NULL |

结果可能出乎意料，但 Members 属性的值是一个对象，因此函数返回 NULL，因为 JSON 路径处于默认的 lax 模式。如果指定了 strict 模式，返回的表将为空并且会引发错误。要解决此问题并显示 Members 属性的值，你需要使用 AS JSON 选项告知 SQL Server 期望的数据已正确格式化为 JSON，如以下代码所示：

```
SELECT * FROM OPENJSON(@json)
WITH
(
    AlbumName NVARCHAR(50) '$.Album',
    AlbumYear SMALLINT '$.Year',
    IsVinyl   BIT '$.IsVinyl',
    Members   VARCHAR(MAX) '$.Members' AS JSON
);
```

