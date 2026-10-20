

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_296_0.png)

图 5-33. 基于选择的 y 轴会自动更新为正确货币的可视化

## 优点和局限性

动态格式字符串可以使报表更易于阅读，并自动根据数据显示的上下文进行调整。它们通过以正确的格式显示值，例如货币或百分比，帮助用户正确解释值，而无需创建同一度量的多个版本。这使得报表更易于使用，并减少了维护工作量。

然而，你需要知道动态格式字符串仅适用于显式度量，并且在实时连接的 DirectQuery 模式下不受支持。尽管如此，它们提供了一种实用的方法，使报表更具动态性和可读性，而不会给模型增加不必要的复杂性。

## 大型语义模型存储格式

有时，尤其是在处理大量数据时，即使我们通过实施星型模式来优化数据模型，创建计算组和字段参数也可能使数据集变得如此庞大，以至于达到 Power BI 导入模式的限制。默认情况下，使用 Power BI Pro 许可证的 Power BI 数据集在导入模式下限制为 1 GB。但是，如果你在 premium 工作区中工作，你可以切换到 *大型语义模型存储* 格式，这允许你的数据集增长超过 10 GB。该格式专为企业级模型设计，并在处理大量数据时提供更大的灵活性。

大型模型存储模式的限制由 Fabric 容量的限制定义。表 5-2 提供了此类限制的概述。

表 5-2. 大型模型存储模式限制

| Capacity | Limit1 |
|---|---|
| F2 | 1 GB |
| F4 | 2 GB |
| F8/EM1/A1 | 3 GB |
| F16/EM2/A2 | 5 GB |
| F32/EM3/A3 | 6 GB |
| F64/P1/A4 | 10 GB |
| F128/P2/A5 | 10 GB |
| F256/P3/A6 | 10 GB |
| F512/P4/A7 | 10 GB |
| F1024/P5/A8 | 10 GB |
| F2048 | 10 GB |

### NOTE

此表中的限制对应于 Power BI 中数据集的压缩大小。源表中的实际数据量可能大得多。Power BI 使用高效的列式压缩，典型的压缩比范围为 5 倍至 10 倍。这意味着 Power BI 中的 5 GB 数据集可能代表 25–50 GB 的原始源数据。

要启用大型语义模型存储格式，我们需要进入 Power BI 服务，即在线 Power BI 环境。在那里，我们可以创建具有大型存储格式的新工作区或更改现有工作区。

要创建新工作区，我们选择“New workspace”，进入工作区设置，并选择“Large semantic model storage format” （见图 5-34）。

## Create a workspace

License mode ①

Pro
Select Pro to use basic Power BI features and collaborate on reports, dashboards, and scorecards. To access a Pro workspace, users need Pro per-user licenses. Learn more

Premium per-user
Select Premium per-user to collaborate using Power BI Premium features, including advanced dataflows, and datamarts. To collaborate and share content in a Premium per-user workspace, users need Premium per-user licenses. Learn more

Premium capacity
Select premium capacity if the workspace will be hosted in a premium capacity. When you share, collaborate on, and distribute Power BI and Microsoft Fabric content, users in the viewer role can access this content without needing a Pro or Premium per-user license. Learn more

Embedded
Select embedded if the workspace will be hosted in an Azure embedded capacity. ISVs and developers use Power BI Embedded to embed visuals and analytics in their applications. Learn more

Fabric capacity
Select Fabric capacity if the workspace will be hosted in a Microsoft Fabric capacity. With Fabric capacities, users can create Microsoft Fabric items and collaborate with others using Fabric features and experiences. Explore new capabilities in Power BI, Data Factory, Data Engineering, and Real-Time Intelligence, among others. Learn more

Trial
Select Trial to assign this workspace to a Fabric trial capacity. A Microsoft Fabric trial capacity allows user to explore the capabilities of Microsoft Fabric like Data Factory, Data

Apply Cancel

图 5-34. 创建新工作区时选择存储模式

如果我们已经有工作区并想更改存储模型，我们可以进入要更改的语义模型的设置，向下滚动到“Large semantic model storage format” 部分，并为现有语义模型启用该选项（见图 5-35）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_301_0.png)

图 5-35. 在语义模型设置中更改语义模型的存储模式

在 Power BI 中处理大型数据集时，切换到大型语义模型存储格式允许你超出默认大小限制。此选项在 Power BI 服务中易于启用，并且取决于你的 Fabric 容量。对于需要更多增长空间同时保持性能和灵活性的企业级模型，它也特别有用。

## 设计和构建复合模型

当你处理企业规模的模型时，你通常会有来自多个来源的数据。一些数据可能存储在导入模式中，而其他数据可能需要使用 DirectQuery 进行实时连接。这种结合不同存储模式的数据模型称为复合模型。

复合模型允许我们在单个报表中组合来自多个来源的数据。我们可以将导入模式与一个或多个 DirectQuery 连接混合，甚至可以组合多个 DirectQuery 数据源。这在处理具有不同刷新计划的数据源或需要实时访问时提供了灵活性。例如，我们可能每天导入一次更新的销售数据，同时使用实时连接获取实时库存数据。

复合模型允许数据集具有来自不同源组的两个或多个连接。这些可以如下：

- 一个或多个 DirectQuery 连接与导入模式组合
- 来自不同数据源的多个 DirectQuery 连接
- 支持源的任何其他组合

## 创建复合模型

在 Power BI 中创建复合模型只需几个步骤。假设我们在 Power BI 中有一个导入模式语义模型。我们意识到我们需要来自其中一个 Fabric 事件中心的某个表，并且我们需要与数据进行实时连接。我们可以将此数据加载到 Power BI，一旦这样做，我们将看到 Power BI 的消息询问我们希望数据如何连接：通过导入模式还是通过 DirectQuery。在这种情况下，因为我们希望与数据进行实时连接，所以我们选择 DirectQuery。Power BI 然后会添加新表，你可以通过其顶部栏加粗且颜色不同来识别它（见图 5-36 中的 Tutorial 表）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_303_0.png)

图 5-36. 复合模型，其中一个直接查询表与导入模式表并列

一旦我们在 Power BI 中拥有复合模型，然后在右下角，我们将看到图 5-37 中的消息。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_303_1.png)

图 5-37. 指示复合模型的提示

## 优点和局限性

当你想组合来自多个数据源的数据，其中一些数据需要实时，而其他数据可以处于导入模式时，使用复合模型很有意义。尤其是在企业环境中，你会经常看到这种情况，不同部门使用不同的工具和不同的数据存储。

你可以在 Power BI 中在新加载的表与其他表之间创建关系，就像你通常做的那样。然而，你需要知道默认情况下，两个不同来源（导入模式和 DirectQuery）之间的关系将是多对多。你可以更改此设置，但你需要主动点击关系并将其更改为例如一对多。

> **NOTE**
> 
> 在双模式复合模型中，此默认行为不适用，因为关系的处理方式不同，取决于哪些表处于导入模式，哪些处于 DirectQuery 模式。

在选择复合模型而不是导入模式之前，你还需要考虑性能。使用复合模型可能会影响性能，尤其是在可视化或计算依赖于多个来源时。单个可视化可能会触发多个查询，其中一个来源的结果传递到另一个来源。

复合模型允许我们在单个数据集中组合来自不同来源、存储模式和刷新策略的数据。它们提供了构建更智能、更具响应性的报表的灵活性，而无需将所有内容强制到同一数据源或刷新模式。通过对关系行为和性能影响有所了解，你可以使用复合模型构建语义模型，其中数据需要在不同的“新鲜度”级别。

