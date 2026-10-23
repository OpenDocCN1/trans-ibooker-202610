

# 表 9.1 Azure Monitor 限制

| 资源 | 默认限制 |
|---|---|
| 警报 | 每个订阅 2000 个 |
| 电子邮件警报通知 | 每小时 100 条 |
| 短信 | 每 5 分钟 1 条 |
| KUSTO 查询 | 每 30 秒 200 次 |
| 存储限制 | 500MB–无限 |
| 数据保留 | 7–730 天 |

如果你发现某些日志丢失了，这可能是由于其中一个默认限制造成的。这些限制是为了保护你而设，因为该资源的消耗是有成本的；它不是免费的。如果你需要超过默认限制，可以向 Microsoft 创建支持案例，限制可以提高，或提供一些关于如何优化日志策略的反馈。还要记住，一些限制基于你在创建 Log Analytics 工作区时选择的定价层。存储限制和数据保留基于定价层，因此在确定你的需求时请查看它们。定价层可以在创建后更改，因此也许可以从小开始，随着需求的增长而扩展。

## Log Analytics

与其他许多 Azure 产品一样，这不是你第一次接触 Log Analytics。随着本书接近尾声，绝大多数云概念、Azure 产品和功能都已经涵盖。在之前的练习中，特别是在步骤 5 中，你创建了一个 Log Analytics 工作区。它的创建很简单，只需要一个位置、一个名称和一个定价层即可。如图 9.1 所示，它是存储日志和指标以用于报告和警报功能的地方。Log Analytics 工作区具有许多功能，因此请在 Azure 门户中查看它们。导航到你之前创建的工作区。导航菜单中不应该有太多未知的选项。点击 Logs 导航菜单项。你将看到与图 9.10 所示类似的内容。

在之前的练习中，你为一个或多个 Azure 产品启用了诊断设置。数据开始流入工作区需要几分钟时间，但一旦开始流动，你就可以使用以下查询查询信息。查询和输出也在图 9.10 中可视化。点击 Run 按钮执行查询。

```
AzureDiagnostics
| project Category
| summarize Count = count() by Category
| order by Count desc nulls last
```

### 图 9.10 在 Log Analytics 工作区中查询诊断

![](img/95043b3df926bcdadadb082b49262037_669_0.png)

更新管理、变更跟踪、库存以及其他合规性和跟踪数据也记录在 Log Analytics 工作区中。所有数据以某种方式绑定在一起；该区域仍在不断发展，但对于了解你的应用程序和应用程序依赖项的行为很有用。这些类型的指标有助于采取主动措施来管理增长和消耗；它们也有助于排查当前问题或查找历史问题的根本原因。Log Analytics 工作区也存在限制，如表 9.2 所提供。

# 表 9.2 Log Analytics 限制

| 资源 | 默认限制 |
|---|---|
| 工作区限制 | 10–无限 |
| 门户中返回的行数 | 10000 |
| 表中最大列数 | 500 |
| 最大列名长度 | 500 |
| 数据导出 | 不支持 |
| 数据摄取 | 6GB/分钟 |

Log Analytics 工作区有一个免费层，这是每个订阅的工作区数量有限的那个层。另请注意，目前无法将存储在工作区中的数据导出。这就是当前的情况；未来可能会改变。最后，你配置的诊断日志越多，生成的数据就越多。请注意，每分钟可以放入工作区的数据量是有限制的。

## Application Insights

Application Insights 具有相当大的功能集。首先，你已经知道 Log Analytics 和 Application Insights 的区别，对吧？花点时间提醒自己，在什么场景下你会查看 Log Analytics 数据而不是 Application Insights。图 9.11 显示了一些额外的区分，具体是存储指标数据的表名。

### 图 9.11 Application Insights 与 Log Analytics 工作区日志

![](img/95043b3df926bcdadadb082b49262037_670_0.png)

之前曾尝试比较这两个已合并到 Azure Monitor 服务中的组件。我试图解释 Log Analytics 存储更多平台特定的信息，而 Application Insights 存储应用程序和代码数据。具体来说，Log Analytics 显示的 Event 表更与平台相关，你期望在其中找到你在 Windows 服务器上的事件查看器中看到的内容。通过 Application Insights 可访问的异常或请求表，仅仅从名称上看，就更与代码和应用程序相关，而非平台。现在希望两者之间的关系或缺乏关系更清晰了。此外，如图 9.11 所示，可以跨两个数据存储库执行查询，两者都是 ADX 集群。这将在后面变得更加明显。

