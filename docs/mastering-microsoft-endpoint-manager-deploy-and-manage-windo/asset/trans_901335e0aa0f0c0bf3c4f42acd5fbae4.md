

## 9 了解策略管理

在本章中，你将了解来自 Microsoft Intune 的策略管理与 **组策略对象 (GPO)** 有何不同，以及在你的环境中用于自定义和保护 Windows 10/11 企业版桌面的不同策略选项。本章内容会非常广泛，但它将为你提供理解 Windows 与 Microsoft Intune 之间策略管理如何工作所需的基础信息。

我们在本书中将策略管理分为三章；在这一章之后，*第 10 章，高级策略管理* 将介绍不同场景下的高级策略管理，而 *第 11 章，Office 策略管理* 则介绍来自 Office 云策略服务的 Office 策略管理。

在本章中，我们将涵盖以下主题：

- 策略管理
- 什么是内容安全策略 (CSP) 策略？
- Windows 推送通知服务 (WNS)
- 策略设计的入门
- Microsoft Endpoint Manager 内的策略管理
- 从 Azure 目录 (AD) 迁移现有策略 – 组策略管理（预览）

## 策略管理

使用 Microsoft Endpoint Manager – Intune 管理你的 Windows 10/11 企业版桌面，全部都是关于标准化并简化环境的管理层。如上一章所述，一切都围绕着将配置集（和应用程序）分层，针对来宾操作系统进行分离，从而无需从一开始就创建包含这些内容的自定义映像。

Microsoft Endpoint Manager – Intune 内的策略管理使得可以从“设备”菜单中配置以下选项：

- 合规策略
- 条件访问
- 配置配置文件
- 设置目录
- 脚本
- 组策略分析（预览）
- 注册限制
- 策略集

组策略管理已有 20 多年的历史，一直是配置域中一组用户或计算机行为的方式。今天在本地域中仍然可以实现这一点，但如果你想开始现代化你的策略和设置管理，你应该开始关注 Microsoft Intune 以及其为策略管理提供的功能集。使用 GPO 有一些缺点，其中之一是它需要与域控制器保持可见性。另一个是 GPO 是即发即忘，但这是什么意思呢？GPO 分配给特定的一组用户和设备，并在设备定期连接到域控制器时应用。如果设备正确接收并应用了策略、无法联系到域控制器，或没有应用新的或更改的策略，域控制器不会收到任何报告。

有时，由于配置错误，Windows 设备可能会联系到内部网络上非常遥远且连接非常慢的域控制器，这会导致启动和登录时间非常长。许多这些问题都可以通过纯云加入并管理的设备来避免。

随着 COVID-19 在 2020 年初席卷全球，许多公司被迫让员工在家工作，而许多公司对此并不准备就绪。这也推动了从云端对设备进行 MDM 管理的新方法。Microsoft Intune 是这种新型工作方式的完美匹配，因为它在初始加入 Microsoft Intune 后仅需要互联网连接。

在本章中，我们将重点关注纯云管理的 Azure Active Directory Windows 10 设备，但同样也适用于从 Microsoft Intune 管理的混合域加入设备，甚至在共管状态下。需要在此注意的一点是，GPO 和 MDM 设置管理位于身份层，即用户或设备，而 Microsoft Intune 与 **System Center Configuration Manager (SCCM)** 之间的共管则位于管理平面上。

首先，我们需要回顾传统的 Windows 管理，即所有 Windows 设备都在办公室的本地、生产环境中，或最终用户通过 VPN 在家工作。如果这些设备混合加入 Azure Active Directory，现代策略管理仍然是一个选项。对于新设备而言，最佳的前进方向是纯 Azure Active Directory 加入并通过 Windows Autopilot 上线。我们在本章中介绍的内容涵盖这两种场景。本书致力于云管理，并且某些场景不适用于混合加入的设备，因此你需要做出一些决定，将设备迁移到 Azure AD 加入，以获得最佳的最终用户体验。从小开始，从 **概念验证 (POC)** 开始，并展示现代策略管理的优势。最佳实践方法是，在你的 POC 中阻止本地设备从本地 Active Directory 获取 GPO；否则，你可能会陷入无法 100% 确定设置是从何处应用的情况。

