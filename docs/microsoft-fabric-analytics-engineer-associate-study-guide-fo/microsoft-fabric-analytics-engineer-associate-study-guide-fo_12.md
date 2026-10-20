## 第 9 章. 题库

本章包含我们为你编写的问题。它们与你在实际考试中会遇到的问题类型相似，但请记住，我们无法涵盖你在那里可能看到的所有问题类型，而且你也不能仅靠阅读来通过考试。你将被测试的一些内容只能通过经验和亲手使用整个 Microsoft Fabric 套件来学习。因此，这些问题的一些答案在本书中找不到，只能通过动手实践来学习。

还请注意，虽然问题的措辞和结构乍一看可能显得奇怪，但这就是考试中的样子，我们希望帮助你现在就适应，以便在考试当天尽可能做好准备。

## 问题

- 1. 你有一个 Fabric 租户，其中包含一个分配给 Fabric 容量的工作区。你需要创建一个解决方案，让开发人员可以选择使用第三方工具发布自定义 Direct Lake 语义模型。你应该怎么做？
  
  a. 在容量设置中，将 XMLA 端点设置为读写。
  
  b. 在租户设置中启用用户创建项目。
  
  c. 在租户设置中启用用户在 Power BI 服务中编辑数据模型。
  
  d. 在租户设置中，将“允许 XMLA 端点”和“使用本地语义模型在 Excel 中分析”设置为已启用。
  
  e. 在租户设置中启用“发布到 Web”。

- 2. 你正在 Microsoft Power BI Desktop 中创建语义模型，并且你已对 TMDL 格式的语义模型进行了更改。对于 Power BI Desktop 文件，你应该使用哪种文件格式？
  
  a. .pbip
  b. .pbit
  c. .pbix
  d. .pbids

- 3. 在 Fabric Warehouse 中，你有一个名为 FactSales 的表，其中包含十亿行。你运行以下 T-SQL 语句：

```
CREATE TABLE mirror.FactSales AS CLONE OF dbo.FactSales;
```

选择正确的陈述。

  a. 通过仅复制元数据，在 mirror 模式中创建了 dbo.FactSales 的副本。
  b. 对 dbo.FactSales 的额外模式更改也将适用于 mirror.FactSales。
  c. 对 dbo.Factsales 的额外数据更改将不适用于 mirror.FactSales。

- 4. 你在 Fabric 中有一个包含表的湖仓。该表当前未分区，你计划通过使用管道中的复制活动按特定列实现分区。你首先需要做什么？
  
  a. 在“目标”选项卡中，选择分区列。
  b. 在“目标”选项卡中，将模式设置为追加。
  c. 在“目标”选项卡中，将模式设置为覆盖。
  d. 在“源”选项卡中，选择“启用分区发现”。

- 5. 你计划实现从 Azure Store 帐户导入 CSV 文件的摄取。实现必须支持 Power Query（M）语言并可从管道执行。你应该选择哪个活动？
  
  a. 脚本
  b. 数据流
  c. 笔记本
  d. 复制数据

- 6. 你有一个 Fabric 租户，其中包含两个名为 Workspace1 和 Workspace2 的工作区。
Workspace1 包含一个名为 Lakehouse1 的湖仓。Lakehouse1 包含一个名为 dbo.ClientCases 的表。
Workspace2 包含一个名为 Lakehouse2 的湖仓。Lakehouse2 包含一个名为 dbo.Clients 的表。





你需要确保可以编写引用 dbo.ClientCases 和 dbo.Clients 的查询在同一个 SQL 查询中，而无需对表进行额外复制。
你应该使用什么？

+   a. 一个快捷方式
+   b. 一个视图
+   c. 一个数据流
+   d. 一个托管表

7. 你正在使用 Dataflow Gen2 来实现一个解决方案，该解决方案旨在确保在两个表之间执行连接时系统忽略值中的空格。
你应该怎么做？

+   a. 使用查找表。
+   b. 使用模糊匹配。
+   c. 使用模糊分组。
+   d. 使用引用表。

8. 你正在 Dataflow Gen2 中工作，并且想要将两个查询合并为一个。你需要确保查询返回存在于两个表中的所有行。应使用哪种类型的连接？

+   a. LEFT OUTER JOIN
+   b. LEFT ANTI JOIN
+   c. RIGHT OUTER JOIN
+   d. FULL OUTER JOIN

## e. INNER JOIN

9. 你有一个分配给 F-128 容量的 Fabric 工作区。该工作区包含一个名为 DeviceLogs 的 KQL 数据库。

DeviceLogs 包含 25 个名为 c1 到 c25 的列和 100 亿行。列 c1 包含从 1 到 15 的标识符，分布均匀。

你有以下查询：

```
DeviceLogs
| project c1, c2, c3
| where c1 == 1
```

你按如下方式修改查询：

```
DeviceLogs
| where c1 == 1
| project c1, c2, c3
```

查询的执行时间会受到什么影响？

+   a. 执行时间不会改变。
+   b. 执行时间会更短。
+   c. 查询将失败。
+   d. 执行时间会更长。

10. 你有一个名为 Model1 的语义模型。Model1 包含一个名为 Orders 的单个表，具有以下列：

Order Date

Product Name

Quantity

你有一个包含名为 Orders 的页面的报表，该页面包含 Product Name 字段的切片器和显示按 Order Date 的 Quantity 的聚集柱形图。

你需要允许用户在柱形图中在按 Product Name 的 Quantity 和按 Order Date 的 Quantity 之间进行选择。解决方案必须最大限度地减少开发工作量。

你应该怎么做？

+   a. 创建一个度量值。
+   b. 创建一个字段参数。
+   c. 创建一个动态格式字符串。
+   d. 创建一个计算组。

11. 你有一个名为 Warehouse1 的 Fabric 工作区中的仓库。你有一个名为 User1 的用户，他对同一工作区具有查看者角色。你需要将权限委托给 User1 以满足以下要求：

+   - User1 必须能够将仓库共享给其他用户。
+   - 解决方案必须遵循最小权限原则。

你应该为 User1 分配哪个权限？

+   a. 工作区中的成员角色
+   b. 工作区中的管理员角色
+   c. 工作区中的贡献者角色
+   d. 仓库中的 db_owner 角色

12. 你有一个名为 Workspace1 的 Fabric 工作区，其中包含一个湖屋，并且包含一个名为 User1 的用户。

你需要确保 User1 可以读取湖屋中的所有数据，但不能读取工作区中的任何其他项目或数据。你应该执行哪两个操作？

+   a. 使用项目权限共享湖屋。
+   b. 选择“读取所有 Apache Spark 数据。”
+   c. 将 User1 添加到查看者角色。
+   d. 将 User1 添加到贡献者角色。
+   e. 将 User1 添加到成员角色。

13. 你的任务是将 Fabric 工作区连接到 GitHub 仓库。你需要什么信息来完成此操作？

+   a. 一个 PAT 和仓库的 URL
+   b. 一个 SAS 令牌和仓库的分支名称
+   c. 一个访问密钥和仓库的分支名称
+   d. 一个访问密钥和仓库的 URL

14. 你有一个 Fabric 中的工作区，其中包含一个仓库和一个湖屋。

仓库包含一个名为 dbo.FactSalesSmall 的表，湖屋包含一个名为 dbo.FactSalesConsolidated 的 Delta 表。

你的任务是使用低代码解决方案将数据从 dbo.FactSalesSmall 复制到 dbo.FactSales Consolidated。

你应该怎么做？

+   a. 使用笔记本从 Table1 读取数据并将数据插入 Table2。
+   b. 使用 Dataflow Gen2 从 FactSalesSmall 读取数据并将数据插入 FactSalesConsolidated。
+   c. 使用存储过程从 FactSalesConsolidated 读取数据并将数据插入 FactSalesSmall。
+   d. 使用笔记本从 FactSalesConsolidated 读取数据并将数据插入 FactSalesSmall。

15. 在 Fabric 工作区中，你有一个实现了复杂业务逻辑的 Dataflow Gen2。你需要将输出持久化到工作区中的湖屋。你有两个配置目标的选项？

+   a. 配置默认目标。
+   b. 为当前流配置目标。
+   c. 将目标配置为流窗格中的新步骤。
+   d. 使用笔记本配置目标。
+   e. 使用管道配置目标。

16. 你正在 Microsoft Fabric 中工作，并且有一个工作区包含多个报表和一个包含未来收购信息的语义模型。

你需要确保项目的数据不会落入错误的人手中，并且解决方案必须确保工作区中未来项目继承设置。你应该怎么做？

