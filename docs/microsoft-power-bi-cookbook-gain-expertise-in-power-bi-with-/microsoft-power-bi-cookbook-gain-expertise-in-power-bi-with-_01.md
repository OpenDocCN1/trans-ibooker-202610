## 贡献者

### 关于作者

**Greg Deckler** 是 Fusion Alliance 的副总裁，从事咨询服务业已超过 27 年。Greg 被公认为 Power BI 专家，曾六次获得 Microsoft 数据平台 MVP 称号，并且是 Power BI 社区网站的活跃成员，撰写了超过 5,000 个解决方案，并提交了数百个 Quick Measure Gallery 作品。Greg 于 2016 年在俄亥俄州哥伦布市创立了 Columbus Azure ML 和 Power BI 用户组，并持续每月定期举行会议。

> 我要感谢整个充满活力和朝气的 Power BI 社区，特别感谢所有超级用户和我用户组的成员。最后，我要感谢我的儿子 Rocket，感谢他不断地为我提供出色的图形和视频！

**Brett Powell** 是 Frontline Analytics LLC 的所有者，该公司是一家专注于 Microsoft 数据平台的数据与分析咨询公司。Brett 在商业智能和数据仓库项目方面拥有超过十年的经验，既担任过开发人员，也担任过技术架构师。除了项目工作外，Brett 还通过他广受欢迎的 *Insights Quest* 博客分享技术技巧和示例，并撰写了两本商业上成功的书籍——*《精通 Microsoft Power BI》* 和 *《Microsoft Power BI 实战手册》*。

> 我首先要感谢和赞美我的主和救主耶稣基督，感谢他赐予我这些机会，并每天爱我、引导我。我还要感谢我的母亲 Cathy 和兄弟 Dustin 一直以来给予我的支持和鼓励。最后，我要感谢 Packt 为制作优质、有价值的内容所做的努力。

### 关于审校者

**David R. Eldersveld** 是 Microsoft 的技术专家，目前专注于 Power BI。在加入 Microsoft 之前，他曾是数据平台 MVP，并在 CapstoneBI 和 BlueGranite 担任了近十年的数据与分析顾问。David 作为博客作者、演讲者和 *Workout Wednesday* Power BI 挑战赛的贡献者，为技术社区做出了贡献。
## 前言

Microsoft Power BI 是一组商业智能和分析应用程序及服务的集合，旨在相互协调工作，为数据提供可视化、交互式见解。

本书提供了使用所有主要Power BI工具和功能的详细、分步技术示例，并演示了利用和集成Power BI技术与服务的端到端解决方案。您将熟悉Power BI开发工具和服务；深入了解Power BI的数据连接和转换、建模、可视化和分析能力；并见证Power BI的函数式编程语言DAX和M如何实际应用，以提供强大的解决方案，应对商业智能中常见的挑战性场景。

本书旨在通过详细的分步操作指南、高级设计和开发技巧，以及增强现有Power BI项目的指导，激发您的兴趣并帮助您更好地利用Power BI。

本书第二版包含更多操作指南，涵盖Power BI的最新增强功能和集成。所有其他操作指南都已更新和修订，使其更详细、更易于理解，并且完全基于标准的Adventure Works DW数据库。

### 本书读者对象

本书适合希望加深对Power BI设计和开发知识，同时提升所交付的Power BI解决方案价值的商业智能专业人士。那些希望快速解决常见问题，并需要Power BI功能和设计模式参考指南的读者，一定会发现本书是一本有用的资源。

## 本书涵盖内容

*第1章，配置Power BI工具*，涵盖BI专业人士在设计和开发Power BI内容时所使用的主要工具和服务的安装与配置，包括Power BI Desktop、本地数据网关、在Excel中分析、DAX Studio、Tabular Editor和ALM Toolkit。

*第2章，访问和检索数据*，深入探讨Power BI Desktop的“获取数据”体验，并逐步讲解建立和管理数据源连接与查询的过程。

*第3章，构建Power BI数据模型*，探讨设计和开发稳健数据模型的主要流程。

*第4章，创作Power BI报表*，详细讲解和描述最基本的报表可视化和设计概念。此外，还提供了相关指导，以增强和控制用户在Power BI服务和移动设备上消费和交互Power BI报表时的体验。

