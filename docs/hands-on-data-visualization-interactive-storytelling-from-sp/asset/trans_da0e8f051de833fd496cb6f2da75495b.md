

# 第11章 Chart.js 和 Highcharts 模板

在第6章中，我们研究了强大的拖放工具，例如 Google Sheets、Datawrapper 和 Tableau Public 来构建交互式图表。

在本章中，我们将研究使用两个流行的 JavaScript 库 Chart.js 和 Highcharts 创建交互式图表。由于我们不期望读者精通 JavaScript 或任何其他编程语言，因此我们设计了模板，您可以将其复制到自己的 GitHub 账户，替换数据文件，并在不编写一行代码的情况下将其发布到网络。对于那些对代码好奇的人，我们将展示如何自定义这些模板中的 JavaScript 代码。

为什么有人会更喜欢 JavaScript 而不是易于使用的 Datawrapper 或 Tableau，你可能会想知道？好吧，有几个原因。虽然 JavaScript 代码乍一看可能令人不知所措和令人生畏，但它允许在颜色、填充、交互性和数据处理方面进行更大的定制，而大多数第三方工具无法提供。此外，您永远无法确定第三方应用程序是否会永远保持免费，或者至少有免费层，而开源工具只要有人维护代码，就会永久免费存在。

> NOTE
> 
> 尽管两个库都是开源的，但 Highcharts 采用了更严格的许可证，仅允许将其免费用于非商业项目，例如个人、学校或非营利组织网站。考虑到这一点，我们主要关注 Chart.js，它采用 MIT 许可证分发，让您也可以将该库用于商业项目。

表 11-1 列出了本章中我们将要研究的各种图表类型。两个库都包含许多更多默认图表类型，您可以在 Chart.js Samples 和 Highcharts Demos 中探索。然而，我们强烈建议不要使用某些图表类型，例如三维图表，原因我们在第7章的图表设计原则部分讨论过。

## 表 11-1. 图表代码模板、最佳用途和教程

| Chart | Best use and tutorials in this book |
|---|---|
| Bar or column chart | Best to compare categories side-by-side. If labels are long, use horizontal bars instead of vertical columns. |
| Error bars in a bar/column chart | Best to show margin of error bars when comparing categories side-by-side. If labels are long, use horizontal bars instead of vertical columns. |

Bar or column chart

![](img/216890131d46798dfe1cd5f886cc608c_310_0.png)

Best to compare categories side-by-side. If labels are long, use horizontal bars instead of vertical columns.

Power tool: “Bar or Column Chart with Chart.js”

Error bars in a bar/column chart

![](img/216890131d46798dfe1cd5f886cc608c_310_1.png)

Best to show margin of error bars when comparing categories side-by-side. If labels are long, use horizontal bars instead of vertical columns.

Power tool: “Error Bars with Chart.js”

### Chart

#### Line chart

![](img/216890131d46798dfe1cd5f886cc608c_311_0.png)

Best to show continuous data, such as change over time.

Power tool: “Line Chart with Chart.js” (see tutorial note to modify line chart into stacked area chart)

#### Annotated line chart

![](img/216890131d46798dfe1cd5f886cc608c_311_1.png)

Best to add contextual notes inside chart of continuous data, such as change over time.

Power tool: “Annotated Line Chart with Highcharts”

#### Scatter chart

### Chart

Best use and tutorials in this book

Best to show the relationship between two datasets as x and y coordinates to reveal possible correlations.

Power tool: “Scatter Chart with Chart.js”

West Haven
Median Household Income, USD: $52,353
Grade, Relative to Average: -0.6

#### Bubble chart

Best to show the relationship between three or four sets of data, with x and y coordinates, bubble size, and color.

Power tool: “Bubble Chart with Chart.js”

East Hartford
Median Household Income, USD: $50,400
Grade, Relative to Average: -1.4
Enrollment: 7,053 students

## Bar or Column Chart with Chart.js

