
# 计划任务网络访问

如果计划任务在尝试访问网络位置时失败，可以指定 `-RequireNetwork` 参数来解决此问题。如果使用此参数，在注册计划作业时还必须提供凭据。如果更改了密码，请务必使用 `Set-ScheduledJob` cmdlet 更新凭据。

除了在系统上计划新命令外，您可能还想与其他程序或应用程序计划的任务进行交互。其他程序计划的任务没有 PowerShell 那样丰富的作业模型，但经常用于简单的任务调用。如果您有权访问 Windows 8 计算机，请使用 `ScheduledTask` cmdlet：

```
PS > Get-Command -Noun ScheduledTask
(...)

PS > Get-Help -Command Register-ScheduledTask
(...)

PS > Get-ScheduledTask -TaskName ProactiveScan | Format-List

Actions            : {MSFT_TaskComHandlerAction}
Author             : Microsoft Corporation
Date               :
Description        : NTFS Volume Health Scan
Documentation      :
Principal          : MSFT_TaskPrincipal2
SecurityDescriptor : D:AI(A;;FA;;;BA)(A;;FA;;;SY)(A;;FRFX;;;LS)(A;;FR;;;AU)
Settings           : MSFT_TaskSettings3
Source             : Microsoft Corporation
State              : Ready
TaskName           : ProactiveScan
TaskPath           : \Microsoft\Windows\Chkdsk\
Triggers           :
URI                : \Microsoft\Windows\Chkdsk\ProactiveScan
Version            :
PSComputerName     :
```

有关从其他应用程序自动运行 PowerShell 的详细信息，请参阅 [食谱 1.17](#ch01.xhtml#invoke_a_powershell_script_from_outside_)。

## 另请参阅

[食谱 1.6，“调用长时间运行或后台命令”](#ch01.xhtml#invoke-long-running-command)

[食谱 1.17，“从 PowerShell 外部调用 PowerShell 命令或脚本”](#ch01.xhtml#invoke_a_powershell_script_from_outside_)

# 27.15 检索打印机信息

## 问题

您想要获取当前系统上打印机的信息。

## 解决方案

要检索连接到系统的打印机的信息，请使用 `Win32_Printer` WMI 类：

```
PS > Get-CimInstance Win32_Printer | Select-Object Name,PrinterStatus

Name                                                         PrinterStatus
----                                                         -------------
Microsoft Office Document Image Wr...                                    3
Microsoft Office Document Image Wr...                                    3
CutePDF Writer                                                           3
Brother DCP-1000                                                         3
```

要检索特定打印机的信息，请根据其名称应用筛选器：

```
PS > $device = Get-CimInstance Win32_Printer -Filter "Name='Brother DCP-1000'"
PS > $device | Format-List *
Status                      : Unknown
Name                        : Brother DCP-1000
Attributes                  : 588
Availability                :
AvailableJobSheets          :
AveragePagesPerMinute       : 0
Capabilities                : {4, 2, 5}
CapabilityDescriptions      : {Copies, Color, Collate}
Caption                     : Brother DCP-1000
(...)
```

要检索特定属性，请像访问其他 PowerShell 对象的属性一样访问它们：

```
PS > $device.VerticalResolution
600
PS > $device.HorizontalResolution
600
```

## 讨论

解决方案中的示例使用 `Win32_Printer` WMI 类来检索计算机上已安装打印机的信息。虽然 `Win32_Printer` 类提供了对最常用信息的访问，但 WMI 还支持其他几个与打印机相关的类：`Win32_TCPIPPrinterPort`、`Win32_PrinterDriver`、`CIM_Printer`、`Win32_PrinterConfiguration`、`Win32_PrinterSetting`、`Win32_PrinterController`、`Win32_PrinterShare` 和 `Win32_PrinterDriverDll`。有关在 PowerShell 中使用 WMI 的详细信息，请参阅 [食谱 28.1](#ch28.xhtml#access_windows_management_instrumentatio)。

## 另请参阅

[食谱 28.1，“访问 Windows 管理规范和 CIM 数据”](#ch28.xhtml#access_windows_management_instrumentatio)

# 27.16 检索打印机队列统计信息

## 问题

您想要获取当前系统上打印机的打印队列信息。

## 解决方案

要检索连接到系统的打印机的信息，请使用 `Win32_PerfFormattedData_Spooler_PrintQueue` WMI 类：

```
PS > Get-CimInstance Win32_PerfFormattedData_Spooler_PrintQueue |
     Select Name,TotalJobsPrinted

Name                                                      TotalJobsPrinted
----                                                      ----------------
Microsoft Office Document Image Wr...                                    0
Microsoft Office Document Image Wr...                                    0
CutePDF Writer                                                           0
Brother DCP-1000                                                         2
_Total                                                                   2
```

要检索特定打印机的信息，请根据其名称应用筛选器，如 [示例 27-4](#ch27.xhtml#retrieving_information_about_a_specific_) 所示。

### 示例 27-4. 检索特定打印机的信息

```
PS > $queueClass = "Win32_PerfFormattedData_Spooler_PrintQueue"
PS > $filter = "Name='Brother DCP-1000'"
PS > $stats = Get-CimInstance $queueClass -Filter $filter
PS > $stats | Format-List *

AddNetworkPrinterCalls       : 129
BytesPrintedPersec           : 0
Caption                      :
Description                  :
EnumerateNetworkPrinterCalls : 0
Frequency_Object             :
Frequency_PerfTime           :
Frequency_Sys100NS           :
JobErrors                    : 0
Jobs                         : 0
JobsSpooling                 : 0
MaxJobsSpooling              : 1
MaxReferences                : 3
Name                         : Brother DCP-1000
NotReadyErrors               : 0
OutofPaperErrors             : 0
References                   : 2
Timestamp_Object             :
Timestamp_PerfTime           :
Timestamp_Sys100NS           :
TotalJobsPrinted             : 2
TotalPagesPrinted            : 0
```

要检索特定属性，请像访问其他 PowerShell 对象的属性一样访问它们：

```
PS > $stats.TotalJobsPrinted
2
```

## 讨论

`Win32_PerfFormattedData_Spooler_PrintQueue` WMI 类提供了对与打印队列相关的各种 Windows 性能计数器的访问。因此，您也可以通过 .NET Framework 访问它们，如 [食谱 17.3](#ch17.xhtml#access_windows_performance_counters) 所述：

```
PS > Get-Counter "\Print Queue($printer)\Jobs" | Select -Expand CounterSamples |
    Select InstanceName,CookedValue | Format-Table -Auto

InstanceName         CookedValue
------------         -----------
brother dcp-1000 usb           1
```

有关在 PowerShell 中使用 WMI 的详细信息，请参阅 [食谱 28.1](#ch28.xhtml#access_windows_management_instrumentatio)。

## 另请参阅

[食谱 17.3，“访问 Windows 性能计数器”](#ch17.xhtml#access_windows_performance_counters)

[食谱 28.1，“访问 Windows 管理规范和 CIM 数据”](#ch28.xhtml#access_windows_management_instrumentatio)

# 27.17 管理打印机和打印队列

## 问题

您想要清除打印机上的待处理打印作业。

## 解决方案

要管理连接到系统的打印机，请使用 `Win32_Printer` WMI 类。默认情况下，WMI 类列出所有打印机：
