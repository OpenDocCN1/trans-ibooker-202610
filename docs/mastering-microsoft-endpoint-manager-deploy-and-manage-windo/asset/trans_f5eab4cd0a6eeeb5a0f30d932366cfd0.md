

- DiagnosticLogCSP_Collector_DeviceEnrollment.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_1_31_20_4_38.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_2_11_4_18_29.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_2_24_19_11_54.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_3_13_3_31_33.etl
- IntuneManagementExtension-20210228-125401.log
- IntuneManagementExtension-20210319-102847.log
- IntuneManagementExtension.log
- LicensingDiag.cab
- LicensingDiag_Output.txt
- MdmDiagLogMetadata.json
- MdmDiagReport_RegistryDump.reg
- MdmLogCollectorFootPrint.txt
- microsoft-windows-aad-operational.evtx
- microsoft-windows-appxdeploymentserver-operational.evtx
- microsoft-windows-assignedaccess-admin.evtx
- microsoft-windows-assignedaccess-operational.evtx
- microsoft-windows-assignedaccessbroker-admin.evtx
- microsoft-windows-assignedaccessbroker-operational.evtx
- microsoft-windows-crypto-ncrypt-operational.evtx
- microsoft-windows-devicemanagement-enterprise-diagnostics-provider-admin.evtx
- microsoft-windows-devicemanagement-enterprise-diagnostics-provider-debug.evtx
- microsoft-windows-devicemanagement-enterprise-diagnostics-provider-operational.evtx
- microsoft-windows-moderndeployment-diagnostics-provider-autopilot.evtx
- microsoft-windows-moderndeployment-diagnostics-provider-managementservice.evtx
- microsoft-windows-provisioning-diagnostics-provider-admin.evtx
- microsoft-windows-provisioning-diagnostics-provider-autopilot.evtx
- microsoft-windows-provisioning-diagnostics-provider-managementservice.evtx
- microsoft-windows-shell-core-operational.evtx
- microsoft-windows-user device registration-admin.evtx
- Sensor-20210105-105450.log
- Sensor-20210207-191747.log
- Sensor.log
- setupact.log
- TpmHliInfo_Output.txt
- _IntuneManagementExtension.log

从 MDM 诊断工具中获得的最重要的信息如下：

```
Registry Dump
MdmDiagReport_RegistryDump.reg captures the
HKEY_LOCAL_MACHINE
HKEY_CURRENT_USER
Registry values associated with autopilot device provisioning are written to HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Provisioning\Diagnostics\AutoPilot

HKEY_CURRENT_USER\software\microsoft\enterprisemodernappmanagement and HKEY_LOCAL_MACHINE\software\microsoft\enterprisemodernappmanagement
```

在 Microsoft Endpoint Manager 管理中心进行报告的情况下，流程如下。

在 Microsoft Endpoint Manager 管理中心，https://endpoint.microsoft.com/，选择 设备 | 监视 | Autopilot 部署（预览）。

您可以查看最近 30 天的 Windows Autopilot 部署：

![](img/a3e9f26159d3739e434fe676b2987c1d_265_0.png)

图 7.18 – Autopilot 报告

您可以查看以下属性：

- **注册日期**
- **注册方法**
- **序列号**
- **设备名称**
- **用户**
- **Autopilot 配置文件**
- **注册状态页部署状态**
- **部署总时间**

## Company Portal

Company Portal 是面向终端用户的自助服务工具，以便他们可以安装 IT 管理员已为终端用户提供的应用。

对于 Windows 10 Autopilot 部署的设备，建议您将 Microsoft Store for Business 账户与 Intune 关联。Company Portal 应用将在分配给 Autopilot 组时以设备上下文安装，并在用户登录前安装到设备上。您可以使用以下步骤选择安装 Company Portal（离线）应用：

- 1. 在 Microsoft Endpoint Manager 管理中心，`https://endpoint.microsoft.com/`，选择 **应用 | Windows**。
- 2. 从 Windows 应用列表中，选择 **Company Portal**（离线）。
- 3. 将 Company Portal 作为必需应用分配给您选择的 Autopilot 设备组。
- 4. 由于这是一个离线应用，请确保将 **许可证类型**更改为 **设备许可**。

## 为 Autopilot 设备配置自动 BitLocker 加密

使用 Windows Autopilot，可以在自动加密开始前配置 BitLocker 加密设置。此配置可确保默认加密算法不会自动应用。在自动 BitLocker 加密开始前也可以应用其他 BitLocker 策略。

