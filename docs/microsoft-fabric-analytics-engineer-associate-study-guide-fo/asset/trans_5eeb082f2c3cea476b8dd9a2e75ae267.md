

Weather 表正在按 State 聚合 DamageCrops 的总和：

```
Weather
| summarize sum(DamageCrops) by State
```

图 4-33 显示了此脚本的输出。

| State | sum_DamageCrops |
|---|---|
| NORTH CAROLINA | 149,144,000 |
| WISCONSIN | 32,864,100 |
| NEW YORK | 10,000 |
| ALASKA | 0 |
| DELAWARE | 0 |
| OKLAHOMA | 1,087,000 |
| INDIANA | 26,462,000 |
| ILLINOIS | 90,116,500 |
| MINNESOTA | 5,334,500 |
| TEXAS | 35,373,000 |
| SOUTH DAKOTA | 5,000 |
| UTAH | 0 |

图 4-33. 在 KQL 中使用 summarize 运算符的结果

你还会注意到，新列名是自动生成的，基于聚合函数、下划线（_）和表名。你可以通过在聚合函数前输入想要的名称来指定新列名，如以下示例所示：

```
Weather
| summarize SumOfDamageCrops = sum(DamageCrops) by State
```

你可以在图 4-34 中看到新的列名。

| State | SumOfDamageCrops |
|---|---|
| NORTH CAROLINA | 149,144,000 |
| WISCONSIN | 32,864,100 |
| NEW YORK | 10,000 |
| ALASKA | 0 |
| DELAWARE | 0 |
| OKLAHOMA | 1,087,000 |
| INDIANA | 26,462,000 |
| ILLINOIS | 90,116,500 |
| MINNESOTA | 5,334,500 |
| TEXAS | 35,373,000 |
| SOUTH DAKOTA | 5,000 |
| UTAH | 0 |

图 4-34. 在 KQL 中聚合函数中指定新的列名

使用 KQL 的好处之一是可以将多个聚合函数添加到同一行代码中。如果你想同时找到 DamageCrops 列的总和和最大值，可以使用以下示例中的脚本：

```
Weather
| summarize
    SumOfDamageCrops = sum(DamageCrops),
    MaxOfDamageCrop = max(DamageCrops)
    by State
```

图 4-35 显示了此脚本的结果。

| State | SumOfDamageCrops | MaxOfDamageCrop |
|---|---|---|
| NORTH CAROLINA | 149,144,000 | 10,000,000 |
| WISCONSIN | 32,864,100 | 10,000,000 |
| NEW YORK | 10,000 | 10,000 |
| ALASKA | 0 | 0 |
| DELAWARE | 0 | 0 |
| OKLAHOMA | 1,087,000 | 500,000 |
| INDIANA | 26,462,000 | 3,030,000 |
| ILLINOIS | 90,116,500 | 3,450,000 |
| MINNESOTA | 5,334,500 | 750,000 |
| TEXAS | 35,373,000 | 13,000,000 |
| SOUTH DAKOTA | 5,000 | 5,000 |
| UTAH | 0 | 0 |
| COLORADO | 320,000 | 100,000 |

图 4-35. 使用 KQL 在同一行中实现多个聚合函数

## 创建连接

在 KQL 中创建连接的过程与我们在第 3 章中解释连接理论和 Venn 图时介绍的过程相同。然而，KQL 中有三种额外的连接类型：ANTI JOIN、INNERUNIQUE JOIN 和 SEMI JOIN。

首先，我们将向你介绍这三种新的连接类型，然后，我们将带你了解在 KQL 中编写和执行连接的方法：

### ANTI JOIN

这与第 3 章中的 LEFT JOIN 结合只取右表中因与左表中的行不匹配而为空的行过滤器相同。在 KQL 中，你可以用 ANTI JOIN 的一个命令来完成此操作。图 4-36 是此连接的 Venn 图。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_245_0.png)

图 4-36. KQL 中的 ANTI JOIN

### INNERUNIQUE JOIN

如果未声明其他内容，这是 KQL 中的默认连接类型。 （参见本章后面关于默认连接和声明的内容。）此连接会从左表中删除重复的键，并且仅返回两个表中存在匹配的行。

