
![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_439_0.png)

Figure 10.8: Displaying your interface list
图 10.8：显示您的接口列表

现在我们知道了接口编号，让我们构建将为网卡分配新的 IP 地址设置的命令。我将使用一个命令来分配 IP 地址、子网掩码前缀和默认网关。我将使用第二个命令来分配 DNS 服务器地址：

```
New-NetIPAddress -InterfaceIndex 4 -IPAddress 10.10.10.12 -PrefixLength 24 -DefaultGateway 10.10.10.1

Set-DNSClientServerAddress -InterfaceIndex 4 -ServerAddresses 10.10.10.10,10.10.10.11
```

Figure 10.9 shows us the resulting output:
图 10.9 显示了结果输出：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_439_1.png)

Figure 10.9: Assigning the IP and DNS server addresses
图 10.9：分配 IP 和 DNS 服务器地址

> Hold the phone! How did I get two PowerShell prompts open at the same time within the Server Core interface? Make sure to read the Accidentally closing Command Prompt section later in this chapter to discover how you can launch multiple windows and tools inside the Server Core console.
> 暂停一下！我如何在 Server Core 界面中同时打开两个 PowerShell 提示符？请务必阅读本章后面的“意外关闭命令提示符”部分，了解如何在 Server Core 控制台中启动多个窗口和工具。

现在，所有这些 IP 设置都应该已应用到网卡上。让我们使用图 10.10 中所示的 Get-NetIPConfiguration 命令再次确认。或者，您也可以使用老式的 ipconfig 来检查这些设置，但那样有什么乐趣呢？

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_440_0.png)

Figure 10.10: Checking the IP settings
图 10.10：检查 IP 设置

> Remember, you can always utilize DHCP reservations to make this a little bit easier. If you were to run a simple ipconfig /all from your Server Core and jot down the MAC address of your NIC, you could use this address to create a reservation in DHCP and assign a specific IP address to the new server that way.
> 请记住，您始终可以使用 DHCP 保留来让此过程稍微简单一些。如果您从 Server Core 运行简单的 ipconfig /all 并记下网卡的 MAC 地址，您就可以使用该地址在 DHCP 中创建保留，并以这种方式为新服务器分配特定的 IP 地址。

## Setting the server hostname
## 设置服务器主机名

现在我们已经建立了网络连接，下一步是设置服务器的主机名并将其加入域。首先，让我们先查看服务器当前的名称，并将其更改为符合我们标准的内容。当您全新安装 Windows 时，它会为服务器自动分配一个随机主机名。您只需键入 hostname 并按 Enter 即可查看当前主机名。

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_441_0.png)

Figure 10.11: Checking the current hostname
图 10.11：检查当前主机名

要更改服务器的主机名，我们需要使用 PowerShell。如果尚未进入，请打开 PowerShell 提示符，我们只需使用 Rename-Computer cmdlet 来设置新的主机名即可。我决定将这台新服务器命名为 WEB4，因为稍后我们将安装 Web Services 角色并在其上托管网站。请记住，更改计算机名称后，就像在 Windows Server 的 GUI 版本中一样，需要系统重启才能使更改生效。因此，在执行 Rename-Computer 命令后，您可以发出 Restart-Computer 命令来重新启动虚拟机：

