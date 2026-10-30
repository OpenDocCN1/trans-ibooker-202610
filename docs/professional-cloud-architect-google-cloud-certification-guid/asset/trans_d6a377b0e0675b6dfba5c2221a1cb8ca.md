

## Identity-Aware Proxy (IAP)

Google 通过 Cloud IAP 为运行在 GCP 上的 Cloud Engine 实例或应用程序提供额外的访问控制。这允许通过 HTTPS 验证用户的身份，并且仅在允许的情况下授予访问权限。该服务对于远程工作者尤其有用，因为它消除了使用本地网络通过公司 VPN 对用户请求进行身份验证的需求。相反，访问是通过可通过互联网访问的 URL 进行的。当远程用户需要访问应用程序时，请求会被转发到 Cloud IAP，并且如果允许则会授予访问权限。此外，没有传统 VPN 的开销，管理员的可管理性也得以简化。

启用 Cloud IAP 是一个简单的过程，但必须满足一些先决条件。你需要配置防火墙规则以阻止对托管应用程序的 VM 的访问，并且只允许通过 IAP 访问。接下来，你需要导航到 GCP 控制台中的 Security | Identity-Aware Proxy，在那里你会找到你的应用程序或 Compute Engine 资源的列表。只需单击蓝色单选按钮即可启用 IAP。

> **重要提示**
> 你可能首先会被要求进行 OAuth 同意。

最后，你可以从将出现的 information panel 中用相关成员填充 IAP 访问列表。以下屏幕截图显示了 IAP 已为 HTTPS 资源启用：

![](img/de2c97ab017f36b3750a5bade111edd7_500_0.png)

图 15.27 – Identity-Aware Proxy

现在我们已经对 IAP 进行了概述，让我们来看一些重要功能。

## TCP forwarding

IAP 具有端口转发功能，允许我们从公共互联网对后端进行 **Secure Shell (SSH)** 或 **Remote Desktop for Windows (RDP)**，从而防止这些服务公开暴露在互联网上。我们需要在到达目标资源之前通过授权和身份验证检查，从而降低从互联网连接这些服务的风险。

IAP 的转发功能允许我们连接到 Compute Engine 实例上的随机 TCP 端口。IAP 然后会在 localhost 上创建一个监听端口，将所有流量转发到我们的实例。IAP 然后会将客户端的所有流量包装在 HTTPS 中，并在我们成功通过身份验证后允许访问接口和端口。此身份验证过程基于配置的 IAM 权限。

我们应该注意一些限制。端口转发不适用于批量数据传输，因此 IAP 保留对服务滥用进行速率限制的权利。此外，在非活动 1 小时后，IAP 将自动断开我们的会话，因此使用此功能的应用应具有在断开连接后重新建立隧道的逻辑。

## Access Context Manger

Access Context Manager 为 Google Cloud Organization 管理员提供定义项目和 GCP 资源细粒度、基于属性的访问控制的能力。Access Context Manager 根据请求的上下文授予访问权限，例如用户的身份、设备类型或 IP 地址。它在传统的边界安全模型之上增加了另一层保护，在该模型中，网络内部的任何内容都被视为受信任。Access Context Manager 考虑到了移动劳动力和 **Bring Your Own Device (BYOD)**，这增加了边界安全模型未考虑的额外攻击向量。

## Chronicle

Google 安全套件中最新的引入之一是 **Chronicle**。虽然我们怀疑考试中不会就此进行提问，但我们认为值得对此说几句话。

Chronicle 是一种威胁检测解决方案，可帮助企业以速度和规模识别威胁。它可以搜索企业产生的大量安全和网络遥测数据，并对活动提供即时分析和上下文。

## Summary

安全方面有更深入的探讨，但对于考试，你必须理解安全是 Google 所有服务的重要部分，而非事后考虑。到目前为止，我们已经介绍了一些服务，以确保你的 GCP 基础设施安全。我们介绍了 Cloud Identity，介绍了 IAM 模型，并研究了加密。

在下一节中，我们将介绍额外的安全服务。

## Additional security services

GCP 提供了多个高级服务来帮助你保护基础设施和资源。在本节中，我们将简要介绍其他关键服务。

## Security Command Center (SCC)

