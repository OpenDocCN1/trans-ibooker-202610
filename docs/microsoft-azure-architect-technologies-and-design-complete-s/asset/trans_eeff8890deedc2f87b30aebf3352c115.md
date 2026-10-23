

## 图 8.7 在 Hyper-V 中导出虚拟化服务器

![](img/95043b3df926bcdadadb082b49262037_596_0.png)

导出后，如有需要，你可以使用以下 PowerShell 命令将 VHDX 转换为 VHD：

```
Convert-VHD -Path "C:\Temp\Metasploitable.vhdx" `
    -DestinationPath "C:\Temp\Metasploitable.vhd" -VHDType Fixed
```

## Windows Server/Linux 工作负载

一种简单的方法是查看创建虚拟机时默认的镜像列表，以找到 Microsoft 为 Azure VM 提供的最常见或最推荐的操作系统版本。表 8.1 是截至本文撰写时你在该下拉列表中会找到的当前列表。

请注意，镜像下拉列表正下方有一个链接，可浏览所有公共和私有镜像。选择非常多，因此表 8.1 中提供的列表仅为概要，绝非完整列表。该列表还包含第三方镜像，因此列表并非静态的，因此本书并不是尝试创建完整列表的地方。这里请记住两点。第一，Linux 是开源的，这意味着任何人都可以创建和定制其版本。第二，这些定制在部署到 Azure 虚拟机时可能有效也可能无效。这就是为什么你希望使用平台上已有的版本和/或使用经认可即受支持的 Linux 版本的原因。你可以在此处找到它们的列表：

docs.microsoft.com/en-us/azure/virtual-machines/linux/endorsed-distros.

你将用于将本地数据中心迁移到 Azure 的许可证。Azure 上的软件许可费用通常已包含在计算平台的整体使用费用中。如果你已有许可证，那么使用你已有的许可证可能会获得某种折扣。有道理吧？Azure Hybrid Benefit 计划正是为此目的而存在的。如果你已有 Windows Server 和/或 SQL Server 许可证，那么将工作负载迁移到 Azure 时，计算成本可节省 40% 至 55%。如果你将此优惠计划与后面讨论的 Azure 预留 VM 实例（RI）结合使用，可能将节省提高到 80%。

## Azure 预留 VM 实例

云中通常采用的默认定价模式基于你的使用量。当你需要大量计算或其他 Azure 资源时，它随时为你提供。当你不再需要时，硬件和基础设施仍然存在，但如果你不使用则不会为你计费。这对你有利，也是云概念的巨大优势之一。从云服务提供商的角度来看，基础设施和计算资源处于空闲状态是不划算的。对此的解决方案是某种程度上分摊空闲硬件的成本。这种分摊的手段是通过 RI 实现的。目前支持此模式的资源可以在 Azure 门户的预留项刀片中看到。你的预留项刀片应与图 8.8 所示类似。

## 图 8.8 Azure 预留实例

![](img/95043b3df926bcdadadb082b49262037_598_0.png)

与其根据不可预见的使用量产生按需付费费用，你可以承诺在 7 天、30 天、60 天或更长时间内支出特定金额。它需要少于仅按使用量收费的情况，但比在同一时间段内使用量低于预期时多一些。总体成本平均值对客户来说更低，但对云提供商来说更具成本效益。基础设施不会只是闲置无所事事且产生零收入。查看使用 RI 时产生的成本，将其与动态按需付费使用费用进行比较，如果你能节省费用，那么务必利用它。

## 容器

如果你想快速迁移和提升一种方式，可以使用容器。万一你忘记了这种模式，请查看图 4.1 中可视化的容器即服务（CaaS）。另外，回顾图 4.2 中说明的决策树，以再次了解选择容器作为部署或迁移选项的理由。如果你陷入困境或已用尽所有努力迁移工作负载到 Azure，因为应用程序中存在当前不受支持的定制，请不要忽略或忘记此选项。认真考虑联系 Microsoft 支持；认真说，现在是与 Microsoft 支持合作的时候了。他们想要你的业务，并希望你在 Azure 上取得成功。

总之，容器可以让你将应用程序完全与 VM 和操作系统隔离。这两个或其中一个层可能是你的部署或迁移计划未能按预期进展的原因。移除它们可能是最快的解决方案。不过在此要提醒一下。在做出此类决定之前，请认识到这会带来重大的架构变更，以及迁移和支持此类架构所需的技能集影响。

## 数据库迁移

你的数据存储和数据存储需求的迁移需要遵循迁移计算所使用的结构化流程。参考图 8.1，其中指出了评估、迁移、优化以及安全和管理阶段。与评估和迁移一样，评估和迁移数据库的特定工具也存在。你完全可以使用 Azure Migrate 来帮助发现和盘点你想要迁移的机器。

一个决定迁移方法的重要决策点与数据源密切相关。

数据源是 Microsoft SQL Server，还是其他众多数据源之一，如 Oracle、MySQL、Cosmos DB 和 Mongo？

你计划将这些非 Microsoft SQL Server 数据库迁移到 Azure 上的 SQL Server 之一吗？

你是将现有系统迁移到 Azure，还是迁移到 Azure 并更改 DBMS？

如果你是从本地 Microsoft SQL Server 安装迁移到 Azure 上的 SQL Server 产品，那么一个不错的起点是 *数据库管理助手（DMA）*。

## 数据库管理助手

DMA 是一款工具，可帮助你识别当前运行的 SQL Server 版本与迁移目标版本之间的兼容性问题。表 8.2 提供了受支持的源数据库版本和受支持的目标数据库版本的概述。

**表 8.2  SQL Server 支持的源和目标**

| 位置 | 版本 |
|---|---|
| Source | SQL Server 2005 |
| Source | SQL Server 2008 |
| Source | SQL Server 2008 R2 |
| Source | SQL Server 2012 |
| Source | SQL Server 2014 |
| Source | SQL Server 2016 |
| Source | SQL Server 2017 on Windows |
| Source | SQL Server 2019 |
| Target | SQL Server 2012 |
| Target | SQL Server 2014 |
| Target | SQL Server 2016 |
| Target | SQL Server 2017 Windows / Linux |
| Target | SQL Server 2019 |
| Target | Azure SQL single database |
| Target | SQL Server manages instance |
| Target | SQL Server on an Azure virtual machine |

你可能会同意，如果你的源数据库是 SQL Server 2005 而目标是 SQL Server 2019，途中可能有一些功能已被弃用。DMA 特别指出已弃用的功能、行为变更和破坏性变更。其中大部分需要重新设计或重构才能成功迁移。DMA 也提供了一些规避方法提示，并特别显示了关于安全、性能和存储的额外建议。请注意，表 8.2 中 Azure 上最旧的可目标化 SQL Server 版本是 SQL Server 2012；因此，如果你运行的版本早于 2012，使用 DMA 进行迁移前迁移是高度推荐的。如果你想从 SQL Server 以外的 DBMS 迁移到其中一个目标 SQL Server 版本，那么你需要使用名为 SQL Server 迁移助手（SSMA）的工具。

## SQL Server 迁移助手

如果你决定从非 Microsoft SQL Server 产品迁移到 Azure，同时将 DBMS 迁移到 SQL Server，那么 SSMA 就是帮助你实现这一目标的工具。截至本文撰写时，受支持的源数据存储如下所示：

- Microsoft Access
- SAP ASE
- Oracle
- MySQL
- DB2

SSMA 工具可以自动化迁移中许多步骤的比较，包括对现有实现和目标位置的发现和评估。由于源 DBMS 的种类繁多且定制化程度高，可能存在一些问题无法通过简单的补丁或代码修改解决。可能会发现一些问题，需要从根本上重新思考应用程序实现的架构和目的。重点是，不要低估这种迁移。确保你有合适的专业人员从事此类迁移。现在你已涵盖从 SQL Server 到 SQL Server 以及从非 SQL Server 到 SQL Server 的情况，请查看 DMS 也称为 ADMS。

## Azure 数据迁移服务

DMS 工具已在第 1 章中介绍，并在第 5 章中有一些额外的讨论。 第 5 章是你学习数据和存储的地方，而关于数据和存储的章节不可能没有一点关于 DMS 的内容。有关此服务的更多详细信息可在 datamigration.microsoft.com 获取。DMS 对于需要迁移大量数据库的场景是最佳选择。DMA、SSMA 和 DMS 之间的区别如下：

- DMA 用于小型数据集、兼容性报告和从 SQL Server 到 Azure SQL Server 的迁移。
- SSMA 适用于小型到中型数据集、兼容性报告和从非 SQL Server 到 Azure SQL 的迁移。

