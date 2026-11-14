# 第 4 章 循环与流程控制

## 4.0 简介

当你开始编写与未知数据交互的脚本或命令时，循环和流程控制的概念变得日益重要。

PowerShell 的循环语句和命令让你无需重复命令本身即可执行某项操作（或一组操作）。这包括例如执行指定次数的操作、处理集合中的每个项目，或一直工作直到满足特定条件。

PowerShell 的流程控制和比较语句让你能根据未知数据调整脚本或命令。它们允许你根据数据的值执行命令、根据数据的值跳过命令，等等。

循环和流程控制语句共同为你的 PowerShell 工具箱增添了显著的通用性。

## 4.1 使用比较和逻辑运算符进行决策

### 问题

你想要将某些数据与其他数据进行比较，并根据该比较结果做出决策。

### 解决方案

使用 PowerShell 的逻辑运算符来比较数据片段并基于它们做出决策：

**比较运算符**

: 

`‑eq`、`‑ne`、`‑ge`、`‑gt`、`‑in`、`‑notin`、`‑lt`、`‑le`、`‑like`、`‑notlike`、`‑match`、`‑notmatch`、`‑contains`、`‑notcontains`、`‑is`、`‑isnot`

**逻辑运算符**

: 

`‑and`、`‑or`、`‑xor`、`‑not`

有关这些运算符的详细说明（及示例），请参阅 `比较运算符`。

### 讨论

PowerShell 的逻辑和比较运算符让你能够比较数据片段或测试数据是否满足某个条件。运算符要么比较两个数据片段（**二元**运算符），要么测试一个数据片段（**一元**运算符）。所有比较运算符都是二元运算符（它们比较两个数据片段），大多数逻辑运算符也是如此。唯一的一元逻辑运算符是 `‑not` 运算符，它返回所测试数据的 `true/false` 相反值。

比较运算符比较两个数据片段，并返回取决于特定比较运算符的结果。例如，你可能想检查一个集合是否至少包含一定数量的元素：

```
PS > (dir).Count -ge 4
True
```

或者检查字符串是否匹配给定的正则表达式：

```
PS > "Hello World" -match "H.*World"
True
```

大多数比较运算符也会适应其输入的类型。例如，当你将它们应用于字符串等简单数据时，`‑like` 和 `‑match` 比较运算符确定字符串是否匹配指定的模式。当你将它们应用于简单数据的集合时，这些相同的比较运算符会返回该集合中所有与你提供的模式匹配的元素。

> **注意**
>
> `‑match` 运算器接受正则表达式作为其参数。更常见的正则表达式符号之一是 `$` 字符，它代表行尾。`$` 字符也代表 PowerShell 变量的开始！为了防止 PowerShell 将字符解释为语言术语或转义序列，请将字符串放在单引号中而不是双引号中：
>
> ```
> PS > "Hello World" -match "Hello"
> True
> PS > "Hello World" -match 'Hello$'
> False
> ```

默认情况下，PowerShell 的比较运算符不区分大小写。要使用区分大小写的版本，请在前面加上字符 `c`：

```
‑ceq、‑cne、‑cge、‑cgt、‑cin、‑clt、‑cle、‑clike、‑cnotlike、
‑cmatch、‑cnotmatch、‑ccontains、‑cnotcontains
```

有关比较运算符、其区分大小写的对应版本以及它们如何适应输入的详细说明，请参阅 `比较运算符`。

逻辑运算符组合 `true` 或 `false` 语句，并返回取决于特定逻辑运算符的结果。例如，你可能想检查字符串是否匹配你提供的通配符模式*并且*其长度是否大于一定字符数：

```
PS > $data = "Hello World"
PS > ($data -like "*llo W*") -and ($data.Length -gt 10)
True
PS > ($data -like "*llo W*") -and ($data.Length -gt 20)
False
```

一些比较运算符实际上包含了逻辑运算符的某些方面。由于使用比较的相反操作（如 `‑like`）非常常见，PowerShell 提供了比较运算符（如 `‑notlike`），使你无需显式使用 `‑not` 运算符。

