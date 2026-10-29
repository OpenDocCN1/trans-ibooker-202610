

![](img/d31345bbfd15f63be2920f0de5fa30dc_75_0.png)

Figure 3.11 – MAC address bytes

接下来，让我们讨论端口。

## Ports

虽然 MAC 地址唯一标识了 NIC，数据使用它来识别应该发送到的正确设备，但端口标识了 PC 上运行的唯一服务。它充当通信的逻辑端点。每个设备都有多个应用程序通过网络发送或接收数据。例如，你可以在 PC 上浏览网页，同时在后台运行下载，另一个服务正在向服务器上传数据。一旦数据到达你的 PC，它会使用端口来区分数据所属的不同进程。系统总共有 65,535 个端口。其中前 1,024 个中有一些是保留的，不建议使用这些端口。

## Protection

到目前为止，本章我们学习了计算机网络的基础知识，这对于理解本书的其余部分至关重要。现在，我们将开始研究可以用哪些参数来追踪我们，以及我们如何保护自己。在上一章中，我们了解到道德黑客的第一步是信息收集。但在开始收集信息之前，我们必须确保我们的身份得到保护。否则，我们很容易被追踪。你的身份可以通过许多参数被追溯回你。最常见的是你的 IP 地址和你的 MAC 地址。

要掩盖你的公共 IP 地址，你可以使用 **Virtual Private Networks (VPNs)**。我们不会讨论 VPN，因为它们不在本书的范围之内。这里需要注意的一点是，你不应完全信任你的 VPN 提供商。从安全角度来看，使用 VPN 只是意味着你将信任从你的 **internet service provider (ISP)** 转移到另一家提供 VPN 服务的公司。你在选择 VPN 时应非常谨慎，从网络安全角度来看，你应非常谨慎对待免费 VPN，因为其中很多捆绑了恶意软件或使用你的 PC 资源用于其他目的，例如比特币挖矿。一些 VPN 即使声称可以掩盖你的身份，也会泄露你的 **domain name server (DNS)**，即用于将网站名称映射到 IP 地址的服务器。

然而，当我们扫描本地网络时，我们可以通过 MAC 地址被追踪。在 *Chapter 2, Getting Started – Setting Up Your Lab Environment* 中，我们安装了两台虚拟机：Kali 和 Windows 10。Kali 机器将是我们攻击机。我们的机器有一个 NIC，用于与其他设备通信。该 NIC 有一个 MAC 地址。在 *Changing our MAC address* 部分，我们将尝试伪造一个 MAC 地址，以便在扫描时更改它。通过这样做，即使 **intrusion detection system (IDS)** 发现我们在扫描 PC 上的端口，它也找不到我们的真实 MAC 地址。

## Changing our MAC address

在本节中，我们将尝试在 Kali 机器上更改 MAC 地址。让我们启动 Kali 机器并打开一个终端。要更改 MAC 地址，你需要安装 net-tools 包。在大多数 Linux 发行版中，该工具已经可用。但是，如果未安装，你可以使用以下命令安装它：

```
sudo apt-get update -y
```

```
sudo apt-get install -y net-tools
```

它会提示你输入密码，即 kali。安装工具后，你可以使用以下命令查看 MAC 地址：

```
sudo ifconfig
```

如果一切顺利，你将看到类似以下的输出：

![](img/d31345bbfd15f63be2920f0de5fa30dc_77_0.png)

Figure 3.12 – ifconfig command output

这里有很多内容需要解析，让我们逐步分解。这里有两个值，称为 eth0 和 lo。eth0 是 NIC 的名称，而 lo 是回环适配器。现在我们可以忽略回环适配器。inet 字段代表 Kali 机器的私有 IP 地址。inet6 是 Kali 机器的 IPv6 地址。ether 是 MAC 地址，这就是我们想要更改的字段。

如果你想更改 MAC 地址，不能在 NIC 开启时进行。首先，你必须关闭网络接口。要关闭接口，你可以使用以下命令：

