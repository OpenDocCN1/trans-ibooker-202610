
如果前面的 `if` 或 `elseif` 语句中的条件都不为 `true`，则执行其后面的脚本块。

除了用于脚本控制流之外，条件语句通常也是给变量赋值的一种有用方法。PowerShell 让这变得非常简单，允许你将条件语句的结果直接赋值给变量：

```
$result = if (Get-Process -Name notepad) { "Running" } else { "Not running" }
```

对于像这样非常简单的条件语句，你也可以使用 PowerShell 的三元运算符：

```
$result = (Get-Process -Name notepad*) ? "Running" : "Not running"
```

有关这些流控制语句的详细信息，请输入 `Get-Help about_If`。

# 4.3 使用 Switch 语句管理大型条件语句

## 问题

你想找到一种更简单或更紧凑的方式来表示大型的 `if` … `elseif` … `else` 条件语句。

## 解决方案

使用 PowerShell 的 `switch` 语句来更轻松地表示大型的 `if` … `elseif` … `else` 条件语句。

例如：

```
$temperature = 20

switch ($temperature)
{
   { $_ -lt 32 }   { "Below Freezing"; break }
   32              { "Exactly Freezing"; break }
   { $_ -le 50 }   { "Cold"; break }
   { $_ -le 70 }   { "Warm"; break }
   default         { "Hot" }
}
```

## 讨论

PowerShell 的 `switch` 语句让你能轻松地将输入与大量比较条件进行测试。`switch` 语句支持多个选项，允许你配置 PowerShell 如何将输入与条件进行比较——例如使用通配符、正则表达式，甚至任意脚本块。由于扫描文件中的文本是一项非常常见的任务，PowerShell 的 `switch` 语句直接支持该功能。这些增强功能使 PowerShell `switch` 语句比 C 和 C++ 中的功能强大得多。

