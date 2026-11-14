# 第 29 章 远程处理

## 29.0 简介

PowerShell 对本地和交互式计算机自动化的支持使其成为计算机管理和行政管理的一个非常有吸引力的平台。其丰富的面向对象视角将即使是最简单的管理任务也提升到了一个新的水平。

虽然 PowerShell 支持与传统远程技术（`SSH`、`FTP`、`Telnet`、`PsExec` 等）交互，但这种支持与其他任何 Shell 提供的支持相当。毫不奇怪，PowerShell 的远程管理真正大放异彩的地方在于其独特的基于对象的方法。

当然，任何提供对系统远程访问的功能都应谨慎对待。安全性是任何支持网络连接的技术自然关注的问题，PowerShell 远程处理非常重视这一点。此外，整个企业范围内对远程无人值守管理的普遍支持是任何理智的服务器平台提供的核心价值。Windows Server 如何确保两者兼得？从 PowerShell 3 版本开始，PowerShell 远程处理默认针对大多数常见远程管理场景启用：

*   在桌面计算机上（即 Windows 10 客户端），PowerShell 默认不监听网络连接，必须显式激活。
*   在不受信任的网络上（即接受来自互联网连接的服务器），PowerShell 仅监听源自同一子网的网络连接。同一子网上的计算机通常连接到同一物理网络。
*   在受信任的网络上（即显式标记为受信任的域或网络接口），PowerShell 默认监听网络连接。

有了这些保护措施，PowerShell 远程处理提供了一种稳健、可靠且安全的方式来远程管理计算机。

从标准的*交互式远程处理*开始，PowerShell 让你轻松连接到远程系统并与其一对一地工作：

```
PS > Enter-PSSession Lee-Desk
[Lee-Desk]: PS > Get-Process -n PowerShell

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id Process   PSComputer
                                                          Name      Name
-------  ------    -----      ----- -----   ------     -- --------- ---------
    628      17    39084      58908   214     4.26   7540 powers... lee-des...
```

如果你想从远程系统导入命令（但仍让它们在远程系统上运行），*隐式远程处理*通常让你完全忘记你在管理远程系统。在交互式和隐式远程处理的基础上，大规模*扇出*远程处理是自然的下一步。扇出远程处理让你一次性以批量、基于命令的方式管理许多计算机：

```
PS > Invoke-Command Lee-Desk,Lee-Desk { Get-Process -n PowerShell } -Cred Lee

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id Process   PSComputer
                                                          Name      Name
-------  ------    -----      ----- -----   ------     -- --------- ---------
    628      17    39084      58908   214     4.26   7540 powers... lee-des...
    628      17    39084      58908   214     4.26   7540 powers... lee-des...
```

与 PowerShell 的其他部分一样，扇出远程处理提供了一种独特的、面向对象的处理方式，将其体验提升到了基于纯文本的方法之上。

## 29.1 查找支持自身远程处理的命令

### 问题

你想要查找允许访问远程计算机但不需要 PowerShell 远程处理的命令。

### 解决方案

使用 `Get-Command` cmdlet 的 `-ParameterName` 参数来查找所有公开 `-ComputerName` 参数的命令：

```
PS > Get-Command -CommandType Cmdlet -ParameterName ComputerName

CommandType     Name                          Definition
-----------     ----                          ----------
Cmdlet          Clear-EventLog                Clear-EventLog [-LogName]...
Cmdlet          Connect-WSMan                 Connect-WSMan [[-Computer...
Cmdlet          Disconnect-WSMan              Disconnect-WSMan [[-Compu...
Cmdlet          Enter-PSSession               Enter-PSSession [-Compute...
Cmdlet          Get-Counter                   Get-Counter [[-Counter] <...
Cmdlet          Get-EventLog                  Get-EventLog [-LogName] <...
Cmdlet          Get-HotFix                    Get-HotFix [[-Id] <String...
Cmdlet          Get-Process                   Get-Process [[-Name] <Str...
(...)
```

### 讨论

虽然 PowerShell 远程处理提供了强大的功能和一致性，但有时你可能需要针对未安装 PowerShell 的系统调用命令。简单的远程桌面会话是常见方法，但 PowerShell 仍提供了许多独立于其核心远程支持工作的远程管理选项。

`Get-Command` 输出中显示的每个公开 `-ComputerName` 参数的命令都使用其自己的内置远程技术。CIM cmdlet 使用基于 Web Services for Management (`WSMAN`) 的 CIM 专用形式的远程处理。`WSMan` cmdlet 使用基于 SOAP 的远程处理。许多其他 cmdlet 提供基于远程过程调用 (`RPC`) 的远程处理。

通过基于其自身现有的远程协议构建，这些命令可以轻松与已启用 WMI 或事件日志管理等的环境集成。由于这些协议旨在仅处理其特定技术，因此它们通常也能提供性能优势。

尽管有这些优势，但提供 `-ComputerName` 参数的命令在大多数情况下无法替代通用的远程技术。因为每个命令都基于自己的协议，使用该命令意味着要管理防火墙规则、服务等。基于命令的远程处理通常也提供有限的功能，甚至像备用凭据这样简单的功能也很少支持。



有关启用 PowerShell 远程处理的详细信息，请参阅 配方 29.2。

## 另请参阅

配方 29.2

# 29.2 在计算机上启用 PowerShell 远程处理

## 问题

你想通过 PowerShell 远程处理允许对计算机进行远程管理。

## 解决方案

使用 `Enable-PSRemoting` cmdlet 启用 PowerShell 远程处理：

```
PS > Enable-PSRemoting

WinRM Quick Configuration
Running command "Set-WSManQuickConfig" to enable this machine for remote
management through WinRM service.
 This includes:
    1. Starting or restarting (if already started) the WinRM service
    2. Setting the WinRM service type to autostart
    3. Creating a listener to accept requests on any IP address
    4. Enabling firewall exception for WS-Management traffic (for http only).

Do you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"): Y

WinRM has been updated to receive requests.
WinRM service type changed successfully.
WinRM service started.
Configured LocalAccountTokenFilterPolicy to grant administrative rights
remotely to local users.

WinRM has been updated for remote management.
Created a WinRM listener on HTTP://* to accept WS-Man requests to any IP on
this machine.
WinRM firewall exception enabled.
```

## 讨论

正如本章介绍中所述，PowerShell 默认为典型管理场景启用远程处理：受信任网络或域网络中的服务器以及本地子网内的服务器。在客户端计算机或不受信任的网络上，PowerShell 默认不监听网络连接，必须显式启用。

PowerShell 远程处理不需要任何特定配置即可让你连接到远程计算机，但它确实需要一个配置步骤来允许来自远程计算机的连接。

### 在单台本地计算机上启用远程处理

一旦你决定启用远程处理，PowerShell 会让这变得很简单（在告知你影响之后）。只需从提升的 shell 中调用 `Enable-PSRemoting`。解决方案演示了这种方法。要绕过任何用户提示或确认，还要指定 `-Force` 标志。

默认情况下，`Enable-PSRemoting` 在被识别为公共网络的网络上被阻止。如果你通常信任你连接的网络，请使用 `Set-NetConnectionProfile` cmdlet 将网络配置文件更改为专用：

```
Set-NetConnectionProfile -NetworkCategory Private
```

如果你通常不信任你连接的网络但想启用 PowerShell 远程处理，请使用 `-SkipNetworkProfileCheck` 参数。

作为 `Enable-PSRemoting` 过程的一部分，PowerShell 连接到本地 WS-Management 服务以创建和配置 PowerShell 远程处理终结点。这是通过本地网络连接完成的，因此会受到 Windows 对网络连接的限制的影响。例如，Windows 不允许与密码为空的任何账户建立网络连接。如果你的管理员账户密码为空，PowerShell 将无法正确创建和配置 WSMan 终结点。

### 在远程计算机上启用远程处理

远程启用 PowerShell 远程处理带来许多独特的挑战。虽然你当然可以使用远程桌面连接到系统（然后基本上在本地启用它），但远程桌面不适合自动化。

相反，你可以利用另一种适合自动化的远程处理技术：Windows Management Instrumentation (WMI)。WMI 在大多数域计算机上处于启用状态，但它仅提供了一项用于远程命令执行的次要功能：`Win32_Process` WMI 类的 `Create()` 方法。有关此方法的详细信息，请参阅 配方 29.7。

### 在企业中启用远程处理

如果你想在企业中启用 PowerShell 远程处理，组策略是最灵活且可扩展的选项。通过组策略设置，你可以启用 WinRM 终结点和防火墙规则的自动配置。有关此方法的详细信息，请键入 `Get-Help about_remote_troubleshooting`。

## 另请参阅

配方 29.7，“程序：远程启用 PowerShell 远程处理”

# 29.3 启用 SSH 作为 PowerShell 远程处理传输

## 问题

你想使用 PowerShell 远程处理连接到远程 Linux 和 macOS 计算机。

## 解决方案

编辑你要连接的计算机上的 `/etc/ssh/sshd_config` (Linux) 或 `/private/etc/ssh/sshd_config` (macOS)，并将 `/usr/bin/pwsh` (Linux) 或 `/usr/local/bin/pwsh` (macOS) 配置为 SSH 子系统：

```
(...)
Subsystem sftp /usr/lib/openssh/sftp-server
Subsystem powershell /usr/bin/pwsh -sshs -NoLogo
```

然后重启 sshd：

```
sudo service ssh restart
```

## 讨论

