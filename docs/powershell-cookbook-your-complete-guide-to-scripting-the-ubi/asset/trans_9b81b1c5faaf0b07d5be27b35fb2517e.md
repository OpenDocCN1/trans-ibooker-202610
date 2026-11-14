

```powershell
$path = "HKLM:\Software\Policies\Microsoft\Windows\" +
    "PowerShell\ConsoleSessionConfiguration"
if(-not (Test-Path $path)) { New-Item $path -Force }
Set-ItemProperty $path ConsoleSessionConfigurationName Jumpbox
Set-ItemProperty $path EnableConsoleSessionConfiguration 1
```

## 讨论

当你试图配置一个高度安全的系统（如跳板机或自助终端）时，有时可能需要向交互用户公开额外的高级功能。虽然严格锁定系统的传统方法是阻止所有命令行访问，但这要求你为每个需要公开的额外任务编写自定义的一次性应用程序。编写自定义脚本和 PowerShell 函数当然更容易，但你可能不愿意直接暴露 `cmd` 或 PowerShell。

更重要的是，如果你想向非管理员用户公开任何需要管理员访问权限的功能，传统应用程序就完全派不上用场了。要让应用程序以管理员权限运行，用户要么必须是管理员，要么你必须以某种方式让他们成为临时管理员，并严格控制他们在此期间能做什么。

