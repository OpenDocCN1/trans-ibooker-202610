
如果还希望此服务器响应 DNS 查询（大多数域控制器都会这样做），请使用以下命令安装 DNS 角色：

```
Install-WindowsFeature DNS
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_460_0.png)

图 10.32：在 Server Core 上安装 AD DS 和 DNS 角色

## 将此服务器提升为域控制器

安装了角色后，我们现在可以准备将此服务器提升为域控制器。旧的 dcpromo 命令仍然存在，可以用来执行此功能，当然我们也可以利用其中一个图形化管理工具远程处理。尽管如此，由于我们从 PowerShell 开始，我们将使用相同的方式完成整个过程：

```
Install-ADDSDomainController -InstallDns -DomainName "contoso.local"
```

（当然，请将 contoso.local 替换为你自己的域名。）

执行此命令时，系统会要求你输入必要的信息，例如 AD 安全模式密码以及具有创建新域控制器权限的管理员凭据：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_461_0.png)

图 10.33：创建你的第一个 Server Core DC

完成后，服务器将自动重启，你的新的 Server Core 现在已成为环境中的无头域控制器！

## 验证是否成功

我知道你完全信任我，但就为了争论一下，如果我们想验证那些简单的 PowerShell 命令是否真的做到了它们所说的那样怎么办？让我们通过几种不同的方法验证 DC3 是否确实是一个域控制器。

环境中的所有域控制器都会自动将其计算机对象移动到 Active Directory 内部的“Domain Controllers”OU 中。登录到网络中的任何 DC，打开 Active Directory 用户和计算机，并导航到 **Domain Controllers** OU。你可以在 *图 10.34* 中看到 DC3 现在位于此容器内，表明它是我的域的 DC：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_462_0.png)

图 10.34：成功创建的 DC3

为了验证 DNS 方面，你现在可以从网络内部打开 DNS 管理控制台，并进入主要内部 DNS 区域的 **Properties**。就我而言，那就是 contoso.local。打开这些属性后，我可以看到我的 **Name Servers** 选项卡现在显示 DC3 为该区域的名称服务器：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_462_1.png)

图 10.35：DC3 已确认的 DNS 服务器

最后一次测试和验证。让我们使用一个查询并显示我们环境中域控制器服务器之间复制状态的命令。在任何 DC 上打开命令提示符或 PowerShell 并运行以下命令：

```
Repadmin /showrepl
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_463_0.png)

图 10.36：验证 DC3 连接

就是这样！三种不同的证明表明我们的 DC3 Server Core 实例确实是我们域中的域控制器和 DNS 服务器。多酷啊！

## Nano Server 发生了什么？

关于小占用 Windows Server 平台的故事过去并不以 Server Core 结束。任何关注 Server 2016 新功能的人都知道，Server 2016 操作系统还有另一种安装选项，称为 Nano Server。Nano Server 的前提是一个更小、更安全、更高效、超迷你的操作系统，可以运行有限的角色集。尽管有限，但它仍然能够安装在物理或虚拟服务器平台上，作为真正的服务器操作系统运行，并且仍可在其上托管传统工作负载。

不幸的是，对于 Nano Server 爱好者，尤其是已经投入工作安装和使用它的人来说，过去几年 Nano Server 背后的故事完全发生了翻转。长话短说，你现在不能再使用 Nano Server 来做传统服务器能做的任何事情。你不能将其安装在物理硬件上；你甚至不能在 VM 上安装 Nano。此外，诸如 PowerShell 和 WinRM 之类的管理功能已被从 Nano Server 中移除，并且你无法在其上安装任何 Microsoft 基础结构角色。

在所有这些功能都被从 Nano Server 范围中移除后，剩下了什么？它死了吗？Nano Server 还能做 ANYTHING 吗？

答案是容器。如果你有兴趣利用容器来构建和托管云就绪且可扩展的应用程序，这就是 Nano 现在的重点。我们将在第 13 章《容器和 Nano Server》中介绍有关容器以及 Nano Server 完全与其绑定事实的更多信息，但就说下载容器镜像从 Microsoft 将现在是你找到 Nano Server 的唯一地方。

## Summary

我必须诚实地告诉你——编写这一章正是我需要的那一脚，让我开始思考如何缩小自己的服务器。我和你们中的许多人一样：我知道 Server Core 是什么，并且玩过它，但从未采取步骤在支持的环境中真正使用它。现在有了 Sconfig 和新的 Windows Admin Center 等工具，我已经正式没有借口不将新角色部署到 Server Core 机器上了。事实上，当我构建新的 DC3 服务器时，我意识到与完整的图形界面相比，启动一个 Server Core 实例要快得多。重启更快，使用 Sconfig 设置 IP 地址、主机名和域成员资格总共只花了我大约 2 分钟。

虽然学习新东西永远不会有害，但使用 Server Core 不再要求你必须精通 PowerShell。Server Core 的早期日子要求你非常擅长 PowerShell，因为这是配置和与你的迷你服务器交互的唯一可靠方式，但这些新工具使我们能够利用更小的平台并在不记住一堆新 cmdlet 的情况下管理它。

安全性是我们所有人都应该考虑 Server Core 作为新标准的主要原因。Windows 图形界面添加了大量代码，并授予登录服务器的人员大量能力，例如浏览互联网的能力。这打开了许多 Server Core 中根本不存在的漏洞之门。

## Questions

- 1. True or False — Server Core is the default installation option for Windows Server 2019.
- 2. True or False — You can utilize PowerShell to change a Server 2019 from Server Core mode to Desktop Experience mode.
- 3. When sitting at the console of a freshly booted Windows Server 2019 Server Core instance, what application do you see on the screen?
- 4. What cmdlet can be used to view the current networking configuration on a Server Core?
- 5. Which PowerShell cmdlet can be used to configure the hostname of a Server Core?
- 6. Name some of the management tools that can be used to remotely interface with a Server Core.
- 7. What is the name of the utility built into Server Core that can be launched to provide quick task links for configuring IP addresses, hostnames, and domain membership?

## 11 PowerShell

让我们诚实一点，我们中的许多人每天仍在使用命令提示符。如果你已经切换并使用更新的 PowerShell 提示符作为命令提示符的完全替代品，我赞扬你！我自己仍然习惯性地打开 cmd.exe，尽管随着 Windows 10 和 Windows Server 2019 的最新版本，我确实更加有意识地努力使用更新的、更蓝的、更漂亮的、更强大的 PowerShell 界面。在本章中，我们将探讨一些你也应该这样做的理由。除了微软似乎缩小了命令提示符的默认文本大小以阻止我们使用它，我觉得这很有趣之外，我们将看看 PowerShell 远比命令提示符强大和有用的技术原因。

在本章中，我们将涵盖以下主题：

- Why move to PowerShell?
- Working within PowerShell
- Using a pipeline
- PowerShell Integrated Scripting Environment
- Remotely managing a server
- Desired State Configuration