+   a. 规划标签的配置。
+   b. 规划敏感性标签的配置。
+   c. 规划认可的配置。
+   d. 将报表和语义模型拆分为两个工作区。

17. 一名分析工程师正在 Microsoft Fabric 中设计新的数据解决方案。该解决方案将涉及一个经常被访问以进行 T-SQL 和 Spark 查询的中央数据存储。数据工程师需要能够访问半结构化和非结构化数据，并且数据存储还必须支持用户执行 T-SQL 查询时的行级安全性。哪种类型的数据存储最适合此解决方案？

+   a. 一个数据湖
+   b. 一个外部 Hive 元存储
+   c. 一个仓库
+   d. 一个湖屋

18. 你正在创建 Power BI 中的报表可视化，显示客户基于过去 12 个月提交的调查的平均满意度评分。数据需要在源数据更改时立即更新，并且尽可能快。
为了满足此要求，应为语义模型配置哪种存储模式？

+   a. DirectQuery
+   b. Direct Lake
+   c. Import
+   d. Live Connect

19. 一个分析团队需要在语义模型中的一个表上实现行级安全性（RLS），以根据访问报表的用户限制数据可见性。
以下哪个 DAX 函数最常用于定义 RLS 规则？

+   a. FILTER
+   b. LOOKUPVALUE
+   c. USERPRINCIPALNAME
+   d. CALCULATE

20. 你在 Microsoft Fabric 中有一个数据仓库，其中包含一个名为 Stage.Customers 的表，该表从 CRM 系统接收增量更新。你需要编写一个 T-SQL 查询，返回每个客户的最新更新。你应该使用哪个 T-SQL 函数来实现此目的？

+   a. ROW_NUMBER
+   b. RANK
+   c. DENSE_RANK
+   d. GROUP BY

21. 一名数据分析师需要在 Fabric 工作区内创建和共享语义模型与其他数据分析师。该分析师需要对模型和报表拥有完全控制权，并且必须使用最小权限方法。

以下哪个工作区角色将提供必要的权限？

+   a. 管理员
+   b. 成员
+   c. 查看者
+   d. 贡献者

22. 以下哪项描述了语义模型中计算组的用途？

+   a. 通过将用户分组到角色中执行行级安全性（RLS）
+   b. 通过从不同来源分组表创建复合模型
+   c. 对多个度量值进行分组并应用通用的计算修饰符
+   d. 基于单个 DAX 表达式在表中创建新列

23. 一名数据工程师有一个湖屋中的表，其中包含销售数据，但某些记录在 CustomerID 列中具有 NULL 值。使用 Microsoft Fabric 中的数据转换功能，识别和解决这些缺失值的最有效且低代码的方法是什么？

+   a. 创建一个过滤掉 NULL 值的 T-SQL 视图。
+   b. 使用 Python 笔记本用默认值填充 NULL 值。
+   c. 配置语义模型的增量刷新以忽略不良数据。
+   d. 使用 Dataflow Gen2 中的数据分析工具检查列质量并填补缺失值。

24. 以下哪项是 Microsoft Fabric 中部署管道的主要目的？

+   a. 监控报表可视化和查询的性能以帮助优化平台的性能
+   b. 安排语义模型的数据刷新以尽可能保持数据新鲜和最新
+   c. 管理分析解决方案的持续集成和持续部署（CI/CD）
+   d. 为灾难恢复需要创建工作区的副本





25. 一名数据工程师正在为 Microsoft Fabric 中的新分析解决方案设计数据模型。该模型将由各种报表使用，并且必须易于业务用户导航和理解。
对于此目标，哪种模式设计最合适？

+   a. 雪花模式
+   b. 平面模式
+   c. 星型模式
+   d. 规范化模式

26. 你正在创建需要基于用户选择的日期计算上一年度总销售额的 DAX 度量。执行此时间智能计算最有效且推荐的 DAX 函数是什么？

+   a. CALCULATE
+   b. TOTALYTD
+   c. PREVIOUSYEAR
+   d. DATEADD

27. 一名数据分析师正在使用 Microsoft Fabric 中的语义模型构建 Power BI 报表。他们想要创建一个度量来计算上一年度同期总销售额。该度量必须能够尊重用户对报表应用的任何时间筛选器。数据分析师应为此目的使用以下哪个 DAX 函数？

+   a. CALCULATE(SUM('Sales'[SalesAmount]), SAMEPERIODLASTYEAR('Date'[Date]))
+   b. SAMEPERIODLASTYEAR(SUM('Sales'[SalesAmount]))
+   c. CALCULATE(SUM('Sales'[SalesAmount]), DATESYTD(DATEADD('Date'[Date], -1, YEAR)))
+   d. CALCULATE(TOTALYTD(SUM('Sales'[SalesAmount]), SAMEPERIODLASTYEAR('Date'[Date])))

28. 一名数据工程师正在为 Microsoft Fabric 中的湖仓准备来自 SQL 数据库的数据。源表包含一个名为 Price 的列，其中包含一些需要替换为 0 的空字符串值。工程师希望采用低代码解决方案，可以轻松集成到数据管道中。对于数据工程师来说，最合适的操作是什么？

+   a. 编写 Spark 笔记本，使用 PySpark 读取数据并替换值。
+   b. 在数据管道中添加数据流，并使用其转换功能替换空字符串。
+   c. 使用 T-SQL 查询中的 UPDATE 语句替换空字符串。
+   d. 使用 COPY INTO 命令加载数据并处理转换。

29. 一名分析工程师正在设计一个新的 Power BI 语义模型，该模型需要处理来自 Fabric 数据仓库的非常大的数据集。报表必须为聚合和查询提供快速的交互式性能。数据仓库安排在夜间刷新，因此数据新鲜度不是主要问题。

分析工程师应为语义模型选择哪种存储模式？

+   a. 导入
+   b. DirectQuery
+   c. Direct Lake
+   d. Live Connect

30. 一个数据团队使用名为 Analytics 的单个 Microsoft Fabric 工作区进行开发和测试。团队需要将其工作转移到生产工作区，并且还要确保业务用户能够以受控的方式将语义模型和报表从 Analytics 部署到生产。

团队应使用哪种工具？

+   a. 数据管道
+   b. Git 集成
+   c. 部署管道
+   d. 笔记本

31. 你正在为新的分析解决方案创建维度模型。模型包含一个销售事实表和多个维度表，包括供应商和客户。为了提高查询性能并简化业务用户的模型，你希望将供应商和客户维度表合并为一个表。

以下哪种方法最适合实现此目的？

+   a. 通过创建单个宽表将所有维度表的数据合并到销售事实表中，从而对数据进行非规范化。
+   b. 通过合并表并确保所有列都在单个、定义良好的表中，创建符合维度。
+   c. 通过别名其中一个表创建角色扮演维度。
+   d. 在数据仓库中通过连接供应商和客户表创建新视图。

32. 一组数据分析师准备从源数据库转换数据。他们需要创建一个度量来计算每个客户自上次订单以来的天数。团队希望解决方案能够采用逐步的数据转换方法，并且易于调试。

以下哪种工具最适合此任务？

+   a. 带有查找活动的管道
+   b. SQL 查询编辑器中的 T-SQL 脚本
+   c. 使用 Power Query 的 Dataflow Gen2
+   d. 带有 PySpark 的 Spark 笔记本

33. 一名数据工程师在 Power BI 中有一个语义模型，其中销售事实表连接到日期维度表。工程师希望确保只有访问特定产品类别的用户才能查看该类别的销售数据。解决方案必须使用最小权限原则。

以下哪种操作最合适？

+   a. 为每个产品类别创建新的语义模型，并带有筛选后的销售表。
+   b. 在销售事实表上实施对象级安全性 OLS。
+   c. 在销售事实表上实施行级安全性 RLS 以筛选行。
+   d. 在销售事实表上实施列级安全性 CLS 以隐藏 SalesAmount 列。

34. 一名数据工程师正在 Microsoft Fabric 中处理数据仓库。他们需要确保数据加载过程尽可能高效。为此，他们决定使用 MERGE 语句。在此场景中使用 MERGE 语句的主要目的是什么？

+   a. 将两个或多个表连接在一起以生成单个结果集
+   b. 从单个源文件批量加载新数据
+   c. 将 INSERT、UPDATE 和 DELETE 操作合并为单个语句
+   d. 从查询结果创建新表