有关各个逻辑运算符的详细说明，请参阅 `比较运算符`。

比较运算符和逻辑运算符（结合流程控制语句时）构成了我们编写适应其数据和输入的脚本或命令的核心。

另请参阅 `条件语句` 以获取关于这些语句的详细信息。

有关 PowerShell 运算符的更多信息，请输入 **`Get-Help`** **`about_Operators`**。

### 另请参阅

`比较运算符`

`条件语句`

## 4.2 使用条件语句调整脚本流程

### 问题

你想要控制 PowerShell 执行命令或脚本部分的条件。

### 解决方案

使用 PowerShell 的 `if`、`elseif` 和 `else` 条件语句来控制脚本中的执行流程。

例如：

```
$temperature = 90

if($temperature -le 0)
{
   "Balmy Canadian Summer"
}
elseif($temperature -le 32)
{
   "Freezing"
}
elseif($temperature -le 50)
{
   "Cold"
}
elseif($temperature -le 70)
{
   "Warm"
}
else
{
   "Hot"
}
```

### 讨论

条件语句包括以下内容：

`if` *语句*

: 

如果其*条件*评估为 `true`，则执行其后面的脚本块

`elseif` *语句*

: 

如果其*条件*评估为 `true`，且它之前的 `if` 或 `elseif` 语句中的条件均不评估为 `true`，则执行其后面的脚本块

`else` *语句*




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

有关 PowerShell `switch` 语句的详细信息，请参阅“条件语句”或输入 `Get-Help about_Switch`。

## 另请参阅

“条件语句”



# 4.4 使用循环重复操作

## 问题

您想多次执行同一代码块。

## 解决方案

使用 PowerShell 的循环语言之一（`for`、`foreach`、`while` 和 `do`）或 PowerShell 的 `ForEach-Object` cmdlet 多次运行命令或脚本块。有关这些循环语句的详细说明，请参阅“循环语句”。例如：

### `for` 循环

```
for($counter = 1; $counter -le 10; $counter++)
{
  "Loop number $counter"
}
```

### `foreach` 循环

```
foreach($file in dir)
{
  "File length: " + $file.Length
}
```

### `ForEach-Object` cmdlet

```
Get-ChildItem | ForEach-Object { "File length: " + $_.Length }
```

### `while` 循环

```
$response = ""
while($response -ne "QUIT")
{
  $response = Read-Host "Type something"
}
```

### `do..while` 循环

```
$response = ""
do
{
  $response = Read-Host "Type something"
} while($response -ne "QUIT")
```

### `do..until` 循环

```
$response = ""
do
{
  $response = Read-Host "Type something"
} until($response -eq "QUIT")
```

## 讨论

虽然任何循环语句都可以编写为在功能上等同于其他任何语句，但每种语句都适合特定的问题。

当您需要执行操作确切的次数时，通常使用 `for` 循环。因为这种用法非常常见，所以它通常被称为*计数* `for` *循环*。

当您有一组对象并想要访问该集合中的每个项目时，通常使用 `foreach` 循环。如果您还没有在内存中拥有整个集合（如前面 `foreach` 示例所示的 `dir` 集合），`ForEach-Object` cmdlet 通常是一个更高效的替代方案。

与 `foreach` 循环不同，`ForEach-Object` cmdlet 允许您在 PowerShell 生成集合中的每个元素*时*对其进行处理。这是一个重要的区别；在 `foreach` 循环中要求 PowerShell 收集大型命令（如 `Get-Content hugefile.txt`）的全部输出很容易拖垮您的系统。

与面向管道的函数一样，`ForEach-Object` cmdlet 允许您定义在循环开始前、循环期间和循环完成后执行的命令：

```
PS > "a","b","c" | ForEach-Object `
    -Begin { "Starting"; $counter = 0 } `
    -Process { "Processing $_"; $counter++ } `
    -End { "Finishing: $counter" }

