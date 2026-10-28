

## 启用 Windows 自动注册

自动 MDM 注册意味着当 Windows 设备加入 Azure AD 时，设备将自动通过 MDM 注册流程注册到 Intune。

要实现 Windows 自动注册，请按照以下步骤操作：

- 1. 登录到 MEM 管理中心 (https://endpoint.microsoft.com)。
- 2. 选择 Devices | Windows | Windows enrollment 然后选择 Automatic Enrollment：

![](img/a3e9f26159d3739e434fe676b2987c1d_171_0.png)

图 5.6 – MEM 管理中心 – Windows 自动 MDM 注册

如果所有用户都已分配 Intune 许可证，用户注册也可以限定为特定用户组。最佳选择是利用 Intune 注册限制来配置用户可以注册的 Windows 设备。

3. 确保将 MDM 用户范围选择为 All：

![](img/a3e9f26159d3739e434fe676b2987c1d_172_0.png)

图 5.7 – MEM 管理中心 – MDM 用户范围

以下是 MDM 用户范围所有选项的含义：

- None：MDM 自动注册已禁用。
- Some：选择可以自动注册其 Windows 设备的组。
- All：所有用户都可以自动注册其 Windows 设备。

> 重要说明
> 自动 MDM 注册需要 Azure AD 高级订阅。

对于 Windows 自带设备 (BYOD) 设备（个人注册），如果 MAM 用户范围和 MDM 用户范围（自动 MDM 注册）都已为所有用户（或同一组用户）启用，则 MAM 用户范围优先。如果您已配置它们，设备将不会进行 MDM 注册，并且将应用 Windows 信息保护 (WIP) 策略。

## 使用 Azure Virtual Desktop 与 Intune

在 Windows 365 中，以下步骤不需要，因为 Intune 注册会自动完成。此外，请确保在继续之前已执行上一步（将 MDM 用户范围设置为 All 且 MAM 用户范围设置为 None）。

> **重要说明**
> 请确保 RemoteDesktopServices/AllowUsersToConnectRemotely 策略未被禁用。

请注意，以下 Windows 10 桌面设备远程操作不适用于/不建议用于 Azure Virtual Desktop 虚拟机：

- Autopilot reset
- BitLocker key rotation
- Fresh start
- Remote lock
- Reset password
- Wipe

让我们开始执行操作：

- 1. 登录到您的会话主机。
- 2. 打开 **Local Computer Policy** 并单击 **Administrative Templates | Windows Components | MDM：**

![](img/a3e9f26159d3739e434fe676b2987c1d_173_0.png)

图 5.8 – 本地组策略 – MDM

- 3. 将策略设置为 **Enabled.**
- 4. 将凭据类型设置为 **User Credential：**

![](img/a3e9f26159d3739e434fe676b2987c1d_174_0.png)

图 5.9 – 本地组策略 – MDM

- 5. 确认您的会话主机已注册到 Azure AD 的 MDM 注册，其应如下例所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_174_1.png)

图 5.10 – 管理中心 – 所有设备

正如我们刚刚向您展示了将 Windows 设备注册到 Microsoft Intune 的不同选项，我们现在将向您展示如何通过 Microsoft Intune 注册限制来限制 Windows 注册。

## Microsoft Intune Windows 注册限制

如果您阻止个人拥有的 Windows 设备注册，Intune 会检查每个新的 Windows 注册请求是否已作为企业注册获得授权。未经授权的注册将被阻止。

> **重要说明**
> 在默认注册限制中阻止 Windows MDM 注册将阻止一些涉及企业设备的场景。

## Microsoft Intune Windows 设备限制

在本节中，我们将了解如何为 Windows 设备创建注册限制：

- 1. 登录到 MEM 管理中心 (endpoint.microsoft.com)。
- 2. 选择 Devices | Enrollment restrictions：

![](img/a3e9f26159d3739e434fe676b2987c1d_175_0.png)

图 5.11 – 管理中心 – 注册限制

- 3. 创建设备类型限制。输入 `Device type restriction - HR` 作为名称：

![](img/a3e9f26159d3739e434fe676b2987c1d_176_0.png)

图 5.12 – 管理中心 – 注册限制

