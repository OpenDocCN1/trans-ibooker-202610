
```
sporting_event_temp1=pd.merge(ticket_purchase_hist,sporting_event_ticket,left_on="sporting_event_ticket_id",right_on="id", suffixes=('_tph', '_ht'))
sporting_event_temp1.head(5)
```

```
sporting_event_temp2=pd.merge(sporting_event_temp1,seat,left_on=["sport_location_id","seat_level","seat_section","seat_row"], suffixes=('_tph', 'seat'))
sporting_event_temp2.head(5)
```

|  | sporting_event_ticket_id | purchased_by_id | transaction_date_time | transferred_from_id | purchase_price | id | sporting_event_id | sport_location_ |
|---|---|---|---|---|---|---|---|---|
|0|1|3299597|2022-12-15 04:27:27|None|47.02|1|1| |
|1|2|3299597|2022-12-15 04:27:27|None|47.02|2|1| |
|2|22|3299597|2022-12-15 04:27:27|None|47.02|22|1| |
|3|65536|410959|2022-12-07 03:23:29|None|43.13|65536|2| |
|4|65536|5909803|2022-12-15 04:27:30|None|43.13|65536|2| |

Figure 9.71: 将体育赛事表与票务购买历史连接以获取座位详情

有了这些新数据，我们还将把座位类别可视化到之前的散点图中。我们将添加 seat_type 作为 hue 参数，这将根据座位类型对图表中的数据点进行着色，如下所示：

```
import seaborn as sb
plt.xticks(rotation=90)
scatter_plot1=sb.scatterplot(x='transaction_date_time', y='purchase_price',hue='seat_type', data=sporting_event_temp2, ma
```

![](img/ec1933a597ce63afa3bcf7496364f412_344_0.png)

Figure 9.72: 包含座位类型的票务购买历史可视化

这让我们了解到豪华座位价格昂贵且售出频率较低，而高级座位和标准座位则销售量很大。

让我们通过了解每个座位类型类别的平均票价来进一步探索。我们将为此用例绘制条形图。但在此之前，我们需要了解 pandas DataFrame 中 purchase_price 列的数据类型：

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 5431 entries, 0 to 5430
Data columns (total 15 columns):
#   Column                     Non-Null Count  Dtype
---  ------                     --------------  -----
0   sporting_event_ticket_id   5431 non-null   Int64
1   purchased_by_id            5431 non-null   Int64
2   transaction_date_time      5431 non-null   datetime64[ns]
3   transferred_from_id        0 non-null      object
4   purchase_price             5431 non-null   float64
5   id                         5431 non-null   Int64
6   sporting_event_id          5431 non-null   Int64
7   sport_location_id          5431 non-null   Int64
8   seat_level                 5431 non-null   Int64
9   seat_section               5431 non-null   string
10  seat_row                   5431 non-null   string
11  seat                       5431 non-null   string
12  ticketholder_id            5431 non-null   Int64
13  ticket_price               5431 non-null   object
14  seat_type                  5431 non-null   string
dtypes: Int64(7), datetime64[ns](1), float64(1), object(2), string(4)
memory usage: 716.0+ KB
```

Figure 9.73: pandas DataFrame info 函数用于了解列的数据类型

我们可以看到 purchase_price 列是 object 类型，这在条形图上进行聚合时会抛出错误。让我们将其转换为 float 对象类型，然后创建条形图。我们还将在条形上方添加 purchase_price 值，以便更好地了解票价范围，如下所示：

```
sporting_event_temp2["purchase_price"]=sporting_event_temp2.purchase_price.astype(float)
ax=sb.barplot(x='seat_type',y='purchase_price', data=sporting_event_temp2)
for i in ax.containers:
    ax.bar_label(i,)
```

![](img/ec1933a597ce63afa3bcf7496364f412_345_0.png)

Figure 9.74: 显示每种票类型平均购买价格的条形图

这个图表还可以，但如果我们能按 purchase_price 的顺序对图表进行排序，以便更好地可视化会更好。因此，我们将先按购买价格对 DataFrame 进行排序并重置 pandas DataFrame 索引。我们还将使用 alpha 参数以更美观的方式绘制图表来调整条形图的透明度。

```
sporting_event_temp2["purchase_price"]=sporting_event_temp2.purchase_price.astype(float)
sporting_event_temp2 = sporting_event_temp2.sort_values(["purchase_price"]).reset_index(drop=True)
ax=sb.barplot(x='seat_type',y='purchase_price', data=sporting_event_temp2, alpha=0.5)
for i in ax.containers:
    ax.bar_label(i,)