BitLocker 自动设备加密使用 BitLocker 驱动器加密技术，在用户完成 *Modern Standby* 或符合 **硬件安全可测试性规范 (HSTI)** 的硬件上的 OOBE 后自动加密内部驱动器。

在 Modern Standby 要求或 HSTI 验证的情况下，此要求通过以下其中一种方式满足：

- 已实施 Modern Standby 要求。这些要求包括 **统一可扩展固件接口 (UEFI)** 安全启动要求和防止未经授权的 **直接内存访问 (DMA)** 保护。
- 从 *Windows 10 版本 1703* 开始，此要求可通过 HSTI 测试满足。
- 平台安全启动自检（或在注册表中配置的其他自检）必须由 HSTI 报告为已实施并通过。
- 除 Thunderbolt 外，HSTI 必须不报告任何不允许的 DMA 总线。
- 如果存在 Thunderbolt，HSTI 必须报告 Thunderbolt 已安全配置（安全级别必须为 SL1 – *用户授权* 或更高）。

*Windows 10（Modern Standby）* 扩展了 *Windows 8.1 Connected Standby 电源模型*。Connected Standby，进而 Modern Standby，使能类似智能手机电源模型的即时开启/即时关闭用户体验。就像手机一样，S0 低功耗空闲模型使系统在低功耗模式下仍可连接到网络。

通过利用 `powercfg /a` 命令，您可以查看设备是否支持 Modern Standby（S0 低功耗空闲）：

![](img/a3e9f26159d3739e434fe676b2987c1d_267_0.png)

图 7.19 – powercfg

## 重要说明

BitLocker 自动设备加密在 OOBE 体验期间开始。但是，BitLocker 驱动器保护仅在用户使用 AAD 账户登录后启用。在此之前，保护处于暂停状态，数据未受保护。

BitLocker 加密算法在 BitLocker 首次启用时使用。该算法为全卷加密设置强度。可用加密算法为 AES-CBC 128 位、AES-CBC 256 位、XTS-AES 128 位和 XTS-AES 256 位加密。BitLocker 将使用默认加密方法 XTS-AES 128 位或任何设置脚本指定的加密方法。

要确保在 Autopilot 设备的自动加密发生前设置您想要的 BitLocker 加密算法，请确保满足以下要求：

- 设备包含 **受信任平台模块 (TPM)**，TPM 1.2 或 TPM 2.0。
- 已启用 UEFI 安全启动。
- 已启用平台安全启动。
- 已启用 DMA 保护。
- 在 Windows 10 Endpoint Protection 配置文件中配置 **加密方法** 设置为您想要的加密算法。
- 将策略分配给您的 Autopilot 设备组。加密策略必须分配给组中的设备，而不是用户。
- 为这些设备启用 Autopilot ESP。如果未启用 ESP，则策略不会在加密开始前应用。

## 重要说明

当您启用 EncryptionMethodByDriveType 时，必须为所有三个驱动器（操作系统、固定数据和可移动数据）指定值；否则将失败（500 返回状态）。例如，如果您仅为操作系统和可移动驱动器设置加密方法，您将收到 500 返回状态。

按如下方式在 Intune 中创建用于自动 BitLocker 加密的 BitLocker 策略：

- 1. 在 Microsoft Endpoint Manager 管理中心，`https://endpoint.microsoft.com/`，选择 **设备 | Windows | 配置配置文件**。
- 2. 选择 **创建配置文件**。
- 3. 选择 **平台 | Windows 10 及更高版本**。
- 4. 选择 **配置文件类型 | 模板 | 模板名称 | Endpoint Protection**。
- 5. 选择 **名称 | Autopilot Bitlocker | 下一步**。
- 6. 展开 **Windows 加密**。
- 7. 选择 **Windows 设置 | 加密设备 | 要求**。

![](img/a3e9f26159d3739e434fe676b2987c1d_269_0.png)

BitLocker 基础设置应如下所示：

- 1. 选择 **BitLocker 基础设置 | 其他磁盘加密的警告 | 阻止**。

  选择 **阻止** 将禁用 BitLocker 配置提示和其他磁盘加密的警告提示。阻止是自动 BitLocker 加密设备的需要。
- 2. 选择 **允许标准用户在 Azure AD 加入期间启用加密 | 允许**。

  将此设置设为 **允许** 时，即使用户不是本地管理员，也可让终端用户自动加密设备。
- 3. 对于 **配置加密方法**，选择您的组织所需的内容。Windows 默认值为 **XTS-AES 128 位**，这也是 Windows 安全基线中推荐的值：

![](img/a3e9f26159d3739e434fe676b2987c1d_269_1.png)

