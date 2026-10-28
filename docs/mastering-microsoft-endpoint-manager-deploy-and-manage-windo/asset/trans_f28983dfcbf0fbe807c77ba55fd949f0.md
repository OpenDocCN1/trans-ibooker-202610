

每个 App 性能 选项卡上的应用程序都会根据以下内容分配一个应用可靠性评分：

- **崩溃频率**：对于每个应用，有两个指标：
  - 应用崩溃的总数。
  - 在 14 天滚动窗口内的总使用时长用于计算 **App performance** 选项卡中的平均故障时间值。
- **总使用时长**：是 Endpoint analytics 中所有已注册设备使用时长的因素。这确保你获得最具破坏性的应用程序问题的[content]

每个 App 性能 选项卡上的应用程序都会根据以下内容分配一个应用可靠性评分：

- **崩溃频率**：对于每个应用，有两个指标：
  - 应用崩溃的总数。
  - 在 14 天滚动窗口内的总使用时长用于计算 **App performance** 选项卡中的平均故障时间值。
- **总使用时长**：是 Endpoint analytics 中所有已注册设备使用时长的因素。这确保你获得最具破坏性的应用程序问题的[content]

每个 App 性能 选项卡上的应用程序都会根据以下内容分配一个应用可靠性评分：

- **崩溃频率**：对于每个应用，有两个指标：
  - 应用崩溃的总数。
  - 在 14 天滚动窗口内的总使用时长用于计算 **App performance** 选项卡中的平均故障时间值。
- **总使用时长**：是 Endpoint analytics 中所有已注册设备使用时长的因素。这确保你获得最具破坏性的应用程序问题的优先数据，这些问题在应用可靠性评分中得到优先考虑。

## Windows 365 专属指标

另一个巨大的好处是，Endpoint analytics 也是你用来检查物理端点状态的工具。这意味着你可以创建一个单一的玻璃窗，一次性检查你的物理端点和云 PC 端点的状态。

有六个新的 Windows 365 Endpoint analytics 类别来衡量你的环境性能：

- **资源性能：**
  - CPU 峰值时间百分比
  - RAM 峰值时间百分比
- **远程连接：**
  - RD 客户端登录时间
  - RD 客户端登录失败
- **往返时间 Round Trip Time (RTT)**

以下屏幕截图是测量连接到你的 Windows 365 云 PC 环境的延迟的一个示例：

## 远程连接指标

| Metric | Current Value |
|---|---|
| Avg cloud PC round trip time ⓘ | 120ms |
| Avg cloud PC sign-in time ⓘ | 328.73s |

## 每日指标趋势（30 天）

![](img/a3e9f26159d3739e434fe676b2987c1d_537_0.png)

图 14.16 – 远程连接指标

以下是可用于衡量启动性能和登录时长的指标类型：

- **启动性能：**
  - Windows 365 云 PC 端点的启动时间
  - Windows 365 云 PC 端点的登录时间

以下是关于你的物理和云 PC 环境的启动时间、登录时长以及往返时间 RTT 的见解。

![](img/a3e9f26159d3739e434fe676b2987c1d_538_0.png)

图 14.17 – 启动性能

- **主动修复：**
  - 自动操作以修复 Windows 365 云 PC 的常见问题
- **推荐软件：**
  - Windows 10 版本
  - **Azure Active Directory (AAD)** 设备
  - Intune 设备
- **应用程序健康：**
  - 云 PC 应用使用情况和崩溃

## 见解和建议

Endpoint analytics 还会在检测到环境中存在性能问题时为你提供建议。例如，如果在资源性能仪表板上检测到超过 90% 的 CPU 使用率，它会建议升级你的云 PC。这也适用于物理 PC。

![](img/a3e9f26159d3739e434fe676b2987c1d_539_0.png)

图 14.18 – 建议

## Configuration Manager 数据收集

你也可以从由 Microsoft Endpoint Configuration Manager 管理的设备收集用户体验数据以计算分数和见解。我们在 *第 6 章，Windows 部署和管理* 中解释了启用租户附加和共管理的步骤。

在 **Endpoint analytics | Settings** 菜单中，你可以了解连接是否正常工作。如果不正常，如以下示例所示，来自 Configuration Manager 的数据将不会显示在 Endpoint analytics 中。