正如我们在整章中讨论的那样，PowerShell 远程处理是一项极其通用的技术。它为并行脚本块、线程作业、后台作业、跨进程调试、远程调试、Hyper-V 控制台远程处理、容器连接，当然还有 Windows 计算机的交互式和扇出式远程处理，构成了丰富的通信通道。

除了所有这些选项之外，你还可以将 PowerShell 配置为使用 SSH 作为 PowerShell 远程处理连接的传输机制。如果你在远程计算机上安装了 PowerShell Core，只需编辑该计算机的 SSH 配置以启用 PowerShell 作为已识别的子系统。完成此操作后，与 Linux 或 macOS 系统交互将提供强大且丰富的基于对象的管理功能，这是 SSH 本身永远无法提供的：

```
PS > $s = New-PSSession 172.27.124.129 -SSHTransport
lee@172.27.124.129's password: ***************************
PS > Invoke-Command $s { $PSVersionTable } -OutVariable versionTable

Name                           Value
----                           -----
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
GitCommitId                    7.1.0
Platform                       Unix
WSManStackVersion              3.0
PSVersion                      7.1.0
SerializationVersion           1.1.0.1
PSEdition                      Core
OS                             Linux 5.4.0-48-generic #52-Ubuntu SMP
PSRemotingProtocolVersion      2.3

PS > $versionTable.OS
Linux 5.4.0-48-generic #52-Ubuntu SMP Thu Sep 10 10:58:49 UTC 2020
```

## 另请参阅

配方 1.1，“安装 PowerShell Core”

# 29.4 以交互方式管理远程计算机

## 问题

你想以交互方式使用远程计算机，就像它是一个本地 PowerShell 会话一样。

## 解决方案

使用 `Enter-PSSession` cmdlet 连接到远程会话并以交互方式管理它：

```
PS > Enter-PSSession Lee-Desk
[lee-desk]: PS E:\Lee> Get-Process -Name PowerShell

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   2834      14    85500      86256   218 ...22.83   8396 powershell
    421      12    39220      54204   189     7.41   9708 powershell

[lee-desk]: PS E:\Lee> exit
PS >
```

如果你当前的账户无法访问远程计算机，可以使用 `-Credential` 参数提供备用凭据：

```
PS > $cred = Get-Credential LEE-DESK\Lee
PS > Enter-PSSession Lee-Desk -Cred $cred
```

## 讨论

与许多传统 shell 一样，PowerShell 远程处理提供了一种简单、直接、交互式的管理体验，简称为*交互式远程处理*。你可以将交互式远程处理用于远程计算机、Hyper-V 虚拟机、容器，甚至通过 SSH 连接的 Linux 和 macOS 计算机。

就像在本地 PowerShell 会话中一样，你输入命令并查看其输出。这个远程 PowerShell 与你的本地 PowerShell 一样强大；所有的筛选、管道和集成语言功能继续工作。



然而，有两个方面使得交互式远程会话与本地会话有所不同。

首先要注意的是，远程 PowerShell 会话没有关联的桌面或图形用户界面。如果你让它启动记事本，PowerShell 会启动，但 UI 不会显示给任何人。

### 注意

当你在交互式远程处理中使用常规技术（即 `PS > notepad.exe`）启动应用程序时，PowerShell 会等待其关闭后才将控制权返回给你。这最终会阻塞你的会话，因此请按 `Ctrl+C` 以重新获得会话的控制权。如果你想启动图形应用程序，请使用 `Start-Process` cmdlet 或基于命令的远程处理。

此外，如果你启动的程序（如 `edit.com` 或 `ftp.exe` 的交互模式）直接与控制台窗口交互以实现其 UI，则该程序将无法按预期工作。某些应用程序（如 `ftp.exe` 的交互模式）会检测到没有可用的控制台窗口并直接退出。其他程序（如 `edit.com`）会挂起，并导致 PowerShell 的交互式远程处理也变得无响应。要摆脱此类行为异常的应用程序，请按 `Ctrl+C`。

交互式远程处理的第二个方面是所有无需显式凭据即可工作的 Windows 网络技术所共有的：双跳问题。一旦你远程连接到一台计算机，Windows 会授予你对所有本地资源的完全访问权限，就如同你直接登录到该计算机一样。然而，当涉及到*网络*资源时，Windows 会阻止你的用户信息被自动用于另一台计算机。这通常发生在你尝试从远程系统访问受限网络共享或需要隐式身份验证的内部网站点时。有关如何启动支持此类凭据转发的远程会话的信息，请参阅 配方 29.15。

除了向 `Enter-PSSession` cmdlet 提供计算机名称外，还可以使用 `New-PSSession` cmdlet 连接到计算机。连接后，你可以随意进入和退出该会话：

```powershell
PS > $session = New-PSSession Lee-Desk -Cred $cred
PS > Get-PSSession

 Id Name        ComputerName    State    ConfigurationName    Availability
 -- ----        ------------    -----    -----------------    ------------
  1 Session1    lee-desk        Opened   Microsoft.PowerShell Available


PS > Enter-PSSession $session
[lee-desk]: PS E:\Lee> Start-Process calc
[lee-desk]: PS E:\Lee> Get-Process -n calc

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
     64       5     4172       7272    44     0.06   7148 calc


[lee-desk]: PS E:\Lee> exit
PS > Get-Process -n calc
Get-Process : Cannot find a process with the name "calc". Verify the process
name and call the cmdlet again.

PS > Enter-PSSession $session
[lee-desk]: PS E:\Lee> Get-Process -n calc

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
     64       5     4172       7272    44     0.06   7148 calc


[lee-desk]: PS E:\Lee>
```

创建会话后，你甚至可以将交互式远程处理与批量的、基于命令的*扇出*远程处理相结合。有关基于命令的远程处理的详细信息，请参阅 配方 29.5。

### 注意

如果你在使用 Microsoft 帐户登录的家用计算机上试验 PowerShell 远程处理，创建 PowerShell 远程会话时还需要指定 `-EnableNetworkAccess` 参数。否则，你可能会收到类似以下的错误消息：

```
Connecting to remote server localhost failed with the
following error message : WinRM cannot process the
request. The following error with errorcode 0x8009030e
occurred while using Negotiate authentication: A
specified logon session does not exist. It may already
have been terminated.
```

使用 PowerShell 会话时，你可能会用到需要访问网络资源（如文件共享）的命令。如果这是在本地计算机上创建的会话的要求，可以使用不同 PowerShell 远程处理 cmdlet 的 `-EnableNetworkAccess` 参数。如果这是在另一台计算机上创建的会话，则需要使用 `CredSSP` 身份验证机制。有关启用和使用 CredSSP 的详细信息，请参阅 配方 29.15。

## 另请参阅

配方 29.5，“在远程计算机上调用命令”

配方 29.15，“创建具有完全网络访问权限的会话”

# 29.5 在远程计算机上调用命令

## 问题

你想在一台或多台远程计算机上调用命令。

## 解决方案

使用 `Invoke-Command` cmdlet：

```powershell
PS > Invoke-Command -Computer Lee-Desk,LEEHOLMES1C23 -Command { Get-PsDrive } |
    Format-Table Name,Used,Free,PSComputerName -Auto
Name     Used         Free         PSComputerName
----     ----         ----         --------------
Alias                              lee-desk
C        44830642176  105206947840 lee-desk
E        37626998784  61987717120  lee-desk
F        126526734336 37394722816  lee-desk
G        93445226496  6986330112   lee-desk
H        1703936      0            lee-desk
I        349184       18099200     lee-desk
J        40442880     0            lee-desk
C        24018575360  10339061760  leeholmes1c23
D        0                         leeholmes1c23
(...)
```

如果你当前的帐户无法访问远程计算机，可以使用 `-Credential` 参数提供备用凭据：

```powershell
PS > $cred = Get-Credential LEE-DESK\Lee
PS > Invoke-Command Lee-Desk { Get-Process } -Cred $cred
```

## 讨论

如 配方 29.4 所示，PowerShell 提供了简单的交互式远程处理，以应对你想要快速探索或管理单个远程系统的情况。然而，对于许多场景，一对一的交互式远程处理并不现实。简单的自动化（根据定义是非交互式的）是最基本的示例，但另一个关键点是大规模自动化。

对大量计算机运行命令（或一组命令）一直是一项具有挑战性的任务。为了同时解决一对一自动化和大规模自动化问题，PowerShell 支持*扇出*远程处理：一种基于命令的、面向批处理的系统管理方法。

### 提示

除了支持连接到远程计算机外，PowerShell 还支持虚拟机、容器和 SSH。

扇出远程处理集成了你从本地 PowerShell 体验中所期望的所有核心功能：结构丰富的输出、一致性，最重要的是覆盖面。虽然许多 PowerShell cmdlet 支持其自身的原生远程处理形式，但 PowerShell 的支持将其提供给每一个命令——包括 cmdlet 和控制台应用程序。

当你仅使用计算机名称和脚本块调用 `Invoke-Command` cmdlet 时，PowerShell 会自动连接到该计算机，调用命令并返回结果：




# PowerShell 远程处理：序列化、会话和双跳问题

## 初始远程处理示例

```powershell
PS > $result = Invoke-Command leeholmes1c23 { Get-PSDrive }
PS > $result | Format-Table Name,Used,Free,Root,PSComputerName -Auto
```

