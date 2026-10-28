

# 适用于现有设备的 Windows Autopilot

如果你不想将设备收集并上传到 Autopilot，还有其他一些选项可供选择。

对于已在 Intune 中的设备（可以是 Azure AD 加入的设备或共同管理的设备），你可以利用 Autopilot 配置文件中的“将所有目标设备转换为 Autopilot”选项：

![](img/a3e9f26159d3739e434fe676b2987c1d_252_0.png)

图 7.12 – 将所有目标设备转换为 Autopilot

选择“是”以将所有目标设备注册到 Autopilot（如果尚未注册）。下次已注册设备经历 Windows OOBE 时，它们将按照分配的 Autopilot 场景进行。

请注意，某些 Autopilot 场景需要特定最低版本的 Windows。请确保你的设备满足执行该场景所需的最低版本。

删除此配置文件不会从 Autopilot 中移除受影响的设备。要从 Autopilot 中移除设备，请使用 Windows Autopilot 设备视图。

你还可以通过从 System Center Configuration Manager (SCCM) 或 Microsoft Deployment Toolkit (MDT) 执行传统操作系统部署，来为现有设备利用 Windows Autopilot。

你只需将 Autopilot 配置文件放在 `%windir%\provisioning\AutoPilot\AutopilotConfigurationFile.json` 中。

在 Microsoft Intune 中创建 Autopilot 配置文件后，你可以利用 WindowsAutopilotIntune PowerShell 模块导出它：

```
Install-Module WindowsAutopilotIntune -Force
Install-Module Microsoft.Graph.Intune -Force
Connect-MSGraph
Get-Autopilotprofile
```

你将获得租户中所有 Autopilot 配置文件的列表。以下是一个示例。我们将利用 ID 获取它，并将其转换为所需的 JSON 内容：

```
@odata.type                     : #microsoft.graph.
azureADWindowsAutopilotDeploymentProfile
id                              : 264c05b0-683c-4537-87ff-
1ff5151d5b98
displayName                : Intune Book
description                 :
language                    : os-default
createdDateTime            : 4/10/2021 9:59:13 AM
lastModifiedDateTime       : 4/10/2021 9:59:13 AM
enrollmentStatusScreenSettings :
extractHardwareHash        : True
deviceNameTemplate         : OSD-%RAND:5%
deviceType                 : windowsPc
enableWhiteGlove          : True
roleScopeTagIds            : {0}
outOfBoxExperienceSettings : @{hidePrivacySettings=True; hideEULA=True; userType=standard; deviceUsageType=singleUser; skipKeyboardSelectionPage=True; hideEscapeLink=True}
```

使用唯一的 Autopilot 配置文件 ID 调用命令时，你可以将其转换为在 Windows 设备映像处理过程中所需的 JSON 文件：

```
Get-Autopilotprofile -id 264c05b0-683c-4537-87ff-1ff5151d5b98 | ConvertTo-AutopilotconfigurationJSON
{
    "CloudAssignedDomainJoinMethod": 0,
    "CloudAssignedDeviceName": "OSD-%RAND:5%",
    "CloudAssignedAutopilotUpdateTimeout": 1800000,
    "CloudAssignedForcedEnrollment": 1,
    "Version": 2049,
    "CloudAssignedTenantId": "c56dd45b-1da6-4bd0-a53b-1466782d6ee5",
    "CloudAssignedAutopilotUpdateDisabled": 1,
    "ZtdCorrelationId": "264c05b0-683c-4537-87ff-1ff5151d5b98",
    "Comment_File": "Profile Intune Book",
    "CloudAssignedAadServerData": "{\"ZeroTouchConfig\":{\"CloudAssignedTenantUpn\":\"\",\"ForcedEnrollment\":1,\"CloudAssignedTenantDomain\":\"osddeployment.dk\"}}",
    "CloudAssignedOobeConfig": 1310,
    "CloudAssignedTenantDomain": "osdeployment.dk",
    "CloudAssignedLanguage": "os-default"
}
```

然后，你可以将 Autopilot 配置文件的内容复制到记事本，并将其保存为 `AutopilotConfigurationFile.json`。

Windows 设备可以通过 Configuration Manager 使用适用于现有设备的 Autopilot 进行注册时，通过关联 ID 进行分组。关联 ID 是 Autopilot 配置文件的一个参数。Azure AD 设备属性 `enrollmentProfileName` 会自动设置为 `OfflineAutopilotprofile-<correlator ID>`。因此，可以使用 `enrollmentProfileName` 属性，根据关联 ID 创建任意 Azure AD 动态组。

要创建 Windows Autopilot 配置文件，请执行以下步骤：

- 1. 在 Microsoft Endpoint Manager 管理中心（https://endpoint.microsoft.com/）中，选择“设备”|“Windows”|“Windows 注册”|“部署配置文件”。
- 2. 然后，选择“创建配置文件”|“Windows PC”。
- 3. 在“输入配置文件名称”字段中，输入“Autopilot 默认配置文件”。
- 4. 将“将所有目标设备转换为 Autopilot”设置为“否”。
  选择“是”以将所有目标设备注册到 Autopilot（如果尚未注册）。下次已注册设备经历 Windows OOBE 时，它们将按照分配的 Autopilot 场景进行。