35. 你正在开发 Power BI 中的新语义模型，并希望使用 DAX 度量来计算特定产品类别的总销售额。为了使 DAX 代码更易读且更易于调试，你希望将中间计算的结果存储在临时变量中。为此应使用哪个 DAX 关键字？

+   a. VAR
+   b. SET
+   c. DEFINE
+   d. LET

36. 一名数据工程师正在为新的分析解决方案设计维度模型。模型包含一个包含 SalesAmount 等度量的事实表，以及包含 ProductCategory 等属性的产品维度表。工程师希望使模型尽可能简单高效，以便于语义模型。

以下哪项最能描述事实表和维度表之间的关系？

+   a. 事实表包含度量，维度表包含主键。
+   b. 事实表和维度表都包含度量和属性。
+   c. 事实表包含主键，维度表包含度量。
+   d. 事实表包含外键和度量，维度表包含属性和主键。

37. 一名数据分析师正在使用 Fabric 数据仓库中的大型销售数据集。

分析师需要应用一系列简单的转换，例如更改列的数据类型、重命名列和删除重复项。

他们希望使用易于重复的低代码方法。

应使用哪种最合适的工具？

+   a. 带有 Python 代码的 SQL 笔记本
+   b. SQL 查询编辑器中的 SQL 查询
+   c. Fabric 数据仓库中的可视化查询编辑器
+   d. 带有复制活动的管道

38. 一名数据工程师正在设计数据流以摄取和转换数据。为了确保数据质量和完整性，他们希望强制执行一条规则，拒绝 ProductID 列值为 NULL 的任何行。数据工程师可以在哪里强制执行此数据质量规则？

+   a. 在数据源的属性中
+   b. 在目标表中的 SQL WHERE 子句中
+   c. 在数据目标的模式中
+   d. 在 Dataflow Gen2 中的数据质量功能中

39. 一名分析工程师正在为大型语义模型设计增量刷新策略。模型包含按月分区的销售事实表。团队希望保留最近三年的数据，并且仅刷新当前月份和上一个月份的数据。分析工程师必须配置哪两个参数才能实现此目的？

+   a. DateFrom 和 DateTo
+   b. RangeStart 和 RangeEnd
+   c. DateTimeFrom 和 DateTimeTo
+   d. RefreshInterval 和 HistoricalPeriod

40. 一名数据分析师需要创建 DAX 度量来计算上个月的销售额。该度量应尊重 Date 表上的任何筛选器。他们应使用以下哪个 DAX 函数？

+   a. SAMEPERIODLASTYEAR
+   b. PREVIOUSMONTH
+   c. TOTALMTD
+   d. DATEADD

41. 一名数据分析师正在使用跨湖仓和外部 Azure SQL 数据库的大型数据集构建 Power BI 报表。为了为报表提供统一视图，他们需要创建一个可以直接查询这两个源而无需导入数据的语义模型。分析师应使用语义模型的哪个功能？

+   a. 直连




42. 数据工程师有一个仓库，其中包含一个包含敏感财务数据（包括 Salary 列）的表。工程师希望确保特定用户组可以看到该表但看不到 Salary 列。数据工程师应实施哪种安全措施？
- a. 动态行级安全性
- b. 列级安全性 (CLS)
- c. 对象级安全性 (OLS)
- d. 行级安全性 (RLS)

43. 数据团队需要分析来自应用程序的流式日志数据。日志数据量大且半结构化，需要快速的临时查询。在 Microsoft Fabric 中，存储和查询此数据的最合适方法是什么？
- a. 将数据摄取到数据仓库并使用 T-SQL 查询
- b. 将数据摄取到湖仓并使用 T-SQL 查询
- c. 将数据摄取到 KQL 数据库并使用 KQL 查询
- d. 将数据存储在数据湖中并使用 Spark 笔记本查询

44. 分析工程师正在 Microsoft Fabric 中设计新的数据解决方案。该解决方案将涉及一个中央数据存储，该存储会频繁被用于 T-SQL 和 Spark 查询。

数据工程师需要能够访问半结构化和非结构化数据，并且数据存储还必须支持执行 T-SQL 查询的用户使用行级安全性。

对于此解决方案，最合适的存储类型是什么？
- a. 数据湖
- b. 湖仓
- c. 仓库
- d. KQL 数据库

45. 你有一个名为 Workspace1 的 Fabric 工作区，其中包含一个名为 Lakehouse1 的湖仓。你需要确保只有 GroupA 的成员可以访问湖仓中的敏感列。该解决方案必须使用内置的 Fabric 治理功能并遵循最小权限原则。

你应该执行哪两个操作？
- a. 对 Lakehouse1 中的敏感列应用敏感性标签。
- b. 在 Lakehouse1 中配置行级安全性。
- c. 在 Lakehouse1 中配置列级安全性。
- d. 将 Lakehouse1 认可为受信任内容。
- e. 使用工作区级访问控制来限制 GroupA。

46. 你有一个名为 Workspace1 的 Fabric 工作区，其中包含一个名为 Warehouse1 的 SQL 仓库和一个名为 Model1 的语义模型。你需要使用第三方工具进行基于代码的 CI/CD 将更新的语义模型更改部署到生产环境。你应该怎么做来部署更改？
- a. 为 Workspace1 配置版本控制。
- b. 使用 XMLA 端点部署语义模型。
- c. 在开发环境和生产环境之间创建部署管道。
- d. 直接从 Power BI 服务发布 Model1。
- e. 在工作区中使用 .pbids 文件。

47. 你有来自多个来源的数据，需要选择支持查询和基于文件的转换的适当 Fabric 存储方法。你应该选择哪种存储选项？
- a. 湖仓
- b. 仓库
- c. 事件中心
- d. SQL 数据库

48. 你想使用 Fabric KQL 和 DAX 分析和查询数据。你应该确保支持哪两个功能？
- a. 支持 DAX 的语义模型
- b. 使用 SQL 的湖仓查询
- c. 使用 KQL 的事件中心查询
- d. 支持 DAX 的数据流
- e. 支持 KQL 的仓库

49. 你有一个 Fabric 工作区，需要将实时流数据摄取到你的分析解决方案中，同时集成 OneLake。你应该使用哪个组件？
- a. 湖仓
- b. 仓库
- c. 事件中心
- d. 数据流
- e. 语义模型

50. 你正在构建语义模型，并希望支持动态格式和计算灵活性。你应该实现哪两个功能？
- a. 计算组
- b. 动态格式字符串
- c. 字段参数
- d. 星型模式关系
- e. 增量刷新

51. 你正在使用一个名为 SalesTransactions 的湖仓表，其中包含数百万行。该表包含名为 TransactionDate、CustomerID 和 Amount 的行。

你需要创建一个视图，该视图过滤掉 Amount 值为 NULL 的交易，并按客户按月计算总金额。

你应如何完成 T-SQL 语句？
- a. 使用 SELECT INTO 和 HAVING 语句创建汇总表。
- b. 创建一个使用 WHERE 语句和 GROUP BY 的 VIEW。
- c. 创建一个使用 GROUP BY 返回结果集的 PROCEDURE。
- d. 使用 MERGE 和 PARTITION BY 将行合并为单个数据集。

52. 数据工程师需要在湖仓中连接两个表：Orders 和 Products。Orders 表包含 ProductID 列，Products 表包含 ProductID 和 Category 列。

数据工程师应使用哪种类型的连接以包含所有订单，即使产品类别缺失？
- a. 内连接
- b. 左外连接
- c. 右连接
- d. 全外连接

53. 你正在设计一个 Dataflow Gen2，该流程从 CSV 文件摄取数据并将其转换成立方体模型。在转换期间，你需要删除重复行并确保列类型一致。

你应该应用哪两个转换步骤？
- a. 从源表中删除重复项。
- b. 将列数据类型更改为与维度模型匹配。
- c. 通过使用模糊匹配合并查询以丰富产品名称。
- d. 用零填充数值列中的缺失值。
- e. 从文本列中删除前导和尾随空格。

54. 你正在构建一个管道，该管道将数据从外部 SQL 数据库摄取到湖仓中。数据包含一个 Price 列，其中包含空字符串和 NULL 值。你希望使用低代码解决方案将空字符串替换为 0，而不影响 NULL 值。

你应该使用什么？
- a. 使用 PySpark 的 Spark 笔记本
- b. Dataflow Gen2 转换
- c. 仓库中的存储过程
- d. 管道中的 T-SQL UPDATE 语句

55. 你正在 Dataflow Gen2 中工作，该流程从 CSV 文件摄取产品数据。该文件包含不一致的格式，包括产品名称中的前导和尾随空格以及作为文本导入的数值列。你需要在加载到湖仓之前清理数据。

