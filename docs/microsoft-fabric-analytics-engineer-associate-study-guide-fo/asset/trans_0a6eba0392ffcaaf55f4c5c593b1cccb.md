

要创建工作区，请在 Fabric 门户左侧的主菜单中找到 Workspaces 按钮，输入工作区的名称，展开 Advanced 部分，然后选择 Fabric 容量或 Trial 容量（见图 1-7）。

## Create a workspace

Name *

为此工作区命名

Description

描述此工作区

Domain ⓘ

分配到域（可选）

Learn more about workspace settings

Workspace image

Upload

Reset

## Advanced ^

Contact list * ⓘ

输入用户和组

License mode ⓘ

Pro
选择 Pro 以使用基础 Power BI 功能并协作处理报表、仪表板和记分卡。要访问 Pro 工作区，用户需要 Pro 按用户许可。Learn more

Trial
选择 Trial 将此工作区分配给 Fabric 试用容量。Microsoft Fabric 试用容量允许用户探索 Microsoft Fabric 的功能，如 Data Factory、Data Engineering 和 Real-Time Intelligence 等。Learn more

Premium per-user
选择 Premium per-user 以使用 Power BI Premium 功能进行协作，包括高级数据流和数据马特。要在 Premium per-user 工作区中协作和共享内容，用户需要 Premium per-user 许可。Learn more

Premium capacity
如果工作区将托管在 Premium 容量中，请选择 Premium 容量。当您共享、协作和分发 Power BI 和 Microsoft Fabric 内容时，查看者角色中的用户无需 Pro 或 Premium per-user 许可即可访问此内容。Learn more

Embedded
如果工作区将托管在 Azure 嵌入式容量中，请选择 Embedded。ISV 和开发人员使用 Power BI Embedded 将可视化和分析嵌入到其应用程序中。Learn more

Fabric capacity
如果工作区将托管在 Microsoft Fabric 容量中，请选择 Fabric capacity。使用 Fabric 容量，用户可以创建 Microsoft Fabric 项目并使用 Fabric 功能和体验与其他人协作。探索 Power BI、Data Factory、Data Engineering 和 Real-Time Intelligence 等中的新功能。Learn more

Figure 1-7. Selecting a Fabric capacity when creating a new workspace

暂时保持其他设置不变，然后单击 Apply。

## Configuring a Workspace

在 Microsoft Fabric 中创建给定工作区后，您将有一系列用于配置它的选项。您可以在工作区顶部的右侧找到 Workspace settings。图 1-8 显示了工作区设置中的主菜单项。

## Workspace settings

- General
- License info
- Azure connections
- System storage
- Git integration
- OneLake
- Workspace identity
- Network security
- Monitoring
- Power BI
- Delegated Settings
- Data Engineering/Science
- Data Factory

Figure 1-8. Main menu items in the workspace settings menu

TIP
图 1-8 中的大多数设置超出了本书内容的范围。我们将在第 8 章中详细介绍相关设置，但对于其他设置，我们仅提供高层次描述，如本节所述。

图 1-8 中的关键设置如下：

## General

在此部分，您可以配置工作区的主要部分，包括名称和工作区图像。您可以添加描述，将其加入域，编辑工作区联系人，添加 OneDrive 安全组，并删除工作区。请注意，您很少需要更改 General 设置，因为它们是高层次的。

## License info

在此处，您可以查看和更改工作区的许可信息。许可信息是容量使用的配置，您可以更改工作区对 Fabric 容量的使用。

您还可以获取工作区的连接链接，您将使用它将第三方应用程序连接到工作区并对其进行编程更改（例如 API 和 Graph 连接）。

## Azure connections

在此处，您可以配置与 Azure Data Lake Storage Gen 2 帐户的链接，以存储您的数据流的数据（见第 4 章），您还可以将工作区连接到 Log Analytics 工作区，以便将日志发送到那里。

## System storage

在此处，您可以快速查看系统存储使用情况，按总计以及按项目名称和大小分组。

## Git integration

在此处，为了轻松配置工作区的 Git 集成，您可以设置与 GitHub 或 Azure DevOps 的连接。（请注意，Git 集成不是本书的一部分，因为它超出了考试范围。）