Starting
Processing a
Processing b
Processing c
Finishing: 3
```

### 提示

要同时在循环中调用多个操作，请使用 `ForEach-Object` 的 `-parallel` 开关。有关详细信息，请参阅配方 4.5。

`while` 和 `do..while` 循环相似，因为它们只要条件评估为 `true` 就会继续执行循环。`while` 循环在运行脚本块之前检查此条件，而 `do..while` 循环在运行脚本块之后检查条件。`do..until` 循环与 `do..while` 循环完全相似，不同之处在于它在条件返回 `$true` 时退出，而不是在条件返回 `$false` 时退出。

有关这些循环语句的详细说明，请参阅“循环语句”或输入 `Get-Help about_For`、`Get-Help about_Foreach`、`Get-Help about_While` 或 `Get-Help about_Do`。

## 另请参阅

配方 4.5，“并行处理耗时操作”

“循环语句”

# 4.5 并行处理耗时操作

## 问题

您有一组数据或操作想要同时运行。

## 解决方案

使用 `ForEach-Object` cmdlet 的 `-parallel` 开关：

```
PS > Measure-Command { 1..5 | ForEach-Object { Start-Sleep -Seconds 5 } }

(...)
TotalSeconds      : 25.0247856
(...)

PS > Measure-Command { 1..5 | ForEach-Object -parallel { Start-Sleep -Seconds 5 } }

(...)
TotalSeconds      : 5.1354752
(...)
```

## 讨论

在 PowerShell 中，有时您可以通过同时运行长时间运行操作的各个部分来显著加快其速度。这方面的绝佳机会是您的脚本将大部分时间花在等待网络资源（如下载文件或网页）或缓慢操作（如重启一系列缓慢的服务）上的场景。

在这些场景中，您可以使用 `ForEach-Object` 的 `-parallel` 参数同时执行这些操作。在内部，PowerShell 使用后台作业来运行每个分支。它将同时运行的分支数量限制为您在 `-ThrottleLimit` 参数中指定的数量，默认为 `5`。

### 注意

如果您想要并行运行多个命令的原因是要在大量计算机上快速完成某项任务，则应改用 `Invoke-Command`。有关详细信息，请参阅配方 29.5。

由于 PowerShell 将这些分支作为后台作业运行，您需要使用 `$USING` 语法将外部变量引入此后台作业（PowerShell 默认引入 `$_`），或在 `-ArgumentList` 参数中提供变量。例如：

```
PS > $greeting = "World"
PS > 1..5 | ForEach-Object -parallel { "Hello $greeting" }
Hello
Hello
Hello
Hello
Hello

PS > 1..5 | ForEach-Object -parallel { "Hello $USING:greeting" }
Hello World
Hello World
Hello World
Hello World
Hello World
```

PowerShell 在您的主 PowerShell 进程中运行这些后台作业，因此您可以对作为实时实例的输入进行操作：

```
$processes = 1..10 | ForEach-Object { Start-Process notepad -PassThru }
$processes | ForEach-Object -parallel { $_.Kill() }
```

如果您需要并行循环的分支与主 Shell 通信，建议的方法是通过脚本块输出来实现，然后让主 Shell 处理结果。使用实时对象来做这件事很诱人，但请注意，这条路径充满陷阱且困难重重。让我们举一个简单的例子——运行一个并行操作来增加计数器。

最初可能看起来您应该使用：

```
$counter = 0
1..10 | ForEach-Object -parallel {
    $myCounter = $USING:counter
    $myCounter = $myCounter + 1
}
```

然而，当您在 PowerShell 中输入 `$counter = $counter + 1` 时，PowerShell 会更新当前作用域中的 `$counter` 变量。如果您想从后台作业更改对象，则需要通过设置实时对象上的属性来实现，而不是尝试替换该对象。幸运的是，PowerShell 有一种称为 `[ref]` 的类型用于此类场景：

```
$counter = [ref] 0
1..10 | ForEach-Object -parallel {
    $myCounter = $USING:counter
    $myCounter.Value = $myCounter.Value + 1
}
```

最初，这似乎有效：

```
PS > $counter

