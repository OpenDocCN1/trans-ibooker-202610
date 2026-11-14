
## 使用 Format-* Cmdlet 格式化输出

```
PS > ## 获取目录中的项目列表，按长度排序
PS > function Get-ChildItemSortedByLength($path = (Get-Location))
{
    Get-ChildItem $path | Format-Table | Sort-Object Length
}

PS > Get-ChildItemSortedByLength
out-lineoutput : Object of type "Microsoft.PowerShell.Commands.Internal.
Format.FormatEntryData" is not legal or not in the correct sequence. This is
likely caused by a user-specified "format-*" command which is conflicting
with the default formatting.
```

之所以会发生这种情况，是因为 `Format-*` cmdlet 实际上会生成供 `Out-Host` cmdlet 使用的格式化信息。`Out-Host` cmdlet（PowerShell 会自动将其添加到管道末尾）随后使用这些信息生成格式化输出。要解决此问题，请始终确保格式化命令是管道中的最后一个命令，如[示例 13-4](#ch13.xhtml#a_function_that_does_not_generate_format)所示。

### 示例 13-4. 不生成格式化错误的函数

```
PS > ## 获取目录中的项目列表，按长度排序
PS > function Get-ChildItemSortedByLength($path = (Get-Location))
{
    ## 有问题的版本
    ## Get-ChildItem $path | Format-Table | Sort-Object Length

    ## 修复后的版本
    Get-ChildItem $path | Sort-Object Length | Format-Table
}

PS > Get-ChildItemSortedByLength

(...)
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         3/11/2007   3:21 PM         59 LibraryProperties.ps1
-a---          3/6/2007  10:27 AM        150 Get-Tomorrow.ps1
-a---          3/4/2007   3:10 PM        194 ConvertFrom-FahrenheitWithout
                                             Function.ps1
-a---          3/4/2007   4:40 PM        257 LibraryTemperature.ps1
-a---          3/4/2007   4:57 PM        281 ConvertFrom-FahrenheitWithLib
                                             rary.ps1
-a---          3/4/2007   3:14 PM        337 ConvertFrom-FahrenheitWithFunc
                                             tion.ps1
(...)
```

这些示例以 `LibraryDirectory.ps1` 的形式包含在本书的代码示例中。有关获取这些示例的详细信息，请参阅[“使用代码示例”](#preface01.xhtml#code_examples)。

在为用户生成输出时，常见的原因是提供进度消息。PowerShell 实际上通过其 `Write-Progress` cmdlet 以更丰富的方式支持此功能。有关 `Write-Progress` cmdlet 的详细信息，请参阅[配方 13.5](#ch13.xhtml#provide_progress_updates_on_long-running)。

## 另请参阅

[配方 13.5](#ch13.xhtml#provide_progress_updates_on_long-running)

# 13.5 在长时间运行的任务上提供进度更新

## 问题

您希望为长时间运行的任务向用户显示状态信息。

## 解决方案

要提供状态更新，请使用[示例 13-5](#ch13.xhtml#using_the_write-progress_cmdlet_to_displ)中所示的 `Write-Progress` cmdlet。

### 示例 13-5. 使用 Write-Progress cmdlet 显示状态更新

```powershell
##############################################################################
##
## Invoke-LongRunningOperation
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

演示 Write-Progress cmdlet 的功能

#>

Set-StrictMode -Version 3

$activity = "长时间运行的操作"
$status = "正在初始化"

## 初始化长时间运行的操作
for($counter = 0; $counter -lt 100; $counter++)
{
    $currentOperation = "正在初始化项目 $counter"
    Write-Progress $activity $status -PercentComplete $counter `
        -CurrentOperation $currentOperation
    Start-Sleep -m 20
}

$status = "正在运行"

## 初始化长时间运行的操作
for($counter = 0; $counter -lt 100; $counter++)
{
    $currentOperation = "正在运行任务 $counter"
    Write-Progress $activity $status -PercentComplete $counter `
        -CurrentOperation $currentOperation
    Start-Sleep -m 20
}
```

## 讨论

`Write-Progress` cmdlet 使您能够为长时间运行的操作向脚本用户提供结构化的状态信息（请参阅[图 13-2](#ch13.xhtml#example_output_from_a_long-running_opera)）。

![](Images/wps4_1302.png)

与其他详细信息通道（`Write-Debug`、`Write-Verbose` 和其他 `Write-*` cmdlet）一样，PowerShell 允许用户控制他们看到多少此类信息。

有关 `Write-Progress` cmdlet 的详细信息，请键入 `Get-Help Write-Progress`。

# 13.6 编写区域性感知脚本

## 问题

您希望确保脚本在世界各地的计算机上都能正常工作。

## 解决方案

要编写区域性感知脚本，请在开发脚本时记住以下准则：

*   使用 PowerShell 的语言基元创建日期、时间和数字。
*   使用 PowerShell 的内置运算符比较字符串。
*   避免将用户输入视为字符集合。
*   使用 `Parse()` 方法将用户输入转换为日期、时间和数字。

## 讨论

编写区域性感知程序长期以来一直局限于专业软件开发人员的领域。不过，这并不是说简单程序和脚本的用户无法从区域性感知中受益。只是对于非专业程序员来说，遵循最佳实践往往太困难了。然而，PowerShell 使这比传统编程语言容易得多。

当脚本在不同区域性之间移动时，有几个方面会发生变化。

### 日期、时间和数字格式

大多数区域性都有独特的日期、时间和数字格式。为确保脚本在所有区域性中都能工作，PowerShell 首先确保其语言基元无论在何处运行脚本都保持一致。即使脚本在法国的计算机上运行（法国使用逗号作为小数分隔符），您也始终可以依赖语句 `$myDouble = 3.5` 来创建一个介于 3 和 4 之间的数字。同样，您始终可以依赖语句 `$christmas = [DateTime]"12/25/2007"` 来创建一个表示 2007 年圣诞节的日期——即使在按日、月、年顺序书写日期的区域性中也是如此。

区域性感知程序始终使用该区域性的首选项来显示日期、时间和数字。这不会破坏脚本在区域性之间的移植，并且是编写区域性感知脚本的一个重要方面。PowerShell 会为您处理此问题，因为它在显示数据时始终使用当前区域性的首选项。

#### 注意

如果脚本要求用户输入日期、时间或数字，请确保在这样做时遵循用户区域性的格式。要将用户输入转换为特定类型的数据，请使用 `Get-Date` cmdlet：

```powershell
$userInput = Read-Host "请输入一个日期"
$enteredDate = Get-Date -Date $userInput
```

因此，为确保脚本在日期、时间和数字格式方面保持区域性感知，只需在脚本中定义它们时使用 PowerShell 的语言基元即可。当从用户处读取它们时，在将它们从字符串转换时使用 `Parse()` 方法。

### 用户输入和文件内容的复杂性

英语是一种罕见的语言，其字母表非常简单。这导致各种编程技巧将用户输入和文件内容视为字节数组或简单的纯文本（ASCII）字符。在大多数国际语言中，这些技巧都会失效。事实上，许多国际符号在包含它们的字符串中占用两个字符的数据量。

PowerShell 对所有基于字符串的操作使用标准 Unicode 字符集：从用户读取输入、向用户显示输出、通过管道发送数据以及处理文件。

#### 注意