你应该做哪两件事？
- a. 从源表中删除重复项。
- b. 从文本列中删除前导和尾随空格。
- c. 用零填充数值列中的缺失值。
- d. 使用模糊匹配合并查询。
- e. 将列数据类型更改为与维度模型匹配。

56. 你正在使用一个包含名为 MeterReadings 的表的 KQL 数据库。你需要按区域分组计算每个行与其前一行之间的读数值的差异。你应该使用哪个 KQL 函数？
- a. prev()
- b. row_rank_min()
- c. summarize()
- d. window_session()

57. TaxiTrips 湖仓表包含数百万行，包括名为 TaxiCompany 和 tripDistance 的列。你的目标是根据行驶总英里数识别前三家出租车公司。如果总英里数相同，你应包含两者或所有并列的公司。

在限制结果之前，你需要使用哪些 T-SQL 语句来计算每个公司的总英里数？
- a. TOP (3)
- b. FETCH NEXT 3 ROWS ONLY
- c. ORDER BY SUM(tripDistance) DESC
- d. GROUP BY TaxiCompany

58. 你正在使用一个名为 ProductSales 的湖仓表，其中包含列 ProductID、SaleDate 和 Revenue。你需要创建一个视图，该视图仅返回当年每个产品的总收入。

你应该使用哪个 T-SQL 子句来过滤数据？
- a. HAVING YEAR(SaleDate) = YEAR(GETDATE())
- b. WHERE YEAR(SaleDate) = YEAR(GETDATE())
- c. GROUP BY YEAR(SaleDate)
- d. FILTER BY CURRENT_YEAR

59. 作为数据工程师，你正在使用包含名为 CustomerTransactions 的表的 Fabric 仓库。你需要创建一个 T-SQL 语句，该语句返回当年每个客户的总交易金额。

你应该使用哪种子句组合？




+   a. GROUP BY CustomerID, WHERE
+       YEAR(TransactionDate) = YEAR(GETDATE())
+   b. GROUP BY YEAR(TransactionDate), HAVING
+       SUM(Amount) > 0
+   c. ORDER BY CustomerID, TOP (100)
+   d. PARTITION BY CustomerID, ROW_NUMBER()

60. 作为数据分析师，您正在准备一份报告，显示每个地区的订单数量。Orders 表包含 OrderID、Region 和 OrderDate 列。Regions 表包含所有可能的地区，包括没有订单的地区。

您需要准备一份报告，显示每个地区的订单数量，并且需要确保包含没有订单的地区。

您应该使用哪种子句组合？

+   a. SELECT Region, COUNT(OrderID) FROM
+       Orders GROUP BY Region
+   b. SELECT r.Region, COUNT(o.OrderID) AS
+       OrderCount FROM Regions r LEFT JOIN
+       Orders o ON r.Region = o.Region GROUP
+       BY r.Region
+   c. SELECT Region FROM Orders WHERE
+       OrderDate IS NOT NULL
+   d. SELECT DISTINCT Region FROM Orders

61. 作为数据工程师，您正在使用包含名为 EnergyReadings 的表的 KQL 数据库。

EnergyReadings 表包含 Area、Timestamp 和 Energy 列。您需要计算每个行与前一行之间的能量使用差异，按区域分别计算，并按时间戳排序。

您应该使用哪条 KQL 语句？

+   a. extend Delta = Energy - prev(Energy) by Area
+   b. summarize Delta = sum(Energy) - sum(prev(Energy)) by Area
+   c. sort by Timestamp desc
+   d. project Area, Energy, Delta

62. 作为数据工程师，您正在使用名为 SalesData 的湖屋表，该表包含 ProductID、SaleDate 和 Revenue 列。您需要创建一个可重用的转换，计算每个产品的月度收入并将结果存储在新表中。

为了以最少的代码和易于调度的方式实现此操作，您应该使用哪种 Fabric 项？

+   a. A Dataflow Gen2
+   b. A Spark notebook
+   c. A SQL stored procedure
+   d. A Copy activity in a pipeline

63. 一个数据管道从多个源摄取数据到湖屋。该过程必须执行以下操作：

+   - Load raw data.
+   - Perform cleansing and transformation.
+   - Populate the dimensional model.

哪种 Fabric 项或项组合可以保证不同任务之间的此顺序？

+   a. A Dataflow Gen2 with multiple transformations
+   b. Spark notebooks chained manually
+   c. A pipeline with activity dependencies
+   d. Dataflow triggers

64. 您正在 Fabric 工作区中工作，该工作区包含一个名为 Lakehouse1 的湖屋。您创建了一个名为 Pipeline1 的数据管道，用于从 Azure Storage 摄取 CSV 文件。您需要在 Pipeline1 中添加一个活动，将数据复制到 Lakehouse1 并支持 Power Query M 转换。

您应该添加哪种类型的活动？

+   a. A Copy data
+   b. A script
+   c. A Dataflow Gen2
+   d. A notebook
+   e. A stored procedure

65. 作为数据分析师，您正在 Power BI 中开发语义模型。您希望简化跨多个度量的时间智能计算，例如年初至今、上个月和季度比较。

在不进行太多额外步骤的情况下，最佳选项是什么？

+   a. Calculation groups
+   b. Field parameters
+   c. Composite models
+   d. Dynamic format strings
+   e. Aggregation tables

66. 您正在为 Fabric 中的新分析解决方案设计维度模型。模型包括一个包含 SalesAmount 等度量的事实表和一个包含 ProductCategory 等属性的产品维度表。您希望确保模型高效且易于在语义模型中使用。
事实表和维度表之间的关系最好如何描述？

+   a. The fact table contains primary keys and measures; the dimension table contains attributes.
+   b. The fact table contains foreign keys and measures; the dimension table contains attributes and a primary key.
+   c. Both tables contain measures and attributes.
+   d. The dimension table contains foreign keys; the fact table contains attributes.
+   e. The fact table contains calculated columns; the dimension table contains hierarchies.

67. 作为数据工程师，您正在从事 Microsoft Fabric 项目中的数据转换工作。您需要确保解决方案针对性能、可维护性和跨多个管道的重用进行了优化。
您应该遵循哪种方法？

+   a. Using default settings and relying on automatic optimization
+   b. Applying best practices for transformation logic and modular design
+   c. Avoiding advanced features to keep the model simple
+   d. Focusing only on visual design and ignoring backend configuration
+   e. Using notebooks for all transformation logic, regardless of complexity

68. 作为数据分析师，您正在为零售公司设计语义模型。模型包括 Sales 事实表和 Date、Product、Store 维度表。您希望确保模型易于在 Power BI 中导航并且性能良好。您应该使用哪种模式设计？

+   a. Snowflake
+   b. Star
+   c. Flat
+   d. Hybrid
+   e. Normalized

69. 您正在开发一个语义模型，其中包含 Import 和 Direct Query 表。您需要在单个报告中组合它们，同时保持性能和灵活性。哪种方法是最好的选择？

+   a. Using a composite model
+   b. Using dual storage mode with an aggregation table
+   c. Using a calculation group
+   d. Using a field parameter
+   e. Using dual storage mode

70. 您正在创建将在多个报告中重用的语义模型。您希望允许报表作者在视觉对象中动态地在不同维度之间切换（例如 Region、Product、Customer）。您应该实现哪些功能？

+   a. Calculation groups
+   b. Field parameters
+   c. Composite models
+   d. Dynamic format strings
+   e. Role-playing dimensions

71. 作为数据分析师，您正在设计将在多个报告中重用的语义模型。您希望确保模型支持一致的逻辑和集中维护，并且避免重复。您应该使用哪种方法？

+   a. Creating a separate semantic model for each report
+   b. Using a single shared semantic model
+   c. Duplicating the model and customizing it per report
+   d. Deploying the model separately for each report
+   e. Using composite models with DirectQuery

72. 您正在为财务报告构建语义模型，其中包含多个度量，其中一些已具有特定格式（例如货币、百分比）。您希望在所有度量上应用一致的格式，而无需重复度量或手动更改每个度量。您将应用什么？

+   a. Calculation groups
+   b. Dynamic format strings
+   c. Field parameters
+   d. Composite models
+   e. Aggregation tables

73. 您正在设计一个语义模型，其中包含 Sales 事实表和 Date 维度表。您需要创建一个度量，计算上一年同期总销售额，并尊重报表中应用的任何筛选器。
您应该使用哪种 DAX 表达式？

