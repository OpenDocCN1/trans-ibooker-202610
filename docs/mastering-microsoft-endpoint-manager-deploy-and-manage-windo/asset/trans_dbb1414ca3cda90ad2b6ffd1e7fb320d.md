

DHA 使企业能够将组织的安全水平提升至硬件监控和可验证的安全水平，同时对运营成本的影响最小或为零。

DHA 服务与 MDM 解决方案集成，并执行以下操作：

- 将 DHA 服务通过现有 MDM 设备管理通信渠道从设备接收到的信息与 DHA 报告结合。
- 基于可验证的硬件和受保护的数据做出更安全、更可信的安全决策。
- 在合规策略中利用 DHA 时，Windows 10 必须在 Microsoft Intune 中进入合规状态之前重新启动。

在 Windows 合规策略的设备健康部分，有三个设置：

![](img/a3e9f26159d3739e434fe676b2987c1d_413_0.png)

图 10.45 – 合规策略 – 设备健康

按如下方式更新这些设置：

- 要求 BitLocker：要求 – 设备可以在系统关闭或休眠时保护存储在驱动器上的数据免受未经授权的访问。
- 要求设备启用安全启动：要求 – 系统被强制启动到工厂受信任状态。用于启动计算机的核心组件必须具有组织制造设备所信任的正确加密签名。这些签名在 UEFI 固件允许机器启动之前进行验证。如果任何文件被篡改，从而破坏其签名，系统将无法启动。

作为 IT 管理员，您可以监控合规状态的不同部分：

- **合规运营报告**：提供实时运营报告，可帮助 IT 管理员根据在此报告中找到的数据采取行动。

转到 **Home | Devices | Monitor | Windows health attestation report**。在这里，您可以选择查看有多少设备未启用 BitLocker 和安全启动，例如：

![](img/a3e9f26159d3739e434fe676b2987c1d_416_0.png)

图 10.49 – Windows 健康证明报告

- **不合规设备报告**：转到 **Home | Devices | Monitor | Noncompliant devices.**

在此报告中，您可以选择查看所有不合规设备。如果应用 **OS == Windows** 过滤器，您将只看到不合规的 Windows 设备：

![](img/a3e9f26159d3739e434fe676b2987c1d_417_0.png)

图 10.50 – 监控 – 不合规设备

- **不合规策略（预览）**：转到 Home | Devices | Monitor | Noncompliant policies (preview)。

在此报告中，您可以快速了解 Microsoft Intune 中的合规策略概览以及有多少设备不符合特定合规策略：

![](img/a3e9f26159d3739e434fe676b2987c1d_418_0.png)

图 10.51 – 监控 – 不合规策略（预览）

本节关于合规策略以及如何对合规策略进行运营监控到此结束。接下来，我们将介绍合规的组织报告。

点击 Reports，您将获得设备合规性和设备合规性趋势选项：

图 10.53 – 报告 – 设备合规性

使用设备合规性，通过利用过滤器，您可以选择 Windows 设备并仅获取这些设备的报告。您还可以根据合规状态和/或所有权进行过滤：

图 10.54 – 报告 – 设备合规性

## 设备合规性趋势

此报告将为您提供过去 60 天合规状态的历史视图。

在此视图中，我们再次对设置 Windows 作为操作系统进行了过滤：

![](img/a3e9f26159d3739e434fe676b2987c1d_421_0.png)

图 10.55 – 报告 – 设备合规性趋势

本节关于设备合规性组织报告到此结束。

## 总结

在本章中，您了解了不同类型的策略，并查看了通过 Microsoft Endpoint Manager 为物理和云端点以更现代的方式配置 Windows 的场景。我们查看了一些您可以利用的场景和策略配置。在策略管理领域，您需要在企业环境中确定想要采取何种方法。当查看纯云管理的设备和策略时，将是向前看而非向后看的完美时机。例如，首先部署具有安全影响的策略，例如 Edge 安全基线、Windows Defender 策略等。然后，查看部署将帮助最终用户提高生产力的策略，例如配置 OneDrive 已知文件夹移动、帮助最终用户更好地在应用程序中开始工作的策略，例如自动配置 Microsoft Edge、从应用程序中删除提示等。当您完成这两个大领域后，您可能就完成了，或者至少处于良好的起点。在与最终用户测试他们的体验以及他们提供的任何反馈后，您可以返回并更改策略以适应业务需求。当您与业务中的应用程序所有者交谈时，或您的安全部门对设备上的某些策略或设置有特定要求时也是如此。这是一个持续的过程，用于评估您是否在环境中使用正确的策略，就像您在策略配置使用 GPO 时可能有类似的过程一样。

在本章之后，您将能够管理您的物理和云端点，并知道在未来何时遵循哪种配置路径。下一章是策略管理的第三部分。最后一部分介绍了一个云服务，如果您拥有 Office 365，您可以利用它来配置 Microsoft 365 应用程序，无论是否使用 Microsoft Intune。

## 问题

- 1. 什么是 Storage Sense？
  a) 用于优化 SSD 性能的服务。
  b) 具有幽默感的服务。
  c) 用于清理端点上 OneDrive 缓存和配置文件中下载文件夹的服务。
  d) 用于阻止服务运行的服务。

2. 配置 Windows 端点设置的推荐选项是什么？

- a) 设置目录
- b) 管理模板
- c) 安全基线
- d) 设备限制配置文件

3. 可以使用哪种策略类型来配置 Microsoft Edge？

- a) 共享多用户设备
- b) 管理模板
- c) Kiosk 配置文件
- d) 设备限制配置文件

## 答案

- 1. (c)
- 2. (a)
- 3. (b)

## 进一步阅读

如果您想在阅读本章后了解更多信息，请查看以下免费在线资源：

- Device compliance policies in Microsoft Intune – Azure | Microsoft Docs:
  https://docs.microsoft.com/en-us/mem/intune/protect/device-compliance-get-started
- Use templates for Windows 10 devices in Microsoft Intune – Azure | Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/configuration/administrative-templates-windows
- Restrict device features using policies in Microsoft Intune – Azure | Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/configuration/device-restrictions-configure

## 11

## Office 策略管理

在本章中，您将了解 Office 云策略，以及如何使用此云服务来配置最终用户在其 Windows 设备上安装的 Microsoft 365 应用程序，无论是在家中的 Windows 设备还是由 Microsoft Intune 等管理工具管理的公司发放的 Windows 设备。这不是 GPO、Microsoft Intune 中的管理模板或设置目录策略的替代品，但在某些场景中，您可以利用 Office 云策略服务。

我们还将展示 Office 云策略服务具有安全策略顾问，它可以帮助您和您的企业了解是否已实施正确的策略建议。这些建议基于 Microsoft 最佳实践以及有关您现有环境的信息。

安全策略顾问只能与 Office 云策略服务结合使用。

在本章中，我们将涵盖以下主题：

- Office 云策略服务
- 使用 OCP 服务创建策略配置
- 安全策略顾问

