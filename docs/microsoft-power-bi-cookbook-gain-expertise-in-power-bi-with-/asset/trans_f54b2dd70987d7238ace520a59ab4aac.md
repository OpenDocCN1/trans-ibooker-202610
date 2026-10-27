
## 目录

| 条目 | 页码 |
|---|---:|
| 第10章：管理和监控Power BI | 439 |
| 技术要求 | 440 |
| 使用Power BI创建集中式IT监控解决方案 | 440 |
| 构建监控、可视化和分析层 | 446 |
| 导入和可视化动态管理视图（DMV）数据 | 450 |
| 提高DBA工作效率 | 455 |
| 提供文档 | 461 |
| 分析SSAS表格数据库和网关 | 467 |
| 分析扩展事件 | 473 |
| 可视化日志文件数据 | 478 |
| 利用Power BI PowerShell模块 | 484 |
| 小结 | 489 |
| 第11章：增强和优化现有Power BI解决方案 | 491 |
| 技术要求 | 492 |
| 增强数据模型的可扩展性和可用性 | 492 |
| 提升DAX度量值性能 | 499 |
| 将查询处理推回源系统 | 505 |
| 加强数据导入和集成流程 | 510 |
| 隔离和记录DAX表达式 | 514 |
| 通过增量刷新提高数据加载速度 | 518 |
| 小结 | 522 |
| 第12章：部署和分发Power BI内容 | 523 |
| 技术要求 | 524 |
| 为内容创建和协作做准备 | 524 |
| 管理环境之间的内容 | 532 |
| 与同事共享内容 | 535 |
| 配置工作区 | 539 |
| 配置本地网关数据连接 | 545 |
| 发布应用 | 551 |
| 将报表发布到公共互联网 | 556 |
| 启用移动体验 | 561 |
| 通过Teams分发内容 | 568 |
| 小结 | 572 |
| 第13章：将Power BI与其他应用程序集成 | 573 |
| 技术要求 | 574 |
| 集成SSRS和Excel | 574 |
| 从Excel数据的Power Pivot迁移到Power BI | 582 |
| 在Excel中访问和分析Power BI数据集 | 586 |
| 在PowerPoint演示文稿中构建Power BI报表 | 592 |
| 连接到Azure Analysis Services | 596 |
| 与Power Automate和Power Apps集成 | 600 |
| 利用Dataverse和Dynamics 365 | 610 |
| 连接Dynamics 365 Finance and Operations和Power Platform | 615 |
| 小结 | 621 |
| 为什么订阅？ | 629 |
| 其他您可能喜欢的图书 | 625 |
| 索引 | 629 |

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