| 名称 | 已用 | 可用 | 根目录 | PSComputerName |
| ---- | ---- | ---- | ---- | -------------- |
| A | 0 | | A:\ | leeholmes1c23 |
| Alias | | | | leeholmes1c23 |
| C | 24018575360 | 10339061760 | C:\ | leeholmes1c23 |
| cert | | | \ | leeholmes1c23 |
| D | 0 | | D:\ | leeholmes1c23 |
| Env | | | | leeholmes1c23 |
| Function | | | | leeholmes1c23 |
| HKCU | | | HKEY_CURRENT_USER | leeholmes1c23 |
| HKLM | | | HKEY_LOCAL_MACHINE | leeholmes1c23 |
| Variable | | | | leeholmes1c23 |
| WSMan | | | | leeholmes1c23 |

到目前为止，这种远程处理体验看起来与许多其他技术相似。但请注意 `PSComputerName` 属性。PowerShell 会自动将此属性添加到所有结果中，这让你能轻松地一次性处理多台计算机的输出。一旦我们开始处理结果，就能见识到 PowerShell 独特的远程处理方式。例如：

```powershell
PS > $result | Sort-Object Name | Where { $_.Root -like "*\*" }

Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
A                                                    A:\
C                  22.37          9.63               C:\
cert                                                 \
D                                                    D:\

PS > $result[2].Used
24018575360
PS > $result[2].Used * 4
96074301440
```

## 序列化和反序列化

PowerShell 不像其他远程技术那样传输纯文本，而是以一种能保留原始命令输出大量信息的方式传输数据。在将对象发送给你之前，PowerShell 会将它们*序列化*为一种可跨网络移动的格式。此格式保留以下“基元”类型，并将所有其他类型转换为其字符串表示形式：

| 类型 | 类型 | 类型 | 类型 |
| ---- | ---- | ---- | ---- |
| `Byte` | `UInt16` | `TimeSpan` | `SecureString` |
| `SByte` | `UInt32` | `DateTime` | `Boolean` |
| `Byte[]` | `UInt64` | `ProgressRecord` | `Guid` |
| `Int16` | `Decimal` | `Char` | `Uri` |
| `Int32` | `Single` | `String` | `Version` |

> **注意**
>
> 也许最重要的是，序列化会从非基元对象中移除所有方法。通过将这些对象转换为所谓的*属性包*，你的脚本可以依赖一个不会在 PowerShell 版本、.NET 版本或操作系统版本之间发生变化的接口。

当对象到达你的计算机时，PowerShell 会对其进行*还原*。在此过程中，它会创建具有原始结构的对象并重新填充属性。任何属于基元类型的属性将再次变得完全可用：整数属性可以排序和计算，XML 文档可以导航，等等。

当 PowerShell 重新组装对象时，它会在其类型名称前加上 `Deserialized`。当 PowerShell 显示反序列化对象时，它将使用适用于全保真对象的任何格式定义：

```powershell
PS > $result[2] | Get-Member

   TypeName: Deserialized.System.Management.Automation.PSDriveInfo

Name               MemberType   Definition
----               ----------   ----------
ToString           Method       string ToString(), string ToString(stri...
Free               NoteProperty System.UInt64 Free=10339061760
PSComputerName     NoteProperty System.String PSComputerName=leeholmes1c23
PSShowComputerName NoteProperty System.Boolean PSShowComputerName=True
RunspaceId         NoteProperty System.Guid RunspaceId=33f45afd-2381-44...
Used               NoteProperty System.UInt64 Used=24018575360
Credential         Property     Deserialized.System.Management.Automati...
CurrentLocation    Property     System.String {get;set;}
Description        Property     System.String {get;set;}
Name               Property     System.String {get;set;}
Provider           Property     System.String {get;set;}
Root               Property     System.String {get;set;}
```

## 使用 `New-PSSession` 创建持久会话

除了向 `Invoke-Command` cmdlet 提供计算机名称外，还可以使用 `New-PSSession` cmdlet 连接到计算机。连接后，你可以随时在该会话中调用命令：

```powershell
PS > $session = New-PSSession leeholmes1c23 -Cred $cred
PS > Get-PSSession

 Id Name        ComputerName    State    ConfigurationName    Availability
 -- ----        ------------    -----    -----------------    ------------
  1 Session1    leeholmes1c23   Opened   Microsoft.PowerShell Available

PS > Invoke-Command -Session $session { Get-Process -Name PowerShell }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id Process  PSCompu
                                                          Name     terName
-------  ------    -----      ----- -----   ------     -- -------- -------
    716      12    48176      65060   201    23.31   4684 power... leeh...
```

创建会话后，你甚至可以将命令与交互式远程处理相结合，如食谱 29.4 所示。

> **注意**
>
> 如果网络中断导致会话中断，PowerShell 会尝试自动断开你的会话，以便你在网络恢复可用时重新连接。有关详细信息，请参阅食谱 29.6。

使用这些技术，你可以轻松地将自动化扩展到许多许多台机器上。有关此技术的详细信息，请参阅食谱 29.19。

## 双跳问题

在扇出远程处理中，你将遇到的主要挑战之一，是所有无需显式凭据即可工作的 Windows 网络技术所共有的：*双跳问题*。一旦你远程连接到一台计算机，Windows 会授予你对所有本地资源的完全访问权限，就如同你直接登录到该计算机一样。然而，当涉及到*网络*资源时，Windows 会阻止你的用户信息被自动用于另一台计算机。这通常发生在你尝试从远程系统访问受限网络共享，或访问需要隐式身份验证的内部网站点时。有关如何启动支持此类凭据转发的远程会话的信息，请参阅食谱 29.15。

## 另请参阅

- 食谱 29.4，“以交互方式管理远程计算机”
- 食谱 29.15，“创建具有完全网络访问权限的会话”
- 食谱 29.19，“在多台计算机上调用命令”

# 29.6 断开并重新连接 PowerShell 会话

## 问题

你有一个活动的 PowerShell 会话，想要从中断开连接或重新连接到它。

## 解决方案

如果你有一个活动的 PowerShell 会话，请使用 `Disconnect-PSSession` cmdlet 从中断开连接。稍后使用 `Connect-PSSession` 重新连接。




## 讨论

使用许多远程管理技术时，一个常见问题是关闭本地实例会自动关闭你连接的任何远程 Shell。如果你的远程 Shell 中包含有价值的信息或正在运行作业，这些信息就会丢失。

为解决此问题，PowerShell 支持 `Disconnect-PSSession` 和 `Connect-PSSession` cmdlet。它们分别允许你断开和连接到远程会话。

当你想要发现远程计算机上的断开连接的会话时，请使用 `Get-PSSession` cmdlet 的 `-ComputerName` 参数来检索它们。

除了断开特定会话外，你还可以在计算机上调用长时间运行的命令，然后立即从中断开连接。为此，`Invoke-Command` cmdlet 提供了 `-InDisconnectedSession` 参数：

```
PS > $s = New-PSSession RemoteComputer -Name ConnectTest
PS > Invoke-Command $s { $saved = "Hello World" }
PS > Disconnect-PSSession $s

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  7 ConnectTest     RemoteComputer  Disconnected  Microsoft.PowerShell

## From potentially another shell or computer
PS > $s2 = Get-PSSession -ComputerName RemoteComputer -Name ConnectTest
PS > Connect-PSSession $s2

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  7 ConnectTest     RemoteComputer  Disconnected  Microsoft.PowerShell

PS > Invoke-Command $s2 { $saved }
Hello World
```

```
PS > Invoke-Command RemoteComputer {
    1..10 | % { Start-Sleep -Seconds 10; $_ } } -InDisconnectedSession

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  3 Session2        RemoteComputer  Disconnected  Microsoft.PowerShell

PS > $s = Get-PSSession -Computername RemoteComputer -Name Session2

PS > Connect-PSSession $s

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  3 Session2        RemoteComputer  Opened        Microsoft.PowerShell

PS > Receive-PSSession $s
1
2
3
(...)
```

当你使用 `-InDisconnectedSession` 参数时，运行的命令很可能会生成输出。如果在断开连接时生成的输出超过 1 兆字节，PowerShell 会暂停你的命令，直到你连接并调用 `Receive-PSSession` 命令来检索它。在计算这 1 兆字节输出时，PowerShell 仅考虑通常会直接发送给已连接客户端的输出——即你通常在屏幕上看到的输出。存储在变量中的输出不计入此限制，重定向到文件的输出也不计入。

如果你希望 PowerShell 无论生成多少输出都继续运行你的命令，可以使用 `-SessionOption` 参数的 `OutputBufferingMode` 属性：

```
Invoke-Command RemoteComputer {
    ... commands ...
} -InDisconnectedSession -SessionOption @{ OutputBufferingMode = "Drop" }
```

当你将 `OutputBufferingMode` 指定为 `Drop` 时，PowerShell 仅保留最后 1 兆字节的输出。

## 另请参阅

食谱 29.4，“以交互方式管理远程计算机”

食谱 29.5，“在远程计算机上调用命令”

# 29.7 程序：远程启用 PowerShell 远程处理

虽然 Windows Server 默认不需要此操作，但有时你可能需要远程启用 PowerShell 远程处理。遗憾的是，大多数计算机未配置为支持远程任务管理。然而，大多数都配置为支持 WMI 连接。作为引导步骤，我们可以使用 `Win32_Process` 类的 `Create()` 方法启动 PowerShell 实例，然后向 PowerShell 提供启用 PowerShell 远程处理的命令。

示例 29-1 中所示的脚本可自动完成这个繁琐的过程。

### 示例 29-1. Enable-RemotePSRemoting.ps1