**配置服务提供程序 (CSP)** 是用于在设备上读取、设置、修改或删除配置设置的接口。这些设置映射到注册表项或文件。某些 CSP 支持 WAP 格式，某些支持 SyncML，而某些两者都支持。SyncML 仅用于通过空中传输的 **开放移动联盟设备管理 (OMA DM)**。另一方面，WAP 可用于通过空中传输的 OMA 客户端配置，或作为 `.provxml` 文件包含在手机映像中，该文件在启动期间安装。

### 什么是 CSP 策略？

某些策略只能在设备级别配置，而其他策略可以在用户级别配置。这意味着设备级策略将独立于登录设备的用户而产生效果，而用户级策略将取决于登录设备的用户。

用户范围是指策略仅适用于登录设备的用户，并且策略可以根据登录设备的用户而变化。以下是配置用户策略时 CSP 树的示例：

- `./User/Vendor/MSFT/Policy/Config/AreaName/PolicyName` 用于配置策略
- `./User/Vendor/MSFT/Policy/Result/AreaName/PolicyName` 用于获取结果

设备范围是指策略仅适用于设备本身，而与登录设备的用户无关。以下是配置设备策略时 CSP 树的示例：

- `./Device/Vendor/MSFT/Policy/Config/AreaName/PolicyName` 用于配置策略
- `./Device/Vendor/MSFT/Policy/Result/AreaName/PolicyName` 用于获取结果

GPO 与 CSP 策略之间最大的区别在于，CSP 策略还具有结果通道，因此设备上配置的每个设置都会向 MDM 系统报告 – 在本例中为 Microsoft Intune。

如果我们更仔细地查看策略结构，它看起来像 Windows 注册表以树结构排列：

![](img/a3e9f26159d3739e434fe676b2987c1d_355_0.png)

通过使用 ADMXInstall，你可以为在操作系统版本之间添加的那些 Win32 或 Desktop Bridge 应用添加 ADMX 支持的策略。ADMX 支持的策略通过使用 CSP 策略 URI 在你的设备中摄取：`./Vendor/MSFT/Policy/ConfigOperations/ADMXInstall`。

OMA-URI 字符串需要进入 CSP 策略 URI：

- `./Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/Applicationname/Policy/ADMXFileName`。
- `./Vendor/MSFT/Policy/Config/` 保持不变，用于你部署到设备的所有机器策略。

Applicationname 和 ADMXFileName 都是用户定义的。在本例中，Applicationname 为 App1，你可以使用与 ADMXFileName 相同的名称。只需记住 ADMXFileName 必须唯一，这意味着你不能在同一设备上部署两个同名的 ADMX 文件。

以下是我的 ADMX 文件的内容 – 这也可能是 Google Chrome、Microsoft Office、Internet Explorer 或其他：

![](img/a3e9f26159d3739e434fe676b2987c1d_356_0.png)

图 9.2 – AdmxInstalled 的注册表项

然后，如果你更仔细地查看注册表，它们首先被写入的地方是 HKLM\SOFTWARE\MICROSOFT\PolicyManager\AdmxInstalled。

策略始终在 GUID 下声明，并使用你在 Microsoft Intune 中创建策略时为策略指定的名称。

然后，你将能够在创建策略设置时看到你使用的策略类别的命名：

```
HKLM\Software\Microsoft\PolicyManager\AdmxDefault
```

如果策略是设备策略，你将能够在以下位置看到适用于设备的直接结果：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PolicyManager\current\device。

最终，策略在 Windows 设备上所做的全部工作就是设置一些注册表项，而 MDM 策略也是如此。所有策略设置都进入这里：HKEY_LOCAL_MACHINE\SOFTWARE\Policies\。

