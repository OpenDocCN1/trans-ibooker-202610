

![](img/95043b3df926bcdadadb082b49262037_694_0.png)

## Azure Backup

在你能够从数据丢失、服务器崩溃或全面灾难中恢复之前，你必须有东西可以恢复。通常你会使用备份来从丢失数据或服务器的事件中恢复。Azure Backup 是你应该为 Azure VM、Azure Files 和 SQL Server 虚拟机捕获备份时选择的工具。关于创建备份的一些细节已在前几章中介绍过，例如第 4 章的 Azure VM 和第 5 章的数据存储。如果您直接跳到本节并希望了解更多信息，请查看这些章节以作复习。对于本节，将从 Azure 平台的角度讨论以下主题：

- Description of a backup
- How backups work on Azure
- Storing and managing backups
- Backing up other products
- Linux versus Windows backups

像运行一个成功的 IT 组织所需的许多其他活动一样，制作备份是一项复杂的工作。备份、管理和从备份恢复系统并没有什么简单之处。你首先需要确定需要备份哪些特定服务器，如前所述。之后，你需要确定该服务器上的所有内容是否都需要备份。

## Description of a Backup

请记住存储是有成本的，因此你不会想存储不必要的更多内容。存储并不是 Azure 上最昂贵的产品之一；然而，花费超过必须的费用永远不是一个好主意。当对 Azure VM 执行 Azure Backup 时，最常见的更新组件是文件、文件夹、系统状态和应用数据。前两个组件不言自明，即文件和文件夹，例如你在 Windows 资源管理器中看到的内容。系统状态将包含注册表，其中可能包含自定义设置、环境设置或进程执行路径。你会同意，许多应用程序依赖于位于特定位置的其他程序集。那些配置被视为系统状态，并在你配置 Azure Backup 时默认备份。应用数据通常用于本地临时存储和存储应用程序特定信息。例如，如果应用程序需要解析一个文件，该文件需要在处理过程中临时存储在某处。

从数据存储的角度来看，如 SQL Server，运行它的 Azure VM 需要备份，正如刚才讨论的那样，数据库也需要备份。正如你在第 5 章中学到的，数据库本身只是一个 DBMS 管理访问的大文件。因此，备份数据库就是备份文件和任何专门的数据库配置。它的工作原理将在下一节中介绍；现在让我们先看看不同类型的备份，这些备份在表 9.9 中提供。

### TABLE 9.9 Azure Backup Types

| Backup Type | Azure Product | Description |
|---|---|---|
| Full | VM/SQL | A complete backup of the entire source |
| Incremental | VM | A backup of changes since last backup |
| Differential | SQL | A backup of changes since last backup |
| Transactional | SQL | A log which enabled PITR restoration |

增量备份和差异备份之间的区别在于，差异备份需要一个完整备份才能执行恢复，因为未更改的数据不包含在内。当执行差异备份时，只备份已修改的数据。与其他 Azure Backup 类型相比，差异备份减少了存储需求、网络消耗以及执行备份所需的时间。支持每天执行一次完整数据备份；然而，你需要确定在你的场景中是否需要。再次记住，存储是有相关成本的，如果你每天备份一个 5GB 的数据库，成本，就像所需的存储空间一样，可能会迅速变得很大。注意这一点并确保优化备份的内容和频率。正如你可能还记得点时间恢复（PITR）来自第 5 章；参见图 5.48，在那里你接触到这个概念。PITR 允许你将数据库恢复到特定秒之前的状态。相比之下，增量或差异备份支持每日或每周计划，这意味着如果没有事务备份，你可能会丢失一天的数据。你能接受吗？值得这个成本吗？这些是你需要根据数据的关键性和其丢失对公司造成的影响来决定的问题。

## How Backups Work on Azure?

Azure Backup 依赖于许多不同的 Azure 产品和服务来执行备份。备份 Azure VM 或 SQL Server 虚拟机的最简单方法是利用 Azure Backup 扩展，该扩展安装到在 VM 上运行的 Azure VM 代理中。在为 VM 配置 Azure Backup 后，如你在练习 9.3 中将要做的那样，当你导航到 Azure 门户中 VM 的 Extensions 刀片时，你会看到已安装 Microsoft Monitoring Agent；这之前被称为代理。