```
##############################################################################
##
## Enable-RemotePsRemoting
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Enables PowerShell Remoting on a remote computer. Requires that the machine
responds to WMI requests, and that its operating system is Windows Vista or
later.

.EXAMPLE

PS > Enable-RemotePsRemoting <Computer>

#>

param(
    ## The computer on which to enable remoting
    $Computername,

    [Switch] $SkipNetworkProfileCheck,

    ## The credential to use when connecting
    [PSCredential] $Credential
)

Set-StrictMode -Version 3

$VerbosePreference = "Continue"

Write-Verbose "Configuring $computername"
$skipNetworkProfileCheckFlag = '$' + $SkipNetworkProfileCheck.IsPresent
$command = "powershell -NoProfile -Command" +
    "Enable-PSRemoting -SkipNetworkProfileCheck:$skipNetworkProfileCheckFlag -Force"

if($Credential)
{
    $null = Invoke-WmiMethod -Computer $computername -Credential $credential `
        Win32_Process Create -Args $command

    Start-Sleep -Seconds 10

    Write-Verbose "Testing connection"
    Invoke-Command $computername {
        Get-WmiObject Win32_ComputerSystem } -Credential $credential
}
else {
    $null = Invoke-WmiMethod -Computer $computername Win32_Process Create -Args $command
    Start-Sleep -Seconds 10

    Write-Verbose "Testing connection"
    Invoke-Command $computername { Get-WmiObject Win32_ComputerSystem }
}
```

有关运行脚本的详细信息，请参阅食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 28.1，“访问 Windows 管理规范和 CIM 数据”

食谱 29.2，“在计算机上启用 PowerShell 远程处理”

# 29.8 程序：在远程计算机上调用 PowerShell 表达式

PowerShell 通过其 PowerShell 远程处理功能，为在远程计算机上执行命令提供了极佳的支持。这些功能仅要求远程系统可用 PowerShell 且已启用远程处理。

如果远程计算机上不可用 PowerShell 远程处理，许多命令支持通过 WMI 或 DCOM 进行自己的远程处理。这些不需要在远程系统上配置 PowerShell 远程处理，但确实需要启用特定协议（WMI 或 DCOM）。

如果这些先决条件都不具备，示例 29-3 提供了一种替代方案。它使用 [PsExec](https://oreil.ly/QVeM9) 来支持实际的远程命令执行。

然而，此脚本提供的功能不仅限于远程命令执行。正如示例 29-2 所示，它利用 PowerShell 导入和导出强类型结构化数据的能力，因此你可以使用许多相同的技术来处理命令输出，就像处理本地系统上的命令输出一样。它通过在远程系统上筛选命令输出但在本地系统上对其进行排序来演示这种功能。

### 示例 29-2. 在远程计算机上调用 PowerShell 表达式



# 29.9 测试两台计算机之间的连接性

## 示例 29-3：Invoke-RemoteExpression.ps1

### 使用示例

```
PS > $command = { Get-Process | Where-Object { $_.Handles -gt 1000 } }
PS > Invoke-RemoteExpression \\LEE-DESK $command | Sort-Object Handles
Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)    Id ProcessName
-------  ------  -----  -----  -----  ------    -- -----------
   1025       8   3780   3772     32  134.42   848 csrss
   1306      37  50364  64160    322  409.23  4012 OUTLOOK
   1813      39  54764  36360    321  340.45  1452 iTunes
   2316     273  29168  41164    218  134.09  1244 svchost
```

由于这些高度结构化的数据来自另一台系统上的对象，`PowerShell` 不会重新生成这些对象的功能（极少数情况除外）。有关导入和导出结构化数据的详细信息，请参阅 Recipe 10.5。

### 脚本实现

```
##############################################################################
##
## Invoke-RemoteExpression
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Invoke a PowerShell expression on a remote machine. Requires `PsExec` from
`http://live.sysinternals.com/tools/psexec.exe`. If the remote machine
supports PowerShell Remoting, use that instead!

.EXAMPLE

PS > Invoke-RemoteExpression LEE-DESK { Get-Process }
Retrieves the output of a simple command from a remote machine

.EXAMPLE

PS > (Invoke-RemoteExpression LEE-DESK { Get-Date }).AddDays(1)
Invokes a command on a remote machine. Since the command returns one of
PowerShell's primitive types (a `DateTime` object,) you can manipulate
its output as an object afterward.

.EXAMPLE

PS > Invoke-RemoteExpression LEE-DESK { Get-Process } | Sort Handles
Invokes a command on a remote machine. The command does not return one of
PowerShell's primitive types, but you can still use PowerShell's filtering
cmdlets to work with its structured output.

#>

param(
    ## The computer on which to invoke the command.
    $ComputerName = "$ENV:ComputerName",

    ## The scriptblock to invoke on the remote machine.
    [Parameter(Mandatory = $true)]
    [ScriptBlock] $ScriptBlock,

    ## The username / password to use in the connection
    $Credential,

    ## Determines if PowerShell should load the user's PowerShell profile
    ## when invoking the command.
    [switch] $NoProfile
)

Set-StrictMode -Version 3

## Prepare the computername for PSExec
if($ComputerName -notmatch '^\\')
{
    $ComputerName = "\\$ComputerName"
}

## Prepare the command line for PsExec. We use the XML output encoding so
## that PowerShell can convert the output back into structured objects.
## PowerShell expects that you pass it some input when being run by PsExec
## this way, so the 'echo .' statement satisfies that appetite.
$commandLine = "echo . | powershell -Output XML "

if($noProfile)
{
    $commandLine += "-NoProfile "
}

## Convert the command into an encoded command for PowerShell
$commandBytes = [System.Text.Encoding]::Unicode.GetBytes($scriptblock)
$encodedCommand = [Convert]::ToBase64String($commandBytes)
$commandLine += "-EncodedCommand $encodedCommand"

## Collect the output and error output
$errorOutput = [IO.Path]::GetTempFileName()

if($Credential)
{
    ## This lets users pass either a username, or full credential to our
    ## credential parameter
    $credential = Get-Credential $credential
    $networkCredential = $credential.GetNetworkCredential()
    $username = $networkCredential.Username
    $password = $networkCredential.Password

    $output = psexec $computername /user $username /password $password `
        /accepteula cmd /c $commandLine 2>$errorOutput
}
else
{
    $output = psexec /acceptEula $computername cmd /c $commandLine 2>$errorOutput
}

## Check for any errors
$errorContent = Get-Content $errorOutput
Remove-Item $errorOutput

if($lastExitCode -ne 0)
{
    $OFS = "`n"
    $errorMessage = "Could not execute remote expression. "
    $errorMessage += "Ensure that your account has administrative " +
        "privileges on the target machine.`n"
    $errorMessage += ($errorContent -match "psexec.exe :")

    Write-Error $errorMessage
}

## Return the output to the user
$output
```

有关运行脚本的详细信息，请参阅 Recipe 1.2。

## 另请参阅

Recipe 1.2，“运行程序、脚本和现有工具”

Recipe 10.5，“轻松导入和导出结构化数据”

Recipe 29.1，“查找支持自身远程处理的命令”

## 问题

你想要确定一台计算机的网络可用性，或两台计算机之间的网络可用性。

## 解决方案

使用 `Test-Connection` cmdlet 执行传统的网络 ping：

```
PS > Test-Connection leeholmes.com

Source    Destination     IPV4Address      IPV6Address
------    -----------     -----------      -----------
LEE-DESK  leeholmes.com   66.186.25.131    {}
LEE-DESK  leeholmes.com   66.186.25.131    {}
LEE-DESK  leeholmes.com   66.186.25.131    {}
LEE-DESK  leeholmes.com   66.186.25.131    {}
```

或者，`ping.exe` 实用程序仍然可用：

```
PS > ping leeholmes.com

Pinging leeholmes.com [66.186.25.131] with 32 bytes of data:
Reply from 66.186.25.131: bytes=32 time=38ms TTL=115
Reply from 66.186.25.131: bytes=32 time=36ms TTL=115
Reply from 66.186.25.131: bytes=32 time=37ms TTL=115
Reply from 66.186.25.131: bytes=32 time=41ms TTL=115

Ping statistics for 66.186.25.131:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 36ms, Maximum = 41ms, Average = 38ms
```

## 讨论

作为命令行 Shell，`PowerShell` 当然继续支持传统的命令行实用程序。最常见的网络诊断工具之一是 `ping.exe`，它在 `PowerShell` 中按预期工作。

`Test-Connection` cmdlet 提供与 `ping.exe` 相同的功能，外加大量额外功能。大多数 ping 实用程序只能验证当前计算机与目标计算机之间的连接，但 `Test-Connection` cmdlet 还允许你为网络测试指定*源计算机*。

`Test-Connection` cmdlet 或许最大的优势在于其基于对象的输出——这使得筛选、排序和分析变得容易得多。例如，一个用于监控一组域名平均响应时间的简单脚本：

```
$topTen = "google.com","facebook.com","youtube.com","yahoo.com",
    "live.com","wikipedia.org","blogger.com","baidu.com","msn.com",
    "qq.com"

## Test all of the connections, grouping by destination
$results = Test-Connection $topTen -ErrorAction SilentlyContinue |
    Group Destination

## Go through each of the addresses
$averages = foreach($group in $results)
{
    ## Figure out the average response time
    $averageResponse = $group.Group |
        Measure-Object -Average Latency | Select -Expand Average

    ## Create a new custom object to output the Address and ResponseTime
    [PSCustomObject] @{
        Address = $group.Name;
        ResponseTime = $averageResponse
    }
}