回顾图 9.9，你会看到导航菜单中的 Applications 链接。导航回 Azure Monitor 刀片并点击该链接。在该链接上，你将获得添加新的 Application Insights 服务的能力。这是一个简单的过程，只需要一个订阅、一个资源组和一个名称。创建后，导航到它并点击 View Application Insights data 链接并注意两个具体项目。第一个在 Overview 刀片上；有一个名为 Instrumentation Key 的属性，看起来很像 GUID。这是唯一标识你的 AI 日志的信息。Instrumentation Key 是从应用程序代码中配置 AI 日志记录所必需的、必需的信息。在 ASP.NET Core 应用程序中，你在配置文件中将两者连接起来；它类似于以下内容：

```
{
  "ApplicationInsights": {
    "InstrumentationKey": "appInsightsInstrumentationKey"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
```

然后你安装并配置 Microsoft.ApplicationInsights.AspNetCore NuGet 包以按需向 AI 服务创建日志。这就是真正所说的“应用程序代码监控”的含义，因为日志记录功能确实在运行在 Azure 平台上的自定义代码内部。接下来，查看 AI 可用的导航菜单项，你会找到一个名为 Logs 的项。点击 Logs，你将看到与图 9.10 中相同的查询界面；但是，你会看到特定于 AI 的表，如图 9.11 所示。在 AI 的 Overview 刀片上，你会看到一个 Application Dashboard 链接。点击它，你会看到一些令人印象深刻的默认信息。见图 9.12。

### 图 9.12 Application Insights—应用程序仪表板

![](img/95043b3df926bcdadadb082b49262037_671_0.png)

在继续下一节之前，请注意，对于所有 Azure 产品，在 Azure 门户的导航菜单中都将存在一个 Monitoring 部分。该部分的内容是标准的，包含与图 9.13 类似的内容。请注意，在大多数情况下，它们都使用相同的后端技术来存储数据。这些工具允许你配置、查看，并在许多情况下操作数据。它还支持直接访问，以便你可以根据自己的确切要求创建自己的工具和报告。

![](img/95043b3df926bcdadadb082b49262037_672_0.png)

目前，只有 Azure App Services 和 Azure Functions 具有内置的、默认的 Application Insights 支持。在门户中创建其中任何一个时，你会注意到有一个步骤是启用该功能。可以在稍后为这些 Azure 产品启用 Application Insights，也可以为任何应用程序启用，只需创建 AI 服务并在代码中使用 Instrumentation Key 即可。

## Azure Data Explorer

如果你想构建自己的自定义监控和报告系统，这是开始旅程的好地方。你可能已经意识到 Log Analytics 和 Application Insights，这两个 Azure Monitor 的组件，将数据存储在同一类型的数据源上。此存储位置通常称为 *KUSTO 集群* 或 *ADX 集群*。请查看图 9.14。

你会看到 Azure Monitor 使用的是共享集群，这意味着你配置为使用 Azure Monitor 的所有 Azure 产品都将日志存储到该集群中。这是你在练习 9.1 中创建的日志存储位置，具体在步骤 5 中，你为 Azure VM 启用了监控并选择了 Log Analytics 工作区。在幕后，它配置并链接到一个 ADX 集群。Application Insights 在创建时也在幕后执行类似的工作。在 Application Insights 场景中，ADX 集群仅绑定到具有 Instrumentation Key 的客户端，因此被视为私有。最后，回忆练习 5.7 中你创建了一个独立的 ADX 集群和数据库；然后在练习 6.5 中你配置了 Event Hub 将信息发送到集群。IoT Hub 和 Azure Blob 容器除了 Event Hub 外，还内置了对 ADX 集成的支持。还存在一个 Azure Data Explorer SDK；其 NuGet 包名为 `Microsoft.Azure.Kusto.Ingest.NETStandard`，可从任何客户端用于向集群发送日志。你

