

寻找领先指标在业务中可以极具价值，因为它们提供了可预测的指标，企业可以根据预期未来行为做出决策并采取行动。要找到领先指标，相关性分析中的值必须根据适当的时间段数量相互偏移。这通常会导致在各种时间段偏移下进行相关性计算，以确定一个指标是领先于还是滞后于另一个指标。

## 另请参阅

- ► 斜率和截距方程描述：http://bit.ly/2tdzrgA
- ► 去季节化相关系数：https://bit.ly/2SeTSsp

## 分组和分箱

Power BI 中的分组和分箱会创建分组列，然后可以像模型中的其他列一样使用，以简化报表可视化和自助分析，因为其粒度降低。此外，分组可以在 Power BI Desktop 中进行管理和编辑，为数据集所有者提供灵活的选项，以便快速响应不断变化的需求或偏好。

本食谱提供了在 Power BI Desktop 中使用分组和分箱功能示例。

## 准备工作

为本食谱做准备，请按以下步骤操作：

- 1. 在本地打开 Power BI Desktop 文件并通过单击 **Home** 选项卡功能区中的 **Transform Data** 访问 **Power Query Editor**
- 2. 创建一个名为 AdWorksDW 的查询，类似于以下内容：
- 3. 禁用 AdWorksDW 查询的加载并将其放入 **Data Sources** 查询组中
- 4. 右键单击 AdWorksDW 查询并选择 **Reference**
- 5. 重命名查询为 **Internet Sales** 并选择 **FactInternetSales** 表

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV",
    "AdventureWorksDW2019")
in
    Source