SCC 为企业提供对其在多个 GCP 服务中的云数据的整体视图。SCC 的真正好处在于它协助收集数据和识别威胁，并且可以在任何业务影响发生之前采取行动。它还提供反映我们资源整体健康状况的仪表板。它还可以与其他 GCP 工具（如 Cloud Security Scanner）和第三方（如 Palo Alto Networks）集成。GCP 将 SCC 中的可能安全威胁称为 findings。要访问这些 findings，你必须拥有相关的 IAM 角色，其中包括 Security Center Findings Viewer 的权限，然后浏览到 SCC 的 Findings 选项卡。当然，像所有 GCP 服务一样，有 API 集成，允许我们列出任何 findings。SCC 还允许我们使用 security marks，这使我们能够注释资产或 findings 并使用这些标记进行搜索或过滤。

要使用 SCC，你必须从你的 GCP 组织中查看它。此外，你必须是组织管理员并拥有当前组织的 Security Center Admin 角色。这允许你选择所有当前和未来的项目以包含在内（如果愿意）。或者，我们可以根据发现的任何安全风险包含或排除单个项目。

Security Command Center 事件可以导出到 Chronicle，它专为安全威胁检测和调查而构建。它建立在 Google 基础设施之上，让我们能够利用 Google 的规模来减少调查时间。

## Forseti

Forseti 是一款开源安全工具，可帮助保护你的 GCP 环境。如果希望监控资源以确保访问控制与预期一致，则很有用。它还可用于在任何内容更改时通过电子邮件或 Slack 频道发布进行提醒。此外，它允许你对资源进行快照，以便你始终知道云的样子。它还可以通过将策略与当前状态进行比较并使用 GCP API 纠正任何违规行为，对敏感的 GCP 资源强制执行规则。

## Cloud Armor

Cloud Armor 使用 Google 的全球基础设施提供抵御 DDoS 攻击的规模化防御。它用于将访问你的 HTTPS 负载均衡器列入黑名单或白名单。这可用于防止恶意用户或流量联系你的资源，或更糟的是，基于规则集接管你的 VPC。以下屏幕截图显示了一项策略，该策略已设置为拒绝特定 IP 并返回 404 错误：

![](img/de2c97ab017f36b3750a5bade111edd7_503_0.png)

图 15.28 – Cloud Armor

通过单击 Targets 选项卡，我们可以看到该策略正在定位后端负载均衡器，如以下屏幕截图所示：

![](img/de2c97ab017f36b3750a5bade111edd7_504_0.png)

图 15.29 – Cloud Armor – Targets

Cloud Armor 日志允许我们查看任何访问尝试并告诉我们哪个源请求了访问。我们应该注意，这些日志是通过 Stackdriver 提供的，该内容在本章的 Monitoring and logging 部分进行了描述。这将在第 17 章 Monitoring Your Infrastructure 中更详细地介绍。

以下屏幕截图显示了 Cloud Armor 日志的一些示例输出：

![](img/de2c97ab017f36b3750a5bade111edd7_504_1.png)

图 15.30 – Cloud Armor logs

## Secret Manager

Secret Manager 是一个方便且安全的系统，用于存储证书、密码或 API 密钥。将此敏感数据存储在中央位置可提供单一事实来源来管理、访问和审计 Google Cloud 中的机密。Secret Manager 还允许为每个机密设置访问策略。此外，可以为每次机密访问配置审计日志。此服务是 Hashicorp Vault 等第三方机密管理系统的替代方案。

在本节中，我们查看了 GCP 提供的一些其他安全服务。并非所有这些都将在考试中提问，但对每个服务的高层次了解是很有益的。

## Summary

安全方面有更深入的探讨，但对于考试，我们必须理解安全是 Google 所有服务的重要部分，而非事后考虑。在本章中，我们查看了 GCP 提供的许多服务，以确保我们的解决方案安全。在其他方面，我们介绍了 Cloud Identity，介绍了 IAM 模型，并研究了加密。在下一章中，我们将介绍 Google Cloud Management Options。

> Exam Tip
> 请记住，我们绝不应授予用户或服务超出其所需权限的权限。始终使用最小权限原则。

## Further reading

阅读以下文章以获取本章所涵盖内容的更多信息：

- **Cloud Identity**: https://cloud.google.com/identity/docs/
- **IAM**: https://cloud.google.com/iam/docs/
- **Encryption**: https://cloud.google.com/storage/docs/encryption/
- **Cloud IAP**: https://cloud.google.com/iap/docs/
- **Security Command Center**: https://cloud.google.com/security-command-center/docs/