+   a. CALCULATE(SUM(Sales[Amount]), PREVIOUSYEAR(Date[Date]))
+   b. SAMEPERIODLASTYEAR(SUM(Sales[Amount]))
+   c. TOTALYTD(Sales[Amount], Date[Date])
+   d. CALCULATE(SUM(Sales[Amount]), SAMEPERIODLASTYEAR(Date[Date]))
+   e. DATEADD(Date[Date], -1, YEAR)

74. 作为数据工程师，您正在设计一个语义模型，其中包含 Customer 维度表和 Sales 事实表。您希望确保只有访问一个客户的用户只能查看该客户的销售数据。

您应该实现哪种安全机制？

+   a. Object-level security (OLS)
+   b. Row-level security (RLS)
+   c. Column-level security (CLS)
+   d. Calculation groups
+   e. Composite models

75. 您正在设计一个语义模型，其中包含 Import 和 DirectQuery 表。您希望允许用户在同一报表中查询两个数据源，同时保持性能。您应该使用什么？

+   a. A composite model and dual storage mode
+   b. Calculation groups and Import mode
+   c. Field parameters and Direct Lake





+   d. 聚合表和 DirectQuery

+   e. 复合模型和动态格式字符串

76. 作为一名数据工程师，您正在设计一个包含 Sales 事实表和 Date 日期维度表的语义模型。您希望允许用户按财政年度、财政季度和财政月份分析销售。财政日历与公历年度不一致。您应该怎么做？

+   a. 使用计算组来定义财政期间。
+   b. 创建自定义财政日历表并将其与事实表关联。
+   c. 使用日历表配合内置的时间智能函数。
+   d. 直接在 Sales 表中添加财政列。
+   e. 使用带有 YTD、QTD 和 MTD 的计算组来模拟财政期间。

77. 作为一名数据工程师，您正在为一家物流公司设计语义模型。模型包含 Shipments 事实表以及 Date、Customer 和 Carrier 维度表。

您希望确保模型在 Power BI 中支持直观的导航和快速性能。应应用哪种设计原则？

+   a. 使用雪花模式规范化所有维度。
+   b. 使用星型模式并使用清晰的关系和代理键。
+   c. 将所有表展平为单个宽表。
+   d. 对所有维度使用复合模型和 Direct Query。
+   e. 在事实表中为所有维度属性创建计算列。

78. 作为一名数据工程师，您正在设计一个包含 Sales 事实表和 Customer 客户维度表的语义模型。您希望确保关系稳定且不受业务键更改的影响。

您应该使用什么来定义关系？

+   a. 自然键
+   b. 复合键
+   c. 代理键
+   d. 事实表中的外键
+   e. 角色扮演维度

79. 您正在设计一个包含 Sales 事实表和 Date 日期维度表的语义模型。用户需要按多种日期类型分析销售，例如订单日期、发货日期和发票日期。

对于此场景，哪种建模方法是最佳实践？

+   a. 角色扮演维度
+   b. 为每种日期类型复制 Date 表
+   c. 使用计算组在日期类型之间切换
+   d. 使用字段参数在视觉对象中选择日期类型
+   e. 使用复合模型组合多个日期表

80. 作为一名数据分析师，您正在创建将由多个部门的非技术用户使用的语义模型。您希望确保模型直观且易于导航，并支持一致的术语。应应用哪三个设计原则？

+   a. 为表和列使用友好名称。
+   b. 隐藏未使用的列和表。
+   c. 为保持一致性使用技术命名约定。
+   d. 为常用钻取路径创建层次结构。
+   e. 避免关系以简化模型。

81. 您正在设计一个包含 Sales 事实表和 Date 日期维度表的语义模型。您希望允许用户在视觉对象中从年份钻取到季度再到月份。您应该实现什么？

+   a. 字段参数
+   b. 计算组
+   c. 层次结构
+   d. 复合模型

## e. 聚合表

82. 作为一名数据工程师，您正在优化包含大型 Sales 事实表的语义模型。该模型用于多个报表，必须支持聚合和筛选的快速性能。
您应选择哪种存储模式以最大化性能？

+   a. DirectQuery
+   b. Import
+   c. Dual
+   d. Direct Lake
+   e. Live Connection

83. 您正在使用包含大型数据集且每日刷新的语义模型。您希望通过仅刷新最近的数据来减少刷新时间和提高性能。您应该实现什么？

+   a. 复合模式
+   b. 增量刷新
+   c. 聚合表
+   d. Dual 存储模式
+   e. 手动分区事实表

84. 您有一个语义模型中的大型 Sales 事实表。报表经常查询此数据。您希望用户查询快速，但数据集太大无法完全加载到内存中。

哪种存储模式最合适？

+   a. Import
+   b. DirectQuery
+   c. Dual
+   d. Composite
+   e. Live Connection

85. 您有一个包含数百万行的 Sales 事实表和一个 Date 日期维度表。报表仅需要显示最近两年的数据，但必须保留历史数据。

您希望减少刷新时间并将处理限制在最近数据。您将实现什么？

+   a. 聚合表
+   b. 增量刷新
+   c. 计算组
+   d. 字段参数
+   e. 为最近两年创建单独的表

86. 作为一名数据分析师，您正在查看使用多个复杂 DAX 度量的报表。报表加载和交互缓慢。Power BI 提供的哪种工具可用于识别导致性能问题的视觉对象或度量？

+   a. INFO.VIEW
+   b. 性能分析器
+   c. Tabular Editor
+   d. 查询诊断
+   e. SQL 配置文件

87. 您正在设计使用 Direct Lake 模式的语义模型。您希望确保用户可以低延迟查询大型数据集，而无需将数据预加载到内存中。
Direct Lake 提供了哪种好处？

+   a. 实时流式传输
+   b. 查询折叠
+   c. 直接访问 OneLake 中的 Parquet 文件
+   d. 自动聚合
+   e. 内置增量刷新

88. 作为一名数据工程师，您正在为语义模型配置增量刷新。您希望确保仅刷新当前月份和上个月，同时保留三年的历史数据。必须配置哪两个参数？

+   a. DateFrom 和 DateTo
+   b. RangeStart 和 RangeEnd
+   c. RefreshWindow 和 RetentionPeriod
+   d. PartitionStart 和 PartitionEnd
+   e. HistoricalStart 和 HistoricalEnd

89. 您正在优化包含大型 Sales 事实表和多个维度表的语义模型。您希望减少内存使用并提高查询性能。应应用哪种策略？

+   a. 对所有表使用 Import 模式
+   b. 从模型中删除未使用的列
+   c. 为所有度量创建计算列
+   d. 对维度使用 DirectQuery
+   e. 降低列基数以提高压缩

90. 作为一名数据分析师，您正在查看包含多个计算列的语义模型。您注意到报表性能不佳且刷新时间长。您应考虑哪两个操作？

+   a. 用度量替换计算列
+   b. 对所有表使用 Direct Lake 模式
+   c. 从模型中删除未使用的列
+   d. 切换到 Import 模式
+   e. 使用字段参数减少模型大小

91. 您正在为包含 TransactionDate 列的语义模型配置增量刷新。您希望确保仅刷新最近 60 天的数据并保留 3 年的历史数据。应应用哪种配置？

+   a. RangeStart = 今天 - 60 天，RangeEnd = 今天。
+   b. 刷新策略：保留 3 年数据，刷新最近 60 天。
+   c. 按年和月分区。
+   d. 对最近数据使用 DirectQuery。
+   e. 使用 Import 和 DirectQuery 的复合模型。

92. 一个 Fabric 工作区包含必须部署到开发、测试和生产环境的语义模型。团队还要求所有模型更改都必须版本化并随时间跟踪。

您应该使用哪种方法？

+   a. 将语义模型保存为 .pbix 文件并手动上传到每个工作区
+   b. 以 .pbip 格式存储语义模型并将其连接到 Git 以进行版本控制
+   c. 将模型导出为 .pbit 模板并与开发人员共享
+   d. 配置部署管道但不使用源代码控制
+   e. 直接从 Power BI Desktop 发布到生产工作区

93. 您正在管理包含多个开发人员的 Fabric 工作区。您希望确保语义模型和报表在开发、测试和生产环境中一致部署。您应该使用什么？

+   a. GitHub 集成





+   b. 部署管道
+   c. 工作区克隆
+   d. 手动导出和导入
+   e. Power BI 应用

94. 作为一名数据分析师，你正在共享工作区中工作，并且希望在多个报表中重用语义模型。你还希望确保对模型的更新能够自动反映。应使用哪种方法？

