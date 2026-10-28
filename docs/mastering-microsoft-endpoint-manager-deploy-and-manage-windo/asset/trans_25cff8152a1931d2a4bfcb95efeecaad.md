

+   2. 在 MEM 中，用于更改品牌和徽标（作为组织的一部分）的菜单选项叫什么？
+      A. Tenant admin
+      B. Customization
+      C. Tenant administration

+   3. 使用 Windows 365 和 Azure Virtual Desktop 所需的 Azure AD 配置应是什么？
+      A. Azure AD
+      B. Azure AD Domain Services
+      C. Azure AD hybrid-joined

## Answers

+   1. (C)
+   2. (B)
+   3. (C)

## Further reading

如果您在阅读本章后想了解更多关于 MEM 要求的信息，请使用以下免费在线资源：

+   - Network endpoints for Microsoft Intune | Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/fundamentals/intune-endpoints
+   - OSes and browsers supported by Microsoft Intune | Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/fundamentals/supported-devices-browsers
+   - Intune operated by 21Vianet in China | Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/fundamentals/china
+   - Network requirements and bandwidth details for Microsoft Intune | Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/fundamentals/network-bandwidth-use

## 6

### Windows Deployment and Management

在本章中，您将清楚地了解如何使用 Microsoft Endpoint Manager 的 Intune 为不同的云 PC 和物理 PC Microsoft Endpoint Manager 管理的终端场景部署和更新 Windows 10 Enterprise。您将了解不同的部署方法和可用于在企业级别提供 Windows 更新和管理 的其他功能。

+   在本章中，我们将介绍以下主题：
+   
+   - 将现有 Windows 设备部署到 Microsoft Endpoint Manager
+   - 那本地设备呢？
+     - Co-management
+     - Tenant attach
+   - Microsoft Surface 和其他 **Original Equipment Manufacturer (OEM)** 设备
+   - Windows Update for Business
+   - Windows 10 和 Windows 11 更新通道

## Deploying existing Windows devices into Microsoft Endpoint Manager

*此场景仅适用于物理 Windows 终端。*

在当今的企业公司中，正常的做法是利用来自 Microsoft Endpoint Manager Configuration Manager 或 Microsoft Deployment Toolkit 的操作系统部署。

Microsoft Deployment Toolkit 更简单，因为它只需要访问一个共享位置，在那里存储 Windows 10 操作系统驱动程序应用程序。

Configuration Manager 和 Deployment Toolkit 通常要求设备位于本地以加入企业 **Active Directory (AD)**，并访问 **Preboot eXecution Environment (PXE)** 服务器才能开始操作系统部署。

在过去的几十年里，企业公司开始删除 OEM 为使设备以最佳方式运行 Windows 10、驱动程序以及在全新设备上实现最佳性能所需的一系列设置所做的工作。这是通过利用 *wipe and load* 映像概念完成的，并且只有公司批准安装在设备上的内容才会被安装。通常，这将是旧版本的 Windows、旧版本的驱动程序和 Microsoft Enterprise 应用。

如果能更简单地从任何地方完成此操作就好了。随着 Covid-19 的爆发，几乎所有人都开始在家工作，幸运的是，现代技术让我们能够摆脱物理位置的束缚，转向随时随地工作。这也包括将全新设备配置到最终用户的家庭地址，而无需最终用户去办公室领取新设备。今天，这可以通过现代配置来实现，称为 **Windows Autopilot**。我们将在本章中对 Windows Autopilot 进行高层次介绍，并在下一章中通过真实案例进行更深入的介绍。

Windows Autopilot 是一组不同的技术组件，它们汇聚成一项服务，用于开箱即用地配置新设备，使其准备好投入生产使用。Autopilot 的好处是，您也可以在需要重新开始时使用它来重置、重新利用和从头恢复设备。所有这些都是为了让 IT 部门摆脱这些工作；无需管理基础设施，因为 Autopilot 作为 Microsoft Endpoint Manager 内的云服务运行，因此简单易用，可以更快地提高生产力！

## Enrolling devices – Windows enrollment

在本节中，我们将了解 Windows 10 PC 可以通过用户或管理员以多种不同方式注册到 Intune 的方法，您可以在管理中心门户中在 **Devices | Enroll devices | Windows enrollment** 下找到。

> **Important Note**
> 
> Windows 365 Cloud PCs 会自动注册到 Microsoft Endpoint Manager 的 Intune。

## Automatic enrollment

本节介绍配置 Windows 设备在加入或注册到 **Azure Active Directory (AAD)** 时进行注册。我们已在上一章 *Chapter 5, Requirements for Microsoft Endpoint Manager* 中介绍过此内容，因此请返回该章节以了解更多信息。

您在此处看到 Windows enrollment 仪表板，这是配置该部分大多数元素的起点：

![](img/a3e9f26159d3739e434fe676b2987c1d_199_0.png)

Figure 6.1 – Windows enrollment

## Testing company domain CNAME registration for Windows enrollment

在 DNS 中配置 CNAME 可以让您的用户在注册 Windows 设备时无需输入 **Mobile Device Management (MDM)** 服务器的地址。

在 DNS 中配置 CNAME 资源记录后，输入相应的域以确认其已正确配置。

DNS 记录的更改可能需要长达 72 小时才能传播。您可以在以下屏幕截图中找到使此操作成为可能的配置选项：

![](img/a3e9f26159d3739e434fe676b2987c1d_200_0.png)

Figure 6.2 – CNAME validation setup

让我们现在切换到 **Enrollment Status Page** 屏幕，了解有关您注册到 Microsoft Endpoint Manager 的结果和状态的更多信息。

## Enrollment Status Page

**Enrollment Status Page** 屏幕显示新设备注册后以及新用户登录设备时的配置进度。这使 IT 管理员可以选择性地阻止（阻止）设备访问，直到其完全配置完成，同时为用户提供有关配置过程中剩余任务的信息。

默认注册状态页适用于所有用户和所有设备，但您可以创建多个具有不同配置的注册状态页配置文件，以满足您的场景需求。

这些配置文件按优先级顺序指定；将使用适用的最高优先级。每个注册状态页配置文件都可以针对包含设备或用户的组。当确定使用哪个配置文件时，将遵循以下标准：

+   - 首先使用针对设备的最高优先级配置文件。
+   - 如果没有针对设备的配置文件，则将使用针对当前用户的最高优先级配置文件。（这仅适用于有用户的场景。在白手套和自部署场景中，只能使用设备定位。）
+   - 如果没有针对特定组的配置文件，则将使用默认注册状态页配置文件。

注册状态页不仅适用于 Windows Autopilot 设备，还适用于分配给用户或设备的、注册到 Microsoft Intune 的所有设备。

我们将在下一章中深入介绍 Windows Autopilot，包括如何设置它以及如何为不同场景配置注册状态页。

在以下屏幕截图中，您可以看到如何在 Enrollment Status Page 屏幕上捕获设备：

![](img/a3e9f26159d3739e434fe676b2987c1d_201_0.png)

Figure 6.3 – Enrollment Status Page screen