```

![](img/ec1933a597ce63afa3bcf7496364f412_346_0.png)

Figure 9.75: 排序后的条形图，用于了解座位类型及其类别的平均价格

现在我们能够更清晰地可视化不同座位类型对票价区间的影响。我们可以进一步探索如何根据每种座位类型售出的票数，通过对某些座位类型进行折扣来最大化票务销售。

这些库支持更多可视化，您可以使用前面提供的这些库的链接了解更多信息。对于大多数用例，前述的库足以用于可视化用户数据。此外，我们建议使用 Amazon QuickSight 来可视化更复杂的用例。

## 摘要

在本章中，我们学习了如何对数据集执行 pandas 操作以进行数据整理目的。我们探索了数据整理生命周期的各个阶段，通过发现、结构化、清洗、丰富、数据质量验证和可视化，以及使用 pandas 操作无缝执行这些活动。用户可以使用 AWS SDK for pandas，即 awswrangler 集成，与 pandas DataFrame 配合，在 AWS 云服务上执行数据整理活动。

下一章，我们将学习 SageMaker Data Wrangler，它有助于作为 ML 管道的一部分执行数据整理活动。

## 使用 SageMaker Data Wrangler 进行机器学习的数据处理

在 *第 4 章* 中，我们向您介绍了 SageMaker Data Wrangler，这是一款专为机器学习处理数据而设计的工具。我们讨论了为什么机器学习的数据处理是整体机器学习管道中如此关键的组成部分，以及使用不干净或原始数据工作的一些风险。我们还介绍了 SageMaker Data Wrangler 的核心功能，以及它如何有助于解决机器学习数据处理中涉及的一些关键挑战。

在本章中，我们将通过一个实际的逐步数据流进一步深入，对示例数据集进行机器学习预处理。我们将从使用 SageMaker Data Wrangler 预加载的示例数据集开始，然后使用 Data Wrangler 内置分析进行一些基本探索性数据分析。我们还将为数据集中的不平衡和偏差添加几项自定义检查。特征工程是机器学习周期中的关键步骤。它包括选择和转换原始数据特征为新的特征，更好地代表机器学习模型的底层问题。我们将创建一个 Data Wrangler 流程，并在示例数据集上定义数据转换以进行特征工程。SageMaker Data Wrangler 是一个统一的数据处理工具，使您能够完成完整的机器学习周期，包括直接从工具内部训练机器学习模型并部署用于推理。我们将导出处理后的数据并从 Data Wrangler 训练和部署模型。

本章涵盖以下部分：
- 登录 SageMaker Studio
- 导入数据
- 探索性数据分析
- 添加转换
- 导出数据
- 训练机器学习模型

## 技术要求

如果您希望跟随操作，我强烈推荐，您需要一个 **亚马逊网络服务** (**AWS**) 账户。如果您没有现有账户，可以在 Free Tier 下创建 AWS 账户。AWS Free Tier 为客户提供在每个服务的指定限制内免费探索和试用 AWS 服务的能力。如果您的应用程序使用超过 Free Tier 限制，您只需按标准、按使用付费的服务费率付费。在本章中，我们将通过了解如何访问和熟悉 SageMaker Data Wrangler 用户界面开始。跟随操作时，您将使用 AWS 计算资源，并在您的 AWS 账户中创建资源。这尤其适用于本章的 *训练机器学习模型* 部分，该部分计算密集且会创建您需要删除的端点。请记住通过删除任何未使用的资源进行清理。我们将在本章末尾再次提醒您；现在，让我们开始吧。

## 第 1 步 – 登录 SageMaker Studio

在本节中，我们将介绍登录 AWS 控制台和 SageMaker 并在其中导航的步骤。如果您已熟悉使用 SageMaker，可以跳过本节直接进入下一节。

在您创建账户并设置 SageMaker Studio 域并创建用户，如 *第 4 章* 中所述后，您可以登录 AWS 控制台并选择 **SageMaker**。您可以在 **所有服务** 下的 **机器学习** 部分导航到 SageMaker，或在 AWS 控制台顶部搜索框中开始输入 `SageMaker`。

![](img/ec1933a597ce63afa3bcf7496364f412_349_0.png)

Figure 10.1: AWS 控制台 – SageMaker
