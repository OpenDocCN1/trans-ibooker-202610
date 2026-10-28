

following command. Mine, for example, is 1:

```
set session 1
```

这会将你放到后台的会话带回前台。

在 **msf6 exploit(windows/local/bypassuac_fodhelper) >** 提示符下，输入

```
exploit
```

这会在已被劫持的 Windows 10 系统上释放新的 exploit。

这一系列命令劫持了一个注册表项并插入一条命令，该命令将在 C:\Windows\System32\fodhelper.exe 运行 时执行。二进制文件 fodhelper.exe（fod 代表“Features on Demand”）在你从“应用和功能”设置窗口点击“可选功能”时启动“可选功能”窗口。你可以双击 C:\Windows\System32\fodhelper.exe 来查看它启动的窗口。此受信任的二进制文件在运行时不会显示 UAC 窗口，也不会在其他进程从其生成时显示 UAC 窗口。因此，fodhelper.exe 将被调用，并且基于注册表项条目的修改，当 fodhelper.exe 运行时，将以关闭的 UAC 标志生成第二个 shell。这为 Meterpreter 提供了所需的权限。注册表项的修改将在有效载荷运行后恢复，以使此更改为临时性的并防止被检测到。

+   c. 渗透测试人员和攻击者希望掩盖自己的痕迹，这在逻辑上是任何攻击的最后一步。在 Windows 10 VM 中，右键单击“开始”并选择“我的电脑”|“管理”|“系统工具”|“事件查看器”。展开“Windows 日志”并检查其中包含的“应用程序”、“安全”和“系统”日志条目的信息。

+   d. 在 Meterpreter 中，输入

```
clearev
```

现在它起作用了！Windows 10 VM 上的日志条目将会消失。

e. 再次查看事件查看器中的日志。差别很大！

你会注意到“安全”和“系统”日志都包含一条日志条目，表明这些日志已被清除，这会向系统管理员大声宣告发生了网络攻击。然而，具体发生了什么并不清楚；因此，这是一种很好的掩盖痕迹的方式。

6a–6f

## Step 6 命令行界面（CLI）访问很棒，但想象一下，如果你拥有图形用户界面（GUI）会怎样。你现在将通过远程桌面协议来了解。

a. 在 Meterpreter shell 中，输入

```
info post/windows/manage/enable_rdp
```

以获取有关 Windows 管理启用远程桌面模块的信息。

b. 使用以下命令运行它：

```
run post/windows/manage/enable_rdp username=hacker password=AAAbbb111
```

注意正在进行的各种后台操作，包括在必要时在本地防火墙中打开端口、添加用户 hacker（你可以用白帽黑客或黑帽黑客的心态来做这件事）并使用指定的密码、将用户添加到远程桌面用户组、将用户从 Windows 登录屏幕中隐藏起来，并将用户添加到管理员组。你还会注意到一个用于清理和掩盖痕迹的文件路径，该路径将在步骤 6e 中引用。

c. 输入以下命令（请注意，这是一个换行的命令；不要按 ENTER 来拆分此命令或本章其余的命令）：

```
reg setval -k 'HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -v UserAuthentication -d '0'
```

此命令转到注册表项（-k）并使用特定数据（-d）更改值（-v）。这是为了绕过“要求计算机使用网络级别身份验证进行连接”的设置所必需的。

远程桌面设置的高级设置中有一个复选框，默认情况下是选中的；这会阻止从 Kali Linux VM 访问远程桌面。此命令和注册表更改会覆盖该设置。

d. 运行命令
`idletime`

以了解当前是否有人员正在积极使用被利用的机器。此信息对于决定是否立即继续下一步或可能在一段时间后继续非常有用。

e. 在新的 Z shell 终端中，输入
`rdesktop -u hacker -p AAAbbb111 192.168.1.105`

但使用你的 Windows 10 VM 的 IP 地址代替。

