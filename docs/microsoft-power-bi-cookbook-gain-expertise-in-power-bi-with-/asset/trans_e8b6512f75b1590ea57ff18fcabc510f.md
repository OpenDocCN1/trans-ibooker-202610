
DimProduct 表现已添加到数据集中。

## 另请参阅

- 数据流和自助数据准备简介：https://bit.ly/3uCjNZf
- 创建数据流：https://bit.ly/2RiiGPV
- 配置和使用数据流：https://bit.ly/2RhIVHm
- 数据流最佳实践：https://bit.ly/3cZIIQr
- 数据流的限制和注意事项：https://bit.ly/3t2UChM
- 数据流的高级功能：https://bit.ly/3fZXLLA
- 在 Power BI 中构建机器学习模型：https://bit.ly/3d1XGp7
- 将机器学习（认知服务）与数据流结合使用：https://bit.ly/3fT3nYk

## 结论

Power BI 服务提供了创建和共享其他 BI 对象（如仪表板、应用、流式数据集和数据流）的功能；这些组件可以通过警报、订阅、图像和 Web 内容来丰富和增强。本章构建了 Power BI 仪表板，以提供对关键度量和高影响力业务活动的简单、一目了然的监控。本章还介绍了仪表板的独特功能，例如 Q&A 自然语言查询、数据警报，以及与其他报表类型（如 Excel 和 SSRS）的集成。最后，本章还涵盖了流式数据集和数据流的创建和使用。

日期表应包含每个日历日期的单行记录（无间隙）。鉴于其规模较小（20 年跨度包含最后一年约为 7,600 行），请包含所有必要的历史数据以及三年或更久的未来年份。

## 如何构建维度表

要实现此方案，请执行以下步骤：

- 1. 识别并记录日期维度所需的列，并找出与现有表之间的差距。

  与其他维度表一样，团队通常可以通过附加逻辑随着时间的推移逐步改进其日期维度表。例如，如果绝大多数分析围绕会计日历并在特定粒度下进行，那么可以首先针对这些列。成熟的日期表——包括标准和财务日历、顺序代理列以及逻辑或动态列（如 Fiscal Period Status）——通常非常宽，有 40 多列。必需的日期维度列包括以下内容：

  - 适用于标准和财务日历的 Prior Year Date 和 Prior Period Date 列
  - 所有级别的自然层次结构属性，例如 2013、2013-Q3、2013-Sep、2013-Wk39 和 9/25/2013

  自然层次结构（每个子值对应一个父值）允许用户通过向下钻取和下一级命令轻松导航 Power BI 报表视觉对象。如果没有自然层次结构，则在钻取到给定父级时会丢失父值的上下文。例如，向下钻取到 2017 年将显示每个月的名称，但与此月关联的年份只能通过将鼠标悬停在图表上在工具提示中看到。有了自然层次结构，此上下文不会丢失，因为 Calendar Yr-Mo 列可能包含诸如 2017-Aug 之类的值。

  Power BI 提供了一种通过层次结构钻取功能中的“将所有级别向下展开一级”来处理报表视觉对象中非自然层次结构的方法。但是，当使用多个层次结构级别时，这可能导致轴标签过于密集。考虑到日期智能需求以及其他可视化的需求，建议构建日期表以支持自然层次结构。在构建自然层次结构时，请考虑构建以下支持列：

  - 一个整数列，对应于每个字符串列的按时间顺序，例如 Weekday。例如，一个 Weekday Number 列，其值为 1 到 7，将设置 Weekday 的“排序依据”属性。
  - 多个“X in Y”列，例如 Day in Year、Day in Month、Week in Month 和 Week in Year，存储为整数。
  - 指示器列，例如 Weekday Indicator、Holiday Indicator 和 Working Day Indicator。这些列的值应适合报表使用，例如 Holiday 和 Non-Holiday。
  - 针对所支持的不同粒度的开始和结束日期列，例如 Week Ending Date 和 Period Ending Date。

