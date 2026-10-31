
## IPsec 安全策略管理单元

用于操作 IPsec 设置的原始控制台通过 MMC 访问。打开它，并添加 **IP Security Policy Management** 管理单元。有趣的是，在添加此管理单元时，你会注意到可以查看当前登录的机器的本地 IPsec 策略，也可以打开域的 IPsec 策略。如果你有兴趣配置域范围的 IPsec 实施，这里就是处理这些设置的工作区域。但如果只是想探头进来看看，你可以选择 **Local computer** 来查看控制台：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_411_0.png)

图 9.22：探索本地服务器的 IPsec 配置

进入后，你可以看到可能已存在的任何 IPsec 策略，或通过在 **IP Security Policies** 上右键单击时可用的 **Create IP Security Policy...** 操作开始创建自己的策略。执行此操作将启动一个向导，引导你完成自己的 IPsec 策略配置：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_411_1.png)

图 9.23：创建 IP 安全策略

## 改用 WFAS

用于建立 IPsec 连接规则的新平台是 Windows Defender 防火墙高级安全。请打开它，你已经熟悉操作了。进入后，导航到“连接安全规则”部分，该部分列在“入站规则”和“出站规则”正下方。连接安全规则是定义 IPsec 连接规则的位置。如果你在连接安全规则上右键单击并选择 New Rule...，然后你将通过一个与创建防火墙规则类似的向导：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_412_0.png)

图 9.24：WFAS 也可用于创建 IPsec 策略规则

进入创建新规则的向导后，你会发现可用的选项与创建新防火墙规则时显示的选项大不相同。这是建立 IPsec 连接安全规则的平台，这些规则定义 IPsec 隧道的外观，以及需要在哪些机器或 IP 地址上处于活动状态：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_413_0.png)

图 9.25：连接安全规则的类型

这里没有空间涵盖此向导中的所有可用选项，但我强烈建议从这里继续并通过以下 Microsoft Docs 文章进一步深入：https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831807(v=ws.11)。

## Azure AD 密码保护

如果你是 Azure Active Directory 客户，你已经可以访问这个名为 **Azure Active Directory Password Protection** 的新功能，前身称为 **banned passwords**。其思路是：Microsoft 维护一个持续更新的常见弱密码全球列表（例如单词 *password*）并自动阻止该密码的所有变体，例如 *P@ssword*、*Password123* 等。如果用户尝试将其创建为自己的密码，这些潜在密码将被完全阻止。你还可以在 Azure Active Directory 界面中添加自己的自定义禁止密码。一旦在 Azure 中启用了禁止密码，此功能也可以通过实施 Azure Active Directory Password Protection 代理服务移植到你的本地 Active Directory 环境中（哇，这名字太长了）。此代理在本地域控制器和 Azure Active Directory 之间进行接口，确保用户尝试在本地域控制器上设置的密码符合 Azure 禁止密码算法定义的规则。

要使用此技术，你当然必须使用 Azure Active Directory，因此并非人人适用。然而，如果你确实拥有并同步到 Azure Active Directory，那么此功能甚至可回溯到较旧版本的本地域控制器。这些服务器可以老到 Windows Server 2012。

这里有一个关于禁止密码的进一步信息的链接：https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-password-ban-bad-on-premises。

## 细粒度密码策略

正如我们在讨论域级密码策略时早已承诺的，我们在这里逐步介绍细粒度密码策略的构建。大多数组织确实要求用户具有特定的密码复杂度，但几乎总是通过默认域策略 GPO 实现，这意味着域内每个人的密码复杂度和过期设置完全相同。

如果你需要在某些用户账户上启用复杂度，而在其他账户上不启用怎么办？也许你有经常旅行的销售人员，要求他们使用非常强且复杂的密码是有意义的。但假设你还有一个机加工车间，用户每天必须登录计算机，但这些计算机从未离开办公室，并且用户从未在除这些物理安全设备之外的任何系统上输入其凭据。

对于那些机加工车间用户来说，真的有必要拥有与外出销售人员相同的密码复杂度级别吗？他们是否也应该被要求每 30 天更新一次密码，仅仅因为你也要求你的销售人员这样做？

从 Windows Server 2012 开始，登录任何域控制器服务器并启动服务器管理器，在“工具”列表的最顶部你会看到一个名为 **Active Directory Administrative Center (ADAC)** 的项目。此 ADAC 工具可用于操作 Active Directory 中的许多内容，但对我们今天的目的而言最重要的是，此新工具是使你能够创建 **fine-grained password policy (FGPP)** 的机制。FGPP 使我们刚才提到的场景成为可能，即创建多个密码策略并将这些策略分配给不同类别的用户和用户账户。

以域管理员身份登录域控制器并打开 ADAC。在左侧导航窗格中，展开内部域的名称，然后 **System | Password Settings Container**。在我的实验室环境中，我单击的是 **contoso (local) | System | Password Settings Container**。目前，密码设置容器为空，但使用 **New | Password Settings** 任务即可开始操作：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_415_0.png)

图 9.26：细粒度密码策略在 ADAC 内部配置

**Password Settings** 屏幕相当自解释。在这里，你定义与通常在默认域策略 GPO 中配置的相同类型的密码复杂度、长度和过期标准。你可以在图 9.27 中看到，我创建了一个名为 **Sales Users** 的密码设置策略，具有若干特定标准。

我能够配置密码设置以及账户锁定设置（另一种通常通过 GPO 全局完成的安全设置），并筛选此密码策略，使其仅适用于我的 AD 组 Sales Users 中的用户：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_416_0.png)

图 9.27：创建新的密码设置策略

现在我将再重复几次过程，之后你可以看到我的 ADAC 容器中有三个单独的密码设置策略：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_417_0.png)

图 9.28：创建多个密码设置策略

> 这里需要注意的重要一点是，每个密码设置策略都有一个 Precedence 声明。由于我们将这些密码设置应用于组，某个用户可能属于多个组，从而获得多个密码策略是完全可能的。Precedence 数字有助于 FGPP 区分应应用哪些设置。

就是这样！一旦你在 ADAC 中定义了细粒度密码策略，这些新的密码设置现在将优先于默认域策略 GPO 中的设置。而不是通过组策略推送密码设置，使用细粒度密码策略利用 Active Directory 内部自己的对象来完成工作。这些对象称为密码设置对象（PSOs）。如果你想更深入地了解 PSO 的内部情况，现在你在 ADAC 中创建了 FGPP 后，请转到 Active Directory 用户和计算机，并在“查看”菜单下启用高级功能。启用高级功能后，你可以导航到域 | 系统 | 密码设置容器，并查看一分钟前创建的新 PSOs。
