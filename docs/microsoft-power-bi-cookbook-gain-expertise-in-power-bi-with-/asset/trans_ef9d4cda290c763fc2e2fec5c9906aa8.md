

## 使用 R 和 Python 视觉对象

R 和 Python 编程语言（包括其在数据处理、高级分析和可视化方面的强大且可扩展的功能）已与 Power BI 深度集成。R 和 Python 脚本可用作 Power BI 数据集的数据源、M 查询中的数据转换和整形过程，以及 Power BI 报表和仪表板中的独立视觉对象类型。与标准 Power BI 视觉对象一样，R 和 Python 脚本视觉对象直接利用数据模型中定义的关系，并可通过其他视觉对象（如切片器）进行动态筛选。

在本节中，将使用 R 脚本在 Power BI Desktop 中创建两个直方图视觉对象：一个使用 R 的标准基础绘图系统，另一个使用流行的 ggplot2 可视化包。此外，还包含一个 Python 视觉对象示例。

## 准备工作

要准备本节内容，请按照以下步骤操作：

- 1. 使用以下任一方式在本地计算机上下载并安装 R 引擎：
  - https://cran.r-project.org/bin/windows/base/
  - https://aka.ms/rclient/
- 2. 通过以下命令为 R 安装 ggplot2 包：install.packages("ggplot2")。
- 3. （可选）安装用于编辑 R 脚本的 IDE，例如 RStudio（https://www.rstudio.com/）。
- 4. 打开 Power BI Desktop，确认在 Power BI Desktop 的 R 脚本选项中反映了本地 R 安装目录路径（文件 | 选项和设置 | 选项）。
- 5. 类似地，从 http://www.python.org 或 Anaconda（https://www.anaconda.com/）中捆绑的 Python 安装 Python，并在 Power BI Desktop 中验证 Python 脚本选项（文件 | 选项和设置 | 选项）。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_447_0.png)

**检测到的 R IDE** 和 **检测到的 Python IDE** 下拉列表可用于在多个已安装的 IDE 之间进行选择。如果 Power BI Desktop 中尚未使用 R 和 Python 脚本视觉对象，将出现 **启用脚本视觉对象** 提示。单击 **启用**。

- 6. 使用以下命令为 Python 安装 matplotlib：`python -m pip install matplotlib`。
- 7. 使用以下命令为 Python 安装 pandas：`python -m pip install pandas`。
- 8. 通过在 **主页** 选项卡的功能区中单击 **转换数据**，访问 **Power Query 编辑器**。
- 9. 创建名为 AdWorksDW 的查询，如下所示：

```
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

## 应用高级分析和自定义视觉对象

- 10. 禁用 AdWorksDW 查询的加载，并将其放入 **数据源** 查询组。
- 11. 右键单击 AdWorksDW 查询，然后选择 **引用**。
- 12. 将查询重命名为 **Internet Sales**，然后选择 **FactInternetSales** 表。
- 13. 重复步骤 4 和 5，创建指向 **DimDates** 表的 **Dates** 查询。
- 14. 使用以下代码创建 **Products** 查询：

```
let
    Source = AdWorksDW,
    Navigation = Source{[Schema="dbo",Item="DimProduct"]}[Data],
    Subcategory =
        Table.ExpandRecordColumn(
            Navigation, "DimProductSubcategory",
            {"EnglishProductSubcategoryName", "DimProductCategory"},
            {"Product Subcategory", "DimProductCategory"}
        ),
    Category =
        Table.ExpandRecordColumn(
            Subcategory, "DimProductCategory",
            {"EnglishProductCategoryName", "ProductCategoryKey"},
            {"Product Category", "ProductCategoryKey"}
        )
in
    Category