- 2. 考虑将其他日历（如期间或会计日历）集成到源数据库中的同一日期表中。可以创建不同的视图以支持角色扮演维度。参见“还有更多……”一节。

- 3. 识别现有报表中或业务用户在 Excel 或 Power BI 中进行的常见日期操作，并考虑添加日期维度列以消除或简化此工作。

- 4. 确保日期列（例如，Prior Year Date）和数字列（例如 Calendar Year）分别存储为日期和整数数据类型，因为这允许算术和 DAX 函数（如 MAX 和 MIN）在不进行任何类型转换的情况下运行。

  如果日期维度表作为数据仓库提取-转换-加载（ETL）过程的一部分每天更新，则标识当前期间和先前期间的列（如 IsCurrentFiscalPeriod 和 IsPrior60Days）也很常见。

- 5. 考虑向关系数据库中存储的日期表添加两列：一个自然层次结构字符串（例如 2009-Jan）和一个整数列（例如 Calendar Year Month Number (200901)）。

- 6. 创建一个包含三列的表：Calendar Year、Calendar Month Number 以及一个增量值为 1 的标识列 Calendar Yr Mo Index：

```sql
Create Table dbo.TempTblYearMo
( [Calendar Year] int not null
, [Calendar Month Number] tinyint not null
, [Calendar Yr Mo Index] smallint identity(1,1) not null)
```

- 7. 执行一条 INSERT INTO SQL 语句以加载此表。

- 8. 对现有日期表中的 Calendar Year 和 Calendar Month Number 列执行 SELECT、GROUP BY 和 ORDER BY。

  SELECT 语句的 ORDER BY 子句应按 Calendar Year，然后按 Calendar Month Number 排序。临时表的索引列（Calendar Yr Mo Index）现在按月跨年按顺序填充，如图 6.1 所示。

| Calendar Year | Calendar Month Number | Calendar Yr Mo Index |
| 2016 | 11 | 95 |
| 2016 | 12 | 96 |
| 2017 | 1 | 97 |
| 2017 | 2 | 98 |

图 6.1：通过 INSERT INTO 语句加载的 TempTblYearMo 表

- 9. 执行一条 UPDATE 语句，用标识值填充 `Calendar Year Month Number` 列：

```sql
UPDATE DBO.DimFinDateTestTbl
SET [Calendar Year Month Number] = T.[Calendar Yr Mo Index]
FROM
DBO.DimFinDateTestTbl as D INNER JOIN dbo.TempTblYearMo as T
ON D.[Calendar Year] = T.[Calendar Year] AND D.[Calendar Month Number] = T.[Calendar Month Number]
```

图 6.2 中的以下查询显示了日期表中依次递增的 `Calendar Year Month Number` 列。

| Date | Calendar Yr-Mo | Calendar Year Month Number |
| 2017-12-30 | 2017-Dec | 108 |
| 2017-12-31 | 2017-Dec | 108 |
| 2018-01-01 | 2018-Jan | 109 |
| 2018-01-02 | 2018-Jan | 109 |

图 6.2：更新了 Calendar Year Month Number 列的日期表

- 10. 对其他自然层次结构列（如 Year-Qtr 和 Year-Wk）重复此过程，然后删除临时表。

- 11. 加载到模型后，应将代理列从报表视图中隐藏，并用作“排序依据”列。

- 12. 按照*创建层次结构和组*中的方法（在*第 3 章 构建 Power BI 数据模型*中介绍）在字段列表中实现层次结构。

SQL 窗口函数有助于创建某些“X in Y”日期维度列。例如，以下 DENSE_RANK 函数返回给定日历月份的日历周编号：

```sql
OVER(PARTITION BY D.[CALENDAR YEAR MONTH NUMBER] ORDER BY D.[Calendar Week Number in Year])
```

对于 DirectQuery 数据模型（其中定义模型表的 SQL 查询在运行时执行），最好将尽可能多的数据转换逻辑移回源系统。复杂的 SQL 查询、DAX 计算列和 M 查询表达式逻辑都可能导致低效的查询计划，从而对 DirectQuery 解决方案的性能产生负面影响。
