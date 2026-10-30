

# 成本

使用 Google Cloud 的运维套件，您只需为所使用的资源付费。费用可通过 Cloud Billing 报表和警报进行控制。

以下 Google Cloud 运维套件服务可免费使用：

- Cloud Debugger
- Error Reporting
- Cloud Profiler

以下服务在超出月度限制后可能会产生费用：

- Cloud Logging
- Cloud Monitoring
- Cloud Trace

我们故意不列出实际价格，因为价格可能会发生变化。

> 重要提示
> 请查看以下链接以了解最新、最详细的价目表：https://cloud.google.com/monitoring#pricing。

# 配置 Google Cloud 的运维套件

> 考试提示
> 对于 Stackdriver，过去需要手动创建监控工作区。在引入 Google Cloud 的运维套件后，就不再需要这样做了。当您从 Google 控制台导航到 Cloud Monitoring 时，您会看到在项目创建后，该项目已被自动启用监控，并成为所谓的范围项目。

Google Cloud 的运维套件按项目启用。最佳实践是，如果您只使用一个项目，则应为该项目启用它。如果您有多个项目，则应专门为监控创建一个单独的项目。这称为范围项目。从该项目中，您可以指定要监控的其他项目。它们将成为被监控项目。请记住，您不会为额外项目付费，因此这不会产生任何额外成本。

现在，让我们看看如何开始使用 Google Cloud 的运维套件：

1. 从汉堡菜单进入 **Monitoring**。然后选择 **Overview**：

![](img/de2c97ab017f36b3750a5bade111edd7_553_0.png)

图 17.2 – 访问 Cloud Monitoring

2. 这会带您进入 Cloud Monitoring 主屏幕。正如您所见，您有一个格式良好的面板，汇总了与您的 GCP 资源监控相关的最重要的信息，例如资源 **Dashboards**、事件 **Uptime checks** 等：

![](img/de2c97ab017f36b3750a5bade111edd7_553_1.png)

图 17.3 - Cloud Monitoring Overview 屏幕

3. 在多项目场景中，通过在左侧窗格中点击 **Metrics Scope**，然后点击 **Add Cloud projects to metrics scope** 链接 (https://console.cloud.google.com/monitoring/settings/add-projects?project=pca-exam-314409) 为每个项目启用监控：

![](img/de2c97ab017f36b3750a5bade111edd7_554_0.png)

图 17.4 – 添加 Google Cloud 项目

现在我们可以看到带有所选项目的指标范围设置。您可以将项目添加到当前范围项目进行监控，也可以创建一个新的范围项目。选择好项目后，点击 ADD PROJECTS 按钮：

![](img/de2c97ab017f36b3750a5bade111edd7_555_0.png)

# 图 17.5 – Metrics Scope – Settings

4. 您可以选择是否要监控 AWS 资源。这需要您在 AWS 中创建一个 IAM 角色，并列出权限。如果您只使用 GCP，则可以点击 Skip AWS Setup 启动监控。

> 考试提示
> Google Cloud 运维套件也能够监控您 AWS 账户的日志数据。您必须将 AWS 资源与一个 Google Cloud 项目关联。该项目充当与 AWS 的连接器。

现在我们已经配置了 Google Cloud 的运维套件，让我们来看一下监控服务。

# Cloud Monitoring

使用 Monitoring 服务，您可以发现并监控所有 GCP 资源和服务。Monitoring 控制台允许您查看所有资源、创建警报策略、查看正常运行时间检查、组和自定义仪表板。它还允许您导航到调试、跟踪、日志记录和错误报告控制台。

让我们看看这里可以配置什么。我们将研究以下主题：

- Groups
- Dashboards
- Alerting policies
- Change screen
- Uptime checks
- Monitoring agents

# Groups

诸如 VM 实例、应用程序和数据库等资源可以被分组到逻辑组中。这使我们能够一起管理它们并在仪表板中显示它们。约束用于定义筛选资源的标准。它们可以基于名称、区域、应用程序等。组可以相互嵌套，嵌套深度可达六层：

![](img/de2c97ab017f36b3750a5bade111edd7_556_0.png)

图 17.6 – 创建组

上图显示了创建新组的屏幕。我们可以使用多个标准来筛选要添加到组的资源。

# Dashboards

仪表板允许我们在一个视图中对不同指标进行可视化。我们可以创建多个仪表板，其中包含基于预定义或用户定义指标的图表。这使我们能够创建包含最重要的指标的自定义面板。图表可视化指标，有助于很好地了解您的环境性能。

请注意，对于 VM 实例，我们除了标准指标外还可以看到基于代理的指标。基于代理的指标会在您进入 Dashboards 并点击 +CREATE DASHBOARD 按钮时列出。接下来，从库中选择一个图表并点击 Metric 下拉菜单：

![](img/de2c97ab017f36b3750a5bade111edd7_557_0.png)

图 17.7 – Dashboards

# Alerting policies

可以配置警报策略，以便在事件和指标阈值达到时创建通知。策略可以有一个或多个条件来触发警报，并会创建在 Cloud Monitoring 控制台中可见的事件。

可以为处理打开的事件选择以下操作：

- **Acknowledge**
- **Silence Associated condition**
- **View Policy**
- **Edit Alerting Policy**

这些操作的菜单在您点击右上角的三点图标时打开：

![](img/de2c97ab017f36b3750a5bade111edd7_558_0.png)

图 17.8 – Alerting policies

事件可以被确认为已知问题或已解决。关于通知，可以配置电子邮件、短信等多种形式。可使用的渠道可从下拉菜单中选择，如下图所示：

![](img/de2c97ab017f36b3750a5bade111edd7_559_0.png)

图 17.9 – Notifications

为了进一步指导操作找到可能的解决方案，还可以将文档链接附加到警报。

# Uptime checks

正常运行时间检查用于从全球不同位置检查服务的可用性。它们可以与警报策略结合使用，并在仪表板中显示。检查可以使用 HTTP、HTTPS 或 TCP 进行，可用于 URL、App Engine、Elastic Load Balancing (ELB)、Kubernetes 的负载均衡器服务以及 Amazon EC2 和 GCE 实例。探测间隔可设置为 1、5、10 或 15 分钟：

![](img/de2c97ab017f36b3750a5bade111edd7_560_0.png)

图 17.10 – Uptime check

您还需要选择从哪些区域执行检查。您至少需要检查三个区域：

![](img/de2c97ab017f36b3750a5bade111edd7_561_0.png)

图 17.11 – Regions

在后续部分中，您可以添加主机标头、更改端口、添加自定义标头并对其进行加密。您也可以在适用时添加身份验证：

![](img/de2c97ab017f36b3750a5bade111edd7_561_1.png)

图 17.12 – Advanced settings

在 **Response Validation** 部分，您可以设置 **Response Timeout** 并为响应设置内容匹配：

# Response Validation

指定数据以及该数据如何与实际响应数据进行比较。

图 17.13 – Response Validation

最后，您可以设置警报和通知。您可以选择应使用哪个通知渠道发送通知。如果尚未存在渠道，您可以从此处创建一个新的：

# Alert & Notification

定义 Uptime Check Alert Condition。

图 17.14 – Alert & Notification

