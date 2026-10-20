

Power BI desktop，做出更改，然后重新发布它。在 Dataflow Gen2 中，Write 权限允许用户编辑数据转换逻辑。

## Reshare

此强大的权限赋予用户将项目与他人共享的能力。例如，如果报表使用者对特定报表拥有 Reshare 权限，他们可以将该报表的链接与其同事共享。然而，这可能是一把双刃剑，你应谨慎授予 Reshare 权限的对象，以维持对数据传播的控制。

## Configuring Item-Level Access for Users or Groups

如果你点击工作区中大多数项目上找到的三点菜单（也称为 *context menu*），你会在下拉列表中看到“Manage permissions”选项（见图 7-5）。

Name
Git status
Type
SemanticModel
• Uncommitted
Semantic mc
Explore this data (preview)
Analyze in Excel
Create report
Auto-create report
Create paginated report (preview)
Delete
Security
Rename
Open data model
Settings
Refresh history
Manage permissions
View workspace lineage
View item lineage
Move to
Write DAX queries
Version history

Figure 7-5. Context menu of a semantic model

### TIP

请注意，工作区中不同项目的上下文菜单各不相同，因此“Manage permissions”可能出现在下拉列表的不同位置。

还请注意，某些项目（例如 Dataflow Gen1、Dataflow Gen2、eventstreams）没有此选项，因此你无法单独将它们共享给用户。用户必须位于“Implementing Workspace-Level Access Controls”部分中列出的角色之一中。

当你选择“Manage permissions”选项时，它会弹出如图 7-6 所示的窗口。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_367_0.png)

Figure 7-6. “Manage permissions” window and options

窗口可能因项目类型而异，但它将包含我们在图中编号为 1–3 的三个区域。你将看到当前直接访问此项目的用户列表（区域 1）。你还会看到此项目的权限列表（区域 2）。在此情况下，区域 1 中列出的两个用户都可以访问该项目，因为他们是工作区的管理员角色。

如果你想将此项目与其它用户或组共享，你必须点击“+ Add user”按钮（区域 3），这将弹出图 7-7 中的窗口。

## Grant people access

Warehouse

People you share this warehouse with can connect to it. To give additional permissions, select them from the list.

Enter a name or email address

### Additional permissions

- Read all data using SQL (ReadData) ⓘ
- Read all OneLake data (ReadAll) and subscribe to events (SubscribeOneLakeEvents) ⓘ
- Build reports on the default semantic model (Build) ⓘ
- Monitor queries (Monitor) ⓘ
- Audit queries (Audit) - PREVIEW ⓘ
- Share granted permissions (Reshare) ⓘ

### Notification Options

Notify recipients by email

Add a message (optional)

> 要为仓库中特定对象的细粒度对象级安全性（OLS）进行定义，请在 T-SQL 中使用 GRANT 和 DENY 语句。

Figure 7-7. Settings for sharing an item

此弹出窗口来自仓库项目，它具有项目的所有可能设置。（请注意，工作区中的其它项目可能具有不同的设置窗口）。我们在弹出窗口中突出了三个主要区域（编号为 1-3）：

### Input field (area 1)

你使用此字段添加应有权访问此单个项目的用户或组。

### List of options for sharing this item (area 2)

请注意这里的 Reshare 选项，我们之前说过你应谨慎授予。用户不受限制地重新共享可能会导致你在组织中失去对重新共享的控制以及对访问策略的治理。

### Notification options (area 3)

在这里，你可以确定添加的用户或组是否会通过电子邮件收到共享通知以及自定义文本。

## Permissions for an Item

如本章前面所述，项目级访问控制在工作区中的项目之间有所不同。表 7-2 提供了项目级访问控制的不同权限的汇总列表。

Table 7-2. Permissions for item-level access control

