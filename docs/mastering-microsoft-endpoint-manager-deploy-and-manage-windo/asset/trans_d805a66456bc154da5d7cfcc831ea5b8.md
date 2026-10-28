

![](img/a3e9f26159d3739e434fe676b2987c1d_555_0.png)

Figure 15.5 – Azure AD Connect – enable Hybrid Azure AD join

现在，让我们来看一下配置和维护 Universal Print 所需的权限。

## 委派打印机访问权限 – 自定义角色

如果您想要委派访问权限，例如，如果您需要使用除全局管理员之外的账户来配置 Universal Print 服务，那么要实现这一点，您需要为用户账户分配打印机管理员或打印机技术员权限。

请参阅以下屏幕截图以查看管理中心门户中的角色：

- Devices
  - Cloud device admin
  - Desktop Analytics admin
  - Intune admin
  - Printer admin
  - Printer tech

Figure 15.6 – Universal Print role-based access controls

有关增值解决方案简报和联系方式的更多信息，请访问这里：http://aka.ms/uppartners。

## 配置 Universal Print

让我们进入 Universal Print 的配置流程。在本节中，我们将解释您需要了解的有关购买许可证以及配置和为终端用户分配 Universal Print 打印机所需的一切。让我们开始：

- 1. 通过 portal.azure.com 进入 Azure 门户，并以全局管理员身份登录，或使用分配了打印机管理员/技术员角色的账户登录。
- 2. 搜索 Universal Print：

![](img/a3e9f26159d3739e434fe676b2987c1d_557_0.png)

Figure 15.7 – Universal Print – Azure portal

- 3. 是否出现此错误？请确保已为您分配 Universal Print 许可证。

![](img/a3e9f26159d3739e434fe676b2987c1d_557_1.png)

Figure 15.8 – Universal Print subscription prompt

- 4. 设置 Universal Print 连接器以连接到您的本地位置/打印机。

您自己的打印机无法直接与 Universal Print 连接，需要一个网桥/代理，这必须通过 Universal Print 连接器完成。

> **重要说明**
> 您也可以在其中一台旧版打印服务器上安装该软件以将其连接到 Universal Print 服务。

- 5. 在本地物理桌面上安装 Universal Print 连接器，即与您的打印机在同一网络上。

- 6. 在此处下载最新的 Universal Print 连接器：https://aka.ms/UPConnectorMSI。

- 7. 要安装 Universal Print 连接器，请点击 **Install**：

![](img/a3e9f26159d3739e434fe676b2987c1d_558_0.png)

Figure 15.9 – Universal Print – print connector

安装准备就绪后，点击 **Launch**：

![](img/a3e9f26159d3739e434fe676b2987c1d_558_1.png)

Figure 15.10 – Universal Print – print connector

8. 点击 **Ok** 确认诊断数据提示。

9. 点击 **Login**：

![](img/a3e9f26159d3739e434fe676b2987c1d_559_0.png)

Figure 15.11 – Universal Print – print connector

10. 使用您的用户 Azure AD 凭据登录。

**重要说明**
无需使用打印管理员/全局管理员账户。

11. 为连接器输入一个名称 – 连接器稍后将在 Universal Print 管理门户中显示为指定打印机(们)的代理：

![](img/a3e9f26159d3739e434fe676b2987c1d_560_0.png)

Figure 15.12 – Universal Print – print connector

连接器现在将注册到 Universal Print 服务。

12. 在连接器菜单中，您将看到连接器名称显示出来：

Figure 15.13 – Universal Print – print connector

**重要说明**

打印连接器将作为设备在 Azure Active Directory 中创建 – 作为具有对象 ID 的设备对象。

您可以在此处在 Azure 门户中看到一个示例：

Figure 15.14 – Azure AD – print connector

13. 您可以点击连接器名称查看该特定机器的一些详细信息及其状态：

![](img/a3e9f26159d3739e434fe676b2987c1d_562_0.png)

Figure 15.15 – Universal Print – print connector