在上一章中，你了解到 Microsoft Azure Recovery Services (MARS) 可用于将工作负载部署到 Azure 平台。对于执行备份也是如此；请回顾图 1.8，它说明了 MARS 用于备份本地和 Azure VM 并将其存储在 Azure 上的用途。Microsoft Azure Backup Server (MARS) 和 System Center Data Protection Manager (DPM) 是将本地 VM 备份到 Azure 的有用工具。MARS 和 DPM 都需要安装和配置服务器软件，以及在 Azure VM 上或本地机器上预配用于运行服务器软件的机器。如果你目前拥有 DPM 许可证，那么你或你的公司已经具备执行此类备份所需的技能。在你已经拥有 DPM 的情况下，你需要更改存储备份的位置，使其位于 Azure 平台上。如果你没有 DPM 许可证，那么你可以使用 MARS 来实现与 DPM 相同的备份功能。DPM 的好处是它是 System Center 工具套件的一部分。要了解有关 System Center 的更多信息，请阅读此页面：www.microsoft.com/en-us/system-center。

执行备份和恢复的另一个选项是使用 Azure Automation 脚本。Azure Automation 已在上一章中讨论过。通过脚本化备份活动，你可以一次性维护的 VM 范围会增加，并降低复杂性和犯错的可能性。在练习 9.3 中，你将为 Azure VM 配置备份；配置是在单个 VM 上执行的。如果你有许多 Azure VM 和/或需要备份的 SQL Server 虚拟机，一次一个地做是不可行的。相反，你可以使用 Azure Automation 执行的 PowerShell 脚本脚本化备份过程。完成练习 9.3 以学习如何为 Azure 虚拟机配置 Azure Backup。

## EXERCISE 9.3

### Configuring Azure Backup for an Azure VM

- 1. Log in to the Azure Portal at portal.azure.com.
- 2. Navigate to one of the Azure virtual machines that you have created in a previous exercise or create a new one. Make sure the VM is running, click the Backup link from the navigation menu, click the Create New radio button, enter a name, enter the resource group into which you want the Recovery Service vault to be placed into, and click the Create (or Edit) A New Policy link under the Choose Backup Policy drop-down list.
- 3. Enter a policy name; configure the Backup schedule by choosing a frequency, time, and time zone; click the OK button; and click the Enable Backup button.

在练习中，配置 Azure Backup 所需的步骤之一是创建备份策略。它包含三个组件：备份计划、快照恢复和保留范围。备份计划定义备份执行的频率（每日或每周）、时间以及时区。正如你可能还记得第 4 章中的内容，快照是绑定到 VM 的磁盘副本，可用于构建副本或，在本例中，从服务器崩溃中恢复。快照不携带配置和环境设置，就像使用 Azure Backup 执行备份时那样。备份必须保留的时间可能由合规或治理要求驱动。如果你的场景不依赖于两者中的任何一个，那么你需要确定恢复到多远之前是有意义的。时间范围将根据使用情况以及配置或数据在给定时间范围内的变化频率而有所不同。你备份的频率以及保留备份的时间是特定于你的应用程序场景的。然而，在我的经验中，并且不受任何合规、法律或治理约束，我不会考虑恢复到超过一到两周前的数据库状态。有许多约束和条件需要考虑；花时间弄清楚什么最适合你的场景。

在执行练习 9.3 中的 Azure Backup 配置后运行了几次备份后，点击导航菜单中的 Backup 链接将呈现类似于图 9.27 所示的内容。你将看到三种恢复点状态：Crash Consistent、Application Consistent 和 File-System Consistent。

在备份数据存储或 VM 时，它处于关闭状态是最理想的状态。从数据存储的角度来看，关闭意味着数据库不可访问，而从 VM 的角度来看，它意味着没有客户端连接，并且没有作业或服务在上面运行。如果你在备份运行时备份的服务正在使用中，那么尝试备份正在被访问的文件可能会挂起，这可能导致只备份了部分数据或只备份了预期内容/配置的一部分。所有这些场景如果尝试使用其中一个部分

