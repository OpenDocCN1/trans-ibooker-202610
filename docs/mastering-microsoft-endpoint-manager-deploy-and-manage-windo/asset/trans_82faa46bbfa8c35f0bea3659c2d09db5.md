

如果在 Windows Autopilot 过程中出现任何错误，最终用户可以通过点击 **view diagnostics** 查看更多详细信息，如以下屏幕截图所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_611_0.png)

图 16.34 – Windows Autopilot 失败

这将启动适用于 Windows 11 的新的 **Windows Autopilot diagnostics** 页面，目前该页面仅适用于用户驱动的 Windows Autopilot 部署模式。

最终用户随后会看到清晰的图标显示如下等状态：

- **Task failed**
- **Task in progress**
- **Task unknown or incomplete**
- **Task successfully completed**

在 **Enrollment Status** 下，您可以看到已完成的各项检查，例如 **Trusted Platform Module (TPM)** 证明和 MDM 注册，如以下屏幕截图所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_612_0.png)

图 16.35 – Windows Autopilot 注册状态

借助 **Windows Autopilot diagnostics** 概览，您可以展开 **General** 和 **Hardware** 以获取有关设备的更多信息。您还可以深入 **Apps and policies status** 以获取有关此区域不同错误的更多信息，如以下屏幕截图所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_613_0.png)

图 16.36 – Windows Autopilot diagnostics 配置信息

您可以在以下屏幕截图中看到应用失败的不同任务消息示例：

![](img/a3e9f26159d3739e434fe676b2987c1d_613_1.png)

图 16.37 – Windows Autopilot 应用失败消息

您可以在此 **Windows Autopilot diagnostics** 屏幕中看到，LOB MSI 的安装不完整，并且 Win32 应用出现错误。一旦获得应用的 **identifier (ID)**，您就可以返回 Microsoft Intune 并从应用侧继续进行故障排除。

在下一节中，我们将介绍如何确保您的设备已启用位置服务，以便在设备丢失时找到您的 Windows 设备。

## Troubleshooting locating a Windows device

要在地图上获取丢失或被盗的 Windows 设备的当前位置，请在 Microsoft Intune 中使用 **Locate device** 操作。

支持的 Windows 版本如下：

- Windows 10 version 20H2 (10.0.19042.789) 或更高版本
- Windows 10 version 2004 (10.0.19041.789) 或更高版本
- Windows 10 version 1909 (10.0.18363.1350) 或更高版本
- Windows 10 version 1809 (10.0.17763.1728) 或更高版本

Windows **Location** 服务默认处于关闭状态，如以下屏幕截图所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_614_0.png)

图 16.38 – 位置服务已关闭

如果用户希望使用此功能，则需要将其打开，或创建一项策略来打开位置服务。为此，请执行以下步骤：

- 1. 在设置目录中创建策略以在 Windows 设备上启用位置服务，如以下屏幕截图所示：

Home > Devices > Windows >
Create device configuration profile
Windows 10 and later - Settings catalog (preview)
1 Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create
Name * Enable Location Service
Description
Platform Windows 10 and later

图 16.39 – 启用位置服务配置文件

- 2. 搜索 privacy 并选择 Let Apps Access Location，如以下屏幕截图所示：

Settings picker
Use commas "," among search terms to lookup settings by their keywords
privacy Search
+ Add filter
Browse by category
Administrative Templates\Windows Components\Internet Explorer\Privacy
Administrative Templates\Windows Components\Windows Media Player\User Interface
Privacy
99 results in the "Privacy" category Select all these settings
Setting name
Let Apps Access Location
Let Apps Access Location Force Allow These Apps
Let Apps Access Location Force Deny These Apps
Let Apps Access Location User in Control Of These Apps
Let Apps Access Messaging
...

图 16.40 – 允许应用访问位置

3. 将设置更改为 Force allow，如以下屏幕截图所示：

Home > Windows >

# Create device configuration profile

Windows 10 and later - Settings catalog (preview)

Basics Configuration settings Assignments Scope tags Review + create

+ Add settings

Privacy Remove category

98 of 99 settings in this category are not configured

Let Apps Access Location Force allow.

图 16.41 – 强制允许

4. 将策略部署到设备后，您将看到以下消息：Some of these settings are hidden or managed by your organization。在这种情况下，Location for this device is on 字段会被激活，最终用户无法将其关闭，因为它已通过 Microsoft Intune 作为策略进行配置。这在以下屏幕截图中有所说明：

Settings Home Find a setting Privacy Windows permissions General Speech Inking & typing personalization Diagnostics & feedback Activity history App permissions Location Camera

# Location

*Some of these settings are hidden or managed by your organization.

# Allow access to location on this device

If you allow access, you will enable Windows to use your device's capabilities to determine your location and Microsoft will use your location data to improve location services. People using this device will be able to choose if their apps have access to location by using the settings on this page. Denying access blocks Windows from providing location to Windows features, Microsoft Store apps, and most desktop apps.

Location for this device is on Change

# Allow apps to access your location

If you allow access, you can use the settings on this page to choose which apps can access your device's precise location and location history to enable location-based experiences such as directions and weather. If you are signed in with a Microsoft account on this device, your last known location is saved to the cloud, and shared with other devices where you are signed in with your Microsoft account. Denying access only blocks the apps listed on this page from accessing your location.

On

图 16.42 – 位置服务已开启

## Troubleshooting Microsoft Edge

Microsoft 已发布基于 Chromium 的新版 Microsoft Edge。此新版本提供与扩展和网站的最佳兼容性。这也意味着有新的故障排除方式以及可管理 Edge 浏览器的新策略。

让我们先来看看浏览器内置的策略故障排除工具。通过在地址栏中输入 Edge : //policy/，您可以访问策略页面，如以下屏幕截图所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_618_0.png)

图 16.45 – Edge 策略

您可以看到所有策略设置以及应用于 Edge 浏览器的每个设置的策略值。

要检查浏览器是否已更新到最新版本，您可以在 Edge 浏览器的地址栏中输入 edge : // settings/help。这将带您进入以下屏幕：

## Summary

在本章中，您了解了在配置 MEM 期间可能出现的绝大多数部署错误。

阅读本章后获得的知识将帮助您在试用过程中排查一些常见错误，并使您成为维护企业生产工作负载所需的专家！

在下一章中，我们将学习有关 Windows 365 的故障排除。

## Questions

- 1. Microsoft Support 是否包含在您的 Microsoft Intune 订阅中？
  a) Yes
  b) No
- 2. 从 Microsoft Intune 工作时，是否需要在 Windows 上启用 Windows 位置服务？
  a) Yes
  b) No

## Answers

- 1. (A)
- 2. (A)

## Further reading

如果您在阅读本章后想了解更多信息，请使用以下免费在线资源之一：

- *Troubleshoot app installation issues—Intune | Microsoft Docs*: https://docs.microsoft.com/en-us/troubleshoot/mem/intune/troubleshoot-app-install#:~:text=Intune%20provides%20app%20troubleshooting%20details%20based%20on%20the,Select%20user%20to%20select%20a%20user%20to%20troubleshoot
- *Troubleshooting packaging, deployment, and query of Windows apps—Win32 apps | Microsoft Docs*: https://docs.microsoft.com/en-us/windows/win32/appxpkg/troubleshooting