- 5. 除非你有理由更改，否则将所有其他设置保留为默认值。

在 OOBE 步骤中，配置以下值，然后单击“下一步”：

| 设置 | 值 |
|---|---|
| 部署模式 | 用户驱动。 |
| 加入 Azure AD 为 | 已加入 Azure AD。 |
| Microsoft 软件许可条款 | 隐藏。 |
| 隐私设置 | 隐藏。 |
| 隐藏更改帐户选项 | 隐藏。 |
| 用户帐户类型 | 标准。 |
| 允许 White Glove OOBE | 否。 |
| 语言（区域） | 操作系统默认值。 |
| 自动配置键盘 | 是。 |
| 应用设备名称模板 | 你可以选择应用设备名称模板。使用一个有助于你通过此配置识别设备的前缀，例如：`OSD-%RAND:x%` |

> **重要说明**
> **语言（区域）** 需要在进行 OOBE 时具备以太网连接才能生效；否则，此 OOBE 向导将显示给最终用户。操作系统默认值仅在操作系统为单操作系统时才会生效，因此，例如，如果操作系统同时为 en-US 和 nl-NL，则不会显示此 OOBE 向导页面。
> 
> **自动配置键盘** 需要在进行 OOBE 时具备以太网连接才能生效；否则，此 OOBE 向导将显示给最终用户。

如果在 OOBE 阶段设备上存在 unattended.xml 文件，Windows Autopilot 很可能会失败，因此，如果你正在对现有设备进行映像部署，请确保这些文件不存在于 `%WINDIR%\Panther\Unattend unattended.xml` 和 `%WINDIR%\Panther\unattend.xml` 中。

## 在开箱体验 (OOBE) 期间的 Windows 更新

IT 管理员不能选择退出这些关键更新，因为它们是 Windows Autopilot 预配的一部分，设备需要这些更新才能正常运行。

在用户连接到网络后，关键驱动程序更新和关键 Windows **零日补丁 (ZDP)** 更新将在 OOBE 期间自动开始下载。此后，Autopilot 功能更新和关键更新也会自动下载并安装。

在此 OOBE 阶段，不会更新功能更新和质量更新。

Windows 将提醒用户设备正在检查和应用更新：

![](img/a3e9f26159d3739e434fe676b2987c1d_256_0.png)

图 7.13 – Windows 在 OOBE 期间检查更新

> **重要说明**
> 
> 如果你使用未集成任何驱动程序的自定义映像，Windows 可以在此过程中下载驱动程序，但可能会发生重新启动，从而中断 Windows Autopilot 体验。对于 Windows Autopilot，建议对新设备使用 OEM 映像，对现有设备使用带正确驱动程序的自定义映像；在测试适用于现有设备的 Windows Autopilot 时，也应如此，以获得最接近新设备的体验。

## 在 Intune 中自动分配 Windows Autopilot 配置文件

从 Microsoft Intune 将 Autopilot 配置文件分配给设备有两种方式：静态 Azure AD 组或动态 Azure AD 组。静态组无法提供你在企业中想要的自动化，因此在本节中，我们将指导你完成自动配置文件分配。

ZTDID 是 Windows Autopilot 服务中设备的唯一标识符。ZTDID 和组标签都是 Azure AD 设备对象上的属性。组标签是在将设备上传到 Windows Autopilot 时，你可以放入 CSV 中的值，并且这个值你可以自行选择。使用你创建的组标签，你可以利用组标签将 Autopilot 设备分组用于特定目的，例如信息工作者、共享设备、Microsoft Teams 会议室系统、自助服务终端设备或其他用途。如果你希望设备对象位于不同的 AD 动态设备组中，稍后可以更改 Autopilot 对象中的组标签。请记住在 Autopilot 设备边栏选项卡中单击“同步”；否则，你需要等待更改，直到后台运行同步。

你将拥有标准信息——设备序列号、Windows 产品 ID、硬件哈希——因此你只需创建一个名为 `OrderID` 的自定义列，并使用你想要用于创建 Autopilot 动态组以进行配置文件分配的值，例如 `EdgeKIOSK`。

将 Autopilot 信息导入 Microsoft Intune 后，你可以使用 Microsoft Graph Explorer 通过刚创建的信息查看设备。你需要查找 Azure AD 设备对象，因为 Microsoft Intune 设备对象是在 Windows 在 Microsoft Intune 中执行移动设备管理 (MDM) 注册时首次创建的。

转到 Graph Explorer：https://developer.microsoft.com/en-us/graph/graph-explorer：

![](img/a3e9f26159d3739e434fe676b2987c1d_257_0.png)

图 7.14 – Graph Explorer