```

- 6. 重复步骤 4 和 5，创建一个指向 **DimDates** 表的 **Dates** 查询，以及一个指向 **DimCustomer** 表的 **Customers** 查询
- 7. 将 **Internet Sales** 查询移动到 Facts 查询组，并将 **Dates** 和 **Customers** 查询移动到 **Dimensions** 查询组
- 8. 从 **Home** 选项卡功能区选择 **Close & Apply**
- 9. 切换到 **Model** 视图并在 **Dates** 表的 **DateKey** 列与 **Internet Sales** 表的 **OrderDateKey** 列之间创建关系
- 10. 在 **Customers** 表与 **Internet Sales** 表之间创建 **CustomerKey** 列的关系
- 11. 保存您的工作

## 如何执行分组和分箱

要实施本食谱，请执行以下步骤：

- 1. 右键单击 **Customers** 表中的 **DateFirstPurchased** 列并选择 **New Group**
- 2. 将 **Name** 设置为 **Customer First Purchase Calendar Quarter**，将 **Bin size** 设置为 **3**，从下拉菜单中选择 **Months**，然后单击 **OK**

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_432_0.png)

图 9.24：用于首次购买日期列的分组对话框，分箱大小为 3 个月

- 3. 创建一个 **Matrix** 可视化对象，使用客户表中新创建的 **Customer First Purchase Calendar Quarter** 列作为 **Rows**，来自 **Dates** 表的 **CalendarYear** 列作为 **Columns**，以及来自 **Internet Sales** 表的 **SalesAmount** 列（求和）作为 **Values** 来分析这些季度客户分箱的销售额
- 4. 可选地，通过单击值字段井中列名称右侧的向下箭头并选择 **Conditional formatting** 然后选择 **Data bars**，为 **Values** 字段井中的 **SalesAmount** 列添加条件格式

| Customer First Purchase Calendar Quarter | 2010 | 2011 | 2012 | 2013 | 2014 | Total |
|---|---|---|---|---|---|---|
| January 2014 |  |  |  |  | $26,089 | $26,089 |
| October 2013 |  |  |  | $2,064,938 | $3,907 | $2,068,846 |
| July 2013 |  |  |  | $1,201,124 | $4,391 | $1,205,515 |
| April 2013 |  |  |  | $1,537,392 | $4,975 | $1,542,367 |
| January 2013 |  |  |  | $1,087,279 | $6,332 | $1,093,611 |
| October 2012 |  |  | $1,620,094 | $1,631,978 |  | $3,252,072 |
| July 2012 |  |  | $1,461,896 | $1,581,699 |  | $3,043,595 |
| April 2012 |  |  | $1,320,543 | $1,465,738 |  | $2,786,281 |
| January 2012 |  |  | $1,378,995 | $1,419,521 |  | $2,798,516 |
| October 2011 |  | $2,038,185 | $3,485 | $1,404,207 |  | $3,445,877 |
| July 2011 |  | $1,814,388 | $16,952 | $1,132,372 |  | $2,963,712 |
| April 2011 |  | $1,801,595 | $22,953 | $95,837 |  | $2,780,385 |
| January 2011 |  | $1,421,357 | $17,567 | $853,667 |  | $2,274,591 |
| October 2010 | $43,421 |  |  | $33,798 |  | $77,219 |

图 9.25：矩阵可视化中使用的客户首次购买日历季度分组

## 其工作原理

通过将客户分组为季度分箱，新的分组列（**Customer First Purchase Calendar Quarter**）只有 14 个唯一值，可以在报表可视化中使用。在此分析中，很明显 2017 年的销售额是由首次购买在 2013 年第一季度和第二季度的客户推动的（2013 年 1 月、2013 年 4 月）。有趣的是，首次购买于 2011 年的客户在 2015 年是大型买家，但随后在 2016 年基本消失，并在 2017 年回归。

## 还有更多

在本示例中，目标是根据客户上次购买以来的天数对客户进行分组（分箱）。要了解其工作原理，请执行以下操作：

- 1. 创建以下度量来定义 **Today**：
     `Today = DATE(2014,1,28)`
- 2. 在 **Customers** 表中创建以下列：
     `Last Order Date = CALCULATE(MAX('Internet Sales'[OrderDate]))`
     `Days Since Last Purchase = ([Today] - [Last Order Date]) * 1`

```
Today = DATE(2014,1,28)
```

```
Last Order Date = CALCULATE(MAX('Internet Sales'[OrderDate]))
```

```
Days Since Last Purchase = ([Today] - [Last Order Date]) * 1
```

3. 基于 Days Since Last Purchase 列创建数值分组以帮助分析此数据

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_434_0.png)

图 9.26：基于 Days Since Last Purchase 列创建的分组

4. 创建一个聚类条形图以图形方式显示信息，使用客户表中新创建的 Days Since Last Purchase（分箱）列作为轴，并使用 Internet Sales 表中的 SalesAmount 列作为值

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_434_1.png)

图 9.27：按 90 天自上次购买以来分组的互联网销售额聚类条形图

新的分组列（**90 Days Since Last Purchase**）有助于确定 $15.7 M 的总历史互联网销售额由过去 180 天内购买的客户组成（0 至 90 组为 $7.3 M，90 至 180 组为 $8.4 M）。

由于分组本质上是数据模型中的计算列，并且对源系统不可见，分组列的逻辑最终应迁移到源数据仓库中的新列。分组对于概念验证场景和短期解决方案非常有帮助，但根据其他食谱，Power BI Desktop 中的数据转换过程应保持有限，以使数据集尽可能易于管理和可扩展。如果没有数据仓库选项，可以使用 M 查询转换而不是 DAX 计算列。

## 另请参阅

- ► 在 Power BI Desktop 中使用分组和分箱：https://bit.ly/3xHnMpG

## 检测和分析聚类

聚类是一种数据挖掘和机器学习技术，用于根据一个或多个度量的值对一个维度的项目进行分组（聚类）。鉴于不同维度项目的数量，例如产品或客户，以及描述这些项目的度量数量，聚类是探索数据以发现标准报表和分析技术难以检测的关系的强大方法。Power BI Desktop 提供了内置支持来创建聚类，并允许这些聚类像数据模型中的其他列一样进行管理、修订和使用于 Power BI 报表中。

在本食谱中，基于销售额、订单数量和上次购买以来的天数创建客户聚类。创建 DAX 度量以支持此分析，并创建 **Scatter Chart** 可视化对象以进一步分析聚类。

## 准备工作

为本食谱做准备，请按以下步骤操作：

- 1. 在本地打开 Power BI Desktop 文件并通过单击 **Home** 选项卡功能区中的 **Transform Data** 访问 **Power Query Editor**
- 2. 创建一个名为 AdWorksDW 的查询，类似于以下内容：

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV",
    "AdventureWorksDW2019")
in
    Source
```