+   a. 在每个报表中复制模型
+   b. 使用共享语义模型
+   c. 将模型导出为 .pbids
+   d. 使用字段参数切换模型
+   e. 为每个报表创建复合模型

95. 作为一名数据工程师，你正在准备语义模型进行部署。你希望确保模型具有模块化、可版本控制，并且与外部工具兼容。应使用哪种文件格式？

+   a. .pbix
+   b. .pbip
+   c. .pids
+   d. .xlsx

96. 一个 Fabric 工作区包含多个语义模型和报表。为了指导报表作者并确保生产仪表板中仅使用受信任的模型，需要突出显示哪些模型已获得认证。

应应用哪种治理功能？

+   a. 敏感度标签
+   b. 认可
+   c. 工作区级权限
+   d. 部署管道
+   e. Git 集成

97. 一个 Fabric 工作区包含一个包含敏感财务信息的语义模型，包括薪资和奖金数据。你需要确保此数据得到正确分类和保护，即使导出到 Excel 或在 Power BI 之外访问时也是如此。

应应用哪种治理功能？

+   a. 敏感度标签
+   b. 认可
+   c. 工作区级权限
+   d. 部署管道
+   e. Git 集成

98. 你正在管理一个 Fabric 工作区，并且希望自动化语义模型和报表从开发到生产环境的部署。你还希望在部署期间应用环境特定的规则。应使用哪两个功能？

+   a. 部署管道
+   b. Git 集成
+   c. 工作区克隆
+   d. 参数规则
+   e. 敏感度标签

99. 一个 Fabric 工作区托管多个语义模型和报表。团队需要以下两点：

+   - 对模型的所有更改都必须进行版本控制并随时间跟踪。
+   - 内容必须在开发、测试和生产环境之间自动部署。

你需要配置什么？

+   a. 部署管道
+   b. Git 集成
+   c. 工作区克隆
+   d. 参数规则
+   e. 敏感度标签

100. 一个 Fabric 工作区包含一个语义模型，其中包含 Sales 事实表和 Customers 维度表。

你需要限制访问，以便销售经理只能查看分配给他们的区域的数据，基于其登录身份。应实现哪种安全功能？

+   a. 列级安全性 (CLS)
+   b. 行级安全性 (RLS)
+   c. 对象级安全性 (OLS)
+   d. 敏感度标签
+   e. 工作区权限

## 答案

1. 正确答案：A

要在 Microsoft Fabric 环境中与第三方工具结合使用，必须启用 XMLA 端点。你可以在第 8 章以及此 Microsoft Learn 资源中阅读更多相关信息。

2. 正确答案：B

在 Power BI 格式中，唯一支持 TMDL 语言格式的是 Power BI 模板 (.pbix)。

3. 正确答案：A 和 C

通过创建现有表的元数据副本来克隆表不会更改表及其底层数据结构。当新克隆表中的数据被更新、插入或删除时，引擎会为该特定表创建一组新的文件，并且更改仅适用于 dbo.FactSales。

4. 正确答案：C

当你在没有分区的湖屋中处理特定表时，第一件事就是覆盖该表。你这样做是为了创建底层文件系统所需的新的文件结构，以便支持必要的分区实现。

5. 正确答案：B
在列出的项目中，唯一支持 Power Query (M) 语言的活动是数据流。

6. 正确答案：A
要从湖屋创建对现有表的链接，必须使用快捷方式。此快捷方式仅是指向数据的指针，然后使你能够对其进行查询并像开发查询时表列表中的任何普通表一样使用它。快捷方式不会在湖屋之间复制数据。

7. 正确答案：B
在 Dataflow Gen2 的连接操作中，你可以通过使用模糊匹配方法自动跳过连接中的空格。我们在第 2 章中描述了这一点，但为了更详细的描述，你还可以参考 Microsoft Learn 关于模糊匹配方法的文档。

8. 正确答案：D
在任何数据平台中使用连接时，要获取两个表中的所有行，请使用 FULL OUTER JOIN。参考第 3 章以查看连接类型的图表以及每种连接类型的结果。

9. 正确答案：B
在使用 Kusto 查询语言 (KQL) 时，你必须帮助引擎尽可能高效地执行。你可以通过多种方式实现这一点。在此问题中，查询的更改将使执行更快，因为它有助于引擎在执行阶段更早地进行过滤。

你可能还记得在第 4 章中，我们在描述 KQL 的使用时，也谈到了你可以通过将语句以最佳顺序放置来帮助引擎尽可能高效执行的方法。

10. 正确答案：B

在使用 Power BI 时，创建字段参数是满足特定需求以使用户能够在报表中选择度量的最简单方法。你选择要提供给最终用户的度量（或列），然后 Power BI 会为你创建列表和新的度量以供使用。

11. 正确答案：A

在工作区中，我们有第 7 章中的四个角色。在这种情况下，会员角色赋予用户将工作区中的项目与其他人共享的能力。

12. 正确答案：A 和 B

在第 7 章中，我们谈到了项目共享以及避免必须访问工作区即可共享特定项目的特定数据项的能力。在这里，你可以利用此选项，确保通过使用项目权限共享湖屋，并且还确保用户可以通过使用“读取所有 Apache Spark 数据”来读取底层数据。

13. 正确答案：A

在使用 Microsoft Fabric 中的 GitHub 时，你需要使用个人访问令牌 (PAT) 和存储库的 URL。PAT 是从 GitHub 门户创建的，URL 是存储库的完整 https 字符串。

14. 正确答案：B

Microsoft Fabric 用于移动数据的低代码解决方案是 Dataflow Gen2。这是实现源和目标之间完整 UI 数据移动的唯一方法。

15. 正确答案：A 和 B

在第 3 章中，我们谈到了 Dataflow Gen2 创建目标的选项。这里有两个选项：要么创建默认目标，要么为当前数据流创建目标。

16. 正确答案：B

我们在第 7 章中讨论的敏感度标签使你能够确保特定项目及其下游项目被锁定（重新）共享。这为 Fabric 中的解决方案实施了良好的治理实践。

17. 正确答案：D

在使用半结构化和结构化数据时，Microsoft Fabric 中最好的选择是使用湖屋。它可以处理这两种数据结构，并且还支持你需要操作数据的 T-SQL 和 Spark 语言。

18. 正确答案：B

你在语义模型中实现数据实时更新有两个选项：DirectQuery 模式和 Direct Lake 模式。DirectQuery 模式在你使用报表查询底层数据存储时创建实时查询。Direct Lake 模式也进行实时查询，但它使用由 xVelocity 引擎和 OneLake 功能混合而成的更快的逻辑。

19. 正确答案：C

当你在语义模型中实施 RLS 时，USERPRINCIPALNAME 函数返回当前用户的电子邮件，然后你可以在报表中用于过滤。请参阅第 7 章了解详细信息。

20. 正确答案：A

当你编写 T-SQL 查询时，你可以使用 ROW_NUMBER 函数结合 (over ... partition by...) 窗口函数，以高效地检索基于特定顺序和桶创建的数据子集中的最新行。

21. 正确答案：B

在第 7 章中，我们谈到了工作区中的四个角色。会员角色是给予对工作区中所有报表和模型的访问权限，同时遵循解决方案的最小权限方法的一个角色。

22. 正确答案：C

计算组是 DAX 方法，可以对语义模型中的一组度量（或全部）应用可重用的计算。

23. 正确答案：D

你有两个选项来处理 Microsoft Fabric 中任何表的任何数据质量：


notebook 和 PySpark 方法以及来自 Dataflow Gen2 的数据剖析选项。

这里，你必须选择低代码选项，即 Dataflow Gen2。

24. 正确答案：C

在第 8 章中，我们谈到了部署管道，它们是内置工具，支持在工作区之间进行 CI/CD 的过程，并将部分或全部项目从开发环境通过测试环境推广到生产环境。

25. 正确答案：C

在第 5 章和第 6 章中，我们讨论了语义模型的实现以及如何帮助引擎尽可能高效地运行。使用星型模式是实现语义模型的最佳方式。

26. 正确答案：C

PREVIOUSYEAR 函数为你提供了开箱即用的功能，可将日历筛选器“移动”到前几年。

27. 正确答案：A

这里，我们需要将日期窗口移动到前一年，同时还要尊重用户的当前筛选上下文。为此，我们必须使用 SAMEPERIODLASTYEAR 函数。

28. 正确答案：B

你使用数据流在 Microsoft Fabric 中实现低代码数据移动，并能够在飞行中转换数据。

29. 正确答案：A

