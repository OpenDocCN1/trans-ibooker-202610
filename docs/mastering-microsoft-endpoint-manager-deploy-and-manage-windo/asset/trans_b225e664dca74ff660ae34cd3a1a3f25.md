

## Office 云策略服务

Microsoft 365 Apps 管理中心（https://config.office.com）提供对 Microsoft 365 企业版应用的基于云的策略管理。从 **Office 云策略 (OCP)** 服务部署的策略将适用于受管设备和未受管设备，因此基本上适用于贵组织 Office 365 用户登录的所有设备，包括公司设备和个人设备。

当用户在设备上登录 Microsoft 365 Apps for enterprise 时，策略设置会漫游到该设备。您还可以强制实施适用于 Office 网页版、iOS、macOS 和 Android 的某些策略设置。

Office 云策略服务包含许多与 Group Policy 或 Microsoft Intune 中可用的基于用户的策略设置相同的设置。OCP 服务与来自 **组策略对象 (GPO)** 或 Microsoft Intune 的策略设置的冲突处理方式为 Microsoft Intune 或 GPO 始终获胜。用户可以是具有冲突策略设置多个 Azure Active Directory 组 的成员。通过在 OCP 上为 Office 策略设置优先级，您可以控制应用哪个策略设置。优先级最高者将被应用，其中 0 是可在 OCP 服务中分配给策略的最高优先级。

使用 Office 云策略服务的条件如下：

- 至少为 Microsoft 365 Apps for enterprise（以及 Project 和 Visio 桌面应用的订阅版本）的 1808 版本。
- 用户帐户必须在 **Azure Active Directory (AAD)** 中创建或同步。用户必须使用基于 AAD 的帐户登录 Office 365 ProPlus。
- 安全组必须在 **AAD** 中创建或同步，并将适当的用户添加到这些组中。

基于云的策略管理的优点如下：

- 它适用于所有设备 – 受管和未受管设备，均适用于 Microsoft Intune 和 SCCM。
- 它适用于 BYOD 和个人设备。
- 设置始终是最新的，并且由于它是来自 Microsoft Office 团队的服务，因此无需下载更新的 Administrative Template 文件（ADMX/ADML）。

## 使用 OCP 服务创建策略配置

以下是从 MEM 管理中心创建策略配置的基本步骤。

要创建策略配置，您必须在 AAD 中被分配以下角色之一：全局管理员、安全管理员或 Office 应用管理员。Intune 服务管理员无权创建 OCP 策略。

- 1. 转到 **Home | Apps | Policies for Office apps.**

如果这是您第一次查看 Office 云策略服务，您将没有任何策略配置。

点击 **Create** 创建您的第一个策略配置。

![](img/a3e9f26159d3739e434fe676b2987c1d_428_0.png)

图 11.2 – Policies for Office apps

2. 在 Name 字段中输入 Office policy，然后点击 Select type 以展开到以下屏幕截图中显示的视图。选择 This policy configuration applies to users。

选择此选项时，您可以从 AAD 中选择目标用户组。如果您选择 This policy configuration applies to users that access documents anonymously using Office web apps，它将应用于 Office Online，如果文档是匿名访问的。IT 管理员始终可以设置外部共享，以便访客用户必须登录或提供验证码：

![](img/a3e9f26159d3739e434fe676b2987c1d_429_0.png)

图 11.3 – Create policy configuration – Select type

3. 对于 Select group 选项，选择 Office 365 Users（自定义组）。

然后点击 Select group 以展开到以下屏幕截图中显示的视图。您可以选择 AAD 安全组，但一个安全组只能分配一个 OCP 服务策略：

![](img/a3e9f26159d3739e434fe676b2987c1d_430_0.png)

图 11.4 – Create policy configuration – Select group

- 4. 如果已将 OCP 分配给 AAD 组，它将在 UI 中变灰，因为您只能在 Azure AD 中为每个组分配一个 Office 云策略：

![](img/a3e9f26159d3739e434fe676b2987c1d_430_1.png)

图 11.5 – Select the group for this policy configuration

