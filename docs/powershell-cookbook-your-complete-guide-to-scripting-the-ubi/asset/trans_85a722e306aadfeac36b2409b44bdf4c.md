
```
foreach($item in $myHashtable.GetEnumerator() | Sort-Object Name)
{
    $item.Value
}
```

如果您控制哈希表的定义，请在定义哈希表时使用 `[Ordered]` 类型转换，以使其保留定义中提供的顺序。

```
$orderedHashtable = [Ordered] @{ Item1 = "Hello"; Item2 = "World" }
```

## 讨论

由于哈希表的主要焦点只是将键映射到值，因此它通常完全不保留任何顺序——例如您添加项的顺序、键的排序顺序或值的排序顺序。这在 [示例 7-3](#ch07.xhtml#a_demonstration_of_hashtable_items_not_r) 中变得清晰。

### 示例 7-3. 哈希表项不保留其顺序的演示

```
PS > $myHashtable = @{}
PS > $myHashtable["Hello"] = 3
PS > $myHashtable["Ali"] = 2
PS > $myHashtable["Alien"] = 4
PS > $myHashtable["Duck"] = 1
PS > $myHashtable["Hectic"] = 11
PS > $myHashtable

Name                           Value
----                           -----
Hectic                         11
Duck                           1
Alien                          4
Hello                          3
Ali                            2
```

然而，哈希表对象支持 `GetEnumerator()` 方法，该方法允许您处理单个哈希表条目——所有这些条目都具有 `Name` 和 `Value` 属性。一旦您拥有这些，我们可以像排序任何其他 PowerShell 数据一样轻松地按它们进行排序。[示例 7-4](#ch07.xhtml#sorting_a_hashtable_by_name_and_value) 演示了此技术。

### 示例 7-4. 按名称和值对哈希表进行排序

```
PS > $myHashtable.GetEnumerator() | Sort-Object Name

Name                           Value
----                           -----
Ali                            2
Alien                          4
Duck                           1
Hectic                         11
Hello                          3

PS > $myHashtable.GetEnumerator() | Sort-Object Value

Name                           Value
----                           -----
Duck                           1
Ali                            2
Hello                          3
Alien                          4
Hectic                         11
```

通过使用 `[Ordered]` 类型转换，您可以创建一个保留您定义和添加项顺序的哈希表：

```
PS > $myHashtable = [Ordered] @{
    Duck = 1;
    Ali = 2;
    Hectic = 11;
    Alien = 4;
 }

PS > $myHashtable["Hello"] = 3
PS > $myHashtable

Name                           Value
----                           -----
Duck                           1
Ali                            2
Hectic                         11
Alien                          4
Hello                          3
```

有关使用哈希表的更多信息，请参阅 [“哈希表（关联数组）”](#app01.xhtml#hashtables_associative_arrays)。

## 另请参阅

[“哈希表（关联数组）”](#app01.xhtml#hashtables_associative_arrays)

# 第 8 章. 实用任务

## 8.0 简介

当您编写脚本或仅使用交互式 Shell 时，会出现一些简单但有用的需求：度量命令、获取随机数等。

## 8.1 获取系统日期和时间

### 问题

您想要获取系统日期。

### 解决方案

要获取系统日期，请运行命令 `Get-Date`。

### 讨论

`Get-Date` 命令生成丰富的基于对象的输出，因此您可以将其结果用于许多与日期相关的任务。例如，要确定当前是星期几：

```
PS > $date = Get-Date
PS > $date.DayOfWeek
Sunday
```

如果您想要格式化日期以用于输出（例如，作为日志文件时间戳），请参阅 [配方 5.13](#ch05.xhtml#format_a_date_for_output)。

有关 `Get-Date` cmdlet 的更多信息，请输入 `Get-Help Get-Date`。

有关使用 .NET Framework 类的更多信息，请参阅 [配方 3.8](#ch03.xhtml#work_with_net_objects)。

### 另请参阅

[配方 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

[配方 5.13，“格式化日期以用于输出”](#ch05.xhtml#format_a_date_for_output)

## 8.2 测量命令的持续时间

### 问题

您想知道命令执行需要多长时间。

### 解决方案

要测量命令的持续时间，请使用 `Measure-Command` cmdlet：

```
PS > Measure-Command { Start-Sleep -Milliseconds 337 }

Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 0
Milliseconds      : 339
Ticks             : 3392297
TotalDays         : 3.92626967592593E-06
TotalHours        : 9.42304722222222E-05
TotalMinutes      : 0.00565382833333333
TotalSeconds      : 0.3392297
TotalMilliseconds : 339.2297
```

### 讨论

在交互式使用中，通常需要测量命令的持续时间。一个例子可能是对您开发的应用程序运行性能基准测试。`Measure-Command` cmdlet 使这变得容易。因为该命令生成丰富的基于对象的输出，您可以将其输出用于许多与日期相关的任务。有关更多信息，请参阅 [配方 3.8](#ch03.xhtml#work_with_net_objects)。

如果命令测量的准确性很重要，一般的系统活动很容易影响结果的计时。提高准确性的常用技术是重复测量多次，忽略异常值（前 10% 和后 10%），然后计算剩余结果的平均值。[示例 8-1](#ch08.xhtml#measure_commandperformance) 实现了此技术。

#### 示例 8-1. Measure-CommandPerformance.ps1

```
##############################################################################
##
## Measure-CommandPerformance
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Measures the average time of a command, accounting for natural variability by
automatically ignoring the top and bottom 10%.

.EXAMPLE

PS > Measure-CommandPerformance.ps1 { Start-Sleep -m 300 }

Count    : 30
Average  : 312.10155
(...)

#>

param(
    ## The command to measure
    [Scriptblock] $Scriptblock,

    ## The number of times to measure the command's performance
    [int] $Iterations = 30
)

Set-StrictMode -Version 3

## Figure out how many extra iterations we need to account for the outliers
$buffer = [int] ($iterations * 0.1)
$totalIterations = $iterations + (2 * $buffer)

## Get the results
$results = 1..$totalIterations |
    Foreach-Object { Measure-Command $scriptblock }

## Sort the results, and skip the outliers
$middleResults = $results | Sort TotalMilliseconds |
    Select -Skip $buffer -First $iterations

## Show the average
$middleResults | Measure-Object -Average TotalMilliseconds
```

有关 `Measure-Command` cmdlet 的更多信息，请输入 `Get-Help Measure-Command`。

### 另请参阅

[配方 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

## 8.3 从剪贴板读取和写入

### 问题

您想要与 PowerShell 剪贴板交互。

### 解决方案

使用 PowerShell 的 `Get-Clipboard` 和 `Set-Clipboard` cmdlet：

```
PS > 1..5 | Set-Clipboard
PS > Get-Clipboard | Where-Object { ([int] $_) -lt 3 }
1
2
```

如果您想要一次性检索剪贴板的全部内容（不使用 PowerShell 默认的逐行行为），请使用 `-Raw` 参数：

```
PS > $allContent = Get-Clipboard -Raw
PS > $allContent.Replace("`r`n", ".")
1.2.3.4.5
```

### 讨论

虽然 Windows 包含一个命令行实用程序 (`clip.exe`) 可将文本放入 Windows 剪贴板，但它不支持直接输入（例如 `clip.exe "Hello World"`），并且没有相应的实用程序从 Windows 剪贴板检索内容。

`Set-Clipboard` 和 `Get-Clipboard` cmdlet 解决了这两个问题。