语义模型的导入模式使数据分析师能够安排将数据导入模型。在此场景中，数据每天更新一次，且数据新鲜度对终端用户而言不是主要问题。导入模式是这里的正确选择。

30. 正确答案：C

使用我们在第 8 章中讨论的内置部署管道即可满足所需需求。部署管道使终端用户能够使用易于使用的 UI 部署全部或部分项目。

31. 正确答案：C

语义模型中的角色扮演选项可使用同一张表实现维度与事实之间多个关系。缺点是只有一个关系可以处于活动状态，并且你必须使用 DAX 代码中的 USERELATIONSHIP 函数为非活动关系实现度量值。

32. 正确答案：C

数据流选项是你唯一可以用来以逐步解决方案实现每个转换要求的选项。在第 3 章中，我们谈到了数据流右侧的步骤窗格，以及使用鼠标和公式栏以简单方式调试它的能力。

33. 正确答案：C

为了确保对语义模型中特定行的强制筛选，我们可以使用 RLS 选项。这会强制应用一个定义的筛选器，终端用户无法更改。请参阅第 7 章以了解有关此主题的更多信息。

34. 正确答案：C

用于数据加载的 MERGE 方法将 INSERT、UPDATE 和 DELETE 操作合并为一条语句。

35. 正确答案：A

在处理 DAX 时，你可以使用 VAR 函数创建新的内部变量。这使代码更具可读性，并让你只需声明一次 DAX 逻辑即可重用变量。

36. 正确答案：D

在语义模型中创建关系时，事实表对一个或多个维度表具有外键。外键是指向行的指针和标识符，可从中在维度表中收集属性。维度表包含此维度的主键和属性。有关更多详细信息，请参阅第 5 章。

37. 正确答案：C

在处理 Fabric 仓库时，你可以选择使用可视化查询编辑器，它提供了一种 UI 方法来操作数据。在第 4 章中，我们谈到了这一点，并且还提供了幕后知识，即查询实际上是 Power Query，在将查询保存为仓库中的视图时会被翻译成 T-SQL。

38. 正确答案：D

在 Dataflow Gen2 项目中，我们有数据质量选项，可控制和查看每个列背后的指标。我们还可以通过右键单击列并筛选数据来删除包含 NULL 值的行。

39. 正确答案：B

在导入模式的语义模型上使用增量刷新时，必须声明 RangeStart 和 RangeEnd 参数。这些是语义模型用于从源读取数据的锁定名称。

40. 正确答案：B

PREVIOUSMONTH 函数使 DAX 度量值能够尊重应用于日期表的当前筛选器，并且还可以帮助“移动”显示数据的窗口。

41. 正确答案：D

只有使用复合模型，开发人员和分析师才能为模型中的单个表使用两个或多个数据源。

42. 正确答案：B

在第 7 章中，我们谈到了语义模型中安全性的不同实现方式。CLS 选项是这里使用的方法，因为它让我们能够强制对模型中的特定列进行访问或不访问。即使它在开箱即用的工具中不可用，这种方法也是可行的。

43. 正确答案：C

你可以使用 eventhouse、KQL 数据库和 KQL 语言来分析日志数据和流数据。其他服务可以轻松处理批量加载，但 Microsoft Fabric 中唯一能够同时处理半结构化和结构化流数据的服务是 KQL 数据库和 KQL 语言。

44. 正确答案：B

Spark 查询在 Microsoft Fabric 的湖屋中对半结构化和结构化数据执行。这也让你可以使用 T-SQL 和 Spark 查询来操作和查询数据。我们在第 2 章中讨论了这一点。

你通过 SQL 分析端点在数据库中实现 RLS 用于 T-SQL，正如我们在第 7 章中讨论的那样。

45. 正确答案：A 和 C

从 Fabric 应用敏感性标签并在湖屋中配置列级安全性满足了正确的<|message|> 要求。你必须在 SQL 分析端点中以 T-SQL 语句中的 DENY SELECT 或 GRANT SELECT 实现列级安全性，其中列为需求所指定的所需列。

46. 正确答案：B

当使用第三方提供商的代码化 CI/CD 实现来更新和维护语义模型时，你需要启用 XMLA 端点，正如我们在第 8 章中讨论的那样。这使第三方工具能够连接到语义模型，在其中执行 XMLA 命令。

47. 正确答案：A

Microsoft Fabric 中唯一基于文件的转换是通过湖屋完成的，它使用底层的 OneLake Delta Parquet 存储格式为每个表存储数据。

48. 正确答案：A 和 C

语义模型是唯一可以使用 DAX 语言查询数据的服务。

eventhouse 和 KQL 数据库是 Microsoft Fabric 中唯一使用 KQL 语言的服务。

49. 正确答案：C

我们需要实时处理流数据，而 Microsoft Fabric 中唯一开箱即用地执行此操作的服务是具有底层 KQL 数据库的 eventhouse。我们在第 2 章中讨论了这一点。

50. 正确答案：A 和 C

在计算组内部，你可以为每个度量值和从计算组中选择的<|message|> 元素定义特定的格式字符串。字段参数使终端用户能够在度量值列表或维度属性中选择特定元素。

## 51. 正确答案：B

我们使用 VIEW，因为它让我们可以保存一个可随时重用的查询，而无需创建新表。这对于 SalesTransactions 等大型表很有用，因为复制数百万行会效率低下。WHERE 子句移除了金额为 NULL 的交易，而 GROUP BY 子句按每个客户每月计算总计。

## 52. 正确答案：B

答案是 LEFT JOIN。我们希望保留 Orders 表中的所有订单，即使 Products 表中对应的 ProductID 没有匹配的类别。LEFT JOIN 保留左表（Orders）的每一行，并为右表（Products）中缺失的匹配项填充 NULL。

## 53. 正确答案：A 和 B

我们应用这两个步骤，因为它们直接解决了需求：删除重复行可确保数据完整性，而更改列类型可确保数据与维度模型匹配，以便进行一致分析。

## 54. 正确答案：B

Dataflow Gen2 转换提供了一种低代码、可视化的方法来清理和转换数据。在这种情况下，我们可以在管道中直接用 0 替换空字符串，而不会直接影响 NULL 值。使用 Dataflow Gen2 可保持源数据不变，同时确保摄取的数据干净且可用于分析。

## 55. 正确答案：B 和 E




我们应用这些步骤在将数据加载到湖仓之前对其进行清理。修剪空格可以修复不一致的文本格式，确保产品名称标准化。更改列类型可确保作为文本导入的数值列被转换为正确的类型，使其与维度模型匹配，从而实现准确的分析。

56. 正确答案：A

你应该使用 prev() 函数，因为它允许你在指定的分区内访问上一行的值，在本例中按区域分组。这使得可以计算当前和之前的 MeterReadings 之间的差异。

57. 正确答案：C 和 D

在将结果限制为前几家公司之前，你需要计算每家公司的总里程。GROUP BY TaxiCompany 子句会为每个出租车公司汇总行程距离，而 ORDER BY SUM(tripDistance) DESC 按总里程降序对公司进行排序，以便我们识别表现最好的公司。

58. 正确答案：B

你使用 WHERE 子句在聚合之前过滤行。在本例中，WHERE YEAR(SaleDate) = YEAR(GETDATE()) 仅选择当年发生的销售。

59. 正确答案：A

你使用 WHERE 将行过滤为仅包含当年交易。然后，使用 GROUP BY CustomerID 按客户计算总交易金额。这种组合可确保聚合仅在相关行上进行，并且因此能给出每个客户的正确总额。

60. 正确答案：B

从 Regions 表对 Orders 表使用 LEFT JOIN，因为它可确保包含所有区域，即使没有订单的区域。COUNT(o.OrderID) 将为没有匹配订单的区域返回 0，这正是报表所需的结果。

61. 正确答案：A

你应该使用 extend Delta = Energy - prev(Energy) by Area，因为它会按时间戳排序后，为每个区域分别计算每一行与其上一行之间的差异。

62. 正确答案：A

Dataflow Gen2 提供了一种低代码、可视化的方法来转换数据，按产品和月份汇总收入，并将结果加载到新表中。它也是可重用的，并且易于在 Fabric 中调度。

63. 正确答案：C

具有活动依赖关系的管道可确保每个步骤按正确顺序运行：加载原始数据、清理和转换数据，然后填充维度模型。每个任务仅在前一个任务完成后才开始。这种方法可确保流程始终按正确顺序进行，这对于获得准确的结果非常重要。

