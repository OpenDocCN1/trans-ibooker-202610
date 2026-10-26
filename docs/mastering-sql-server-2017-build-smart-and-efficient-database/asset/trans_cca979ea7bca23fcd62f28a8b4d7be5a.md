

# 为数据科学应用设置 Visual Studio 2017

这里介绍了 Visual Studio 2017。你可以使用 Professional 版或免费的 Community Edition（https://www.visualstudio.com/downloads/）来开发 Python 和 R 代码。

在安装 Visual Studio 2017 时，务必选择 Python 开发工作负载，然后选择数据科学和数据分析应用程序，如以下屏幕截图所示。这样将安装 Python 语言模板，包括数据科学模板，并安装 R Tools for Visual Studio 2017：

![](img/07d1225e179d5df9f2b2d26b026386c6_72_0.png)

好了，你几乎准备就绪了。这里有一个小技巧。VS 2017 也会安装其自己的 Python 解释器。为了使用随 SQL Server 安装的可扩展版本，该版本可在 Database Engine 上下文中执行代码并包含 Microsoft 可扩展库，你需要设置一个额外的 Python 环境，指向可扩展版本的解释器。如果安装了 SQL Server 的默认实例，该可扩展解释器的路径为 `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\python.exe`。你可以在以下屏幕截图中看到如何设置此环境。请注意，由于 VS 2017 也安装了 Microsoft R Client，即包含 Microsoft 可扩展库的开放 R 引擎，因此开发 R 代码无需任何额外步骤：

![](img/07d1225e179d5df9f2b2d26b026386c6_73_0.png)

设置 Python 环境

就这样。你已准备好开始 Python 编程。只需新建一个项目并从 Python 文件夹中选择 **Python Application** 模板。你还可以探索 **Python Machine Learning** 模板，其中包括 **Classifier**、**Clustering** 和 **Regression** 项目。如果你选择了 **Python Application** 模板，你应该会打开第一个空的 Python 脚本，其默认名称与项目名称相同，默认扩展名为 `.py`，等待你编写和交互式执行 Python 代码。

## Summary

在本章中，我们回顾了 SQL Server 2017 开发者工具集中新增的内容。已进行了一些期待已久的改进，尤其是将 SQL Server Management Studio 与 SQL Server 本身的发布周期分离。SSMS 加速的发布周期带来了大量新功能和支持，将有助于开发者在日常使用 SQL Server 的工作中。

SSMS 的一些特色新增功能非常强大，将使我们作为开发者更高效。Live Query Statistics 为我们提供了查询实际处理方式的出色洞察，去除了在尝试重构或调优查询时猜测的部分。Vulnerability Assessment 工具应能更快地识别数据库中潜在的安全威胁。更安全的数据库意味着更安全的业务！

对于 SQL Server 开发者来说，有两个新的开发环境可用于开发 R 代码。当然，其中之一 RStudio IDE 在 R 开发者中是众所周知的。由于它被广泛使用，它很可能会是开发 R 代码的首选，也是 SQL Server 开发者的首选。尽管如此，如果你习惯并热爱 Visual Studio IDE，你可以尝试 R Tools for Visual Studio。

在简要介绍了随 SQL Server 2017 交付的工具之后，现在，是时候检查 SQL Server 2017 中的 JSON 支持了。

## 3
## JSON Support in SQL Server

在过去几年中，JSON 已确立为应用程序和服务之间数据交换的标准格式。XML 仍然是交换标准（并且将继续如此），但许多应用程序通过交换 JSON 数据而不是 XML 文档进行通信。因此，最重要的关系型数据库管理系统产品需要支持 JSON。

社区在两个发布周期后请求该功能，微软已在 SQL Server 2016 中实现了内置的 JSON 支持。支持不如 XML 那么全面，但对于大多数数据库和工作负载来说，它会相当不错。

本章探讨了 SQL Server 如何存储和处理 JSON 数据，并全面比较了 SQL Server 中 JSON 与 XML 的支持。

与 SQL Server 中的 JSON 数据相关的最重要操作已详细演示：

- Formatting and exporting JSON data from SQL Server
- Converting JSON data to a tabular format
- Importing JSON data to SQL Server
- Validating, querying, and modifying JSON data

最后，你将了解到由于缺少 JSON 数据类型和索引而导致的限制，并获得有关如何在这些限制下仍提高 JSON 查询性能的建议。

## Why JSON?