虽然这看似是一个难以逾越的挑战，但 PowerShell 已经拥有一项主要功能，能让你在很大程度上解决这个问题：*刚好足够的管理* (JEA)。JEA 主要面向远程场景，即你希望让用户在服务器上执行管理任务，而不必将他们设为服务器上的管理员。有关创建任务特定远程终结点的详细信息，请参阅[食谱 18.18](#ch18.xhtml#create_task_specific_endpoint)。使用 JEA，你可以创建任务特定的终结点，以实现你想向用户公开的所有功能。

将其应用于交互式使用的最后阶段是通过 PowerShell 的 `ConsoleSessionConfiguration` 策略。当你将其设置为 PowerShell JEA 配置名称时，所有 PowerShell 控制台实例都将连接到该终结点，而不是加载本地交互式 Shell——这类似于用户直接运行 `Enter-PSSession` 连接到该会话。

创建此策略后，它将应用于该计算机上的所有用户。借助 JEA 中动态角色定义的强大功能，你可以轻松设置此终结点，使其根据连接的用户调整其行为。下面是一个为终结点创建“Unrestricted”（无限制）用途角色功能的示例——你可能希望将其公开给预期为无限制管理员的本地用户：

```powershell
$modulePath = Join-Path $env:ProgramFiles "WindowsPowerShell\Modules\Unrestricted"
$null = New-Item -ItemType Directory -Path $modulePath
New-ModuleManifest -Path (Join-Path $modulePath Unrestricted.psd1)

## Create an initialization script for the Unrestricted role that
## re-enables Full Language mode
Set-Content -Path (Join-Path $modulePath init.ps1) -Value @"
`$null = [PowerShell]::Create().AddScript(@'
    param(`$rs)
    while(`$rs.RunspaceAvailability -ne `"Available`") {
        Start-Sleep -Milliseconds 500 }
    `$rs.LanguageMode = `"FullLanguage`"
'@).AddArgument([Runspace]::DefaultRunspace).BeginInvoke()
"@

# Create the RoleCapabilities folder and copy in the PSRC file
$rcFolder = Join-Path $modulePath RoleCapabilities
$null = New-Item -ItemType Directory $rcFolder

New-PSRoleCapabilityFile -Path Unrestricted.psrc  -VisibleAliases * `
    -VisibleCmdlets * -VisibleFunctions * -VisibleExternalCommands * `
    -VisibleProviders * -ScriptsToProcess (Join-Path $modulePath init.ps1)

Copy-Item -Path .\Unrestricted.psrc -Destination $rcFolder
```

有了此角色功能，你可以将其与受限角色功能结合使用（例如，来自[食谱 18.18](#ch18.xhtml#create_task_specific_endpoint) 的 `InventoryReader` 功能）：

```powershell
New-PSSessionConfigurationFile -Path c:\temp\jumpbox.pssc `
    -RoleDefinitions @{
        'Users' = @{ RoleCapabilities = 'InventoryReader' }
        'Administrators' = @{ RoleCapabilities = 'Unrestricted' }
     } -SessionType RestrictedRemoteServer -RunAsVirtualAccount
Register-PSSessionConfiguration -Name Jumpbox `
    -Path c:\temp\jumpbox.pssc -Force
```

与所有 JEA 终结点一样，请注意，某些编码错误——主要是与代码注入相关的错误——很容易成为安全漏洞。有关如何审核和防范代码注入风险的信息，请参阅[食谱 18.20](#ch18.xhtml#detect_prevent_code_injection)。

有关 PowerShell 远程处理的详细信息，请参阅[第 29 章](#ch29.xhtml#chapter_remoting)。

## 另请参阅

[食谱 18.18，“创建任务特定的远程终结点”](#ch18.xhtml#create_task_specific_endpoint)

[食谱 18.20，“检测和防止代码注入漏洞”](#ch18.xhtml#detect_prevent_code_injection)

[第 29 章](#ch29.xhtml#chapter_remoting)

# 18.20 检测和防止代码注入漏洞

## 问题

你有一个跨安全边界公开的脚本或函数（例如 JEA 终结点），并希望确保它不易受代码注入攻击。

## 解决方案

从 PowerShell 库安装并运行 `InjectionHunter` 模块。

```powershell
Install-Module -Name PSScriptAnalyzer -Scope CurrentUser -Force
Install-Module -Name InjectionHunter -Scope CurrentUser -Force

@'
function Set-PersonalDisplayName($DisplayName)
{
    $c = "Set-AdUser -UserPrincipalName DOMAIN\user -DisplayName $DisplayName"
    Invoke-Expression $c
}
'@ > $env:TEMP\injectable.ps1

Invoke-ScriptAnalyzer -Path $env:TEMP\injectable.ps1 `
    -CustomRulePath (Get-Module InjectionHunter -List).Path
```

```
RuleName                       ScriptName     Line Message
--------                       ----------     ---- -------
InjectionRisk.InvokeExpression injectable.ps1    4 Possible script injection risk
                                                   via the Invoke-Expression
                                                   cmdlet. Untrusted input can
                                                   cause arbitrary PowerShell
                                                   expressions to be run.
                                                   (...)
```

## 讨论

JEA 和相关功能中令人难以置信的强大之处之一在于，它们让你能够有效地减少组织中拥有广泛特权的管理员数量。当你有员工只需要帮助用户重置遗忘的密码时，将他们全部设为域管理员会极大地增加操作风险。

JEA 通过实现*受信任子系统*让你能极大地改善这一状况——经过批准的低权限用户可以连接到该终结点，但操作本身（作为你编写的脚本或函数公开）是在高度特权的上下文中执行的。这种低信任与高信任之间的转换称为*信任边界*，而你向等式低信任一侧公开的任何代码称为*攻击面*，并成为潜在的安全风险。

PowerShell 脚本可能成为攻击面一部分的情况有多种：

*   你在 JEA 终结点中公开的函数或脚本
*   作为对管理 Web UI 响应而运行的辅助脚本
*   部署了 Windows Defender 应用程序控制的系统上的签名脚本

作为信任边界一部分的 PowerShell 脚本中，导致安全漏洞的最常见原因称为*脚本注入*：即你编写的代码以不安全的方式合并了用户输入。这随后允许不受信任的用户代码在攻击面的“高信任”侧运行。这类问题以各种方式出现在涉及信任边界的每项技术中：SQL 注入、跨站脚本和缓冲区溢出只是其他几个例子。

