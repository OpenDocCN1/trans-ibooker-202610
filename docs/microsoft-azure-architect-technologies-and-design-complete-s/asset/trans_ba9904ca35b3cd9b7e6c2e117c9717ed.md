

- Azure Blueprints
- Azure Policy
- Tags

Azure 状态和 Azure Monitor 都在第 9 章中有详细介绍；然而，这里需要特别指出，因为它们在合规性和治理的上下文中很有帮助。Azure 状态为你提供了 Azure 在全球及按区域划分的整体状态的全局视图（status.azure.com/en-us/status）。这有助于检查你部署 Azure 资源的区域是否存在已知问题。也可能提供有关如何解决任何正在进行问题的信息。Azure 健康监视器和 Azure Monitor 都可以配置为通知你影响资源的 Azure 事件。这种通知功能高度可配置和可定制，以满足你的需求。Azure Monitor 专注于更深层次；它直接与你的应用程序协同工作。因此，如果你想了解 Azure 的情况，请查看 Azure 状态。如果检查无误而你仍遇到一些问题，下一步是使用 Azure Monitor，因为这可能是应用程序问题而不是 Azure 平台问题。你将在第 9 章中了解更多相关信息。

## Azure Blueprints

Azure Blueprints 是基于特定合规标准的指南，遵循这些指南可导致符合该标准的要求（例如 ISO 9001）。专门为行业、政府或区域标准认证创建的蓝图会指出不同的控制映射，识别实现该标准所需的特定组件。以下是不同控制映射主题的示例：

- 职责分离
- 远程访问
- 最小功能
- 边界保护
- 最小权限
- 恶意代码保护
- 漏洞扫描
- 账户管理
- 拒绝服务保护

每个控制映射都会引发一些关于如何在 Azure 产品、功能和资源周围实施此类控制以验证合规性并实现认证的问题。这正是 MTC、Service Trust Center 和 Azure Blueprint 服务所提供的。你将获得实现所选合规标准的指南。阅读前面的列表时，你可能会回忆起本书中迄今为止学到的一些 Azure 产品，作为实现其中一些控制映射的手段。花点时间反思一下，并思考你将实施哪些产品来实现合规。

## Azure Policy

在你开始学习 Azure Policy 的文本部分之前，完成练习 6.1 以查看 Azure 策略列表。这将帮助你了解有哪些可用策略，以及它可能如何使用。

### EXERCISE 6.1

### Viewing Azure Policies Using Azure PowerShell

- 1. 打开 Azure PowerShell IDE 并使用以下 cmdlet 登录：

```
Connect-AzAccount
$subscription = Get-AzSubscription -SubscriptionId "<SubscriptionId>"
Set-AzContext $subscription
```

- 2. 输入以下 Azure PowerShell cmdlet，其结果输出如 Figure 6.1 所示：

```
$azurePolicy = (Get-AzPolicyDefinition).Properties.metadata
$azurePolicy | Group-Object category | Select-Object Name, Count `
| Sort-Object Count -Descending
```

### FIGURE 6.1 Azure PowerShell output that renders a list of Azure policies

![](img/95043b3df926bcdadadb082b49262037_507_0.png)

- 3. 执行以下 Azure PowerShell cmdlet，它将列出所有针对监管合规的 Azure 策略。输出如 Figure 6.2 所示。

```
(Get-AzPolicyDefinition).Properties `
| where-object {$_ .metadata.category -eq "Regulatory Compliance"} `
| Format-Table displayName
```

### FIGURE 6.2 Azure PowerShell output that renders a targeted list of Azure policies

```
PS C:\> (Get-AzPolicyDefinition).Properties | Where-Object {$_.metadata.category -eq "Regulatory Compliance"} | Format-Table displayName
displayName
Microsoft Managed Control 1599 - Developer Configuration Management | Software / Firmware Integrity verification
Microsoft Managed Control 1375 - Incident Response Assistance | Automation Support For Availability Of Information / Support
Microsoft Managed Control 1605 - Developer Security Testing And Evaluation | Static Code Analysis
Microsoft Managed Control 1142 - Security Assessment And Authorization Policy And Procedures
Microsoft Managed Control 1089 - Security Training Records
Microsoft Managed Control 1285 - Telecommunications Services | Provider contingency Plan
Microsoft Managed Control 1709 - Security Function Verification
Microsoft Managed Control 1052 - Session Lock
Microsoft Managed Control 1034 - Least Privilege
Microsoft Managed Control 1623 - Boundary Protection
Microsoft Managed Control 1515 - Personnel Termination
Microsoft Managed Control 1327 - Authenticator Management | Password-based Authentication
Microsoft Managed Control 1229 - Information System Component Inventory | No Duplicate Accounting of Components
Microsoft Managed Control 1123 - Audit Review, Analysis, And Reporting | Audit Level Adjustment
Microsoft Managed Control 1474 - Emergency Power | Long-term Alternate Power Supply - Minimal Operational Capability
Microsoft Managed Control 1227 - Information System Component Inventory | Automated Unauthorized component Detection
Microsoft Managed Control 1361 - Incident Handling
Microsoft Managed Control 1594 - Developer Configuration Management
Microsoft Managed Control 1572 - Acquisition Process
Microsoft Managed Control 1331 - Authenticator Management | Password-based Authentication
Microsoft Managed Control 1132 - Protection of Audit Information | Audit Backup on Separate Physical systems / components
Microsoft Managed Control 1225 - Information System Component Inventory
Microsoft Managed Control 1640 - Transmission Confidentiality And Integrity
Microsoft Managed Control 1420 - Maintenance Personnel
Microsoft Managed Control 1658 - Secure Name / Address Resolution Service (Recursive or Caching Resolver)
```

在下一个练习中，你将应用其中一个 Azure 策略，但正如 Figure 6.1 所示，你可以在订阅或资源组级别应用大量 Azure 策略定义。定义代表一组规则，包括要求所有应用服务启用 HTTPS 或限制订阅所有者数量等操作。定义通常针对满足政府、行业或区域认证要求。定义很多，其中规则也很多。由你决定需要哪种认证以及使用哪个模板。Azure Blueprint 应能让你相当快速地进入正轨。使用 Azure Policy 你可以相当轻松地应用实际限制。如果需要帮助，你可以向 Microsoft 提交支持案例，有人会帮助你完成此过程。此外，如果内置规则不符合你的要求，猜猜看——你可以创建自定义 Azure Policy 并添加自己的规则组。

总体而言，Azure Policy 使管理、创建和分配策略定义成为可能。这些策略定义对你的 Azure 资源强制执行规则。这种强制执行有助于你保持与公司服务级别和标准的合规性。Azure Policy 还提供监控和扫描功能，可识别任何不符合策略的资源。请查看 Figure 6.3，它说明了可供你使用的一些报告和管理功能。

我开始审查我的 Azure 消费足迹时首先认识到的一件事是我已配置了多少资源。这只是表明资源可以多快地配置，以及你拥有的资源的概览可能会丢失。Azure Policy 概览刀片显示了策略描述。它还指出应用了它的资源是否合规以及给定策略的合规百分比。完成练习 6.2，在其中你将创建并应用自定义策略定义。