## Output the results
$averages | Sort-Object ResponseTime | Select Address,ResponseTime
```

该脚本产生以下输出：

```
Destination   ResponseTime
-----------   ------------
google.com              22
blogger.com           22.5
facebook.com         35.25
yahoo.com             37.5
youtube.com          86.25
wikipedia.org           99
baidu.com           203.25
qq.com              259.25
```



关于此脚本输出有一点需要注意：并非所有前 10 个网站都在列表中。Ping 请求是一种简单的基于网络的握手，但许多网站会阻止它们以节省网络带宽或出于感知上的安全加固考虑。当 `Test-Connection` cmdlet 无法建立连接时，会生成以下错误消息：

```
Test-Connection : Testing connection to computer 'bing.com' failed: Error
due to lack of resources
```

要验证与这些资源的连接，可以使用 Recipe 12.13 中提供的 `Send-TcpRequest` 脚本的 `-Test` 参数：

```
PS > Send-TcpRequest bing.com -Test
True
PS > Send-TcpRequest bing.com -Test -Port 443
True
PS > Send-TcpRequest bing.com -Test -Port 23
False
```

有关在尝试管理网络资源之前使用 `Test-Connection` cmdlet 验证网络资源的有效方法，请参阅 Recipe 29.10。

## 另请参阅

Recipe 12.13，“程序：与 Internet 协议交互”

Recipe 29.10，“将网络脚本限制为响应的主机”

# 29.10 将网络脚本限制为响应的主机

## 问题

您有一个分布式网络管理任务，希望避免由脱机或无响应的主机导致的延迟。

## 解决方案

使用 `Test-Connection` 的 `-Quiet` 参数来筛选计算机集合，仅保留响应网络 ping 的主机：

```
$computers = "MISSING",$env:ComputerName,"DOWN","localhost"
$skipped = @()

foreach($computer in $computers)
{
    ## If the computer is not responding, record that we skipped it and
    ## continue. We can review this collection after the script completes.
    if(-not (Test-Connection -Quiet $computer -Count 1))
    {
        $skipped += $computer
        continue
    }

    ## Perform some batch of networked operations
    Get-CimInstance -Computer $computer Win32_OperatingSystem
}
```

## 讨论

编写管理大量计算机集合的脚本时，一个困难是其中通常有少数处于关闭或无响应状态。如果不处理这种情况，脚本在尝试反复管理无法访问的系统时，很可能会遇到许多错误和延迟。

在大多数域中，网络 ping 是确定计算机响应能力的最可靠方法。`Test-Connection` cmdlet 在 PowerShell 中提供 ping 支持，因此解决方案基于此构建。

有关 `Test-Connection` cmdlet 的详细信息，请参阅 Recipe 29.9。

## 另请参阅

Recipe 29.9，“测试两台计算机之间的连接”

# 29.11 在计算机上启用远程桌面

## 问题

您想要在计算机上启用远程桌面。

## 解决方案

将远程桌面注册表项的 `fDenyTSConnections` 属性设置为 `0`：

```
$regKey = "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server"
Set-ItemProperty $regKey fDenyTSConnections 0
```

## 讨论

远程桌面是事实上的交互式管理协议，但很难自动启用。幸运的是，其配置设置来自 Windows 注册表，因此您可以使用 PowerShell 的注册表提供程序来启用它。

要禁用远程桌面，请将 `fDenyTSConnections` 属性设置为 `1`。

要在远程计算机上启用远程桌面，请使用 PowerShell 远程处理来更改注册表属性，或直接远程管理注册表设置。要了解如何直接管理远程注册表设置，请参阅 Recipe 21.12。

## 另请参阅

Recipe 21.12，“处理远程计算机的注册表”

# 29.12 配置远程处理的用户权限

## 问题

您想要控制允许远程连接到计算机的用户。

## 解决方案

只需将用户添加到内置的 Remote Management Users 组中：

```
PS > net localgroup "Remote Management Users" /add DOMAIN\User
The command completed successfully.
```

如果您希望使用其他或替代用户组来实现同等功能，请创建一个新的 Windows 组以定义哪些用户可以连接到计算机，然后使用 `Set-PSSessionConfiguration` cmdlet 将此组添加到终结点的权限列表中：

```
PS > net localgroup "PowerShell Remoting Users" /Add
The command completed successfully.

PS > net localgroup "PowerShell Remoting Users" Administrators /Add
The command completed successfully.

PS > Set-PSSessionConfiguration Microsoft.PowerShell -ShowSecurityDescriptorUI
```

## 讨论

与 Windows 中的许多对象一样，提供 PowerShell 远程处理访问的 WS-Management 终结点具有关联的访问控制列表。默认情况下，PowerShell 中的此访问控制列表为计算机的 Administrators 以及内置的 Remote Management Users 组提供访问权限。

在某些高级场景中，您可能需要比内置 Remote Management Users 组更精细的控制。在这些情况下，您可以创建自己的用户组并将其添加到 PowerShell 远程处理终结点的访问控制列表中。

对于一次性配置，`Set-PSSessionConfiguration` cmdlet 的 `-ShowSecurityDescriptorUI` 参数允许您像管理文件、目录或计算机共享一样管理访问控制列表。

不过，要自动执行此过程，您需要直接使用安全规则语言——一种称为 `SDDL` 的语言：安全描述符定义语言。这种格式并非真正设计供人类直接使用，但它是 `Set-PSSessionConfiguration` cmdlet 的 `-SecurityDescriptorSddl` 参数所公开的格式。尽管它不便于用户使用，但您可以使用 .NET Framework 中的几个类来创建安全规则或 `SDDL` 字符串。示例 29-4 演示了这种方法。

##### 示例 29-4. 自动化 PowerShell 远程处理的安全配置

```
## Get the SID for the "PowerShell Remoting Users" group
$account = New-Object Security.Principal.NTAccount "PowerShell Remoting Users"
$sid = $account.Translate([Security.Principal.SecurityIdentifier]).Value

## Get the security descriptor for the existing configuration
$config = Get-PSSessionConfiguration Microsoft.PowerShell
$existingSddl = $config.SecurityDescriptorSddl

## Create a CommonSecurityDescriptor object out of the existing SDDL
## so that we don't need to manage the string by hand
$arguments = $false,$false,$existingSddl
$mapper = New-Object Security.AccessControl.CommonSecurityDescriptor $arguments

## Create a new access rule that adds the "PowerShell Remoting Users" group
$mapper.DiscretionaryAcl.AddAccess("Allow",$sid,268435456,"None","None")

## Get the new SDDL for that configuration
$newSddl = $mapper.GetSddlForm("All")

## Update the endpoint configuration
Set-PSSessionConfiguration Microsoft.PowerShell -SecurityDescriptorSddl $newSddl
```

有关使用 .NET Framework 的详细信息，请参阅 Recipe 3.8。有关使用 `SDDL` 字符串的详细信息，请参阅 Recipe 18.17。

## 另请参阅

Recipe 3.8，“使用 .NET 对象”

Recipe 18.17，“以 SDDL 形式管理安全描述符”

# 29.13 启用到工作组计算机的远程处理

## 问题

您想要连接到工作组中的计算机或通过 IP 地址连接。

## 解决方案

更新 `wsman:\localhost\client` 路径上的 `TrustedHosts` 集合：




## WinRM TrustedHosts 配置

```powershell
PS > $trustedHosts = Get-Item wsman:\localhost\client\TrustedHosts
PS > $trustedHosts.Value += ",RemoteComputer"
PS > Set-Item wsman:\localhost\client\TrustedHosts $trustedHosts.Value
```

```text
WinRM 安全配置。
此命令修改 WinRM 客户端的 TrustedHosts 列表。
TrustedHosts 列表中的计算机可能未经过身份验证。
客户端可能会将凭据信息发送到这些计算机。
您确定要修改此列表吗？
[Y] 是  [N] 否  [S] 挂起  [?] 帮助 (默认为 "Y")：Y
```

```powershell
PS > Get-Item wsman:\localhost\client\TrustedHosts


   WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Client

Name                      Value
----                      -----
TrustedHosts              Lee-Desk,RemoteComputer
```

## 讨论

任何远程技术中客户端安全的主要方面之一，是能够信任您要连接的对象。当您在咖啡馆时，可以在浏览器中连接到银行网站。如果您使用 `SSL`，则可以保证它真的是您的银行，而不是攻击者通过操纵网络流量建立的某个虚假代理。这类拦截攻击称为 *中间人攻击*。

`PowerShell Remoting` 提供相同的保证。当您连接到域内的计算机时，`Kerberos` 身份验证可保护连接安全。`Kerberos` 身份验证可保证终结点的身份，确保没有攻击者可以拦截您的连接。当您在域外时，`SSL` 是保证这一点的唯一标准方式，这也是 `HTTPS` 成为互联网上如此重要协议的原因。

有两种情况是内置身份验证机制无法防范中间人攻击的：

*   通过 IP 地址连接到主机（无论是否在域内）
*   使用除 `Kerberos`、`SSL` 或 `CredSSP` 之外的任何身份验证机制

工作组远程（或跨林远程）就是一个例子。当您尝试在这两种场景中的任意一种中建立连接时，`PowerShell` 会给出错误消息：

```powershell
PS > Enter-PSSession SomeComputer