```

- 15. 将 **Internet Sales** 查询移动到 **事实** 查询组，并将 **Dates** 和 **Products** 查询移动到 **维度** 查询组。
- 16. 在 **其他查询** 组中，创建一个名为 **Calculations** 的空白查询。
- 17. 在 **主页** 选项卡的功能区中选择 **关闭并应用**。
- 18. 切换到 **模型** 视图，并在 **Dates** 表中的 **DateKey** 列与 **Internet Sales** 表中的 **OrderDateKey** 列之间创建关系。
- 19. 在 **Products** 表和 **Internet Sales** 表的 **ProductKey** 列之间创建关系。
- 20. 保存你的工作。

## 如何使用 R 和 Python 视觉对象

要实现本节内容，请使用以下步骤：

- 1. 在 **Calculations** 表中创建以下度量：
  **Internet Gross Sales = SUM('Internet Sales'[SalesAmount])**
  
  **Internet Sales Discounts = SUM('Internet Sales'[DiscountAmount])**
  
  **Internet Net Sales = [Internet Gross Sales] - [Internet Sales Discounts]**
- 2. 在 **报表** 视图中，使用 **筛选器** 窗格为 **Dates** 表中的 **CalendarYear** 列添加筛选器，并仅选择 **2013**
- 3. 从 **可视化** 窗格添加一个 R 脚本视觉对象，并将 **Products** 表中的 **ProductKey** 和 **ListPrice** 列、**Dates** 表中的 **CalendarYear** 列以及 **Internet Net Sales** 度量添加到 **值** 字段井中
- 4. 在 R 脚本编辑器中输入以下 R 脚本，并单击 **运行脚本** 图标：
- 5. 在同一页面上复制并粘贴该 R 脚本视觉对象

```
par(bg = "#E6E6E6")
hist(dataset$'ListPrice', breaks = seq(from=0, to = 2500, by = 500), col = "#2C95FF",
main = "Current Year Online Sales List Price Distribution", cex.main = 1.75, cex.axis = 1.2, cex.lab = 1.4, ylab = "Count of Products", xlab = "Product List Price Bins", las = 1, labels = TRUE, border = "black", ylim=c(0,50))
```

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_449_0.png)

图 9.37：通过基础绘图系统在 Power BI 中呈现的 R 脚本视觉对象

- 6. 选择粘贴的 R 脚本视觉对象，并将代码更改为以下内容：

```
prices <- as.numeric(as.character(dataset$'ListPrice'))
breakpoints <- seq(from=0, to = 2500, by = 500)
library("ggplot2")
ggplot(dataset, aes(x = prices)) + geom_histogram(breaks = breakpoints, fill = "#2C95FF", color = "black") + xlab("Product List Price Bins") + ylab("Count of Products") + ggtitle("Current Year Online Sales List Price Distribution") + stat_bin(breaks = breakpoints, geom="text", aes(label=..count..), vjust = -1) + coord_cartesian(ylim=c(0,50)) + theme(text = element_text(size = 16)) + theme(plot.title = element_text(hjust = .5))
```

- 7. 此代码更改的结果如图 9.38 所示

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_450_0.png)

图 9.38：通过 ggplot2 包在 Power BI 中呈现的 R 脚本视觉对象

## 工作原理

如果按照“准备工作”部分安装了受支持的外部 R IDE，并在“检测到的 R IDE”R 脚本选项中选择了该 IDE，则可以单击“在外部 R IDE 中编辑脚本”（向上箭头图标）。这将启动 IDE 应用程序（如 RStudio），并从 Power BI Desktop 导出数据帧。R 脚本 IDE 的常见功能（如 IntelliSense 和变量历史记录）对于开发复杂的 R 脚本视觉对象很有帮助（甚至必不可少）。

本节中两个视觉对象的要求是显示当前日历年内已在线销售的产品列表价格的分布。第一个示例使用 R 基础绘图系统中的标准 hist() 函数，第二个示例使用 R 的 ggplot2 包提供的 ggplot() 函数。

通过 `par()` 函数设置浅灰色背景，`hist()` 函数的参数定义 x 轴和 y 轴、标题的文本字符串、数据标签、字体大小以及条形的浅蓝色。`seq()` 函数用于配置 x 轴间隔（分箱），宽度或箱大小为 500 美元，最大价格为 2,500 美元。

`ggplot2` 的 `geom_histogram()` 需要连续变量，因此 **List Price** 列被转换为 `prices` 变量中的数值数据类型。之前示例中用于 x 轴（按 500 到 2,500 分箱）的向量表达式（`seq()`）被用作 `geom_histogram()` 和 `stat_bin()` 的 **breaks** 参数。同样，之前示例中 y 轴的表达式被重用，但作为参数传递给 `coord_cartesian()` 函数。`qplot()` 函数（快速绘图的缩写）在 `ggplot2` 包中也可用，并且可以用更少的代码（通常只需一行）更快地开发相对复杂的可视化。

Power BI Desktop 中的 R 脚本编辑器会自动创建数据帧，并根据加载到 **值** 字段井中的列删除重复行。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_451_0.png)

图 9.39：Power BI Desktop R 脚本编辑器数据帧和重复项删除

在本示例中，其目的是统计单个产品（包括那些具有相同列表价格的产品），因此有必要在数据帧中添加一个单独的列（**product key** 列），使数据帧的每一行都是唯一的。在其他场景中，可以使用 `Table.AddIndexColumn()` M 函数来创建唯一性。

在 R 脚本视觉对象的数据帧中包含 **Internet Net Sales** 度量，可以使视觉对象通过 **Date** 维度列和报表页面上的其他维度表进行筛选。

默认情况下，根据此筛选上下文，没有任何销售记录的产品将不会包含在数据帧中，这符合本节的要求。如果要在此视觉对象中包含没有销售记录的产品，可以使用 **值** 字段井中 **Product Key** 列的 **显示无数据的项目** 选项。

R 脚本数据源连接器可在 **获取数据** 对话框的 **其他** 类别中使用。在查询编辑器的 **转换** 窗口中提供了 **运行 R 脚本** 命令图标。许多使用 R 构建的自定义视觉对象已在 AppSource 中提供，并且截至 2017 年 7 月，R 自定义视觉对象可以包含交互功能，例如选择和缩放。

