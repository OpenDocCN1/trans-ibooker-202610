
# Get-InstalledSoftware 脚本

出自 *PowerShell Cookbook*（O'Reilly），作者 Lee Holmes（http://www.leeholmes.com/guide）

## 概要

列出当前计算机上安装的软件。

## 示例

```powershell
PS > Get-InstalledSoftware *Frame* | Select DisplayName

DisplayName
-----------
Microsoft .NET Framework 3.5 SP1
Microsoft .NET Framework 3.5 SP1
Hotfix for Microsoft .NET Framework 3.5 SP1 (KB953595)
Hotfix for Microsoft .NET Framework 3.5 SP1 (KB958484)
Update for Microsoft .NET Framework 3.5 SP1 (KB963707)
```

## 参数

```powershell
param(
    ## 要搜索的软件名称
    $DisplayName = "*"
)
```

## 脚本主体

```powershell
Set-StrictMode -Off

## 获取 Uninstall 键中列出的所有软件
$keys =
    Get-ChildItem HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall

## 获取这些项的所有属性
$items = $keys | Foreach-Object { Get-ItemProperty $_.PsPath }

## 对于每个项，显示 DisplayName 和 Publisher
foreach($item in $items)
{
    if(($item.DisplayName) -and ($item.DisplayName -like $displayName))
    {
        $item
    }
}
```

有关在 PowerShell 中使用 Windows 注册表的详细信息，请参阅[第 21 章](#ch21.xhtml#the_windows_registry)。有关运行脚本的详细信息，请参阅[食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [第 21 章](#ch21.xhtml#the_windows_registry)

# 27.10 卸载应用程序

## 问题

你想要卸载特定的软件应用程序。

## 解决方案

要卸载应用程序，请使用[食谱 27.9](#ch27.xhtml#program_list_all_installed_software)中提供的 `Get-InstalledSoftware` 脚本来检索卸载软件的命令。由于 `UninstallString` 使用批处理文件语法，请使用 `cmd.exe` 启动卸载程序：

```powershell
PS > $software = Get-InstalledSoftware UnwantedProgram
PS > cmd /c $software.UninstallString
```

或者，使用 `Win32_Product` WMI 类进行无人值守安装：

```powershell
$application = Get-CimInstance Win32_Product -filter "Name='UnwantedProgram'"
$application | Invoke-CimMethod -Name Uninstall
```

## 讨论

应用程序提供的 `UninstallString` 会启动交互式体验，这与你通过控制面板中的“添加/删除程序”项卸载应用程序时看到的一样。如果你需要以无人值守的方式删除软件，有两个选项：使用应用程序卸载程序的“静默模式”（例如，`msiexec.exe` 的 `/quiet` 开关），或使用解决方案中演示的 `Win32_Product` WMI 类的软件移除功能。

有关在 PowerShell 中使用 WMI 的详细信息，请参阅[食谱 28.1](#ch28.xhtml#access_windows_management_instrumentatio)。

## 另请参阅

- [食谱 27.9，“程序：列出所有已安装软件”](#ch27.xhtml#program_list_all_installed_software)
- [食谱 28.1，“访问 Windows 管理规范和 CIM 数据”](#ch28.xhtml#access_windows_management_instrumentatio)

# 27.11 管理计算机还原点

## 问题

你想要创建计算机还原点、将计算机还原到之前的还原点，或管理自动还原点的计划。

## 解决方案

使用 `Enable-ComputerRestore` 和 `Disable-ComputerRestore` cmdlet 启用和禁用自动计算机检查点。使用 `Get-ComputerRestorePoint` 和 `Restore-Computer` cmdlet 分别列出所有还原点并将计算机还原到其中之一。使用 `Checkpoint-Computer` cmdlet 创建新的系统还原点。

```powershell
PS > Get-ComputerRestorePoint |
    Select Description,SequenceNumber,RestorePointType |
    Format-Table -Auto

Description           SequenceNumber RestorePointType
-----------           -------------- ----------------
Windows Update                   122                0
Windows Update                   123                0
Scheduled Checkpoint             124                7
Scheduled Checkpoint             125                7
Windows Update                   126                0
Scheduled Checkpoint             127                7
Scheduled Checkpoint             128                7
Windows Update                   129                0
Scheduled Checkpoint             130                7
Windows Update                   131                0
Scheduled Checkpoint             132                7
Windows Update                   133                0
Manual Checkpoint                134                0
Before driver updates            135                0

PS > Checkpoint-Computer "Before driver updates"
```

## 讨论

计算机还原点 cmdlet 为你提供了一种管理 Windows 系统还原点的简便方法。你可以使用 `Checkpoint-Computer` 在可能造成破坏的安装或系统更改之前创建新的还原点。[图 27-1](#ch27.xhtml#managing_restore_points) 显示了正在进行的 `Checkpoint-Computer` cmdlet。如果你需要将计算机还原到以前的状态，可以使用 `Get-ComputerRestorePoint` cmdlet 列出现有的还原点，然后使用 `Restore-Computer` cmdlet 将计算机还原到其先前保存的状态。

![](Images/wps4_2701.png)

系统还原点经过精细调整，旨在管理操作系统的状态，并非设计用于保护用户数据。系统还原点主要保护 Windows 注册表、核心操作系统文件、本地用户配置文件以及 COM 和 WMI 注册数据库。

为了节省磁盘空间，Windows 限制还原点占用的空间量，并根据需要删除最旧的还原点。如果你计划比 Windows 自动安排的频率更频繁地创建手动检查点，请考虑增加专用于系统还原点的空间量。如果不这样做，你就有无法从那些你花费很长时间才检测到的系统错误中恢复的风险。

默认情况下，Windows 为主系统卷安排自动还原点。要为此卷（或任何卷）启用或禁用这些自动检查点，请使用 `Enable-ComputerRestore` 和 `Disable-ComputerRestore` cmdlet。

控制面板允许你配置 Windows 为还原点保留多少空间。为此，请在控制面板中打开“系统”组，然后打开“系统保护”。

# 27.12 重启或关闭计算机

## 问题

你想要重启或关闭本地或远程计算机。

## 解决方案

使用 `Restart-Computer` cmdlet 重启计算机：

```powershell
PS > Restart-Computer -ComputerName Computer
```

使用 `Stop-Computer` cmdlet 将其完全关闭：

```powershell
PS > Stop-Computer -ComputerName Computer
```

如果你想要在许多计算机上执行相同的操作，请使用 cmdlet 的限流支持：

```powershell
PS > $computers = Get-Content computers.txt
PS > Restart-Computer -ComputerName $computers -ThrottleLimit
```

## 讨论

`Restart-Computer` 和 `Stop-Computer` cmdlet 都允许你管理本地或远程计算机的重启和关闭过程。因为它们建立在 PowerShell 的 WMI 支持之上，所以它们还提供 `-ThrottleLimit` 参数，让你控制同时控制多少台计算机。

默认情况下，如果有用户登录到计算机，这些 cmdlet 会拒绝重启或关闭。要仍然重启计算机，请使用 `-Force` 参数覆盖此行为。

> **注意**
>
> 重启计算机时，你有时可能希望计算机在重新联机后执行某些操作。为此，请创建一个带有 `-AtStartup` 触发器的新计划任务。有关详细信息，请参阅[食谱 27.14](#ch27.xhtml#manage_scheduled_tasks_on_a_computer)。
