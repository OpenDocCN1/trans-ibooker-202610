

这适用于低代码用户（Power BI 中的 Power Query 也是如此）。

两种方法都适用于创建星型模式；它们使用不同的方法来实现组织和转换数据的相同目标。无论您选择哪种方法，您都需要创建一个事实表（例如 Sales）和维度表（例如 Customer、Product、Date 和 Store）。

每个维度都应有一个唯一的键，即主键，而事实表应存储引用这些维度的外键。在图 3-36 中，您可以看到湖屋中的一个事实表。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_167_0.png)

图 3-36. Fabric 湖屋中的一个事实表，包含与维度表主键相关的所有外键

事实表显示了与维度表主键相关的所有外键。例如，Customer_ID 376 对应于图 3-37 中 Customer 维度表中的记录。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_167_1.png)

图 3-37. 与图 3-36 中的事实表相关的 Customer 维度表中的记录

您通常需要自己创建这些表，因为大多数数据源不会以这种结构提供数据。对于每个维度表，您需要定义一个主键，对于事实表，您需要包含引用维度表中键的外键。通过创建不同的表并提供键，您为 Power BI 可消费的湖屋中的星型模式奠定了基础。

## 使用 PySpark 笔记本实现

要通过 PySpark 笔记本创建表，您进入 Fabric 中的湖屋并点击 Open notebook > New notebook（见图 3-38）。这让您可以启动一个附加到湖屋的新的 Spark 笔记本。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_168_0.png)

图 3-38. 在湖屋中打开新的笔记本

打开笔记本后，您可以编写 Python 代码来创建维度和事实表。我们在图 3-39 中提供了一些示例代码。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_168_1.png)

图 3-39. 用于创建客户表的笔记本中的 Python 代码

使用 Python 代码，您可以为 Customer 维度创建一个表，其中包含一个将链接到事实表的主键。Customer 维度表将包括客户的姓名、类别和购买组（见图 3-40）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_169_0.png)

图 3-40. Fabric 湖屋中的 Customer 维度表

## 使用 Dataflow Gen2 实现

您也可以使用 Dataflow Gen2 执行相同的实现。这对于开发人员（尤其是那些在 Power BI 中有数据转换经验的人）来说非常方便，他们希望采用低代码/无代码方法。

要使用 Dataflow Gen2 转换数据，请进入 Fabric 湖屋，点击 Get Data，然后选择 New Dataflow Gen2（见图 3-41）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_170_0.png)

图 3-41. 在 Fabric 湖屋中创建新的 Dataflow Gen2

这将打开 Power Query 编辑器，您可以在其中连接到湖屋中现有的表、转换它们，或创建新表以构建您想要的维度和事实表。整个体验与我们在 Power BI 中的体验相似，因此不需要编码经验。对表进行所有更改后，您可以将数据流发布到湖屋。

无论您选择 Notebooks 还是 Dataflows Gen2，您的目标保持不变：将您的数据塑造成具有明确定义的维度和事实表关系。这样的结构允许在 Power BI 中进行高效的查询和报告。两种方法都会产生相同类型的表，这些表将存储在湖屋中并可用于星型模式模型。您应该选择哪种方法取决于您的专业水平以及您是更喜欢编写代码还是使用图形界面。

接下来，我们将探讨如何在 Fabric 仓库中实现星型模式，其过程略有不同。

## 使用 SQL 在 Fabric 仓库中实现星型模式

就像在湖屋中一样，您可以通过创建维度和事实表在 Fabric 仓库中构建星型模式。主要区别在于仓库支持 T-SQL，并且默认使用结构化表。这种方法非常适合喜欢编写 SQL 或希望在更经典的数据仓库环境中工作的人员。您可以将自己的数据或示例数据加载到仓库中，并使用 T-SQL 将数据塑造成维度和事实表。

就像在湖屋中一样，仓库中的每个维度表都应具有主键，事实表存储引用这些维度的外键。这里的主要优势是仓库支持模式定义、数据类型、约束和索引，使其非常适合结构化报告工作负载。