Microsoft Connect 网站是你可以留下对 Microsoft 产品反馈、建议和愿望的地方。SQL Server 最受欢迎的功能请求是 JSON 支持请求。它于 2011 年 6 月创建，截至撰写时（2017 年 10 月）已有 1,138 票。该请求仍处于开放状态，如以下屏幕截图所示，你将在本章后面看到为什么它仍然有意义保持活动状态：

![](img/07d1225e179d5df9f2b2d26b026386c6_76_0.png)

社区成员用来证明该请求的论据如下：

- JSON 已是标准，应该得到支持，类似于 XML。
- 其他厂商支持它（Oracle、PostgreSQL 等）
- 由于缺乏 JSON 支持，我的客户希望从 SQL Server 迁移到支持 JSON 的其他数据库系统。

正如 vox populi（多数人的意见或信仰）一样，一些论据和给出的示例代表了开发和业务需求。有些则非常个人化，有时受热情驱使。但有一点他们都同意，并且几乎所有评论中都很常见：*一个严肃的关系型数据库管理系统应该对 JSON 有显著的支持*。该项目创建近五年后，微软在 SQL Server 2016 中添加了 JSON 支持。

当然，Microsoft Connect 网站上的投票数量并不是该功能的唯一原因。其他竞争对手（PostgreSQL、Oracle、DB2、MySQL）已经引入了 JSON 支持；其中一些，如 PostgreSQL，非常严谨和健壮。如果你还想成为一个受人尊敬的厂商，你需要提供 JSON 支持。

## What is JSON?

**JavaScript Object Notation (JSON)** 是应用程序和服务之间数据交换的开放标准格式。JSON 对象是人类可读的键值对列表。尽管其名称暗示了这一点，但 JSON 是语言无关的。它在 ECMA-404 标准中指定（http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf）。

![](img/07d1225e179d5df9f2b2d26b026386c6_77_0.png)

> ECMA International 是一个成立于 1961 年的行业协会，致力于 **信息和通信技术 (ICT)** 和 **消费电子 (CE)** 的标准化。你可以在 https://www.ecma-international.org 找到更多关于它的信息。

JSON 非常简单且非常流行。它通常用于 AJAX 应用程序、配置、RESTful Web 服务、社交媒体应用以及 MongoDB 和 CouchDB 等 NoSQL 数据库管理系统。许多开发者更喜欢 JSON 而非 XML，因为他们认为 JSON 更简洁、更易读。

SQL 中的 JSON 支持在最新的 SQL 标准——SQL:2016 中定义。该标准描述了如何存储、发布和查询 JSON 数据，并定义了 SQL/JSON 数据模型和路径语言。规范文档（*ISO/IEC TR 19075-6:2017*）可在以下网页地址获得：https://www.iso.org/standard/67367.html。

## Why is it popular?

JSON 是一个简单的数据格式，但其简单性并不是使其成为 Web 服务和应用程序之间交换数据的领先标准唯一原因。它流行的最重要原因是 JSON 格式是许多编程语言的原生格式，例如 JavaScript。它们可以原生生成和消费 JSON 数据，而无需序列化。近年来软件开发中最大的问题之一是对象关系阻抗。JSON 数据格式灵活且自描述，允许你在不为数据定义严格模式的情况下有效使用它，而 XML 可能需要这样做。这允许快速集成数据。

## JSON versus XML

JSON 和 XML 都是简单、开放且可互操作的。由于 JSON 通常包含的数据更少，使用 JSON 时，网络中传输的数据更少。两种格式都可读；JSON 稍微更干净，因为它包含的文本更少。这是因为 JSON 支持的数据格式数量远少于 XML。

JSON 方便共享数据。JSON 中的数据存储在数组和对象中，而 XML 文档形成树结构。因此，使用 JSON 进行数据传输更容易且对许多编程语言原生：JavaScript、Python、Perl、Ruby 等。另一方面，XML 可以存储更多数据类型（JSON 甚至没有日期数据类型）；它可以包含照片、视频和其他二进制文件。XML 更健壮，更适合复杂文档。XML 还提供数据表示选项，而 JSON 只是传输数据，而不建议或定义如何显示它。

总体而言，JSON 更适合作为数据交换格式，而 XML 更方便作为文档交换格式。

## JSON objects

根据 ECMA 规范，JSON 文本是由 Unicode 代码点组成的令牌序列，符合 JSON 值语法。一个 JSON 值可以是：

- **Primitive:** 这是字符串、数字、true/false 或 null 值
- **Complex:** 这是对象或数组