Value
-----
   10
```

现在我们很得意，让我们真正并行地做这件事：

```
$counter = [ref] 0
1..10000 | ForEach-Object -throttlelimit 100 -parallel {
    $myCounter = $USING:counter
    $myCounter.Value = $myCounter.Value + 1
}
```

```
PS > $counter

Value
-----
 9992
```

哎呀！因为我们大规模并行执行此操作，`$myCounter.Value` 可能会在 PowerShell 运行 `$myCounter.Value = $myCounter.Value + 1` 的管道部分中的任何时间发生变化。这称为*竞争条件*，常见于任何允许来自多个同时代码块的代码同时运行的语言中。为了消除这些奇怪的中间状态，我们必须使用 .NET Framework 中的 Interlocked Increment 类：

```
$counter = [ref] 0
1..10000 | ForEach-Object -throttlelimit 100 -parallel {
    $myCounter = $USING:counter
    $null = [Threading.Interlocked]::Increment($myCounter)
}
```

这正确地给出了我们：

```
PS > $counter

Value
-----
10000
```



这些问题棘手，且经常困扰甚至专业程序员。处理这类问题的最佳实践是完全避开该领域，即不处理或操作共享状态。

### 另请参阅

食谱 4.4，“使用循环重复操作”

食谱 29.5，“在远程计算机上调用命令”

## 4.6 添加暂停或延迟

### 问题

你想暂停或延迟你的脚本或命令。

### 解决方案

要暂停直到用户按下 Enter 键，请使用 `pause` 命令：

```
PS > pause
Press Enter to continue...:
```

要暂停直到用户按下任意键，请对 `$host` 对象使用 `ReadKey()` 方法：

```
PS > $host.UI.RawUI.ReadKey()
```

要暂停脚本指定的时间量，请使用 `Start-Sleep` cmdlet：

```
PS > Start-Sleep 5
PS > Start-Sleep -Milliseconds 300
```

### 讨论

当你想暂停脚本直到用户按下键或设定的时间量时，`pause` 和 `Start-Sleep` 是你最可能使用的两个 cmdlet。

#### 注意

如果你想获取用户输入而不仅仅是暂停，`Read-Host` cmdlet 允许你从用户读取输入。更多信息，请参阅 食谱 13.1。

在其他情况下，你有时可能想在脚本中编写一个以恒定速度运行的循环——例如每分钟一次或每秒 30 次。这通常是一项困难的任务，因为循环中的命令可能会占用大量时间，甚至是不一致的时间量。

过去，许多电脑游戏因错误解决此问题而受影响。为控制游戏速度，游戏开发者添加命令以减慢游戏。例如，经过大量调整和摆弄，开发者可能会发现，如果让计算机每次更新屏幕时数到 100 万，游戏在典型机器上运行正常。不幸的是，这些命令（如计数）的速度高度依赖计算机速度。因为快速计算机数到 100 万比慢速计算机快得多，游戏最终在更快的计算机上运行得快得多（往往快到难以理解）！

要使循环以固定速度运行，你可以测量循环中命令完成所需的时间，然后延迟剩余的时间，如 示例 4-1 所示。

#### 示例 4-1. 以恒定速度运行循环

```
$loopDelayMilliseconds = 650
while($true)
{
   $startTime = Get-Date

   ## Do commands here
   "Executing"

   $endTime = Get-Date
   $loopLength = ($endTime - $startTime).TotalMilliseconds
   $timeRemaining = $loopDelayMilliseconds - $loopLength

   if($timeRemaining -gt 0)
   {
      Start-Sleep -Milliseconds $timeRemaining
   }
}
```

有关 `Start-Sleep` cmdlet 的更多信息，请输入 `Get-Help Start-Sleep`。

### 另请参阅

食谱 13.1，“读取一行用户输入”