在本节中，我们将向您展示如何使用 Chart.js 创建条形图或柱形图。我们将使用一个 Chart.js 代码模板，该模板从 CSV 文件中提取数据，如图 11-1 所示。该柱形图显示了康涅狄格州五个学区在 2018-2019 学年中是英语学习者的学生人数。

### English Learners by Select School Districts in Connecticut, 2018-19

![](img/216890131d46798dfe1cd5f886cc608c_313_0.png)

Source: CT Department of Education
Download data powering this visualization.

Figure 11-1. Bar chart with Chart.js: explore the interactive version.

要使用我们的 Chart.js 模板使用 CSV 数据创建自己的条形图或柱形图：

- 1. 前往包含图 11-1 中图表代码的 GitHub 仓库，登录您的 GitHub 账户，点击“Use this template”创建一个可编辑的副本。

## TEMPLATE FILE STRUCTURE

如果您不记得如何使用 GitHub，我们建议您重温 **Chapter 10**。该仓库包含三个与图表直接相关的文件：

*index.html*

包含 HTML（markdown）和 CSS（样式表），告诉浏览器如何设置包含图表的文档的样式以及加载哪些库。

*script.js*

包含从 CSV 文件读取数据并构建交互式图表的 JavaScript 代码。

*data.csv*

是存储图表中所有数据的逗号分隔文件，可以在文本编辑器或 Google Sheets/Excel 等中编辑。

其余两个文件是描述仓库内容的 *README.md* 和可在 README 中看到的图像 *bar.png*。本章中的所有其他 GitHub 模板结构都将类似。

- 2. 以 CSV 格式准备您的数据并上传到 *data.csv* 文件。将将沿轴显示的标签放在第一列中，每个数据系列放在自己的列中。您的 CSV 必须至少包含两列（标签和一个数据系列）。您可以添加任意数量的数据系列列。

| district | nonlearner | learner |
|---|---:|---:|
| Hartford | 15656 | 4111 |
| New Haven | 17730 | 3534 |

- 3. 在 *script.js* 中自定义变量的值。由于您可能不熟悉 JavaScript，让我们看一下文件中描述单个变量的代码片段：

```
// `false` for vertical column chart, `true` for horizontal bar chart
var HORIZONTAL = false;
```

第一行以 `//` 开头，是注释，帮助您理解下一行中变量的职责。它不影响代码。如您所见，如果变量 HORIZONTAL 设置为 *false*，图表将具有垂直条形（也称为柱形）。如果设置为 *true*，图表将包含水平条形。第二行包含变量声明本身。等号 (=) 将您在右侧看到的值（*false*）分配给左侧称为 HORIZONTAL 的变量（var）。该行以分号（;）结束。

以下是您可以在 *script.js* 中自定义的一些变量：

```
// `false` for vertical column chart, `true` for horizontal bar chart
var HORIZONTAL = false;

// `false` for individual bars, `true` for stacked bars
var STACKED = false;

// Which column defines 'bucket' names?
var LABELS = 'district';

// For each column representing a data series, define its name and color
var SERIES = [
  {
    column: 'nonlearner',
    name: 'Non-learners',
    color: 'gray'
  },
  {
    column: 'learner',
    name: 'Learners',
    color: 'blue'
  }
];

// x-axis label and label in tool tip
var X_AXIS = 'School Districts';

// y-axis label, label in tool tip
var Y_AXIS = 'Number of Enrolled Students';

// `true` to show the grid, `false` to hide
var SHOW_GRID = true;
```

```
// `true` to show the legend, `false` to hide
var SHOW_LEGEND = true;
```

这些基本变量应该足以让您入门。自然地，您会想要移动图例、编辑工具提示的外观或更改网格线的颜色。我们建议您查看官方 Chart.js 文档以获得帮助。

## Error Bars with Chart.js

如果您的数据带有不确定性（误差范围），我们建议您在可视化中使用误差线来显示。图 11-2 中显示的条形图模板显示了不同规模地理区域的中位数和平均（平均）收入：美国科罗拉多州、博尔德县、博尔德市以及该市的一个人口普查区。