![](img/a3e9f26159d3739e434fe676b2987c1d_539_1.png)

图 14.19 – 设置菜单

## 自定义你的基线

通过设置菜单将你的基线更改为自己的值，因为你可能有高于或低于默认设置的标准。这将允许组织根据其环境和应用程序的期望调整评分。

基线定义了分数以及指标在 Endpoint analytics 中显示为绿色或红色。谨慎定义自己的基线，以确保你的 Windows 10 物理和云 PC 的质量和性能水平。

你可以在以下屏幕截图中看到如何更改基线回归阈值。你可以在 Endpoint analytics | Settings 下找到此设置。

![](img/a3e9f26159d3739e434fe676b2987c1d_540_0.png)

图 14.20 – 更改基线

## 主动修复

使用 Endpoint analytics 的另一个好处是你可以在设备上创建和运行脚本包，以主动查找并修复组织中排名靠前的支持问题。

例如，你可以创建检测脚本来搜索 Windows 10 端点上的设置——如果设置或注册表项不再存在，它会触发修复脚本以恢复设置。这是一种非常独特和主动的方式来确保你的环境保持一致。

以下屏幕截图中显示的部分允许你查看已部署脚本包的状态并监控检测和修复结果。结果显示为受影响的设备数量。

![](img/a3e9f26159d3739e434fe676b2987c1d_541_0.png)

图 14.21 – 主动修复

如果你想自己创建一个主动修复脚本包操作，只需点击 + Create script package。

为脚本输入名称并使用正确的设置配置检测和修复脚本，如以下示例所示。

## 重要说明

如果你向 64 位 Windows 端点添加注册表项，请确保启用 Run script in 64-bit PowerShell 以确保它位于正确的位置！

## 创建自定义脚本

从你编写的脚本创建自定义脚本包。默认情况下，脚本每天在分配的设备上运行。

Detection script file *

Select a file

Detection script

Input script text

Remediation script file

Select a file

Remediation script

Input script text

Run this script using the logged-on credentials

Yes No

Enforce script signature check

Yes No

Run script in 64-bit PowerShell

Yes No

Previous

Next

图 14.22 – 创建自定义脚本

将自定义脚本操作分配给正确的组/用户，无论是通过 AAD 还是按设备进行，检测将立即开始：

![](img/a3e9f26159d3739e434fe676b2987c1d_543_0.png)

图 14.23 – 检测状态

## Azure Monitor 集成

也可以将诊断信息导出到 Azure Monitor，以便你可以查询数据并随心所欲地使用它。例如，你可以创建工作簿来映射你的环境状态。

要启用此功能，请激活其中一个诊断设置。还涉及额外的成本，因为它需要存储帐户和 Log Analytics 工作区。

以下日志列表可以自动导出到 Log Analytics 工作区：

- AuditLogs
- OperationalLogs
- DeviceComplianceOrg
- Devices

通过在诊断设置中为 Microsoft Endpoint Manager 启用日志分析集成，如以下屏幕截图所示，你将能够基于收集在 Azure Monitor 中的遥测数据创建查询和自定义仪表板：

![](img/a3e9f26159d3739e434fe676b2987c1d_544_0.png)

图 14.24 – 诊断设置

## 生产力评分

Endpoint analytics 包含很棒的见解，但大多数人无法看到它们，因为它需要访问 Microsoft Endpoint Manager 管理中心。如果你是 Intune 管理员或具有全局管理员角色，你可以显示分析。

通过生产力评分，你可以量化 Microsoft Endpoint Manager 如何在你的环境中实现生产力。

生产力评分中的一些重要指标如下：

- 了解用户生产力如何受到底层技术性能因素的影响。
- 专注于需要改进的领域。
- 根据整体体验影响，在技术领域分配分数。

以下是作为你的 Windows 10 端点一部分的 Microsoft 365 活动健康的示例。

![](img/a3e9f26159d3739e434fe676b2987c1d_545_0.png)

图 14.25 – 生产力评分

## 服务运行状况

除了 Microsoft Endpoint Manager 中前面章节中解释的 Endpoint analytics 和其他监控功能外，你还可以从高层次监控服务可用性。

