
离开主屏幕，虽然它很有用，尝试点击屏幕左侧列出的其中一个工具，例如 **事件**。如果没有 WAC，如果你想在 Server Core 上排查问题，查看该服务器上的 Windows 事件日志是合理的，但你将如何从命令行界面执行此操作？我猜你可以登录到 Server Core 控制台并使用 **任务管理器** 启动 EventVwr，但打开 WAC 并简单点击 **事件** 要容易得多：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_453_0.png)

图 10.27：系统事件日志

WAC 内部其他有用的功能示例，尤其是在使用 Server Core 实例时，就是使用 **文件和文件共享** 来浏览 WEB4 硬盘的文件和文件夹结构，或使用此处的 **防火墙** 功能在 WEB4 上创建或删除 Windows 防火墙规则。还有一个 **网络** 工具，可用于操作 IP 地址配置。

虽然 Windows Admin Center 内部还有许多其他工具，但我想指出的最后一个是，我们再次拥有 PowerShell 选项（类似于我们可以在 Server Manager 内部启动的选项）。此 PowerShell 按钮将为我们调用并显示到 WEB4 Server Core 实例的远程 PowerShell 连接，如果我们找不到 WAC 内部所需的函数并且需要深入一点以从命令行界面完成某项操作。最好的部分是，你实际上从未启动过 PowerShell！所有这些仍在你的互联网浏览器窗口内进行：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_454_0.png)

图 10.28：在 WAC 中使用 PowerShell

在 Windows Admin Center 内部还可以完成更多操作。编辑注册表、添加角色和功能、检查服务的状态，甚至与 Windows Update 交互。如果你还没有使用 WAC，你就错过了！

## Sconfig 实用工具

现在我们将退一步，查看一个在 Server Core 内部可用的工具，但它通常仅在你在服务器控制台上工作时才有用。正如你所见，每次启动 Server Core 时，你都会进入一个命令提示符窗口，从那里可以切换到 PowerShell，然后使用传统的 Windows cmdlet 来配置你的新 Server Core 实例。

或者，你可以使用 Sconfig 实用工具。这是一组工具，类似于命令行快捷方式，用于实现将新服务器上线并连接到网络所需的基本项目。Sconfig 的目的是在安装操作系统后成为第 1 步，处理新服务器上的初始配置，以便你随后可以转到使用更强大的管理界面，例如 Server Manager 或 Windows Admin Center。

在启动 Server Core 实例后，你会发现自己处于命令提示符，等待输入。在此屏幕中，只需键入 Sconfig 并按 Enter。你应该会看到从黑色快速变为蓝色并看到以下屏幕：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_455_0.png)

图 10.29：服务器配置实用工具

Sconfig 内部可用的选项相当自解释，但我们将介绍在此执行的常见任务。同样，这些都是你可以通过 PowerShell cmdlet 完成的事情，但我发现使用 Sconfig 方法更容易。此界面的最常见用途是通过按 8 来配置初始网络设置，或通过使用选项 2 和 1 来配置服务器主机名和域成员身份。

我将继续按键盘上的 2 然后按 Enter，我立即被提示指定新的计算机名称。这是配置新 Server Core 服务器主机名的极快方式。由于我实际上不想重命名 WEB4，我将保持选择为空并按 Enter 返回主屏幕。

现在我想查看网络设置。按 8 然后按 Enter 会将我带入网络设置，在那里我可以看到 WEB4 的 NIC 上的当前 IP 地址是 10.10.10.12。这是对的，但让我们为了演示一个实际的 Sconfig 设置更改而更改该地址。

我首先选择了我的网络适配器索引，即编号为一。现在我可以看到有关此 NIC 上已配置内容的附加信息，并且有选项可以更改此信息。再次选择选项一将允许我设置网络适配器地址：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_456_0.png)

图 10.30：使用 Sconfig 配置 IP 地址