图 4-37 说明了这一点。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_246_0.png)

图 4-37. KQL 中的 INNERUNIQUE JOIN

### SEMI JOIN

这有点像正常的 INNER JOIN，但区别在于引擎会自动仅返回右表的列。这对于根据左表中的行过滤右表中的数据很有用，但左表仅作为引用表，并且在数据集中不发挥其他作用。

图 4-38 说明了这一点。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_247_0.png)

图 4-38. KQL 中的 SEMI JOIN

你还可以将连接类型组合在 KQL 中以创建额外的连接类型。例如，LEFTANTISEMI 是一个仅返回右表列的左 ANTI JOIN。你可以在本节后面看到可用连接类型的完整列表。

## 连接方法

现在我们已经向你介绍了 KQL 中的新连接类型，我们可以为你提供有关在脚本中编写和使用不同连接运算符的语法的一般介绍。

首先，你可以在 KQL 中使用两种不同的方法执行连接操作：join 和 lookup。你将根据连接中表的特征在两者之间进行选择，我们将分别介绍每种方法。

### *连接方法*

当右表在行数上明显长于左表时应使用此方法。例如，假设你有一个记录数百万事件数据的表，并且你需要从包含数十万台设备数据的引用表中向其添加列。你应该使用连接方法，因为表在长度上没有显著差异。

要使用连接方法，你需要指定连接类型（就像在 T-SQL 方法中一样），但你必须使用此处列出的连接类型之一：

- innerunique
- inner
- leftouter
- rightouter
- fullouter
- leftsemi
- leftanti, anti, or leftantisemi
- rightsemi
- rightanti or rightantisemi

在语法中编写连接类型时，你可以选择两种语法之一。

- 如果连接两侧的列名相同，可以使用此语法：

```
<LeftTableName>
| join kind=<jointype> <RightTableName> on <ColumnName>
```

这里有一个例子：

```
Weather
| join kind=innerjoin Locations on LocationId
```

- 如果连接两侧的列名不相同，则需要使用此语法：
  <LeftTableName>
  | join kind=<jointype> <RightTableName>
    on $left.<ColumnNameLeftTable> = $right.
  <ColumnNameRightTable>

这里有一个例子：

```
Weather
| join kind=innerjoin Locations on $left.LocationId =
$right.LocationKey
```

请注意，$left 和 $right 表引用必须始终按此顺序：$left 在前，$right 在后。如果你将 $right 作为第一个引用，你将收到执行错误。

### 查找方法

当右表在行数上明显短于左表时应使用此方法。例如，假设你有一个基于事实的表，其中包含来自全市流式发出设备的温度和湿度等数据，并且你需要从你自己办公楼的设备列表中向其添加列。由于你办公楼的设备列表明显短于事实表，因此你应该使用查找方法。

要使用查找方法，你必须在两个表中具有相同的列名，因为你没有显式声明连接列的选项。

查找的语法如下：

```
<LeftTableName>
| lookup <RightTableName> on <ColumnName>
```

这里有一个例子：

```
Weather
| lookup Locations on LocationId
```

现在我们已经介绍了 KQL 中的连接方法，你就可以开始创建和完善自己的 KQL 查询了。

## 创建可视化

KQL 脚本语言的一个非常惊人的功能是能够直接从脚本创建可视化，而无需执行任何额外步骤或使用特殊的可视化应用程序。KQL 语言为直接从 KQL 代码中可视化数据提供了内置功能。这种方法不如你在 Power BI 中看到的那样广泛和可定制，因为 KQL 引擎是为在大量数据上实现速度而构建的，可视化必须足够快以基于查询的结果集进行渲染。

现在，不再进一步讨论 Power BI 与 KQL 中可视化的差异，让我们直接深入了解如何基于 KQL 语言创建可视化。

这就是 render 查询运算符发挥作用的地方。此运算符是直接在 KQL 脚本中从结果集可视化数据的关键。

此示例脚本基于查询渲染条形图：

```
Weather
| summarize SumDamageProperty = sum(DamageProperty) by State
| take 10
```
