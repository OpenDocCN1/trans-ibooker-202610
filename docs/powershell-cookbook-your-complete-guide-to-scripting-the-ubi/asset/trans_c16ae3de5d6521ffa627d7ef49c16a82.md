
当在企业中跨 Windows 系统工作时，经常会出现这个问题：“如何在 PowerShell 中执行<某任务>？”在管理员的理想世界里，任何设计具有管理意义功能的人也会支持（通过 PowerShell cmdlet）管理该功能的任务。不过，许多管理任务存在的时间比 PowerShell 还长，所以答案有时会是：“和 PowerShell 出现之前一样的方式。”

这并不是说 PowerShell 的引入不会改善你作为管理员的生活。PowerShell 出现之前的管理任务通常属于以下几种模型之一：命令行实用程序、Windows Management Instrumentation (WMI) 交互、注册表操作、文件操作、与 COM 对象交互，或与 .NET 对象交互。

PowerShell 使与所有这些任务模型的交互变得更加容易，因此也使管理依赖于它们的功能变得更加容易。

# 27.1 将计算机加入域或工作组

## 问题

你想要将计算机加入域或工作组。

## 解决方案

使用 `Add-Computer` cmdlet 的 `-DomainName` 参数将计算机加入域。使用 `-WorkGroupName` 参数将其加入工作组：

```powershell
PS > Add-Computer -DomainName MyDomain -Credential MyDomain\MyUser
PS > Restart-Computer
```

## 讨论

`Add-Computer` cmdlet 的名称相当直观：它允许你将计算机加入域或工作组。由于域加入只有在重启计算机后才生效，因此在加入域后始终调用 `Restart-Computer` cmdlet。

除了支持本地计算机外，`Add-Computer` cmdlet 还允许你将远程计算机加入域。将远程计算机加入域要求其已启用 WMI，并且你在远程计算机上拥有管理权限。

`Add-Computer` cmdlet 或许最复杂的参数是 `-Unsecure` 参数。当你将计算机加入域时，通常会创建一个具有唯一密码的机器账户。非安全加入（由 `-Unsecure` 参数启用）则改用默认密码：计算机名称的前 14 个字符，全部小写。域加入完成后，系统会自动更改密码。此参数主要用于无人值守安装。

要将计算机从域中移除，请参阅[配方 27.2](#ch27.xhtml#remove_computer_domain_workgroup)。

## 另请参阅

[配方 27.2](#ch27.xhtml#remove_computer_domain_workgroup)

# 27.2 将计算机从域中移除

## 问题

你想要将计算机从域中移除。

## 解决方案

使用 `Remove-Computer` cmdlet 离开域。

```powershell
PS > Remove-Computer
PS > Restart-Computer
```

## 讨论

`Remove-Computer` cmdlet 允许你将计算机从域中移除。除了支持本地计算机外，`Remove-Computer` cmdlet 还允许你移除远程计算机。将远程计算机从域中移除要求其已启用 WMI，并且你在远程计算机上拥有管理权限。

一旦你将计算机从域中移除，它将恢复到其默认工作组。由于域更改只有在重启计算机后才生效，因此在离开域后始终调用 `Restart-Computer` cmdlet。

一旦你将计算机从域中移除，你将无法再使用域凭据来管理该计算机。在离开域之前，请确保你知道（或创建）该计算机的本地管理员账户。

要重新加入域，请参阅[配方 27.1](#ch27.xhtml#join_computer_domain_workgroup)。

## 另请参阅

[配方 27.1](#ch27.xhtml#join_computer_domain_workgroup)

# 27.3 重命名计算机

## 问题

你想要重命名工作组或域中的计算机。

## 解决方案

使用 `Rename-Computer` cmdlet 重命名计算机。

```powershell
PS > Rename-Computer -NewName <NewName>
PS > Restart-Computer
```

## 讨论

`Rename-Computer` 允许你重命名计算机。除了支持本地计算机外，`Rename-Computer` cmdlet 还允许你重命名远程计算机。从域中重命名远程计算机要求其已启用 WMI，并且你在远程计算机上拥有管理权限。

由于名称更改只有在重启计算机后才生效，因此在重命名计算机后始终调用 `Restart-Computer` cmdlet。

# 27.4 程序：列出用户的登录或注销脚本

Windows 中的组策略系统将登录和注销脚本存储在两个注册表键下：`HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\State\<UserSID>\Scripts\Logon` 和 `HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\State\<UserSID>\Scripts\Logoff`。每个键都有一个子键，对应每个应用的组策略对象。这些子键中的每一个都有另一层键，对应应用于用户的单个脚本。

当你不知道相关用户的安全标识符 (SID) 时，这可能很难调查，因此[示例 27-1](#ch27.xhtml#get-userlogonlogoffscriptps1) 自动完成用户名到 SID 的映射，以及访问此信息所需的所有注册表操作任务。

### 示例 27-1. Get-UserLogonLogoffScript.ps1

```powershell
##############################################################################
##
## Get-UserLogonLogoffScript
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Get the logon or logoff scripts assigned to a specific user

.EXAMPLE

Get-UserLogonLogoffScript LEE-DESK\LEE Logon
Gets all logon scripts for the user 'LEE-DESK\Lee'

#>

param(
    ## The username to examine
    [Parameter(Mandatory = $true)]
    $Username,

    [Parameter(Mandatory = $true)]
    [ValidateSet("Logon","Logoff")]
    $ScriptType
)

Set-StrictMode -Version 3

## Find the SID for the username
$account = New-Object System.Security.Principal.NTAccount $username
$sid =
    $account.Translate([System.Security.Principal.SecurityIdentifier]).Value

## Map that to their group policy scripts
$registryKey = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\" +
    "Group Policy\State\$sid\Scripts"

if(-not (Test-Path $registryKey))
{
    return
}

## Go through each of the policies in the specified key
foreach($policy in Get-ChildItem $registryKey\$scriptType)
{
    ## For each of the scripts in that policy, get its script name
    ## and parameters
    foreach($script in Get-ChildItem $policy.PsPath)
    {
        Get-ItemProperty $script.PsPath | Select Script,Parameters
    }
}
```

有关在 PowerShell 中使用 Windows 注册表的更多信息，请参阅[第 21 章](#ch21.xhtml#the_windows_registry)。有关运行脚本的更多信息，请参阅[配方 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[配方 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[第 21 章](#ch21.xhtml#the_windows_registry)

# 27.5 程序：列出计算机的启动或关机脚本

Windows 中的组策略系统将启动和关机脚本存储在注册表键 `HKLM:\SOFTWARE\Policies\Microsoft\Windows\System\Scripts\Startup` 和 `HKLM:\SOFTWARE\Policies\Microsoft\Windows\System\Scripts\Shutdown` 下。每个键都有一个子键，对应每个应用的组策略对象。这些子键中的每一个都有另一层键，对应应用于计算机的单个脚本。

[示例 27-2](#ch27.xhtml#get-machinestartupshutdownscriptps1) 允许你轻松检索和访问计算机的启动和关机脚本。

### 示例 27-2. Get-MachineStartupShutdownScript.ps1