输入字母 S，这告诉 Sconfig 你想要输入静态 IP 地址，然后输入你希望在此 NIC 上配置的新 IP 地址。我将把 WEB4 改为 10.10.10.30，只是为了证明这有效。输入 IP 地址后，我还必须定义新的子网掩码和网关地址：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_457_0.png)

图 10.31：设置子网掩码和默认网关

WEB4 的 NIC 已立即更新为新的 IP 地址 10.10.10.30，如结果输出所示。虽然在 Server Core 实例的初始配置后访问 Sconfig 工具可能不是常见情况，但此工具在用于任何新 Server Core 实例的网络和命名设置的初始配置时可以节省大量时间。

## Server Core 中可用的角色

Server Core 显然是操作系统的受限形式，Windows Server 中的某些角色根本不是为在这种有限环境中正常工作而设计的。幸运的是，我们大多数角色都是可以的，这使得 Server 2019 管理员能够通过更安全的 Server Core 平台部署其大部分关键基础设施。以下是当前支持在 Windows Server 2019 Server Core 实例上运行的角色列表，我用粗体标记了我所在业务中最常使用的角色：

- Active Directory 证书服务
- **Active Directory 域服务**
- Active Directory 联合服务
- Active Directory 轻型目录服务
- Active Directory 权利管理服务
- 设备健康证明
- **DHCP 服务器**
- **DNS 服务器**
- **文件和存储服务**
- 主机守护服务
- **Hyper-V**
- 打印和文档服务
- 远程访问
- 远程桌面服务*
- 卷激活服务
- **Web 服务器 (IIS)**
- Windows Server 更新服务

你会注意到我在前面的列表中在远程桌面服务旁边放置了一个 *。虽然你可以在 Server Core 上安装 RDS 角色，但你不能使用 Server Core 实例执行远程桌面的所有功能。你可以使用 Server Core 作为你的 RDS 连接代理或 RDS 许可服务器，但不能将 Server Core 用作远程桌面会话主机。其他角色也存在类似的限制。

要获取始终更新的、关于哪些角色和功能的哪些部分和组件在 Server Core 上受支持的全面列表，请查看以下 Microsoft 文档：https://docs.microsoft.com/en-us/windows-server/administration/server-core/server-core-roles-and-services.

## 构建 Server Core 域控制器

你现在已具备启动 Server Core 实例、为其配置主机名、IP地址和域成员身份，并使用各种管理工具集管理这些新服务器的能力。我在生产环境中看到 Server Core 最频繁的用途之一是作为辅助域控制器。让我们逐步介绍设置过程，以便在这种情况适用于你时拥有确切的步骤。

首先准备新服务器。你已经知道所有这些内容，但以下是你在考虑将此服务器转变为域控制器之前想要完成的基本步骤概述：

- 1. 启动新服务器并安装 Server Core。
- 2. 使用控制台上的 Sconfig 或 PowerShell 配置静态 IP 地址和永久主机名。请记住，一旦你将此服务器转变为域控制器，不支持更改名称，并且更改 IP 也不容易——因此要明智选择。我将其命名为 DC3，因为我的实验室中已有 DC1 和 DC2。
  - 确保指定现有域控制器作为 NIC 的 DNS 服务器地址。
- 3. 使用 Sconfig 或 PowerShell 将此新服务器加入到现有域。
- 4. 此时，你的服务器已在线并可在网络上通信，但未安装任何角色。你可以从这里继续安装任何角色以用于任何目的。

## 安装 AD DS 角色

可以通过多种方式完成此步骤。使用 Windows Admin Center 或通过另一台机器上的 Server Manager 远程操作新服务器都是安装新角色的有效方法。你应该已经熟悉这些流程，基于我们迄今为止在本章中涵盖的内容。

相反，让我们坚持使用纯 PowerShell 将此新 Server Core 转换为无头域控制器。从 DC3 的控制台，确保你已登录到域管理员帐户。然后，使用 powershell 命令将我的命令提示符更改为 PowerShell 提示符。然后，运行以下命令将 AD DS 角色安装到新服务器上：

```
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```
