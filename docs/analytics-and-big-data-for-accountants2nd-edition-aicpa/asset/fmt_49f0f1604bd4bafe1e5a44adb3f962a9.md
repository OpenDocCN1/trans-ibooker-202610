
# 业务分析工具与平台概述

## 核心优势与平台特性

1.  业务流程运行速度提升 10,000 至 100,000 倍。

2.  结合 SAP IQ 这一数据仓库解决方案，使用大数据分析。

3.  跨逻辑大数据仓库虚拟化数据，无需移动数据即可获取洞察。

14. Teradata Aster 拥有一个分析引擎，该引擎是用于跨大数据集进行图分析的原生图处理引擎。利用这一新一代分析引擎，组织可以解决复杂的业务问题，例如社交网络或影响力分析、欺诈检测、供应链管理、网络分析和威胁检测以及洗钱检测。[^(15)](#c05.xhtml#c05-fn-15)

15. 新一代数据分析师使 R 成为当今市场上最流行的分析软件。Teradata Aster R 通过预构建的并行 R 函数、并行构造函数，以及在 Aster SNAP Framework 中集成开源 R，消除了开源 R 的局限性。

> **What is R?**
>
> R is a language and environment for statistical computing and graphics. It is a GNU project which is similar to the S language and environment developed at Bell Laboratories (formerly AT&T, now Lucent Technologies). R can be considered as a different implementation of S. There are some important differences, but much code written for S runs unaltered under R.
>
> R provides a wide variety of statistical (linear and nonlinear modeling, classical statistical tests, time-series analysis, classification, clustering ...) and graphical techniques, and is highly extensible. The S language is often the vehicle of choice for research in statistical methodology, and R provides an open-source route to participation in that activity.
>
> One of R’s strengths is the ease with which well-designed publication-quality plots can be produced, including mathematical symbols and formulae where needed. Great care has been taken over the defaults for the minor design choices in graphics, but the user retains full control.
>
> R is available as free software under the terms of the Free Software Foundation’s GNU General Public License in source code form. It compiles and runs on a wide variety of Unix platforms and similar systems (including FreeBSD and Linux), Windows and MacOS.[^(16)](#c05.xhtml#c05-fn-16)

Aster Discovery Platform 还提供 SQL 和 SQL-MapReduce 分析引擎，支持适合这些引擎的多种分析，如 SQL 分析、路径或模式分析、统计分析和文本分析。

## 知识检验

### 3. What is MapR?

1.  A program to reduce the size of big data analyzed.
2.  Incomplete distribution of Apache Hadoop.
3.  **Complete distribution for Apache Hadoop that packages more than a dozen projects.**
4.  A relational database for big data.

### 4. What is Teradata?

1.  Agriculture applications of big data.
2.  DA software.
3.  A native graph processing engine for graph analysis.
4.  **A relational database.**

## 企业级顶尖数据分析工具

以下企业级顶尖数据分析工具列表基于 KDNuggets（一个商业分析网站）的 Alex Jones 制定的指南。他根据工具的“免费可用性（个人使用）、易用性（无需编码且设计直观）、强大功能（超越基础 Excel）和文档资源完善度”（如简单的 Google 搜索即可支持业务需求）来推荐工具。[^(17)](#c05.xhtml#c05-fn-17)

16. **Tableau** 是一款数据可视化软件，旨在将大量原始数据转换为更易于解读的格式，以支持决策。Tableau 软件允许用户查询关系型数据库、联机分析处理（OLAP）立方体、云数据库和电子表格，以生成图形化数据可视化。

**OpenRefine**（前身为 Google Refine），根据其网站介绍，“清理混乱的数据并将其从一种格式转换为另一种格式；通过 Web 服务进行扩展；并将其链接到像 Freebase 这样的数据库。”使用该软件，用户可以执行以下操作：[^(18)](#c05.xhtml#c05-fn-18)

1.  导入各种数据格式。
2.  在几秒钟内探索数据集。
3.  应用基础和高级单元格转换。
4.  处理包含多个值的单元格。
5.  在数据集之间创建即时链接。
6.  使用正则表达式轻松筛选和分区数据。
7.  对全文字段使用命名实体提取以自动识别主题。
8.  使用通用精炼表达语言执行高级数据操作。

17. **KNIME** 可通过可视化编程操作、分析和建模数据。用户通过拖拽活动之间的连接点来操作，而非编写代码块。该软件可扩展以运行 R、Python、文本挖掘、化学数据等，提供了进行更高级代码驱动分析的选项。

18. **RapidMiner** 是一个数据科学平台，集数据准备、机器学习和预测模型部署于一体。为数据科学家提供深度，为其他人提供简化操作。它通过可视化编程运行，可操作、分析和建模数据。

19. **Google Fusion Tables** 已停止服务，但您仍可使用 Excel 的后续版本结合 Google Maps 执行相同的分析。Google 拥有领先的地图软件。（详见后文“Google Maps”部分。）[表 5-3](#c05.xhtml#tab5-3) 展示了来自 FBI 的 2013 年犯罪统计数据。[^(19)](#c05.xhtml#c05-fn-19)

![Table 5-3](images/tab5-3a.png)

**Table 5-3**

使用 Excel 上传数据后，创建了按城市划分的暴力犯罪地图。

随后可点击地图查看 2013 年 Green Bay 市的暴力犯罪情况。使用 Excel 2016 或 2019 及其 3D Maps 工具也可达到相同效果。首先将数据准备为表格。创建一个包含五列的表格。

![](images/ufig5-6.png)

从“插入”选项卡最右侧调用 3D Maps。

![](images/ufig5-7.png)

3D Maps 会自动将信息放置在美国地图上。在下图中，地图已缩小至威斯康星州。图标的颜色和大小均已更改。

![](images/ufig5-8.png)

1.  **NodeXL** 是一款用于网络和关系的可视化与分析软件。[第 6 章](#c06.xhtml) 演示了一个概念相似的 LinkedIn 连接示例。NodeXL 更进一步，提供精确计算。

2.  **Import.io** 提供快速访问 Web 数据的功能。该软件高亮显示相关数据，并在“几分钟内”“学习”用户所需内容。随后，Import.io 将为用户提取数据以供分析或导出。

3.  **Google 搜索运算符** 往往是一个未被充分利用的研究工具。运算符允许快速筛选 Google 结果，以获取最有用和相关的信息。例如，可以通过访问主要 CPA 事务所的网站来获取 CFO 调查信息，如下所示：

    ![](images/ufig5-9.png)

    不要忘记利用搜索的强大功能，使用时间搜索等附加工具。

4.  **Solver** 是 Excel 中的一个优化和线性规划工具，允许用户设置约束条件。它不是最强大的优化包，但如果公司从未探索过优化分析，它将非常有帮助。对于高级优化，可考虑使用 R 的 `optim` 包等程序。