Enter-PSSession : 连接到远程服务器失败，显示以下错误消息：WinRM 客户端无法处理该请求。如果身份验证方案不同于 Kerberos，或者客户端计算机未加入域，则必须使用 HTTPS 传输，或者必须将目标计算机添加到 TrustedHosts 配置设置中。使用 winrm.cmd 配置 TrustedHosts。请注意，TrustedHosts 列表中的计算机可能未经过身份验证。您可以运行以下命令获取有关此内容的详细信息：winrm help config。有关详细信息，请参阅 about_Remote_Troubleshooting 帮助主题。
```

虽然冗长，但此错误消息准确地解释了问题。

由于 `PowerShell` 在这种情况下无法保证远程计算机的身份，因此它会安全失败并生成错误。所有远程协议都会遇到此问题：

*   远程桌面：“……无法验证您要连接的计算机的身份……”
*   `SSH`：“无法建立主机的真实性……”

其他协议实现了相当于“我知情并希望继续”的功能，但 `PowerShell` 的体验不幸的是更复杂。

如果您想连接到 `PowerShell` 无法验证的计算机，可以更新 `TrustedHosts` 配置设置。遗憾的是，它的名称含糊不清，因为它实际上意味着，“我在连接到此计算机时信任我的网络。”

配置 `TrustedHosts` 设置时，您有三个选项：显式列表（如解决方案所示）、`<local>` 以绕过子网或工作组中所有计算机的此消息，或 `*` 以完全禁用该消息。

有关详细信息，请键入 `Get-Help about_Remote_Troubleshooting`。

# 29.14 从远程计算机隐式调用命令

## 问题

您在远程计算机上有一些命令，想要像调用本地命令一样调用它们。

## 解决方案

使用 `Import-PSSession` cmdlet 将它们导入当前会话：

```powershell
PS > $cred = Get-Credential

PS > $session = New-PSSession -ConfigurationName Microsoft.Exchange `
    -ConnectionUri https://ps.outlook.com/powershell/ -Credential $cred `
    -Authentication Basic -AllowRedirection


PS > Invoke-Command $session { Get-OrganizationalUnit } |
    Select DistinguishedName

DistinguishedName
-----------------
OU=leeholmes.com,OU=Microsoft Exchange Hosted Organizations,DC=prod,DC=...
OU=Hosted Organization Security Groups,OU=leeholmes.com,OU=Microsoft Ex...


PS > Import-PSSession $session -CommandName Get-OrganizationalUnit

ModuleType Name                      ExportedCommands
---------- ----                      ----------------
Script     tmp_1e510382-9a3d-43a5... Get-OrganizationalUnit


PS > Get-OrganizationalUnit | Select DistinguishedName

DistinguishedName
-----------------
OU=leeholmes.com,OU=Microsoft Exchange Hosted Organizations,DC=prod,DC=...
OU=Hosted Organization Security Groups,OU=leeholmes.com,OU=Microsoft Ex...
```

## 讨论

当您频繁使用来自远程系统的命令时，持续调用 `Invoke-Command` 并经过 `PowerShell` 远程基础设施所带来的心智和概念开销会迅速累积。当您编写的脚本主要使用来自远程系统的命令时，脚本的大部分最终都变成了远程基础设施本身的代码。当将命令通过管道传递给彼此时，这会变得更加痛苦：

```powershell
PS > Invoke-Command $session { Get-User } |
    Where-Object { $_.Identity -eq "guide@leeholmes.com" } |
    Invoke-Command $session { Get-Mailbox } |
    Select Identity,OriginatingServer,ExchangeVersion,DistinguishedName


Identity            OriginatingServer  ExchangeVersion    DistinguishedName
--------            -----------------  ---------------    -----------------
guide@leeholmes.com BL2PRD0103DC006... 0.10 (14.0.100.0)  CN=guide@leeh....
```

为了解决这些问题，`PowerShell Remoting` 支持 `Import-PSSession` cmdlet，允许您导入并无缝使用来自远程会话的命令。这在 Exchange Online 等场景中特别有用。仅为了管理云中的邮箱而安装整套命令工具包是不合理的。

一旦您导入了这些命令，`PowerShell` 就会在它们上启用 *隐式远程*：

```powershell
PS > Import-PSSession $session -CommandName Get-Mailbox,GetUser

PS > Get-User | Where-Object { $_.Identity -eq "guide@leeholmes.com" } |
    Get-MailBox |
    Select Identity,OriginatingServer,ExchangeVersion,DistinguishedName


Identity            OriginatingServer  ExchangeVersion    DistinguishedName
--------            -----------------  ---------------    -----------------
guide@leeholmes.com BL2PRD0103DC006... 0.10 (14.0.100.0)  CN=guide@leeh....

PS > Get-Help Get-User -Examples

NAME
    Get-User

SYNOPSIS
    使用 Get-User cmdlet 检索林中与指定条件匹配的所有用户。

    -------------------------- 示例 1 --------------------------
    此示例检索 Marketing OU 中用户的信息。

    Get-User -OrganizationalUnit "Marketing"
(...)
```

进一步扩展，`PowerShell` 甚至允许您将会话中的命令导出到模块中：

```powershell
PS > $commands = "Get-Mailbox","Get-User"
PS > Export-PSSession $session -CommandName $commands -Module ExchangeCommands


    Directory: E:\Lee\WindowsPowerShell\Modules\ExchangeCommands


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         2/19/2010  11:11 PM      13177 ExchangeCommands.psm1
-a---         2/19/2010  11:11 PM         99 ExchangeCommands.format.ps1xml
-a---         2/19/2010  11:11 PM        605 ExchangeCommands.psd1
```




当你导入模块时，PowerShell 会为你导出的所有命令创建新的隐式远程命令。当你调用一个命令时，它会重新创建远程会话（如果需要），然后在该新会话中调用你的命令——即使是在一个全新的 PowerShell 实例中：

```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS > Import-Module ExchangeCommands
PS > Get-User | Where-Object { $_.Identity -eq "guide@leeholmes.com" } |
    Get-MailBox |
    Select Identity,OriginatingServer,ExchangeVersion,DistinguishedName

Creating a new session for implicit remoting of "Get-User" command...

Identity            OriginatingServer  ExchangeVersion    DistinguishedName
--------            -----------------  ---------------    -----------------
guide@leeholmes.com BL2PRD0103DC006... 0.10 (14.0.100.0)  CN=guide@leeh....
```

有关基于命令的远程处理的更多信息，请参阅 Recipe 29.5。有关 PowerShell 模块的更多信息，请参阅 Recipe 1.28。

## 另请参阅

Recipe 1.28, “Extend Your Shell with Additional Commands”

Recipe 29.5, “Invoke a Command on a Remote Computer”

# 29.15 创建具有完全网络访问权限的会话

## 问题

你想要创建一个 PowerShell 远程会话（交互式、扇出式或隐式），该会话具有对网络资源的完全访问权限。

## 解决方案

使用 `-Authentication` 参数，并选择 `CredSSP` 作为身份验证机制：

```
PS > Invoke-Command leeholmes1c23 {
    "Hello World"; dir \\lee-desk\c$ } -Authentication CredSSP -Cred Lee

Hello World

    Directory: \\lee-desk\c$

Mode                LastWriteTime     Length Name           PSComputerName
----                -------------     ------ ----           --------------
d----          2/5/2010  12:31 AM            inetpub        leeholmes1c23
d----         7/13/2009   7:37 PM            PerfLogs       leeholmes1c23
d-r--         2/16/2010   3:14 PM            Program Files  leeholmes1c23
(...)
```

## 讨论

当使用 PowerShell 远程处理连接到计算机时，你可能会在运行访问网络位置的命令时看到错误：

```
PS > Invoke-Command leeholmes1c23 {
    "Hello World"; dir \\lee-desk\c$ } -Cred Lee

Hello World
Get-ChildItem: Cannot find path '\\lee-desk\c$' because it does not exist.
```

当你在域中远程连接到计算机时，Windows（和 PowerShell 远程处理）默认使用一种称为 `Kerberos` 的身份验证机制。虽然以这种方式连接时你拥有对本地资源的完全访问权限，但 `Kerberos` 的安全特性会阻止远程计算机使用你的帐户信息连接到其他计算机。

这降低了连接到已被入侵或以其他方式运行恶意软件的远程计算机的风险。如果没有这些保护，恶意软件可以代表你在整个网络上行事——如果你使用功能强大的域凭据进行连接，这将是一个特别危险的情况。

尽管此 `Kerberos` 策略可以通过在域级别将计算机标记为“受信任以进行委派”来管理，但更改域级策略以完成临时管理任务是一个繁琐的过程。

为了解决这个问题，PowerShell 支持另一种称为 `CredSSP` 的身份验证机制——这是远程桌面和终端服务使用的相同身份验证机制。由于其安全影响，你必须在你连接的客户端和你连接的服务器上显式启用支持。

### 注意

如果你是连接回本地计算机而不是网络上的另一台计算机，可以改用远程处理 cmdlet 的 `-EnableNetworkAccess` 参数来启用网络访问。

从客户端，向 `Enable-WsManCredSSP` cmdlet 指定 `-Role Client`。你可以在 `-DelegateComputer` 参数中指定特定的计算机名称，或指定 `*` 以对所有目标计算机启用该设置。

```
PS > Enable-WSManCredSSP -Role Client -DelegateComputer leeholmes1c23

CredSSP Authentication Configuration for WS-Management
CredSSP authentication allows the user credentials on this computer to be
sent to a remote computer. If you use CredSSP authentication for a
connection to a malicious or compromised computer, that computer will have
access to your username and password. For more information, see the
Enable-WSManCredSSP Help topic.
Do you want to enable CredSSP authentication?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

