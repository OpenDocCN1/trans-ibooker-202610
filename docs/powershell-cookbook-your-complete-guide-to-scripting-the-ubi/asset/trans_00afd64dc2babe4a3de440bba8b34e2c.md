

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

