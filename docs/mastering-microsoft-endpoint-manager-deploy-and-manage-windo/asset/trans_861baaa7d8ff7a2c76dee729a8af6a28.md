
最常见的批量激活方法要求设备连接到组织网络，或通过 VPN 连接，以便定期与组织的激活服务*签到*以维护其许可证。然而，当人们在家工作且不在公司或学校网络中时，其设备接收或维护激活的能力会受到限制。

有几种方法可以进行 Windows 激活：

- 密钥管理服务
- 多次激活密钥
- 基于 Active Directory 的激活
- Windows 10 订阅激活

## 通过 Windows 365 云电脑使用 Windows

前面的激活 Windows 10 企业版的选项已内置到 Windows 365/云电脑中。如果你打算在 Azure 上自行管理和部署虚拟桌面，这意味着 Windows 10 企业版将通过 Azure **密钥管理服务 (KMS)** 自动激活。

## Azure KMS – 云电脑/Windows 365/AVD

如果你的 Windows 桌面是虚拟的并在 Azure 上运行，例如使用 **Azure 虚拟桌面 (AVD)**，你需要连接到 Azure `kms.core.windows.net:1688` KMS 服务器才能激活你的虚拟机。使用 Azure 映像库中的映像时，这会自动发生。确保你的 Azure **虚拟网络 (vNET)** 能够连接到该地址，并相应配置防火墙策略和规则。稍后将在云电脑/Windows 365 和要求章节中了解更多相关内容。

Windows 11 将在今年秋季晚些时候该操作系统正式发布后加入产品阵容。本书后面还会介绍更多关于 Windows 11 的内容。

通过 Active Directory 激活的设备在与域最近一次联系后的最多 180 天内保持激活状态。设备会在此期间结束前定期尝试重新激活（默认每 7 天一次），并在 180 天结束时再次尝试。

## Windows 10 及更高版本的订阅激活

从 Windows 10 版本 1703 开始，Windows 10 专业版支持订阅激活功能，如果用户订阅了 Windows 10 企业版 E3 或 E5，则可以自动从 Windows 10 专业版*升级*到 Windows 10 企业版。

借助 Windows 10 版本 1703，Windows 10 企业版 E3 和 Windows 10 企业版 E5 都可以作为在线订阅服务提供。现在，在组织中部署 Windows 10 企业版无需密钥，也无需重启。

## 适用于 Windows 10 版本 1703 或更高版本

拥有当前 Windows 10 专业版许可证的设备可以无缝升级到 Windows 10 企业版。基于产品密钥的 Windows 10 企业版软件许可证可以转换为 Windows 10 企业版订阅。

拥有企业协议的组织也可以使用传统的加入 Active Directory 的设备，从这项新服务中受益。在这种情况下，在其设备上登录的 Active Directory 用户必须使用 Azure AD Connect 同步与 Azure AD 进行同步。

订阅激活功能消除了在每台目标设备上手动部署 Windows 10 企业版或教育版映像，然后建立本地密钥管理服务（例如基于 KMS 或 MAK 的激活）、输入**通用批量许可密钥 (GVLK)**，以及随后重启客户端设备的需要。

## WUfB 是管理 Windows 服务的新方式

WUfB 允许 IT 专业人员利用基于云的 Windows 更新服务来部署和管理 Windows 更新。你可以使用组策略或 MDM 解决方案（如 Microsoft Intune）来配置 WUfB 设置，以控制 Windows 10 设备的更新方式和时间。

WUfB 自 Windows 10 版本 1511 起可用，并在后续 Windows 10 版本中得到了增强。其中一些改进旨在为 IT 专业人员提供更大的控制权，同时为最终用户提供更好的体验。

## 谁应该使用 WUfB？

WUfB 面向组织中管理的运行 Windows 10 教育版、专业版或企业版的设备。WUfB 不支持 Windows 10 **长期服务渠道 (LTSC)**：

- 所有组织都可以利用它来更高效地维护其联网设备。
- 中小型企业可以在继续使用 Windows 更新的同时获得灵活性，并且比之前不使用 System Center Configuration Manager、Microsoft Intune、**Windows Server 更新服务 (WSUS)** 或第三方解决方案时拥有更多控制。

## 为什么需要 WUfB？

WUfB 可以帮助你为最终用户提供最佳的 Windows 更新体验，并降低维护 Windows 的成本，同时为你提供许多更复杂、更耗时的解决方案所具备的灵活性和控制力：

- 降低批准、部署和监控更新的成本。
- 管理组织生态系统中的应用程序兼容性。
- 找到合适的权衡方案来保护设备，同时尽量减少对员工工作的干扰。
- 管理支持快速更新速度所需的基础设施配置，包括找到合适的方法来处理很少连接到企业的设备。

## WUfB 允许我配置什么？

当你使用配置设置从 Microsoft Intune 配置 WUfB 策略时，WUfB 将被启用。如果你传统上使用 WSUS 进行服务，一旦你设置了 WUfB 设置，你的客户端将同时连接到云中的 Windows 更新以及你的 WSUS 服务器。如果你想更改此行为，则需要删除端点上旧的 WSUS 配置设置。

通过 Windows 更新客户端设置（通过组策略、MDM 或 UX），WUfB 提供了延迟应用更新的能力。质量更新和功能更新分开处理，以提供更大的灵活性。

此外，如果你需要在早期环中测试更新后有时间对更新实施补救，则可以利用“暂停”功能：

- **功能更新**：以前称为**升级**，功能更新不仅包含安全性和质量修订，还包含重要的功能新增和更改。功能更新于每年秋季和春季每半年发布一次。
- **质量更新**：质量更新是传统的操作系统更新，通常于每月第二个星期二发布（尽管它们可以随时发布）。其中包括安全更新、关键更新和驱动程序更新。WUfB 还将非 Windows 更新（例如 Microsoft Office 或 Visual Studio 的更新）视为质量更新。这些非 Windows 更新称为 **Microsoft 更新**，你可以设置设备在接收 Windows 更新的同时接收（或不接收）此类更新。
- **驱动程序更新**：适用于与你设备相关的非 Microsoft 驱动程序的更新。驱动程序更新默认开启，但如果你愿意，可以使用 WUfB 策略将其关闭。
- **Microsoft 产品更新**：其他 Microsoft 产品的更新，例如通过 Windows Installer (MSI) 安装的 Office 版本。通过 Click-to-Run 安装的 Office 版本无法使用 WUfB 更新。产品更新默认关闭。你可以使用 WUfB 策略将其开启。

Windows 10 功能更新策略与你的*Windows 10 更新环*策略协同工作，以防止设备收到比功能更新策略中指定的值更新的 Windows 功能版本。

要使 Windows 10 功能更新策略生效，需要 Windows 10 版本 1709 或更高版本。设备需要注册到 Intune MDM，并加入混合 AD、加入 Azure AD 或注册到 Azure AD。Windows 遥测需要打开，最低设置为“基本”。如果遥测关闭，Windows 可能会升级到比功能更新策略中定义的版本更高的 Windows 版本。