64. 正确答案：C
你应该使用 Dataflow Gen2，因为它可以从 CSV 文件等源复制数据到湖仓，并在过程中应用 Power Query M 转换。这使其适用于在单个低代码活动中同时进行摄取和数据清理。

65. 正确答案：A
我们使用计算组来简化 Power BI 中的时间智能计算。它们允许我们为年至今、上个月和季度比较等操作定义可重用的逻辑并将其应用于多个度量。这减少了重复并提高了模型的维护性。

66. 正确答案：B
在维度模型中，事实表存储数值度量（如 SalesAmount）和链接到维度表的外部键。维度表存储描述性属性（如 ProductCategory）和与事实表中的外部键匹配的主键。

67. 正确答案：B
你遵循最佳实践和模块化设计，以使解决方案具有性能、可维护性和可重用性。模块化设计将复杂的转换分解为更小、可重用的组件，使其更易于管理和优化。

68. 正确答案：B
你应该使用星型模式，因为它使用中心事实表（Sales）组织模型，该表直接连接到维度表（Date、Product 和 Store）。这种设计易于导航且对用户直观，并且在 Power BI 中性能良好，因为查询可以无需多次连接即可访问维度属性。

69. 正确答案：A

你应该使用复合模型，因为它允许你在单个 Power BI 模型中组合导入和 DirectQuery 表。这种方法提供了灵活性，可以利用内存中查询的快速性能来查询导入的数据，同时仍可查询 DirectQuery 模式下表的实时数据。

70. 正确答案：B

你应该使用字段参数，因为它们允许报表作者在视觉对象中动态地在不同维度之间切换，例如区域、产品和客户，而无需更改底层模型。这使模型灵活且可在多个报表中重复使用。

71. 正确答案：B

你应该使用单个共享语义模型，因为它可确保报表之间的逻辑一致，允许集中维护，并避免重复计算或表。报表作者可以连接到共享模型，这可减少错误并提高效率。

72. 正确答案：B

动态格式字符串允许你在不重复度量或手动更改每个度量的情况下在多个度量之间应用一致的格式。这在财务报表中尤其有用，其中度量可能具有不同的格式，例如货币和百分比。

73. 正确答案：D

此 DAX 度量计算去年同期总销售额并保留报表中应用的任何筛选器，如特定月份或产品。CALCULATE 应用筛选器，而 SAMEPERIODLASTYEAR 将日期移动到上一年。

74. 正确答案：B

RLS 用于根据用户权限限制对表中特定行的访问。在此场景中，它可确保分配给特定客户的用户只能查看该客户的销售数据。在 Power BI 中使用 RLS 是正确的方法，因为它提供逐行访问控制，可确保敏感客户数据得到保护，同时允许用户在允许查看的数据上正常与模型交互。

75. 正确答案：A

复合模型允许你在同一 Power BI 模型中组合导入和 DirectQuery 表。对表使用双重存储模式可让某些表使用导入以实现快速内存查询，而其他表使用 DirectQuery 访问实时数据。这种方法在同一报表中提供灵活性和性能。

76. 正确答案：B

自定义财政日历表允许你根据公司的财政日历定义财政年度、季度和月份，该日历可能与标准日历不一致。通过将其与 Sales 事实表关联，用户可以按财政期间正确分析销售。

77. 正确答案：B

星型模式使用中心事实表（Shipments）组织模型，该表使用代理键直接链接到维度表（Date、Customer 和 Carrier）。这种设计为报表作者提供直观的导航，并在 Power BI 中提供快速的查询性能。

78. 正确答案：C

代理键是维度表中使用的唯一标识符。使用它们来定义关系可确保 Sales 事实表与 Customer 维度表之间的链接保持稳定，即使业务键（如源系统中的 CustomerID）随时间变化。

79. 正确答案：A

角色扮演维度允许单个日期表通过为每个角色从事实表创建单独的关系来提供多种用途（提供订单日期、发货日期和发票日期）。这避免了表重复，并使模型高效且易于维护。

80. 正确答案：A、B 和 D

这些做法使模型对非技术用户来说直观且易于导航。使用友好名称可确保术语易于理解且在部门间保持一致。隐藏未使用的列和表可减少杂乱并使用户专注于相关数据。创建层次结构支持自然的下钻路径，从而提高报表中的可用性。

81. 正确答案：C

在日期维度中实现层次结构允许用户在视觉对象中轻松从年份下钻到季度再到月份。这使导航直观，并在 Power BI 报表中改善用户体验。

82. 正确答案：B

导入模式将数据加载到 Power BI 的内存引擎中，以提供聚合、筛选和视觉对象的快速性能。这对于在多个报表中使用的大型事实表是理想的。

83. 正确答案：B

增量刷新仅更新最近或更改的数据，而不是刷新整个数据集，这可减少刷新时间并提高大型表的性能。

84. 正确答案：B

DirectQuery 将数据保留在源系统中并按需查询，允许用户在不将所有内容加载到内存的情况下处理非常大的数据集。这提供了对完整数据集的访问，同时避免了内存限制。

85. 正确答案：B

增量刷新允许你仅刷新最近的数据（例如最近两年），同时


保持历史数据完整。这减少了刷新时间和处理负载，而不会丢失历史记录。因此，增量刷新是对于大部分为历史数据的超大数据集最有效且最易维护的解决方案。

86. 正确答案：B

性能分析器是 Power BI 的工具，旨在衡量报表中每个视觉对象和 DAX 度量的性能。它分解查询数据、渲染视觉对象和显示结果所花费的时间，帮助您识别导致缓慢的元素。

87. 正确答案：C

Direct Lake 允许 Power BI 直接从 OneLake 中的 Parquet 文件读取大型数据集，而无需将所有内容加载到内存中。这使得查询即使对于非常大的表也快速高效。

88. 正确答案：B

RangeStart 和 RangeEnd 定义了增量刷新使用的 Date 列上的筛选器。Power BI 使用这些参数来确定要刷新的行（例如当前月和上个月），同时保持历史数据完整。

89. 正确答案：B

删除未使用的列可以减小模型大小并提高查询性能，因为 Power BI 在内存中存储的数据更少。

90. 正确答案：A 和 C

用度量值替换计算列可以减少内存使用，因为度量值是在运行时计算而不是存储在模型中。删除未使用的列可以减小模型大小，从而提高查询性能和刷新时间。

91. 正确答案：B

刷新策略允许您定义要保留多少历史数据以及要刷新哪些最近数据。在这种情况下，将其设置为保留 3 年的历史数据并仅刷新最近 60 天的数据可确保历史数据得以保留，同时减少刷新时间。

92. 正确答案：B

以 .pbip 格式保存模型并将其连接到 Git 允许团队跟踪更改、管理版本，并在开发、测试和生产环境之间协作。这种方法支持适当的源代码控制并确保可重复性。

93. 正确答案：B

Fabric 中的部署管道允许您以受控、一致的方式将语义模型和报表从开发环境提升到测试环境再到生产环境。这可确保更改得到系统应用，并降低错误风险。

94. 正确答案：B

共享语义模型允许多个报表连接到单个模型，确保对模型的任何更新都会自动反映在所有

报表中。这避免了重复并保持一致性。

95. 正确答案：B

.pbip 格式专为模块化、可版本控制的语义模型而设计。它可以存储在 Git 或其他源代码控制系统中，支持协作，并与用于开发和部署的外部工具兼容。

96. 正确答案：B

认可允许您将语义模型标记为已认证或已推广，这向报表作者表明哪些模型是受信任的并推荐用于生产仪表板。

97. 正确答案：A

敏感性标签对敏感数据（如财务和个人信息）进行分类和保护。它们会随数据一起移动，即使导出到 Excel 或在 Power BI 之外访问时也能确保合规性和安全性。

98. 正确答案：A 和 D

部署管道提供了一种结构化的方法，将内容（语义模型和报表）从开发环境移动到测试环境再到生产环境。参数规则允许您在部署期间应用特定于环境的设置（如连接字符串和数据源）。

99. 正确答案：A 和 B

使用 Git 集成允许团队跟踪对语义模型的所有更改、维护版本历史记录并有效协作。每次更新都会存储并可以审查，这可确保不会丢失任何更改，并且开发过程可审计。部署管道可实现内容从开发环境到测试环境再到生产环境的自动推广。这可确保更改在各环境中一致应用，并降低手动部署导致错误的风险。

100. 正确答案：B

RLS 根据用户身份限制对数据的访问，允许销售经理仅查看与其分配区域相关的行。它与登录凭据动态配合使用，并确保每个用户只能看到其被允许查看的数据。