```
Rename-Computer WEB4

Restart-Computer
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_441_1.png)

Figure 10.12: Changing the hostname and restarting your system
图 10.12：更改主机名并重启系统

## Joining your domain
## 加入您的域

下一步逻辑上当然是加入域。这些是我们会对环境中任何新服务器执行的标准功能，但由于我们完全通过命令提示符和 PowerShell 界面执行，因此您可能从未遇到过这种方式。要将 Server Core 加入域，请进入 PowerShell，然后使用 Add-Computer cmdlet。您将被要求指定域名称和用于加入域的凭据——这与将 Windows Server 2019 Desktop Experience 模式加入域时需要指定的信息相同。首先，您必须指定用于完成此域加入的凭据：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_442_0.png)

Figure 10.13: Entering credentials authorized to join this system to your domain
图 10.13：输入有权将此系统加入域的凭据

然后告诉它您想要加入的域：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_443_0.png)

Figure 10.14: Specifying the domain to join
图 10.14：指定要加入的域

或者，您也可以使用 -DomainName 参数结合原始的 Add-Computer cmdlet，在原始命令中指定域的名称。当然，加入域后，您需要再次执行 Restart-Computer 以完成此更改。

## Remote PowerShell
## 远程 PowerShell

一旦新服务器已分配 IP 地址、命名并加入域，我们就可以开始对这个新的 Server Core 实例进行真正的管理。您当然可以继续登录并直接通过控制台进行交互，但与管理环境中的任何其他服务器一样，必须有远程处理的方式，对吧？在不坐在服务器前的情况下操作 Server Core 的方法之一是使用远程 PowerShell 连接。

我们将在第 11 章《PowerShell》中更详细地介绍使用远程 PowerShell 操作服务器（GUI 和无头）的过程，但这里先展示在域加入环境中的工作站 PowerShell 提示符下实现远程会话时所需的命令和可用功能。

从另一台系统打开 PowerShell——这可以是服务器甚至是客户端操作系统。此 PowerShell 窗口显然在您当前登录的机器的上下文中打开，通过 PowerShell 发出的任何命令都会从本地系统获得响应。为了将 PowerShell 连接到 WEB4 Server Core 实例，我将发出以下命令。运行后，我会被提示输入对应管理员帐户的密码，然后就可以针对我们的 Server Core 发出远程 PowerShell 命令：

```
Enter-PSSession -ComputerName WEB4 -Credential administrator
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_444_0.png)

Figure 10.15: Using Enter-PSSession to remotely connect to WEB4
图 10.15：使用 Enter-PSSession 远程连接到 WEB4

现在我们坐在 PowerShell 提示符下，已远程连接到 WEB4 Server Core 机器。您可以通过提示符左侧列出的 [WEB4] 看到这一点。也许您不信任这个小标识符，并想确保此 PowerShell 窗口现在正在访问和操作远程 WEB4 服务器？让我们发出几个快速命令，例如 hostname 和 ipconfig，以证明在此 PowerShell 会话中提供给我们的信息确实来自新的 WEB4 服务器：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_444_1.png)

Figure 10.16: PowerShell now responds as if I was logged in to WEB4
图 10.16：PowerShell 现在响应就好像我已登录到 WEB4

现在我们已与这个新的 Server Core 建立了远程 PowerShell 连接，我们可以从此控制台对该服务器执行几乎任何操作。在此远程 PowerShell 会话中发出的命令和 cmdlet 将操作 WEB4，而不是我们的本地工作站。

## Server Manager
## 服务器管理器

虽然服务器的初始配置在一定程度上将通过控制台可用的命令行界面处理，但一旦服务器已在网络上建立，您可能会发现拓宽视野更有优势。您可能可以找到允许您管理并操作新服务器中任何内容的 PowerShell cmdlet，但这对我们大多数人来说仍然是一种比较新的思维方式——我们通常更习惯于使用图形工具，如服务器管理器。您已经知道服务器管理器可用于管理多个本地和远程服务器，并且是 Microsoft 集中管理拼图的一部分。我们在书中前面探索的服务器管理器中的远程管理功能不仅允许您访问基于 GUI 的 Windows 服务器，还允许访问 Server Core 实例。

我想在新的 WEB4 服务器上安装一个角色。我可以在服务器控制台上使用 PowerShell 来完成，但不如尝试将 WEB4 添加到服务器管理器中，而服务器管理器正在我的另一台服务器上运行。我将登录到 WEB3 并从那里使用服务器管理器。就像我们已经看到的那样，我可以使用“管理”菜单并选择“添加服务器”将新服务器添加到服务器管理器中：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_445_0.png)

Figure 10.17: Adding new servers to be managed inside Server Manager
图 10.17：在服务器管理器中添加要管理的服务器
