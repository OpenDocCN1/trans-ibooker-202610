

## 管理参数

图 6-25. 配置增量刷新的范围开始和结束

接下来，你将在 Sales 表的 Date/Time 列中使用这两个参数。确保你引用的是你创建的参数。图 6-26 显示了你应该点击的位置。

## 过滤行

对该表中的行应用一个或多个筛选条件。

Basic   Advanced

保留 'current_time' 的行

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_347_0.png)

图 6-26. 基于范围参数筛选 Date/Time 列

现在，你需要创建一个在模型发布后使用的策略。拥有此策略对于创建和管理表分区以及执行刷新操作至关重要。要定义该策略，你在 Power BI 中打开模型视图，然后右键单击你的表或单击三个点。你会看到一个标记为“增量刷新”的选项，点击后会打开一个弹出窗口（见图 6-27）。

当你配置增量刷新时，有多个选项可用：

### Archive data starting ____ before refresh date

在此处，你定义想要保留多少历史数据（例如五年）。旧数据将被自动删除。

### Incrementally refresh data starting ____ before refresh date

在此处，你定义想要刷新的最近数据的范围（例如最近七天）。

### Get the latest data in real time with DirectQuery (Premium only)

你可以选中此框以添加 DirectQuery 分区来获取超出刷新周期之外的最新更改。

### Only refresh complete days

你可以选中此框以确保仅刷新完整的日期。这可防止部分更新，如果你的指标依赖于完整的每日数据，这一点很重要。

### Detect data changes

你可以选中此框以使 Power BI 仅刷新发生更改的期间，基于单独的 Last modified 列。如果数据稳定，这可以进一步减少刷新时间。

## 增量刷新和实时数据

1. Select table

Incremental

2. Set import and refresh ranges

Incrementally refresh this table

Archive data starting 7 Days before refresh date

Data imported from 1/25/2026 to 1/31/2026 (inclusive)

Incrementally refresh data starting 1 Days before refresh date

Data will be incrementally refreshed from 2/1/2026 to 2/1/2026 (inclusive)

3. Choose optional settings

- Get the latest data in real time with DirectQuery (Premium only) Learn more
- Only refresh complete day Learn more
- Detect data changes Learn more

4. Review and apply

Archived
Incremental Refresh
7 days before refresh date
1 day before refresh date
Refresh date

Apply

Cancel

图 6-27. 配置增量刷新

## 发布

设置增量刷新后，你将模型发布到 Power BI 服务。你应该手动执行第一次刷新，以便监控其进度。由于分区被创建、历史数据被加载、关系和层次结构被重建，以及计算被处理，此初始刷新可能需要一段时间。

第一次刷新后，后续刷新会快得多，因为只有增量分区会被更新。

## 限制

在使用 Power BI 中的增量刷新时，有几件重要的事情需要牢记。一旦语义模型发布到 Power BI 服务，就无法再将其下载为 .pbix 文件。然而，当你使用 Power BI 项目文件（.pbip）时，此限制并不适用。Power BI 项目文件可以从服务中下载，因为它们不包含物理分区；它们仅包含刷新策略定义。这意味着即使配置了增量刷新，你也可以在本地打开和管理项目，而不会丢失模型结构。

如果你重新发布带有增量刷新的 .pbix 文件，服务中的模型将被覆盖，并且所有分区和先前加载的数据将被删除，并在下次刷新期间重建。这意味着刷新操作可能非常消耗资源。事实上，在 Power BI Pro 工作区中，刷新最多可运行两个小时，而在 Fabric 容量（FSKU）中，计划刷新的限制增加到五小时。然而，当刷新通过 XML for Analysis（XMLA）端点触发时，此限制不适用。

最后，使用 DirectQuery 的实时数据仅在 Fabric 容量工作区中受支持。即使启用了增量刷新，在非容量工作区中也不可用。

## 总结

在本章中，你学习了如何提高 Power BI 中大型语义模型的性能。你首先使用 INFO.VIEW 函数来检查模型的元数据并发现潜在的瓶颈。你还使用 Power BI Desktop 中的性能分析器来衡量视觉对象加载所需的时间以及延迟发生的位置。你练习了优化 DAX 度量以使其更快。你还探索了 Direct Lake 存储模式，该模式对于大型模型很有用，因为它减少了内存压力并加快了查询速度。最后，你学习了如何设置增量刷新，以便仅更新新数据，从而避免不必要的完全刷新并节省时间。

## 第三部分. 维护数据和分析解决方案

在数据驱动的世界中，数据是组织最宝贵的资产之一。然而，这种价值伴随着巨大的责任：确保数据安全且只有合适的人可以访问它。在像 Microsoft Fabric 这样全面的平台中，不同团队的各种用户与各种数据项交互，控制访问至关重要。这不仅仅是为了防止数据泄露；更是为了满足监管合规、维护数据完整性并培养信任文化。

在整个组织中管理访问可能看起来是一项艰巨的任务，但 Microsoft Fabric 提供了强大的工具和功能来有效处理此问题。

实施健全的安全架构是维护可靠且合规的数据环境的基础。

在本部分，我们将引导你了解这些基本的安全和治理功能。

在第 7 章中，我们将首先探讨如何实施工作区级访问控制，该控制管理谁可以访问和在你的 Fabric 工作区中工作。接下来，我们将转向更精细的控制，通过实施项目级访问控制和管理对计算资源的访问控制。然后，我们将深入研究高级数据安全，包括行级、对象级和文件级访问控制，以确保用户只能查看其有权查看的特定数据。之后，我们将学习如何应用敏感性标签来分类和保护你最敏感的数据项。最后，我们将介绍认可项目，这是一个建立信任并在整个组织中推广使用高质量、经过验证的数据的过程。

在此安全基础上，第 8 章将重点转向维护分析开发生命周期，以确保你的解决方案在演进过程中保持可靠。我们首先从版本控制开始，它帮助你跟踪更改并在工作区内保持工作有序。接下来，我们探讨 Power BI 项目（.pbip）文件格式和可重用资产，例如模板文件（.pbit）、数据源文件（.pbids）和共享语义模型，以简化开发并促进一致性。然后，我们将介绍部署管道，它提供了一种结构化的方法来在开发、测试和生产阶段之间推广内容。然后，我们将检查影响分析工具，如血缘视图，以便在进行更改之前识别下游依赖关系，最后，我们将以 XMLA 端点作为高级管理和自动化语义模型的结论。

## 第 7 章. 实施安全和治理

Microsoft Fabric 提供了强大的工具和功能来帮助你有效处理安全和治理，本章将引导你了解它们。我们将首先探讨如何实施工作区级访问控制，该控制管理谁可以访问和在你的 Fabric 工作区中工作。接下来，我们将转向更精细的控制，通过实施项目级访问控制和管理对计算资源的访问控制。然后，我们将深入研究高级数据安全，包括行级、对象级和文件级访问控制，以确保用户只能查看其有权查看的特定数据。之后，我们将学习如何应用敏感性标签来分类和保护你最敏感的数据项。最后，我们将介绍如何认可项目，这是一个建立信任并在整个组织中推广使用高质量、经过验证的数据的过程。

## 实施工作区级访问控制

你可以配置对 Microsoft Fabric 中每个工作区的访问。考虑图 7-1 中的场景。

这是一个演示设置，你有五个不同的工作区（工作区 A 至 E）和具有五种不同访问控制要求的用户组（用户组 1 至 5）。此设置并非详尽无遗，仅显示了可能的各种组合的一个子集。用户组 1