## 配置策略

截至目前，Office 云策略服务中共有 2,133 个不同的策略设置。

配置要包含在策略配置中的策略设置。您可以搜索策略设置名称以查找要配置的策略设置。您还可以按应用程序筛选，按策略是否为推荐的安全基线，以及策略是否已配置进行筛选。Platform 列指示策略是应用于 Windows 设备的 Microsoft 365 Apps for enterprise、Office for the web，还是两者都适用。

![](img/a3e9f26159d3739e434fe676b2987c1d_431_0.png)

图 11.6 – Create policy configuration

在本例中，我们将配置所有 Microsoft 365 企业版应用的默认文件格式：

- 1. 搜索 `Default file format`。
- 2. 选择 Word 的 **Default file format**。
- 3. 从 **Not configured** 更改为 **Enabled**。

![](img/a3e9f26159d3739e434fe676b2987c1d_432_0.png)

对其他 Microsoft 365 企业版应用执行相同的操作。

点击 Create。

![](img/a3e9f26159d3739e434fe676b2987c1d_433_0.png)

图 11.8 – Create policy configuration

至此，关于 Microsoft Endpoint Manager 中 Office 云策略的部分结束。我们向您展示了如何创建可部署到组织中所有终端的 Office 策略。在下一节中，我们将向您展示 OCP 服务中的一些技巧。

如果您输入 Word，您将只看到适用于 Word 的策略：

![](img/a3e9f26159d3739e434fe676b2987c1d_435_0.png)

图 11.10 – Filters on Application

如果您选择 Security Baseline，您将只看到适用于 Windows 的策略；其他操作系统没有安全基线：

![](img/a3e9f26159d3739e434fe676b2987c1d_436_0.png)

图 11.11 – Filters on Recommendations

当您选择安全基线中的策略设置时，您只需选择 Microsoft recommended security baseline 即可将策略配置为正确的值：

![](img/a3e9f26159d3739e434fe676b2987c1d_436_1.png)

图 11.12 – Microsoft recommended security baseline

如果您勾选 **Configured**，您将只看到策略中已配置的策略：

图 11.13 – Filters by Status

如果您使用 **Configured** 作为筛选器，您可以看到所有已配置的策略设置：

图 11.14 – Filter – Configured

> Note
> 如果您仅在设备上安装 Microsoft Teams，则不会安装 Click-to-Run 服务，并且 Office 云策略不会应用于设备。

可以通过转到 File | Account | Switch account 在 Office 应用程序中切换帐户：

![](img/a3e9f26159d3739e434fe676b2987c1d_439_0.png)

图 11.16 – Office – Account

Microsoft Intune ADMX 策略或 GPO 用户策略可以位于本地注册表中，

```
HKCU\software\policies\Microsoft\Office\16.0\
```

![](img/a3e9f26159d3739e434fe676b2987c1d_439_1.png)

图 11.17 – Registry path for Office ADMX

来自 GPO 或 Microsoft Intune ADMX 策略的许多策略都是按设备设置的。在本例中也是如此，这也是只有一个策略值显示的原因。

从 Office 云策略服务检索的策略设置存储在注册表中的 HKEY_CURRENT_USER\Software\Policies\Microsoft\Cloud\Office\16.0。此键在每次在签入过程中从策略服务检索新一组策略时都会被覆盖。

![](img/a3e9f26159d3739e434fe676b2987c1d_440_0.png)

图 11.18 – Registry path for Office cloud policy

策略服务签入活动存储在注册表中的 HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\CloudPolicy。删除此键并重新启动 Office 应用将在下次启动 Word 或 Excel 等 Office 应用时触发策略服务签入：

![](img/a3e9f26159d3739e434fe676b2987c1d_440_1.png)

图 11.19 – Registry path for Office cloud policy check-in

至此，关于使用 Office 云策略服务配置策略的部分结束。在下一节中，我们将介绍 Apps 管理中心中的 Security Policy Advisor 如何帮助您改进在 Office 云策略服务中配置的策略的安全设置。