- 4. 选择同时对 MDM 和个人拥有的设备在特定操作系统上进行阻止和允许，以允许或阻止设备注册。

如果您阻止特定操作系统注册，则适用于企业和个人设备注册。

如果您允许 Windows (MDM) 平台注册，您可以阻止个人设备；请参阅以下部分以了解 *阻止个人 Windows 设备* 的含义。

**Allow min/max range** 对于操作系统版本仅在注册时阻止设备，对已注册到 Microsoft Intune 的设备没有影响：

> **重要说明**
> 
> Windows 仅对 Windows 10 支持 `major.minor.build.rev`。
> 
> Windows 10 在注册期间不提供 rev 编号，因此，例如，如果您输入 `10.0.17134.100` 而设备为 `10.0.17134.174`，它将在注册期间被阻止。

Home > Devices >

# Create restriction

Device type restriction

Basics 1 Platform settings 2 Scope tags 3 Assignments 4 Review + create

Specify the platform configuration restrictions that must be met for a device to enroll. Use compliance policies to restrict devices after enrollment. Define versions as major.minor.build. Version restrictions only apply to devices enrolled with the Company Portal. Intune classifies devices as personally-owned by default. Additional action is required to classify devices as corporate-owned. Learn more

| Type | Platform | versions | Personally owned | Device manufacturer |
| Android Enterprise (work profile) | Allow Block | Allow min/max range: Min Max | Allow Block | Manufacturer name here |
| Android device administrator | Allow Block | Allow min/max range: Min Max | Allow Block | Manufacturer name here |
| iOS/iPadOS | Allow Block | Allow min/max range: Min Max | Allow Block | Restriction not supported |
| macOS | Allow Block | Restriction not supported | Allow Block | Restriction not supported |
| Windows (MDM) | Allow Block | Allow min/max range: Min Max | Allow Block | Restriction not supported |

图 5.13 – 管理中心 – 注册限制 – 平台设置

5. 在 Assignments 步骤中，选择 HR Department。

当您创建自定义注册限制时，您可以将其范围限定为应用于组织中的特定用户组、部门、国家/地区等。

更改分配设置以根据您想要提供的任何限制进行筛选，以避免组注册到 MDM Intune：

Home > Devices >

# Create restriction

Device type restriction

Basics Platform settings Scope tags 4 Assignments 5 Review + create

Included groups

Add groups

Groups

HR Department Remove

图 5.14 – 管理中心 – 注册限制 – 分配

在下一节中，让我们了解如何阻止个人设备。

## 阻止个人 Windows 设备

如果您阻止个人拥有的 Windows 设备注册，Intune 会检查每个新的 Windows 注册请求是否已作为企业注册获得授权。未经授权的注册将被阻止。

以下方法被视为已作为 Windows 企业注册获得授权：

- 注册用户正在使用设备注册管理器帐户。
- 设备通过 Windows Autopilot 注册。
- 设备已使用 Windows Autopilot 注册，但不是来自 Windows 设置的仅 MDM 注册选项。
- 设备的 IMEI 号码列在 **Device enrollment | Corporate device identifiers** 中。
- 设备通过批量预配包注册。
- 设备通过 **Group Policy Object (GPO)** 注册，或通过 Configuration Manager 的自动注册进行共管。

以下注册会被 Intune 标记为企业。但由于它们不提供 Intune 管理员按设备控制，它们将被阻止：

- 在 Windows 设置期间使用 Azure AD 加入的自动 MDM 注册*
- 从 Windows 设置使用 Azure AD 加入的自动 MDM 注册*

以下个人注册方法也将被阻止：

- 从 Windows 设置使用 **Add Work Account** 的自动 MDM 注册*
- 来自 Windows 设置的仅 MDM 注册选项

*如果已使用 Autopilot 注册，这些将不会被阻止。

## Microsoft Intune Windows 设备限制限制

在本节中，我们将学习如何限制设备的限制。让我们开始：

- 1. 登录到 MEM 管理中心 (endpoint.microsoft.com)。
- 2. 选择 **Devices | Enrollment restrictions | Create restriction | Device limit restriction**。输入 `Device type restriction - HR` 作为名称：