如果你想在工作组（而不是域）中使用 `CredSSP` 身份验证，则需要一个额外的步骤。工作组内的身份验证使用一种称为 `NTLM` 的协议，它不提供 `Kerberos` 那样的安全保证——具体来说，你无法保证你连接的计算机的身份。这是驱动 `TrustedHosts` 配置要求的相同警示，如 Recipe 29.13 所述。要在 `NTLM` 连接上启用 `CredSSP`，请打开 `gpedit.msc`，然后导航到“计算机配置”→“管理模板”→“系统”→“凭据分配”。启用“允许仅使用 NTLM 服务器身份验证委派新凭据”设置，然后将 `wsman/`*`computername`* 添加到支持的计算机列表中。在前面的示例中，这将是 `wsman/leeholmes1c23`。与 `-DelegateComputer` 参数一样，你也可以指定 `wsman/*` 以对所有目标计算机启用该设置。

从服务器端，向 `Enable-WsManCredSSP` cmdlet 指定 `-Role Server`。如果需要，你可以远程调用此 cmdlet：

```
PS > Enable-WsManCredSSP -Role Server

CredSSP Authentication Configuration for WS-Management
CredSSP authentication allows the server to accept user credentials from a
remote computer. If you enable CredSSP authentication on the server, the
server will have access to the username and password of the client computer
if the client computer sends them. For more information, see the
Enable-WSManCredSSP Help topic.
Do you want to enable CredSSP authentication?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
```

远程启用 `CredSSP` 同样简单：

```
PS > Invoke-Command -ComputerName leeholmes1c23 {
     Enable-WsManCredSSP -Role Server }

CredSSP Authentication Configuration for WS-Management
CredSSP authentication allows the user credentials on this computer to be
sent to a remote computer. If you use CredSSP authentication for a
connection to a malicious or compromised computer, that computer will have
access to your username and password. For more information, see the
Enable-WSManCredSSP Help topic.
Do you want to enable CredSSP authentication?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

完成这些配置步骤后，你的远程会话将具有不受限制的网络访问权限。

## 另请参阅

Recipe 29.7, “Program: Remotely Enable PowerShell Remoting”

Recipe 29.13, “Enable Remoting to Workgroup Computers”

# 29.16 将变量传递给远程会话

## 问题

你想要在远程计算机上调用命令，但将其部分信息作为动态参数提供。

## 解决方案

在远程会话上下文中使用来自你计算机的变量时，在变量名前添加 `$USING`：

```
PS > $s = New-PSSession
PS > $myNumber = 10
PS > Invoke-Command $s { 2 * $myNumber }
0
PS > Invoke-Command $s { 2 * $USING:myNumber }
20
```

## 讨论

在远程系统上处理命令时，你有时需要来自本地系统的动态信息——例如变量的值或每次调用都会变化的内容。




# 29.16 将本地变量传递给远程会话

该解决方案给出了这种方法的示例。当你单独提供 `$myNumber` 时，它指的是 `$s` 所代表的远程会话上下文中的 `$myNumber`。如果你添加 `$USING:` 前缀，PowerShell 将从你的本地会话（即创建并控制 `$s` 的那个会话）获取该变量的值。

`$USING:` 前缀是在 PowerShell 3.0 版本中添加的，即使连接到仅支持 PowerShell 2.0 的计算机也能工作。

不过，在某些场景中，你可能需要以 `$USING:` 前缀以外的方式传递数据。示例 29-5 演示了如何解决此问题。在客户端计算机上，我们向用户请求数据（例如凭据）。我们使用该凭据连接到 `RemoteComputer` 并调用命令。在本示例中，该命令本身又建立了另一个连接——这次连接到 `leeholmes1c23`。最后一个命令仅检索远程系统的计算机名称。它不再硬编码用户名和密码（或再次请求它们），而是使用传递给最初的 `Invoke-Command` 调用的 `$cred` 变量。

## 示例 29-5. 通过 ArgumentList 参数传递值

```
PS > $cred = Get-Credential

PS > $command = {
    param($cred)

    Invoke-Command leeholmes1c23 {
        "Hello from $($env:Computername)" } -Credential $cred
}

PS > Invoke-Command RemoteComputer $command -ArgumentList $cred -Credential $cred
Hello from LEEHOLMES1C23
```

为了支持此功能，`Invoke-Command` cmdlet 提供了 `-ArgumentList` 参数。提供给此参数的变量将被转换为适合远程处理的版本，然后可供 `-ScriptBlock` 参数内的命令使用。

### 注意

提供给 `-ArgumentList` 参数的参数在发送到远程计算机之前会经历一个序列化过程。尽管它们的属性与原始对象非常相似，但它们不再拥有方法。有关 PowerShell 序列化的更多信息，请参阅 配方 29.5。

与其他脚本、函数和脚本块中的参数一样，`Invoke-Command` 中使用的脚本块可以直接通过 `$args` 数组访问参数，或通过 `param()` 语句使脚本更易读。然而，与大多数 `param()` 语句不同，这些参数语句必须全为位置参数。不支持命名参数（例如 `-ArgumentList "-Cred","$cred"`），也不支持高级参数属性（如 `[Parameter(Mandatory = $true)]`）。

有关参数和 `param()` 语句的更多信息，请参阅 配方 11.11。

## 另请参阅

- 配方 11.11，“访问脚本、函数或脚本块的参数”
- 配方 29.5，“在远程计算机上调用命令”

# 29.17 在远程计算机上管理和编辑文件

## 问题

你有一台远程计算机，想要从中发送和接收文件。或者，你想直接编辑远程计算机上的文件。

## 解决方案

使用 `Copy-Item` 的 `-ToSession` 和 `-FromSession` 参数，通过活动的 PowerShell 远程会话在计算机之间移动文件：

```
PS > "Hello World" > myfile.txt
PS > $s = New-PSSession -Computername lee-desk
PS > Copy-Item myfile.txt -ToSession $s -Destination c:\temp\copied.txt
PS > Invoke-Command $s { Get-Content c:\temp\copied.txt }
Hello World
PS > Invoke-Command $s { Set-Content c:\temp\copied.txt "Heya!" }
PS > Copy-Item -FromSession $s -Path c:\temp\copied.txt -Destination newfile.txt
PS > Get-Content newfile.txt
Heya!
```

在 PowerShell ISE 或 Visual Studio Code 中，使用 `psedit` 命令直接编辑远程计算机上的文件：

```
====> PowerShell Integrated Console v2020.6.0 <=====

PS > $s = New-PSSession localhost
PS > "Hello World" > c:\temp\hello.txt
PS > Enter-PSSession $s

[localhost]: PS > psedit c:\temp\hello.txt

(... edit the file in the window that pops up ...)

[localhost]: PS > exit

PS > Get-Content c:\temp\hello.txt

Back at you!
```

## 讨论

当你使用远程计算机时，常面临的一个问题是如何将本地工具和环境带到那台计算机上。使用文件共享或 FTP 传输是在系统间共享工具的常用方法，但这些选项并不总是可用。特别是如果你小心地保护了它们，它们很可能未启用 SMB 或远程管理共享。

通常，PowerShell 远程处理是你连接到该计算机的唯一途径。

为了解决这些问题，`Copy-Item` cmdlet 提供了 `-FromSession` 和 `-ToSession` 参数。当你提供一个活动的 PowerShell 远程会话作为这些参数之一时，PowerShell 将自动代表你来回传输文件。

### 注意

当你连接到“刚够用的管理”任务特定端点时，此功能默认处于禁用状态。要安全地启用此功能，你的端点需要启用用户驱动器功能。有关“刚够用的管理”的更多信息，请参阅 配方 18.18。

正如解决方案所示，`psedit` 命令是快速编辑远程计算机上脚本和文件的另一种方式。此命令要求你已使用 PowerShell ISE 或 Visual Studio Code 进入交互式会话。当你运行此命令时，这些宿主程序会将远程文件的副本带回你的计算机。你对本地副本所做的编辑会透明地发送回远程计算机。

Visual Studio Code 甚至在交互式调试会话期间也扩展了这种远程编辑体验。如果你在连接时远程计算机上的脚本触发了调试断点，Visual Studio Code 将自动在 UI 中打开目标脚本的副本，让你以交互方式调试和编辑该脚本。要查看此功能的实际演示，请参阅 配方 14.8。

## 另请参阅

- 配方 14.8，“在远程计算机上调试脚本”
- 配方 18.18，“创建任务特定的远程端点”

# 29.18 配置高级远程处理配额和选项

## 问题

你想为远程会话配置压缩、配置文件、代理身份验证、证书验证或区域性信息。

## 解决方案

对于客户端配置设置，请调用 `New-PSSessionOption` cmdlet 并为你想要自定义的参数提供值：

```
PS > $options = New-PSSessionOption -Culture "fr-CA"
PS > $sess = New-PSSession Lee-Desk -Cred Lee -SessionOption $options
PS > Invoke-Command $sess { Get-Date | Out-String }