如果你完成了第 14 章，你会看到使用 GPO 制作的登录警告。如果是这样，请单击“确定”按钮。

你会看到用户 hacker 的用户名以及以下消息：“另一用户已登录。如果继续，他们将被断开连接。你仍要登录吗？”这就是 **idletime** 命令派上用场的地方。如果你看到的 **idletime** 值表明用户可能不在机器前（例如，如果他们大约 10 分钟左右没有触摸键盘），现在是尝试的好时机。单击“是”按钮。

在 Windows 10 VM 上，你现在会看到以下消息（显示你的机器名称）：“你是否允许 WEISSMAN-CLIENT\hacker 连接到此机器？”单击“确定”立即断开你的会话，或单击“取消”保持连接。没有任何操作将在 30 秒内断开你的会话。假设用户不在其计算机前（基于 **idletime** 的输出），30 秒将过去，hacker 账户将被登录。现在你不受 Meterpreter shell 或 Windows 命令提示符的限制，可以自由地与受害机器交互！窃取 Windows 密码哈希以及许多其他操作现在变得轻而易举！

在你在被入侵机器上完成活动后，

从 Meterpreter 关闭远程桌面窗口并在 Meterpreter 中运行以下清理脚本以删除添加的账户并掩盖你的痕迹：

```
run multi_console_command -r <file listed in output after you completed Step 6b>
```

由于 Windows 10 对注册表的更改，最后一个操作将失败。别担心。

在第 11 章中，你使用 Mimikatz 破解了 Windows 密码哈希。此步骤说明了你如何进入系统以获取哈希值。想象一下，现在攻击者拥有完整的 GUI 访问权限，会有什么可能性！

f. 从 Meterpreter，要么重启，要么关闭 Windows 10 VM（是的，你可以从 Meterpreter 执行这些操作），分别使用 **reboot** 或 **shutdown**。

就这样！

⏱ 10 MINUTES

## Lab Exercise 22.04: Armitage

Armitage 是 Metasploit Framework 的 GUI 前端。你将在本实验练习中使用它来发现主机系统。此实验练习将产生的流量高峰检测也是事件的良好指示，这可能导致事件响应。

## Learning Objectives

在本实验练习中，你将为你的工具库增加另一项工具。完成本实验练习后，你将能够

- 使用 Armitage 发现主机系统
- 理解这在目标网络上将如何表现为事件
- 能够将此实验练习中的信息用于端口扫描，这是逻辑上的下一步

pane at the **msf6 >** prompt.

b. 在 Armitage 界面中，在顶部菜单中，选择 Hosts | Nmap Scan | Quick Scan (OS Detect) 以发现子网上的所有主机。你需要提供你的网络 ID 和子网掩码，在大多数情况下将是 192.168.1.0/24，如对话框中所示。仅扫描你有权控制且已获授权扫描的网络，如第 16 章所述。

输入信息后单击“确定”按钮。

扫描完成后，所有发现设备的列表，包括 IP 地址、端口信息以及在某些情况下的操作系统，将出现在右上窗格中。大量有价值的信息将出现在下窗格中，如图 22-2 所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_757_0.png)

FIGURE 22-2 Armitage 发现了 Weissman 家中的许多系统。

下一步的逻辑步骤将是一些端口扫描，使用不同类型的扫描，如第 16 章所述。

## Lab Analysis

- 1. 什么是事件？
- 2. 什么是事件响应？
- 3. 为什么 Metasploit Framework 实验练习对于理解事件响应很重要？

## Key Term Quiz

使用下面的术语列表来完成以下句子。

- exploit
- front end
- payload
- shell
- vulnerability

- 1. SMB 是 a(n) ________。
- 2. EternalBlue 是 a(n) ________。
- 3. windows/meterpreter/reverse_tcp 是 a(n) ________。
- 4. 在受损的 Windows 系统上运行原生 Windows 命令之前，必须运行 ________ 命令。
- 5. Armitage 是 Metasploit Framework 的 a(n) ________。