作为 `switch` 语句实际应用的另一个示例，请考虑如何确定当前操作系统的 SKU。例如，脚本是在 Windows 7 Ultimate 上运行？还是 Windows Server Cluster Edition？`Get-CimInstance` cmdlet 允许你确定操作系统 SKU，但遗憾的是它以简单数字的形式返回结果。`switch` 语句让你能根据[官方文档](https://oreil.ly/1fI6H)将这些数字映射为其英文对应词：

```
##############################################################################
##
## Get-OperatingSystemSku
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Gets the sku information for the current operating system

.EXAMPLE

PS > Get-OperatingSystemSku
Professional with Media Center

#>

param($Sku =
    (Get-CimInstance Win32_OperatingSystem).OperatingSystemSku)

Set-StrictMode -Version 3

switch ($Sku)
{
    0   { "An unknown product"; break; }
    1   { "Ultimate"; break; }
    2   { "Home Basic"; break; }
    3   { "Home Premium"; break; }
    4   { "Enterprise"; break; }
    5   { "Home Basic N"; break; }
    6   { "Business"; break; }
    7   { "Server Standard"; break; }
    8   { "Server Datacenter (full installation)"; break; }
    9   { "Windows Small Business Server"; break; }
    10  { "Server Enterprise (full installation)"; break; }
    11  { "Starter"; break; }
    12  { "Server Datacenter (core installation)"; break; }
    13  { "Server Standard (core installation)"; break; }
    14  { "Server Enterprise (core installation)"; break; }
    15  { "Server Enterprise for Itanium-based Systems"; break; }
    16  { "Business N"; break; }
    17  { "Web Server (full installation)"; break; }
    18  { "HPC Edition"; break; }
    19  { "Windows Storage Server 2008 R2 Essentials"; break; }
    20  { "Storage Server Express"; break; }
    21  { "Storage Server Standard"; break; }
    22  { "Storage Server Workgroup"; break; }
    23  { "Storage Server Enterprise"; break; }
    24  { "Windows Server 2008 for Windows Essential Server Solutions"; break; }
    25  { "Small Business Server Premium"; break; }
    26  { "Home Premium N"; break; }
    27  { "Enterprise N"; break; }
    28  { "Ultimate N"; break; }
    29  { "Web Server (core installation)"; break; }
    30  { "Windows Essential Business Server Management Server"; break; }
    31  { "Windows Essential Business Server Security Server"; break; }
    32  { "Windows Essential Business Server Messaging Server"; break; }
    33  { "Server Foundation"; break; }
    34  { "Windows Home Server 2011"; break; }
    35  { "Windows Server 2008 without Hyper-V for Windows Essential Server"; break; }
    36  { "Server Standard without Hyper-V"; break; }
    37  { "Server Datacenter without Hyper-V (full installation)"; break; }
    38  { "Server Enterprise without Hyper-V (full installation)"; break; }
    39  { "Server Datacenter without Hyper-V (core installation)"; break; }
    40  { "Server Standard without Hyper-V (core installation)"; break; }
    41  { "Server Enterprise without Hyper-V (core installation)"; break; }
    42  { "Microsoft Hyper-V Server"; break; }
    43  { "Storage Server Express (core installation)"; break; }
    44  { "Storage Server Standard (core installation)"; break; }
    45  { "Storage Server Workgroup (core installation)"; break; }
    46  { "Storage Server Enterprise (core installation)"; break; }
    46  { "Storage Server Enterprise (core installation)"; break; }
    47  { "Starter N"; break; }
    48  { "Professional"; break; }
    49  { "Professional N"; break; }
    50  { "Windows Small Business Server 2011 Essentials"; break; }
    51  { "Server For SB Solutions"; break; }
    52  { "Server Solutions Premium"; break; }
    53  { "Server Solutions Premium (core installation)"; break; }
    54  { "Server For SB Solutions EM"; break; }
    55  { "Server For SB Solutions EM"; break; }
    56  { "Windows MultiPoint Server"; break; }
    59  { "Windows Essential Server Solution Management"; break; }
    60  { "Windows Essential Server Solution Additional"; break; }
    61  { "Windows Essential Server Solution Management SVC"; break; }
    62  { "Windows Essential Server Solution Additional SVC"; break; }
    63  { "Small Business Server Premium (core installation)"; break; }
    64  { "Server Hyper Core V"; break; }
    72  { "Server Enterprise (evaluation installation)"; break; }
    76  { "Windows MultiPoint Server Standard (full installation)"; break; }
    77  { "Windows MultiPoint Server Premium (full installation)"; break; }
    79  { "Server Standard (evaluation installation)"; break; }
    80  { "Server Datacenter (evaluation installation)"; break; }
    84  { "Enterprise N (evaluation installation)"; break; }
    95  { "Storage Server Workgroup (evaluation installation)"; break; }
    96  { "Storage Server Standard (evaluation installation)"; break; }
    98  { "Windows 8 N"; break; }
    99  { "Windows 8 China"; break; }
    100 { "Windows 8 Single Language"; break; }
    101 { "Windows 8"; break; }
    103 { "Professional with Media Center"; break; }

    default {"UNKNOWN: " + $SKU }
}
```

虽然 `switch` 语句被用作一种更清晰地表达大型条件语句的方式，但其运作方式更像是一大串 `if` 语句，而不是一大串 `if` … `elseif` … `elseif` … `else` 语句。对于你提供的输入，PowerShell 会将该输入与 `switch` 语句中的*每个*比较条件进行求值。如果比较结果为 `true`，PowerShell 随后执行其后面的脚本块。除非该脚本块包含 `break` 语句，否则 PowerShell 将继续求值后续的比较条件。

有关 PowerShell `switch` 语句的详细信息，请参阅[“条件语句”](#app01.xhtml#conditional_statements)或输入 `Get-Help about_Switch`。

## 另请参阅

[“条件语句”](#app01.xhtml#conditional_statements)
