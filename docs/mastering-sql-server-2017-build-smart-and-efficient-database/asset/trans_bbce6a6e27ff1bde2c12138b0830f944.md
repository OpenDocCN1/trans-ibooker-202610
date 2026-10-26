

再次注意，`ValidFrom` 和 `ValidTo` 列中的值以简短格式显示以提高清晰度。历史表中 `ValidTo` 列中的值与当前表中 `ValidFrom` 值相同；没有间隙。

![](img/07d1225e179d5df9f2b2d26b026386c6_209_0.png)

开始和结束时间段列以 UTC 时区存储时间！

现在，假设你在第二天将价格降至 180。以下是此操作的代码：

```sql
UPDATE dbo.Product SET Price = 180.00 WHERE ProductId = 1; -- on 29th November
```

执行前述语句后，当前表的状态如下：

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 180 | 29.11.2017 | 31.12.9999 |

执行前述语句后，历史表的状态如下：

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 150 | 12.11.2017 | 28.11.2017 |
| 1 | Fog | 200 | 28.11.2017 | 29.11.2017 |

你可以看到历史表中又有一条记录，表明价格 200 有效期为一天。如果你在 11 月 30 日再次执行相同的语句会怎样？没有实际变化；业务逻辑属性未发生变化，但这对时间表意味着什么？以下是此操作的代码：

```sql
UPDATE dbo.Product SET Price = 180.00 WHERE ProductId = 1; -- on 30th November
```

执行前述语句后，当前表的状态如下：

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 180 | 30.11.2017 | 31.12.9999 |

执行前述语句后，历史表的状态如下：

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 150 | 12.11.2017 | 28.11.2017 |
| 1 | Fog | 200 | 28.11.2017 | 29.11.2017 |
| 1 | Fog | 180 | 29.11.2017 | 30.11.2017 |

正如你在历史表中看到的，即使当前表中属性没有实际变化，也会创建历史表中的条目，并且会更新时间段日期列。

![](img/07d1225e179d5df9f2b2d26b026386c6_210_0.png)

你可以将此视为一个错误，但尽管没有属性被更改，如果你使用时间表进行审计，你可能希望在主表中看到所有数据操作尝试。

以下是时间表中单行 UPDATE 如何影响当前表和历史表：

- **当前表**：当前表中的值更新为 `UPDATE` 语句提供的值，时间段开始日期列设置为系统日期，时间段结束日期列设置为 `DATETIME2` 数据类型的最大值。
- **历史表**：更新前的当前表中的行被复制到历史表中，并且仅将时间段结束日期列设置为系统日期。

你还可以看到历史表中行的日期没有间隙。甚至可能出现重复；历史表没有约束来防止它们！因此，可能存在具有相同时间段列值的同一行的多条记录。而且，时间段列中的值甚至可能相同！唯一强制执行的约束是，表示时间段结束日期列的日期不能早于表示时间段开始日期的日期（因此，可以保证 `ValidFrom <= ValidTo`）。

## 在时间表中删除数据

你最终将从当前表中删除该行以演示 DELETE 语句如何影响时间表。以下是此操作的代码：

```sql
DELETE FROM dbo.Product WHERE ProductId = 1; -- on 15th December
```

执行前述语句后，当前表的状态如下：

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|

执行前述语句后，历史表的状态如下：

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 150 | 12.11.2017 | 28.11.2017 |
| 1 | Fog | 200 | 28.11.2017 | 29.11.2017 |
| 1 | Fog | 180 | 29.11.2017 | 30.11.2017 |
| 1 | Fog | 180 | 30.11.2017 | 15.12.2017 |

正如你所预期的那样，当前表中没有行，但历史表中又添加了一行。针对时间表中的单行执行 DELETE 语句后：

- **当前表：** 该行已被删除。
- **历史表：** 删除前的当前表中的行被复制到历史表中，并且仅将时间段结束日期列设置为系统日期。

利用这个机会清理本节中创建的时间表：

```sql
ALTER TABLE dbo.Product SET (SYSTEM_VERSIONING = OFF);
ALTER TABLE dbo.Product DROP PERIOD FOR SYSTEM_TIME;
DROP TABLE IF EXISTS dbo.Product;
DROP TABLE IF EXISTS dbo.ProductHistory;
```

## 查询 SQL Server 2017 中的时间数据

系统版本化表主要用于跟踪历史数据更改。对系统版本化表的查询通常倾向于检索在给定时间点或在任意两个给定时间点之间的表内容。

正如你所见，Microsoft 已根据 SQL:2011 标准实现它们，这意味着存在两个物理表：一个包含实际数据的表和一个历史表。为了简化对时间表的查询，SQL:2011 标准引入了一个新的 SQL 子句 `FOR SYSTEM_TIME`。此外，还添加了一些新的时间特定子子句。SQL Server 2017 不仅实现了这些扩展，还添加了另外两个扩展。以下是你可以在 SQL Server 2017 中用于查询时间数据的完整子句和扩展列表：

- `FOR SYSTEM_TIME AS OF`
- `FOR SYSTEM_TIME FROM TO`
- `FOR SYSTEM_TIME BETWEEN AND`
- `FOR SYSTEM_TIME CONTAINED_IN`
- `FOR SYSTEM_TIME ALL`

### 在特定时间点检索时间数据

当你想要检索在给定时间点有效的历史数据时，结果集可能同时包含实际数据和历史数据。例如，以下查询将返回 `WideWorldImporters` 示例数据库中 `Application.People` 时间表中在 2013 年 5 月 20 日上午 8 点有效的所有行：

```sql
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo
FROM Application.People WHERE ValidFrom <= '20130520 08:00:00' AND ValidTo
> '20130520 08:00:00'
UNION ALL
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo
FROM Application.People_Archive WHERE ValidFrom <= '20130520 08:00:00' AND
ValidTo > '20130520 08:00:00';
```

查询返回 1,060 行。对于单个人员，只返回一行：实际行或历史行之一。行是有效的，如果其开始日期早于或等于给定日期且其结束日期大于给定日期。

可以使用带有 `AS OF` 子子句的新的 `FOR SYSTEM_TIME` 子句来简化前面的查询。以下是使用时间 Transact-SQL 扩展的相同查询：

```sql
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo
FROM Application.People FOR SYSTEM_TIME AS OF '20130520 08:00:00';
```

当然，它返回相同的结果集，并且执行计划相同，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_213_0.png)

在内部，查询处理器会触及两个表并检索数据，但使用时间扩展的查询看起来更简单。

针对时间表的点查询的特殊情况是指定当前日期作为时间点的查询。以下查询返回同一时间表的实际数据：

```sql
DECLARE @Now AS DATETIME = SYSUTCDATETIME();
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo
FROM Application.People FOR SYSTEM_TIME AS OF @Now;
```

前一个查询在逻辑上等同于以下查询：

```sql
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo
FROM Application.People;
```

