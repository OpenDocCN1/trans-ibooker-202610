

## 约定用法

在本书中，你会找到多种文本样式，用以区分不同类型的信息。以下是这些样式的示例及其含义。文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："The last characters CI and AS are for case insensitive and accent sensitive, respectively." 代码块的设置如下：

```sql
USE DQS_STAGING_DATA;
SELECT CustomerKey, FullName, StreetAddress, City, StateProvince,
CountryRegion, EmailAddress, BirthDate, Occupation;
```

CodeInText: 表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。以下是示例："The simplest query to retrieve the data that you can write includes the SELECT and the FROM clauses. In the SELECT clause, you can use the star character (*), literally SELECT *, to denote that you need all columns from a table in the result set."

代码块的设置如下：

```sql
USE WideWorldImportersDW;
SELECT *
FROM Dimension.Customer;
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目以粗体显示：

```sql
USE WideWorldImporters;
CREATE TABLE dbo.Product
(
    ProductId INT NOT NULL CONSTRAINT PK_Product PRIMARY KEY,
    ProductName NVARCHAR(50) NOT NULL,
    Price MONEY NOT NULL,
    ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL,
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL,
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
)
WITH (SYSTEM_VERSIONING = ON);
```

## 前言

任何命令行输入或输出均按如下方式书写：

```
Customer                         SaleKey  Quantity
--------                         -------  --------
Tailspin Toys (Aceitunas, PR)   36964    288
Tailspin Toys (Aceitunas, PR)   126253   250
Tailspin Toys (Aceitunas, PR)   79272    250
```

**粗体**: 表示新术语、重要词语或你在屏幕上看到的词语。例如，菜单或对话框中的词语在文本中以此方式出现。以下是示例："Go to **Tools | Options** and you are then able to type your search string in the textbox in the top-left of the **Options** window."

![](img/07d1225e179d5df9f2b2d26b026386c6_21_0.png)

> 警告或重要说明以此框形式出现。

![](img/07d1225e179d5df9f2b2d26b026386c6_21_1.png)

提示和技巧以此方式出现。

## 联系我们

我们非常欢迎读者的反馈。

**一般反馈：** 发送邮件至 `feedback@packtpub.com`，并在邮件主题中提及图书标题。如对本书的任何方面有疑问，请发送邮件至 `questions@packtpub.com`。

**勘误：** 尽管我们已尽力确保内容的准确性，但错误仍可能发生。如果你发现本书中的错误，我们将不胜感激你向我们报告。请访问 `www.packtpub.com/submit-errata`，选择你的图书，点击 Errata Submission Form 链接并输入详细信息。

**盗版：** 如果你在互联网上发现我们作品的任何形式的非法拷贝，我们将不胜感激你提供位置地址或网站名称。请发送邮件至 `copyright@packtpub.com` 并附上材料链接。

**如果你有兴趣成为作者：** 如果你有专业知识并且有兴趣撰写或为图书做出贡献，请访问 `authors.packtpub.com`。

## 评论

请留下评论。阅读并使用本书后，不妨在你购买的网站上留下评论。潜在读者可以借此了解你的公正意见，我们 Packt 可以了解你对我们产品的看法，而我们的作者也可以看到你对他们图书的反馈。感谢你！

有关 Packt 的更多信息，请访问 `packtpub.com`

## SQL Server 2017 简介

SQL Server 是微软的主要关系数据库管理系统产品。它自 80 年代末以来以一种或另一种形式存在（与 Sybase 合作开发），但作为独立的微软产品，它自 90 年代初就已存在。在过去的 20 年里，SQL Server 发生了变化和演进，沿途获得了新特性和功能。

我们今天所知的 SQL Server 基于其历史上 arguably 最重要的（r）演进性一步：SQL Server 2005 的发布。所引入的变更使得 2005 发布之后的版本能够利用更新的硬件和软件改进，例如：64 位内存架构、更好的多 CPU 和多核支持、与 .NET 框架更好的对齐，以及总体系统架构的许多现代化。

在每个后续版本的 SQL Server 中引入的增量变更继续在此坚实的新基础上改进。幸运的是，微软已更改了多个产品的发布周期，包括 SQL Server，从而导致发布之间的时间间隔缩短。这在一定程度上是由于微软对其广受报道的移动优先、云优先战略的关注。该战略以及 SQL Server Azure SQL Database 云版本的开发，使得微软被迫进入大幅缩短的发布周期。此战略的优势在于，我们不再需要等待 3 到 5 年才能获得新版本（和新功能）。自 SQL Server 2012 引入以来，每两年就有一次发布，而在真正的版本之间还有 Azure SQL Database 的多次发布。

虽然我们可以高兴不再需要等待新版本，但我们也处于明显的劣势。新的版本和功能的快速发布，使得我们开发人员用于掌握这些闪亮新功能的时间越来越少。之前的版本发布间隔多年，让我们能够在必须吸收新信息之前，更深入地了解和掌握可用功能。

在 SQL Server 2016 发布后不久，SQL Server 2017 随之发布，距离 2016 发布仅一年多。许多功能只是 2016 发布版本的更精炼/更新版本，而 2017 发布中也有一些值得注意的增添。

在本章（和本书中），我们将介绍 SQL Server 2017 中的新内容。由于发布周期短，我们将概述产品此版本中的全新功能，并查看自 SQL Server 2016 以来已扩展或改进的功能。

我们将在以下领域概述新功能：

- Security
- Engine features
- Programming
- Business intelligence

## Security

过去几年使 IT 安全的重要性变得极其明显，特别是当我们考虑到爱德华·斯诺登数据泄露事件的后果或通过黑客进行数据盗窃的多个案例时。虽然没有任何系统是完全不可渗透的，但我们应始终考虑如何改进我们构建的系统的安全性。这些考虑范围广泛，有时甚至由规则、法规和法律规定。微软通过提供新功能来应对对安全的关注增加，以帮助开发人员和 DBA 寻找更安全的系统。

## 行级安全

在 SQL Server 2016 中引入的第一个用于解决对增加/改进安全需求的技术是行级安全（RLS）。RLS 提供了根据执行查询的用户控制对表中行的访问的能力。有了 RLS，就可以在数据库中的任何表上实现过滤机制，对任何外部应用程序或直接 T-SQL 访问完全透明。

无需重新设计数据访问层即可实现此类过滤的能力，使得系统管理员能够比以前更精细地控制对数据的访问。能够在无需任何应用程序逻辑重新设计的情况下实现此控制，使得该功能对某些用例更具吸引力。RLS 还使得结合必要的审计功能，能够锁定 SQL Server 数据库，以至于即使传统的 *god-mode* sysadmin 也无法访问底层数据。

## 动态数据屏蔽

我们将要介绍的第二个安全功能是 **动态数据屏蔽（DDM）**。DDM 允许系统管理员定义列级数据屏蔽算法，防止用户读取列的内容，同时仍能查询行本身。此功能最初旨在允许开发人员使用生产数据的副本进行工作，而无需实际 *查看* 底层数据。这在执行数据保护法律的环境中特别有用（例如，信用卡处理系统和医疗记录存储）。数据屏蔽仅在查询运行时发生，并且不影响表的存储数据。这意味着可以通过简单的 DDL 语句屏蔽多 TB 的数据库，而无需诉诸于先前通过物理屏蔽表中要屏蔽的底层数据的解决方案。DDM 的当前实现提供了将固定的一组函数定义到表的列的能力，这些函数将在查询屏蔽表时屏蔽数据。如果用户有权查看屏蔽数据，则不会运行屏蔽函数，而可能无法查看屏蔽数据的用户将通过定义的屏蔽函数看到的数据提供。

## Always Encrypted

在 SQL Server 2016 中引入的第三个主要安全功能是 Always Encrypted。SQL Server 的加密 previously 主要是基于服务器的解决方案。数据库要么在数据库级别受加密保护（整个数据库被加密），要么在列级别（单个列定义了加密算法）。虽然这种加密完全功能且安全，但加密过程的关键部分（例如，加密证书）存储在 SQL Server 内部。这有效地赋予了 SQL Server 实例的所有者潜在访问此加密数据的能力——如果不是直接访问，至少为潜在的恶意访问尝试提供了更大的攻击面。随着越来越多的公司转向托管服务和云解决方案（例如 Microsoft Azure），之前的加密解决方案不再提供所需级别的控制/安全。