| Permission granted while sharing | Effects |
|---|---|
| Read | 接收者可以在数据中心发现该项目，打开它，并连接到湖屋或仓库的仓库或 SQL 分析端点。 |
| Edit | 接收者可以编辑项目或其内容。 |
| Share | 原始接收者可以共享项目并向新接收者授予其（原始接收者）拥有的全部或部分相同权限。例如，如果原始接收者拥有 Share、Edit 和 Read 权限，则原始接收者最多可以向新接收者授予 Share、Edit 和 Read 权限。 |
| Read All with SQL analytics endpoint | 接收者可以通过 TDS 端点从湖屋或仓库数据的 SQL 分析端点读取数据。 |
| Read all with Apache Spark | 接收者可以通过 OneLake API 和 Spark 读取湖屋或数据仓库数据，并且他们还可以通过湖屋资源管理器读取湖屋数据。 |
| Subscribe to OneLake events | 接收者可以订阅湖屋、数据仓库、镜像数据库、SQL 数据库和 KQL 数据库的 OneLake 事件。 |
| Build | 接收者可以在语义模型上构建新内容。 |
| Execute | 接收者可以执行或取消项目的执行。 |

总之，项目级访问控制是实现可扩展、安全且可管理的 data 环境的强大工具。通过将其与更广泛的工作区角色结合使用，你可以有效地管理谁可以在你的 Microsoft Fabric 租户中看到什么。

然而，它也增加了整个设置的复杂性。你可以在工作区中为项目配置显式访问，而被授予权限的用户可能无权访问工作区。想象一下企业设置中配置了这些访问控制，你可以迅速想象出其对治理和维护设置的影响。

## Controlling Access to Compute

访问工作区和特定项目是一回事，但你也可以允许特定用户访问底层计算，即 Fabric 的容量单元。

你有两种访问 Fabric 计算引擎的选项：

### *Through the SQL Analytics endpoint*

SQL 分析端点提供对 OneLake 中表的直接 SQL 访问，你可以通过 SQL 命令在其中原生配置安全性。这些权限仅适用于通过 SQL 发出的查询。

### *Through the Spark engine for the lakehouse*

访问 Spark 引擎可通过 Fabric API 或你在处理笔记本时使用的 Spark 引擎访问底层数据。

> **TIP**
> 
> 目前，OneLake 数据访问控制级别处于预览中，它提供了一个集中位置来控制对数据的访问。由于它目前处于预览阶段，我们不会在本书中介绍它。相反，我们将把你引向 Microsoft Learn，其中包含此信息：
> 
> *OneLake security uses role assignments to apply permissions to its members. You can either assign roles to individuals or to security groups, Microsoft 365 groups, and distribution lists. Every member in the user group gets the assigned role. If someone is in two or more security groups or Microsoft 365 groups, they get the highest level of permission that is provided by the roles. If you nest user groups and assign a role to a group, all of the contained users have permissions.*

## Implementing Row-Level, Column-Level, Object-Level, and File-Level Access Control

当向整个数据环境中的最终用户授予数据访问权限时，你可以根据特定需求和要求从四种特定的访问方法中进行选择：

### *Row-level access control*

最终用户受制于对数据行强制且不可更改的筛选。此类用户可能是销售代表，每个人只能查看自己的个人销售和客户。

### *Column-level access control*

最终用户仅允许查询或获取对数据库中特定列的洞察，这些列包含诸如社会安全号码、信用卡号码等信息。

### *Object-level access control*

最终用户可能完全被阻止访问数据库中的特定对象。这可能适用于一组不允许查看员工表上个人身份信息的最终用户，该表配置为仅供人力资源部门查看和使用。

### *File-level access control*

这特定于可以访问 OneLake 中底层文件的目标用户：delta parquet 文件。在这里，你可以选择授予或移除对特定文件、文件集或 OneLake 结构中文件夹的访问权限。

> **NOTE**
> 在撰写本文时，你无法通过应用程序本身在 Power BI 中创建列级和对象级安全性。你可以用第三方工具实现，但这些工具不属于考试范围，因此也不属于本书。

