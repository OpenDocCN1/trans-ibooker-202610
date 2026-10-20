

## 计数行数

要统计结果集中的行数，可以使用如下所示的 count 操作符：

```
Weather
| count
```

此脚本将给出结果集（见图 4-23）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_229_0.png)

图 4-23. KQL 中 count 操作符的结果集

## 获取数据子集的行和前 n 行

你可以使用 take 操作符从结果集中获取任意数据子集。这会给出 Kusto 引擎从数据库读取的前 n 行数据。以下是使用 take 操作符的示例：

```
Weather
| take 5
```

此脚本将给出类似于图 4-24 所示的结果集。你可能得不到相同的结果集

因为你的数据在数据库磁盘上的物理结构可能不同。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_230_0.png)

图 4-24. KQL 中 take 操作符的结果集

TIP

在 KQL 中，take 操作符有一个名为 limit 的同义词，其工作方式完全相同。

如果你想要基于列排序的特定 n 行，可以在脚本中添加 by <columnname> <可选排序类型 (desc 或 asc)>，如下所示：

```
Weather
| take 5 by EpisodeId
```

这会给出图 4-25 所示的结果集。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_230_1.png)

图 4-25. KQL 中带列排序的 take 操作符的结果集

请注意，EpisodeId 列现在按默认降序（desc）排序。要更改它，请在行末添加 asc。

## 获取列子集数据

要选择数据的列子集，你有两个函数可选：

- project
- project-away

project 函数可主动选择结果集中所需的列。以下是使用它的示例：

```
Weather
| project StartTime, EpisodeId, EventType
```

此示例脚本仅返回列出的列：StartTime、EpisodeId 和 EventType。图 4-26 显示了结果的子集。

| StartTime | EpisodeId | EventType |
|---|---|---|
| 2007-01-01 00:00:00.000 | 2,592 | Thunderstorm Wind |
| 2007-01-01 00:00:00.000 | 4,171 | Winter Storm |
| 2007-01-01 00:00:00.000 | 4,171 | Winter Storm |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 1,930 | Winter Weather |
| 2007-01-01 00:00:00.000 | 765 | Blizzard |
| 2007-01-01 00:00:00.000 | 1,979 | Heavy Rain |

图 4-26. 使用 project 函数主动选择所需的列

另一方面，使用 project-away 函数，你可以从结果集中移除不需要的列。以下是使用方法：

```
Weather
| project-away StartTime, EpisodeId, EventType, StormSummary,
  EventNarrative, EpisodeNarrative, EndLon, EndLat, BeginLat,
  BeginLon
```

此查询返回图 4-27 中的结果集。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_233_0.png)

图 4-27. 使用 project-away 函数从结果集中移除列

## 获取唯一值列表

要从结果集中获取唯一值列表，我们可以使用 distinct 函数。此函数可在多个列上工作。首先输入 distinct 函数，然后列出所需的列，如此示例所示：

```
Weather
| distinct State, Source
```

此查询返回图 4-28 所示的结果。

| State | Source |
|---|---|
| NORTH CAROLINA | Public |
| WISCONSIN | COOP Observer |
| NEW YORK | Department of Highways |
| ALASKA | Department of Highways |
| DELAWARE | Mesonet |
| OKLAHOMA | Emergency Manager |
| INDIANA | COOP Observer |
| INDIANA | Unknown |
| INDIANA | Other Federal |
| ILLINOIS | Unknown |
| MINNESOTA | Other Federal |
| MINNESOTA | Official NWS Observations |
| TEXAS | Official NWS Observations |
| WISCONSIN | Official NWS Observations |
| SOUTH DAKOTA | Other Federal |
| TEXAS | Unknown |
| UTAH | Other Federal |

图 4-28. 使用 distinct 函数检索多列

另请注意，你可以使用聚合函数来获取不同的值，但然后你需要在其上进行聚合的列。但使用 distinct 函数时，你不需要聚合列即可获取唯一值。

## 过滤结果集

要根据列值过滤结果集，你可以在 KQL 语句中使用 where 操作符，如下所示：

```
Weather
| where State == "FLORIDA"
```

此脚本在 Weather 表上按 State 列进行过滤，仅返回匹配 “FLORIDA” 值的行（见图 4-29）。（请注意，图中的结果已截断。）

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_235_0.png)

在此图中，你可以看到结果集现在仅包含 State 列中包含 FLORIDA 的行。

如果需要更多过滤器，你有两个选择。你可以向现有行添加 and 或 or 子句，或添加新行并使用新的 where 子句进一步过滤结果集。以下是两种示例：

```
Weather
| where State == "FLORIDA" and EventType == "Frost/Freeze"
```

```
Weather
| where State == "FLORIDA"
| where EventType == "Frost/Freeze"
```

这两个脚本返回相同的结果集。如果你想在语句中添加 or，必须在 KQL 语句的一行代码中编写它。

## 排序结果集

你可以按任意给定列对结果集进行排序，无论是表本身的列还是计算列。（稍后将介绍如何创建新列。）

以下示例脚本按 EndTime 列升序对结果集进行排序：

```
Weather
| sort by EndTime asc
```

这将给出图 4-30 所示的结果集（此处已截断）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_236_0.png)

你可以通过使用逗号（,）并输入所需的列名来按其他列添加更多排序。你也可以输入 **asc** 使其按升序排列，或保持降序（desc），这是默认值。

## 基于行的计算

在实现基于行计算的要求时，KQL 脚本语言与 T-SQL 语言非常相似。

例如，在 Weather 表中，你可以使用 extend 操作符添加 DamageCrop 和 DamageProperty 列。此操作符向现有结果集添加新列。以下是示例脚本：

```
Weather
| extend TotalDamage = DamageCrop + DamageProperty
```

此脚本给出图 4-31 所示的结果集。请查看最右边的列。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_237_0.png)

图 4-31. 使用 KQL 中的 extend 操作符添加新列

在前面的示例脚本中，我们对两列做了非常简单的加法。当然，我们可以做更高级的列操作。例如，我们可以使用

extend 操作符替换现有列。为此，我们只需为新列提供与现有列相同的名称。

此示例脚本替换结果集中的 State 列：

```
Weather
| extend State = "Demo state"
```

图 4-32 显示了此脚本的输出。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_238_0.png)

图 4-32. 使用 KQL 中的 extend 操作符替换现有列

## 执行聚合

Kusto 引擎的众多优势之一是其能够极快地对海量数据集执行聚合。summarize 操作符是入门的关键。它使你能够在一行代码中创建聚合，这比在 T-SQL 世界中容易得多，在那里你必须记住添加 GROUP BY 以及所有不在聚合函数中的列。

在 KQL 中，你只需从新行开始输入 summarize，输入所需的聚合函数，输入 by，然后输入需要用作分组列的列名。以下是该示例，其中