配置连接器后，我们现在需要为混合 Azure AD 加入准备我们的域配置。

## 启用混合 AD 配置 – 通过 Universal Print 连接器

以下启用混合 AD 配置的步骤仅在终端为 Azure 混合 AD 加入时才需要。当您的物理终端已在 Azure AD 中处于活动状态时，您可以跳过以下步骤。

通过勾选 On 框在 Universal Print 连接器中激活混合 Azure AD：

![](img/a3e9f26159d3739e434fe676b2987c1d_563_0.png)

Figure 15.16 – Universal Print – print connector

## 使用 Universal Print 注册您自己的自定义打印机

我们已完成连接部分；现在我们需要将打印机作为 Universal Print 服务的一部分在云中可用：

- 1. 在可用打印机列表中选择打印机。点击注册：

> 重要说明
> 可用打印机列表是从您安装打印机连接器的桌面/服务器上连接的打印机检测到的。

![](img/a3e9f26159d3739e434fe676b2987c1d_564_0.png)

Figure 15.17 – Universal Print – print connector

2. 打印机已添加到云中。状态为 **in progress**。这需要一分钟左右的时间。

我们现在已完成所有预步骤。打印机现在都可见于 **Registered Printers** 列表中；不过，正如您所见，它们尚未共享。这就是我们现在要做的事情。

## 与用户共享您的打印机

让我们来看一下如何与终端用户共享您的打印机：

- 1. 进入 Universal Print 门户并打开 **Printers**。

> **重要说明**
> 如您所见，它们的状态为 **Not shared**。

- 2. 选择您想要共享的打印机(们)。点击 **Share**：

![](img/a3e9f26159d3739e434fe676b2987c1d_565_0.png)

Figure 15.18 – Universal Print – management console

现在我们已准备好打印共享/发布步骤，用户仍然无权查看和使用打印机。因此，我们需要将 Azure AD 组或用户直接添加到成员列表中。

## 分配使用打印机(们)的权限

在本节中，您将学习如何为终端用户分配正确的权限级别，以便他们可以打印：

> Tip
> 您知道也可以选择允许组织中所有人访问的选项以允许所有用户打印吗？

- 1. 选择您的 Azure AD 用户或组。准备就绪后，点击 Share Printer：

![](img/a3e9f26159d3739e434fe676b2987c1d_566_0.png)

Figure 15.19 – Universal Print – management console

- 2. 打印机现在已准备好在您的桌面上进行测试，因为状态为 Printer Shared：

![](img/a3e9f26159d3739e434fe676b2987c1d_566_1.png)

Figure 15.20 – Universal Print – management console

我们几乎准备就绪。让我们继续下一节，该节将解释如何测试您的打印机！

## 测试您的 Universal Print 连接打印机

在本节中，您将学习如何测试您的打印机以确认配置成功完成。成功后，您将成为一名正式的 Universal Print 专家：

- 1. 首先，您需要以 Azure AD 用户身份登录到您的 Windows 365 云或物理 PC 环境，该环境已分配到 Universal Print 中之前共享的打印机之一。

- 2. 在开始菜单中搜索打印机和扫描仪：

![](img/a3e9f26159d3739e434fe676b2987c1d_567_0.png)

Figure 15.21 – Printers & scanners – Windows 10

3. 点击添加打印机或扫描仪：

Printers & scanners

Add printers & scanners

Add a printer or scanner

Printers & scanners

Fax

Microsoft Print to PDF

Microsoft XPS Document Writer

Let Windows manage my default printer
When this is on, Windows will set your default printer to be the one you used most recently at your current location.

Download over metered connections
To help prevent extra charges, keep this off so device software (drivers, info, and apps) for new devices won't download while you're on metered Internet connections.

Troubleshoot your printer
If you can't get your printer to work, try running the printer troubleshooter.
Open troubleshooter settings

Related settings
Print server properties
Run the troubleshooter

Help from the web
Changing printer status
Downloading print drivers
Scanning documents

Get help
Give feedback

Figure 15.22 – Printers & scanners