```
sudo ifconfig eth0 down
```

此命令将关闭名为 eth0 的 NIC。如果命令输出中没有错误，这意味着命令成功运行。

现在，如果你再次输入 ifconfig 命令，你将看到以下输出：

![](img/d31345bbfd15f63be2920f0de5fa30dc_77_1.png)

Figure 3.13 – Shutting down a network interface card

现在，你将只看到回环适配器，并且 eth0 已被关闭。要更改 MAC 地址，你可以运行以下命令。假设你希望新的 MAC 地址为 00:11:22:33:44:55。这里，你可以执行以下操作：

```
sudo ifconfig eth0 hw ether 00:11:22:33:44:55
```

此命令更改 eth0 接口和该 NIC 的 ether 参数：

![](img/d31345bbfd15f63be2920f0de5fa30dc_78_0.png)

Figure 3.14 – Changing MAC address

现在，如果没有错误，这意味着命令成功运行。此时，我们可以通过运行以下命令再次打开接口：

```
sudo ifconfig eth0 up
```

现在，让我们再次运行 ifconfig 命令，看看我们的更改是否生效：

![](img/d31345bbfd15f63be2920f0de5fa30dc_78_1.png)

Figure 3.15 – Changed MAC address

在这里，你可以看到 MAC 地址已成功更改。现在，如果我们想在网络中扫描某物，将显示此 MAC 地址而不是我们的真实 MAC 地址。

## Creating a Python script

到目前为止，我们已经编写了手动命令来更改 MAC。理想情况下，我们希望编写一个 Python 脚本来帮助我们更改它。为此，我们需要找到一种方法借助 Python 运行 bash 命令。幸运的是，Python 有一个标准库用于运行系统命令，称为 subprocess。该库允许你与底层操作系统交互。

要将此库导入你的模块，你只需编写以下命令：

```
import subprocess
```

要运行命令，subprocess 有一个名为 run 的方法。使用此方法，你可以在操作系统上执行系统命令。如果你想查看 eth0 的信息，你可以运行以下命令：

```
subprocess.run(
    ["ifconfig", "eth0"],
    shell=True,
)
```

此函数需要一个命令列表。另一个参数 shell=true 意味着我们希望将输出打印到控制台。

如果你运行前一个文件，你将看到类似于运行 ifconfig eth0 命令的输出。请注意，你需要以 root 用户身份运行命令，因此它应如下所示：

```
sudo python3 main.py
```

以下是输出：

![](img/d31345bbfd15f63be2920f0de5fa30dc_80_0.png)

Figure 3.16 – Running system commands using Python

现在你知道了如何使用 Python 运行系统命令，你可以使用 Python 重复前面的命令。完整代码如下：

```
import subprocess

if __name__ == "__main__":
    interface = "eth0"
    new_mac = "22:11:22:33:44:57"

    print("Shutting down the interface")
    subprocess.run(["ifconfig", "eth0", "down"])

    print("changing the interface hw address of ", interface, " to ", new_mac)
    subprocess.run(["ifconfig", interface, "hw", "ether", new_mac])
    print("MAC address changed to ", new_mac)
    subprocess.run(["ifconfig", interface, "up"])

    print("network interfaced turned on")
```

如果你再次检查接口，你将能够看到新的 MAC 地址：

![](img/d31345bbfd15f63be2920f0de5fa30dc_81_0.png)

Figure 3.17 – New MAC address

现在我们已经学习了如何在系统上运行命令以及如何使用 Python 更改系统的 MAC 地址，我们将在此停止讨论。在下一章中，我们将研究信息收集。

## Summary

在本章中，我们学习了网络的基础知识，以及我们如何通过伪造 MAC 地址来保护自己在本地网络上的扫描目的。本章帮助我们更深入地了解计算机系统的网络方面，以及我们如何使用 Python 来保护和掩盖我们在本地网络中的身份。在下一章中，我们将学习如何扫描本地网络。

