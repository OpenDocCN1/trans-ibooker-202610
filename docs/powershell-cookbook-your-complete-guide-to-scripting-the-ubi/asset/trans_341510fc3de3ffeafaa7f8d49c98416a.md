
# Get-RemoteRegistryKeyProperty.ps1

```powershell
##############################################################################
##
## Get-RemoteRegistryKeyProperty
##
## 源自 PowerShell 实战指南 (O'Reilly)
## 作者 Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

获取远程注册表键属性的值

.EXAMPLE

PS > $registryPath =
     "HKLM:\software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell"
PS > Get-RemoteRegistryKeyProperty LEE-DESK $registryPath ExecutionPolicy

#>

param(
    ## 要连接的计算机
    [Parameter(Mandatory = $true)]
    $ComputerName,

    ## 要检索的注册表项路径
    [Parameter(Mandatory = $true)]
    $Path,

    ## 要检索的特定属性
    $Property = "*"
)

Set-StrictMode -Version 3

## 验证并提取注册表键
if($path -match "^HKLM:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "LocalMachine", $computername)
}
elseif($path -match "^HKCU:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "CurrentUser", $computername)
}
else
{
    Write-Error ("请指定要打开的注册表键的完整注册表路径 " +
        "(即：HKLM:\Software)。")
    return
}

## 打开键
$key = $baseKey.OpenSubKey($matches[1])
$returnObject = New-Object PsObject

## 遍历键中的每个属性
foreach($keyProperty in $key.GetValueNames())
{
    ## 如果属性与搜索词匹配，将其作为属性添加到输出中
    if($keyProperty -like $property)
    {
        $returnObject |
            Add-Member NoteProperty $keyProperty $key.GetValue($keyProperty)
    }
}

## 返回结果对象
$returnObject

## 关闭键和基键
$key.Close()
$baseKey.Close()
```

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 21.15 程序：设置远程注册表键的属性

虽然 PowerShell 不直接允许你访问和操作远程计算机的注册表，但它仍通过使用 .NET Framework 来支持此功能。.NET Framework 暴露的功能比我们期望的更偏向开发人员，因此我们可以改用脚本使其更易于使用。

[示例 21-8](#ch21.xhtml#set-remoteregistrykeypropertyps1) 允许你设置给定远程注册表键上某个属性的值。要使此脚本成功，目标计算机必须启用并运行远程注册表服务。

## 示例 21-8. Set-RemoteRegistryKeyProperty.ps1

```powershell
##############################################################################
##
## Set-RemoteRegistryKeyProperty
##
## 源自 PowerShell 实战指南 (O'Reilly)
## 作者 Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

设置远程注册表键属性的值

.EXAMPLE

PS >$registryPath =
    "HKLM:\software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell"
PS >Set-RemoteRegistryKeyProperty LEE-DESK $registryPath `
      "ExecutionPolicy" "RemoteSigned"

#>

param(
    ## 要连接的计算机
    [Parameter(Mandatory = $true)]
    $ComputerName,

    ## 要修改的注册表路径
    [Parameter(Mandatory = $true)]
    $Path,

    ## 要修改的属性
    [Parameter(Mandatory = $true)]
    $PropertyName,

    ## 要在属性上设置的值
    [Parameter(Mandatory = $true)]
    $PropertyValue
)

Set-StrictMode -Version 3

## 验证并提取注册表键
if($path -match "^HKLM:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "LocalMachine", $computername)
}
elseif($path -match "^HKCU:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "CurrentUser", $computername)
}
else
{
    Write-Error ("请指定要打开的注册表键的完整注册表路径 " +
        "(即：HKLM:\Software)。")
    return
}

## 打开键并设置其值
$key = $baseKey.OpenSubKey($matches[1], $true)
$key.SetValue($propertyName, $propertyValue)

## 关闭键和基键
$key.Close()
$baseKey.Close()
```

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 21.16 发现程序的注册表设置

## 问题

你想自动化程序的配置，但该程序未记录其注册表配置设置。

## 解决方案

要发现程序的注册表设置，请使用 Sysinternals Process Monitor 观察该程序的注册表访问。Process Monitor 可在 [Sysinternals 下载站点](https://oreil.ly/KGiqe) 获取。

## 讨论

在理想世界中，所有程序都将完全支持通过 PowerShell cmdlet 进行命令行管理和配置。然而，许多程序并不支持，因此解决方法是查阅其文档，希望它们列出了控制其设置的注册表键和属性。虽然许多程序记录了其注册表配置设置，但仍有许多程序没有。

尽管这些程序可能没有记录其注册表设置，但通常可以通过观察其注册表访问活动来确定它们使用的注册表路径。为了演示这一点，我们将使用 Sysinternals Process Monitor 来发现 Windows PowerShell 的执行策略配置键。虽然 Windows PowerShell 记录了这些键，*且*使其自动配置变得轻而易举，但此例说明了通用技术。

#### 注意

PowerShell（相对于 Windows PowerShell）使用配置文件（`powershell.config.json`）来存储这些设置，而不是注册表。如果你想跟随操作，请务必使用 Windows PowerShell！

### 启动并配置 Process Monitor

下载 Process Monitor 后，第一步是筛选其输出，使其仅包含你感兴趣的程序。默认情况下，Process Monitor 会记录系统上几乎所有的注册表和文件活动。

首先，启动 Process Monitor，然后按 Ctrl+E（或单击放大镜图标）暂时阻止其捕获任何数据（请参阅[图 21-2](#ch21.xhtml#process_monitor_ready_to_capture)）。接下来，按 Ctrl+X（或单击带有橡皮擦图标的白纸图标）以清除其自动捕获的额外信息。最后，将目标图标拖放到相关应用程序上。你可以按 Ctrl+L（或单击漏斗图标）查看 Process Monitor 当前应用于其输出的筛选器。

![](Images/wps4_2102.png)

### 准备手动设置配置选项

接下来，准备手动设置程序的配置选项。通常这意味着输入并单击所有属性设置，但暂不单击“确定”或“应用”。对于此 PowerShell 示例，请键入 `Set-ExecutionPolicy` 命令行，但不要按 Enter 键（请参阅[图 21-3](#ch21.xhtml#preparing_to_apply_the_configuration_opt)）。

![](Images/wps4_2103.png)

### 让 Process Monitor 开始捕获信息

切换到 Process Monitor 窗口，然后按 Ctrl+E（或单击放大镜图标）。Process Monitor 现在会捕获相关程序的所有注册表访问。

### 手动设置配置选项

单击“确定”、“应用”或执行任何完成程序配置所需的操作。对于 PowerShell 示例，这意味着按 Enter 键。

### 让 Process Monitor 停止捕获信息