20 février 2010 17:40:16
```

对于服务端配置设置，请查看 `WSMan:\localhost\Shell` 和 `WSMan:\localhost\Service` 下的选项：

```
Set-Item WSMan:\localhost\shell\MaxShellsPerUser 10
```

## 讨论

PowerShell 允许你通过两条路径定义高级客户端连接选项：`New-PSSessionOption` cmdlet 和 `$PSSessionOption` 自动变量。

当你调用 `New-PSSession` cmdlet 时，PowerShell 返回一个保存远程会话配置设置的对象。你可以通过该 cmdlet 的参数自定义所有值，或设置返回对象上的属性。

### 注意

有几个选项涉及超时值：`OperationTimeout`、`OpenTimeout`、`CancelTimeout` 和 `IdleTimeout`。这些参数通常不是必需的（例如，即使在调用长时间运行的命令时），但它们可用于克服在遇到极慢或拥塞的网络条件时出现的错误。





# 配置会话选项与 WSMan 设置

如果要为每个新连接配置会话选项，第二种方法是使用 `$PSSessionOption` 自动变量：

```
PS > $PSSessionOption

MaximumConnectionRedirectionCount : 5
NoCompression                     : False
NoMachineProfile                  : False
ProxyAccessType                   : None
ProxyAuthentication               : Negotiate
ProxyCredential                   :
SkipCACheck                       : False
SkipCNCheck                       : False
SkipRevocationCheck               : False
OperationTimeout                  : 00:03:00
NoEncryption                      : False
UseUTF16                          : False
IncludePortInSPN                  : False
OutputBufferingMode               : None
Culture                           :
UICulture                         :
MaximumReceivedDataSizePerCommand :
MaximumReceivedObjectSize         : 209715200
ApplicationArguments              :
OpenTimeout                       : 00:03:00
CancelTimeout                     : 00:01:00
IdleTimeout                       : -00:00:00.0010000
```

如果在连接尝试期间未提供显式设置，PowerShell 远程处理将查看此变量中的值以获取默认值。

从服务器角度来看，所有配置都位于 `WSMan` 驱动器中。最常见的配置选项来自 `WSMan:\localhost\Shell` 路径。这些设置允许你配置每个用户可以同时打开多少个 Shell、它们可以使用多少内存等。

```
PS > dir WSMan:\localhost\Shell

WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Shell

Type            Name                           SourceOfValue   Value
----            ----                           -------------   -----
System.String   AllowRemoteShellAccess                         true
System.String   IdleTimeout                                    7200000
System.String   MaxConcurrentUsers                             10
System.String   MaxShellRunTime                                2147483647
System.String   MaxProcessesPerShell                           25
System.String   MaxMemoryPerShellMB                            1024
System.String   MaxShellsPerUser                               30
```

除了服务器范围的设置外，还可以通过配置单个端点配置（插件）进一步限制这些设置。如果要增加默认值，必须同时在服务器范围级别和用户将连接的端点（如默认的 `Microsoft.PowerShell` 端点）增加配额。

```
PS WSMan:\localhost\Plugin\Restrictive\Quotas> Set-Item MaxShellsPerUser 1

WARNING: The updated configuration is effective only if it is less than or equal
to the value of global quota WSMan:\localhost\Shell\MaxShellsPerUser. Verify the
value for the global quota using the PowerShell cmdlet "Get-Item WSMan:\localhost
\Shell\MaxShellsPerUser".

WARNING: The configuration changes you made will only be effective after the
WinRM service is restarted.  To restart the WinRM service, run the following
command: 'Restart-Service winrm'

PS WSMan:\localhost\Plugin\Restrictive\Quotas> dir

WSManConfig: Microsoft.WSMan.Management\WSMan::...\Plugin\Restrictive\Quotas

Type            Name                           SourceOfValue   Value
----            ----                           -------------   -----
System.String   IdleTimeoutms                                  7200000
System.String   MaxConcurrentUsers                             5
System.String   MaxProcessesPerShell                           15
System.String   MaxMemoryPerShellMB                            1024
System.String   MaxShellsPerUser                               1
System.String   MaxConcurrentCommandsPerShell                  1000
System.String   MaxShells                                      25
System.String   MaxIdleTimeoutms                               43200000
```

## 另请参阅

- 食谱 29.5，“在远程计算机上调用命令”

# 29.19 在多台计算机上调用命令

## 问题

你想要同时管理多台计算机。

## 解决方案

使用 `-ThrottleLimit` 和 `-AsJob` 参数来配置 PowerShell 如何扩展你的命令：

```
PS > $sessions = $(
    New-PSSession localhost;
    New-PSSession localhost;
    New-PSSession localhost)

PS > $start = Get-Date
PS > Invoke-Command $sessions { Start-Sleep 2; "Test $pid" }
Test 720
Test 6112
Test 4792
PS > (Get-Date) - $start | Select TotalSeconds | Format-Table -Auto

TotalSeconds
------------
     2.09375


PS >
PS > $start = Get-Date
PS > Invoke-Command $sessions { Start-Sleep 2; "Test $pid" } -ThrottleLimit 1
Test 6112
Test 4792
Test 720
PS > (Get-Date) - $start | Select TotalSeconds | Format-Table -Auto

TotalSeconds
------------
        6.25
```

## 讨论

传统网络脚本中最大的困难之一在于同时管理多台计算机。远程计算机管理通常受网络限制，因此大多数脚本将大部分时间花在等待网络上。

解决方案是扩展。与其一次管理一台计算机，不如同时管理多台。但也不能太多，因为很少有机器能承受同时连接到数百或数千台远程机器的需求。

尽管有这些好处，但编写支持智能自动限制的网络脚本超出了许多人的能力范围，也远远超出了大多数人“待办事项大清单”的范围。幸运的是，PowerShell 远程处理的主要重点是解决这些常见问题，限制也不例外。

默认情况下，PowerShell 远程处理一次连接 32 台计算机。在列表中的前 32 台计算机上运行命令后，它会等待命令完成，然后在其他计算机上运行命令。每完成一个命令，PowerShell 就会调用下一个等待的命令。

为了演示这种自动扩展，解决方案展示了使用默认限制调用 `Invoke-Command` 与使用限制为一台计算机调用它的区别。

当同时针对许多计算机工作时，你可能希望在长时间运行的任务在后台处理时继续使用 Shell。为了支持任务的后台处理，`Invoke-Command` cmdlet 提供了 `-AsJob` 参数，允许你将命令作为 PowerShell 作业运行。

有关 PowerShell 作业的更多信息，请参阅 食谱 1.6。

## 另请参阅

- 食谱 1.6，“调用长时间运行或后台命令”
- 食谱 29.5，“在远程计算机上调用命令”

# 29.20 在远程计算机上运行本地脚本

## 问题

你有一个本地脚本，想要在远程计算机上运行它。

## 解决方案

使用 `Invoke-Command` cmdlet 的 `-FilePath` 参数：

```
PS > Get-Content .\Get-ProcessByName.ps1
param($name)

Get-Process -Name $name

PS > Invoke-Command -Computername Lee-Desk `
    -FilePath .\Get-ProcessByname.ps1 -ArgumentList PowerShell `
    -Cred Lee

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id Process   PSComputer
                                                          Name      Name
-------  ------    -----      ----- -----   ------     -- --------- ---------
    628      17    39084      58908   214     4.26   7540 powers... lee-des...
```

## 讨论

对于快速的一次性操作，`Invoke-Command` cmdlet 的 `-ScriptBlock` 参数让你可以轻松针对远程计算机调用命令：




```
PS > Invoke-Command Lee-Desk { Get-Process -n PowerShell } -Cred Lee

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id Process   PSComputer
                                                          Name      Name
-------  ------    -----      ----- -----   ------     -- --------- ---------
    628      17    39084      58908   214     4.26   7540 powers... lee-des...
```

然而，当这些命令变得更加复杂时，将它们全部写在脚本块中会变得笨重。你无法获得语法高亮、行号，也无法享受基于脚本执行所提供的其他便利功能。

为了让你能针对远程计算机编写脚本，PowerShell 在 `Invoke-Command` cmdlet 上提供了 `-FilePath` 参数。当你使用此参数时，PowerShell 会从磁盘读取脚本，并在远程计算机上调用其内容。

在此模式下，PowerShell 不会尝试在该过程中解决依赖关系。如果你的脚本需要任何其他脚本、命令或环境依赖项，请确保它们在远程计算机上可用。

有关将项目传输到远程计算机的一种选项，请参阅 配方 29.17。

## 另请参阅

配方 29.5，“在远程计算机上调用命令”

配方 29.17，“管理和编辑远程计算机上的文件”

# 29.21 确定脚本是否在远程计算机上运行

## 问题

你有一个脚本，需要知道它是在本地还是远程计算机上运行。

## 解决方案

查看 `$host.Name` 属性的输出。如果是 `ServerRemoteHost`，则表示正在远程运行。如果是其他值，则表示在本地运行。

```
PS > $host.Name
ConsoleHost

PS > Invoke-Command leeholmes1c23 { $host.Name }
ServerRemoteHost
```

## 讨论

虽然你的脚本无论是在本地还是远程运行都应该能正常工作，但你可能会遇到需要验证脚本在哪种环境下启动的情况。

`$host` 自动变量公开当前宿主的信息，PowerShell 远程处理就是其中之一。当你在远程会话中访问此变量时，其值为 `ServerRemoteHost`。虽然控制台宿主上的值是 `ConsoleHost`，但你不应将其作为本地脚本的指示器。还有许多其他 PowerShell 宿主——例如 Visual Studio Code（其中 `$host` 为 `Visual Studio Code Host`）、PowerGUI、PowerShell Plus 等。每个都有自定义的宿主名称，但没有一个是 `ServerRemoteHost`。

有关 `$host` 自动变量的更多信息，请参阅 配方 13.9。

## 另请参阅

配方 13.9，“访问宿主 UI 的功能”