如果您使用 GitHub，您必须手动维护个人访问令牌（PAT）。这与使用 Azure DevOps 不同，后者通过 Azure Entra ID 的单点登录（SSO）功能维护安全连接。

## OneLake

在此处，您可以下载 OneLake 文件资源管理器应用，它为您提供对 OneLake 中原始文件的完全集成视图，该视图直接嵌入到您本地计算机上的正常本地文件资源管理器中。

您还可以为快捷方式启用缓存（见第 6 章）。此缓存将存储快捷方式的一些数据，直接存储在工作区的 OneLake 中，以加快数据的读取速度。

## Workspace identity

在此处，您可以配置工作区身份，以便 Microsoft Fabric 之外的其他应用程序可以与工作区中的项目和流程协同工作。

当您单击绿色的 + Workspace identity 按钮时，Fabric 将创建一个托管服务主体，用于在 Fabric 外部进行应用程序访问。

## Network security

在此处，您可以创建到 Azure 中私有端点的连接。您将使用私有端点连接到虚拟专用网络中的服务，它们使 Microsoft Fabric 能够从这些特定服务读取和写入数据和其他信息。

## Monitoring

在此处，您可以设置工作区监控。您可以使用标准的工作区项目集来启用对工作区中发生情况的深度监控，这将创建一个 eventhouse 和一个 eventstream，用于监控工作区中的事件以供进一步分析。

## Power BI

在此菜单部分，您可以访问三个主要区域：

### General

在此处，您可以配置应用程序和模板的访问控制（如果在工作区中启用），并且您可以启用或禁用在 Web 用户界面中直接使用语义模型的工作选项（见第 3 章）。

### Data connections

在此处，您可以启用一项额外的安全功能，如果最终用户编辑报表，该功能将断开报表与底层语义模型的连接。在某些情况下，这可以帮助整个工作区设置的<|message|> 安全和治理。

### Embed codes

使用 Microsoft Fabric，您可以选择将 Power BI 报表嵌入到自己的网站中，而最终用户看不到 Fabric 门户。如果您创建了嵌入式 Power BI 报表，您将在此处找到维护该设置的代码。

## Delegated Settings

在此部分，您可以启用或禁用用户和应用程序使用共享访问签名（SAS）令牌访问工作区的 OneLake 的选项。

## Data Engineering/Science

> **TIP**
> 尽管菜单的这一部分不是 DP600 考试的一部分，但我们在此添加了详细信息，以帮助您全面了解设置。

在此处，您可以指定 Spark 设置中 Spark 的使用选项。您有五个选项：

### Pool

在此处，您可以指定 Fabric 环境的不同 Spark 池大小。默认情况下，将有一个启动池，其启动速度比手动配置的 Spark 池更快（您也可以在此处配置）。

### Environment

在此处，您将有配置工作区中环境设置的选项。默认环境带有配置 Spark 属性、使用库、笔记本如何使用 Spark 以及 Spark 作业定义如何处理数据处理的选项。这里最好的功能之一是启用 Native Execution Engine（NEE）的选项，这将加速笔记本执行的速度。您还可以配置标准 Spark 池的使用和设置，上传自己的自定义 Spark 库，并配置在此特定工作区中可以使用哪些标准库。

### Jobs

在此处，您可以配置 Spark 作业中执行的设置。此设置允许您为 Spark 作业运行保留最大核心数，这将降低可同时运行的并行作业数量。您还可以配置 Spark 池的超时时间，这将帮助您在未使用指定时间后停止 Spark 池。

### High concurrency

在此处，您可以配置同时在同一 Spark 会话上运行多个笔记本的选项，这将帮助您优化并行进程的利用。您也可以对在管道内运行的笔记本执行相同操作（有关笔记本和管道的更多信息，请参阅本书后面的内容）。

### Automatic log

在此处，您可以启用跟踪来自笔记本的机器学习模型和实验的选项，以记录其指标、参数和模型。

## Data Factory

在此处，您可以配置 Apache Airflow 的运行时设置，Apache Airflow 是使用代码构建的流程执行的编排器。（请注意，这超出了本书的范围。）