打开 Fabric 仓库后，您可以自行编写查询来创建表或使用提供的 SQL 模板（见图 3-42）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_171_0.png)

图 3-42. Fabric 仓库中的 SQL 模板

从 SQL 模板开始可以让您更轻松地创建事实表和维度表。

## 维度建模的一般原则

无论您是在湖屋（更灵活，适用于大数据和半结构化源）还是仓库（具有严格的模式，适用于结构化关系数据）中工作，维度建模的原则保持不变：

- 使用事实表来衡量业务事件。
- 使用维度表来描述您的业务和这些事件。
- 对技术和业务用户都保持简单。

您创建的结构构成了 Power BI 数据模型和报告的基础。设计良好的星型模式可实现更快的性能、更好的可用性以及分析的可靠起点。

## 数据反规范化

在上一节中，我们探讨了星型模式如何为我们的数据提供结构和清晰度。它将业务概念（维度）与可衡量事件（事实）分离，以创建清晰易懂的模型。我们还简要解释了雪花模式，其中销售数据连接到 Product 表，而 Product 表又链接到 Product detail 表（见图 3-43）。

这种设置在技术上是正确的，并且遵循规范化的规则，即将数据拆分为较小的相关表以避免重复。然而，它

也可能使您的模型在 Power BI 中更难使用。为什么？因为 Power BI 需要在用户与可视化交互或应用筛选器时每次都遵循这些表之间的关系，这可能导致性能变慢和更复杂的 DAX 计算。它还使报告用户更难自行探索数据，尤其是如果他们不了解底层结构。

这就是反规范化的用武之地。假设您的模型中有以下表：

- Fact: Sales
- Dimension: Product
- Dimension: Product detail

为了反规范化，我们希望合并 Product 和 Product detail 表中的信息，以便我们最终只使用一个表，这将使我们的模型成为星型模式而不是雪花模式。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_174_0.png)

图 3-43. 具有 Product 表和 Product detail 表的雪花模式

## 使用 Power Query 反规范化表

假设您正在 Power BI 中工作，并连接到您之前创建的 Fabric 湖屋或仓库中的表。现在，您有一个 Product 表和一个 Product detail 表（见图 3-44 和 3-45）。

| Product_ID | ProductName | ProductCategory | ProductSubCategory |
|---|---|---|---|
| 1 | Running Shoes | Footwear | Sports |
| 2 | Basketball Shoes | Footwear | Sports |
| 3 | Leather Boots | Footwear | Casual |
| 4 | T-Shirt | Apparel | Casual |
| 5 | Sweatshirt | Apparel | Sportswear |
| 6 | Baseball Cap | Accessories | Headwear |
| 7 | Sunglasses | Accessories | Eyewear |
| 8 | Sports Watch | Electronics | Wearable |
| 9 | Laptop Backpack | Accessories | Bags |
| 10 | Yoga Pants | Apparel | Sportswear |

图 3-44. 带有 Product_ID 列的 Product 表

| Product_Detail_ID | Product_ID | Color | Size | Material |
|---|---|---|---|---|
| 101 | 1 | Blue | 42 | Mesh |
| 102 | 1 | Black | 44 | Mesh |
| 103 | 2 | Red | 43 | Synthetic |
| 104 | 3 | Brown | 44 | Leather |
| 105 | 4 | White | M | Cotton |
| 106 | 5 | Gray | L | Fleece |
| 107 | 6 | Navy Blue | One Size | Polyester |
| 108 | 7 | Black | One Size | Plastic Frames |
| 109 | 8 | Black | N/A | Silicon/Rubber |
| 110 | 9 | Black | N/A | Nylon |

图 3-45. 带有 Product_ID 列的 Product detail 表

如您在图 3-46 中所见，Product detail 表通过同时存在于两个表中的 Product_Detail_ID 列与 Product 表建立了关系。