*第5章，在服务中工作*，涵盖Power BI仪表板的构建，这些仪表板用于提供关键度量和影响力较大的业务活动的简单、一目了然的监控。

*第6章，认真对待日期智能*，包含准备数据模型以支持强大日期智能的操作指南，以及编写自定义日期智能度量值的操作指南。

*第7章，参数化Power BI解决方案*，涵盖Power BI中标准的参数化功能和技术，以及更高级的自定义实现。

*第8章，在Power BI中实现基于用户的动态可见性*，包含为导入模型和DirectQuery模型构建和部署基于用户的动态安全的详细示例，以及开发动态筛选上下文功能以增强用户体验。

*第9章，应用高级分析和自定义视觉对象*，包含多种多样的操作指南，重点介绍Power BI许多最新、最受欢迎的自定义可视化和高级分析功能。

*第10章，管理和监控Power BI*，重点介绍最常见和最有影响力的管理数据源，包括Windows性能监视器、SQL Server查询存储、Microsoft本地数据网关、MSDB系统数据库、扩展事件、Office 365审核日志和Power BI REST API。

*第11章，增强和优化现有Power BI解决方案*，包含顶级的数据建模、DAX度量值和M查询模式，以增强Power BI数据集的性能、可扩展性和可靠性。

*第12章，部署和分发Power BI内容*，包含通过Power BI服务和Power BI移动应用程序部署和分发Power BI内容的详细示例和注意事项。

*第13章，将Power BI与其他应用程序集成*，重点介绍了Power BI与SSAS、SSRS、Excel、PowerPoint、PowerApps、Power Automate和Dynamics 365之间全新而强大的集成。

### 充分利用本书

-   建议具备一些Power BI的基础知识。
-   Power BI Desktop 和 Power BI 服务账户（两者均免费）。
-   Tabular Editor 2.x（免费）。
-   DAX Studio（免费）。
-   SQL Server（任何版本，包括免费的Express和Developer版），并安装**AdventureWorksDW2019**数据库。该数据库可免费下载，安装说明可参见：`http://bit.ly/20VQfG7`。

### 下载示例代码文件

本书的代码包也托管在GitHub上，网址为 `https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition`。我们丰富的图书和视频目录中还有其他代码包，可访问 `https://github.com/PacktPublishing/` 获取。快去看看吧！

### 下载彩色图片

我们还提供一份PDF文件，其中包含本书所用截图/图表的彩色图片。您可以在此下载：`https://static.packt-cdn.com/downloads/9781801813044_ColorImages.pdf`。

### 使用的约定

本书通篇使用了一些文本约定。

`CodeInText`：表示文本中的代码字、数据库表名、文件夹名称、文件名、文件扩展名、路径名、虚拟URL、用户输入和Twitter用户名。例如：“完成这些步骤后，所有报表都将会实时连接回数据集 `CH5_R1_SalesAndFinanceDataset`。”




# 前言

代码块设置如下：

```
let
    Source = AdWorksDW,
    Data = Source{[Schema="dbo",Item="vCH3R2_Products"]}[Data]
in
    Data
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
**Products Rows (Sales)** = CALCULATE(COUNTROWS('Products'),'Internet Sales')
```

**粗体：** 表示新术语、重要词语，或您在屏幕上看到的词语，例如在菜单或对话框中，也会以这种方式出现在文本中。例如：“**Manage roles** 对话框包括设置 **Precedence** 的功能。”

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_14_0.png)

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_14_1.png)

# 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及图书标题。如果您对本书的任何方面有疑问，请发送电子邮件至 questions@packtpub.com。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但仍可能出现错误。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 http://www.packtpub.com/submit-errata，选择您的图书，点击勘误提交表单链接并输入详细信息。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制，我们将不胜感激您提供位置地址或网站名称。请发送电子邮件至 copyright@packtpub.com 并附上材料链接。

**如果您有兴趣成为作者：** 如果您在某个主题方面有专业知识，并且有兴趣撰写或为图书做出贡献，请访问 `http://authors.packtpub.com`。

# 分享您的想法

在您阅读完 *Microsoft Power BI Cookbook, Second Edition* 后，我们很乐意听到您的想法！请点击此处直接前往本书在 Amazon 的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质的内容。

# 1
