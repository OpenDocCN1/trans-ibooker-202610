# 第 11 章. 代码重用

## 11.0 简介

令许多人惊讶的一件事是，仅从交互式提示符就能在 PowerShell 中完成如此多的工作。由于 PowerShell 让将其强大的命令组合成更强大的组合变得非常容易，爱好者们逐渐喜欢上了这种简洁性。事实上，大多数脚本爱好者的心中都有一个特殊的位置，专门留给最紧凑的力量表达：*单行命令*。

尽管交互式操作效率很高，但您可能不想每次需要时都重新输入所有绝妙的想法。当您想要保存或重用编写的命令时，PowerShell 提供了多种途径来支持您：脚本、模块、函数、脚本块等。

## 11.1 编写脚本

### 问题

您想将命令存储在脚本中，以便稍后共享或重用。

### 解决方案

要编写 PowerShell 脚本，请使用您喜欢的编辑器创建一个纯文本文件。将 PowerShell 命令添加到该脚本中（与您在交互式 Shell 中使用的相同 PowerShell 命令），然后使用 `*.ps1` 扩展名保存。

### 讨论

关于 PowerShell 最重要的一点是，运行脚本和在命令行工作本质上是等效的操作。如果您在脚本中看到它，可以在命令行中输入或粘贴。如果您在命令行中输入了它，可以将其粘贴到文本文件中并称之为脚本。

编写脚本后，PowerShell 允许您以调用其他程序和现有工具的相同方式调用它。运行脚本的效果等同于运行该脚本中的所有命令。

#### 注意

PowerShell 引入了一些与运行脚本和工具相关的功能，如果您不了解它们，起初可能会感到困惑。有关如何调用脚本和现有工具的更多信息，请参阅食谱 1.2。

您第一次尝试在 PowerShell 中运行脚本时，可能会看到以下错误消息：

```
File c:\tools\myFirstScript.ps1 cannot be loaded because the execution of
scripts is disabled on this system. Please see "get-help about_signing" for
more details.
At line:1 char:12
+ myFirstScript <<<<
```

由于相对较少的计算机用户编写脚本，PowerShell 的默认安全策略会阻止脚本运行。但是，一旦您开始编写脚本，就应该将此策略配置为限制较少的设置。有关如何配置执行策略的信息，请参阅食谱 18.1。



# 选择脚本名称

谈到脚本的文件名，选择一个描述性的名称是确保你始终记得该脚本的功能（或至少有个大致了解）的最佳方式。PowerShell 以优雅的方式解决了这个问题，它采用 `Verb-Noun`（动词-名词）模式为每个 cmdlet 命名：一个对项目（`noun`，名词）执行操作（`verb`，动词）的命令。作为这种理念实用性的演示，请参考 示例 11-1 中给出的典型 Windows 命令名称。

## 示例 11-1：某些标准 Windows 命令的名称

```
PS > dir $env:WINDIR\System32\*.exe | Select-Object Name

Name
----
accwiz.exe
actmovie.exe
ahui.exe
alg.exe
append.exe
arp.exe
asr_fmt.exe
asr_ldm.exe
asr_pfu.exe
at.exe
atmadm.exe
attrib.exe
(...)
```

将其与 示例 11-2 中给出的某些标准 PowerShell cmdlet 的名称进行比较。

## 示例 11-2：某些标准 PowerShell cmdlet 的名称

```
PS > Get-Command | Select-Object Name

Name
----
Add-Content
Add-History
Add-Member
Add-PSSnapin
Clear-Content
Clear-Item
Clear-ItemProperty
Clear-Variable
Compare-Object
ConvertFrom-SecureString
Convert-Path
ConvertTo-Html
(...)
```

作为改进可发现性的另一种方式，PowerShell 甚至更进一步，秉持（并明确设定目标）这样的理念：“你可以用少于 50 个动词来管理 80% 的系统。”当你学会某个概念的标准动词（例如 `Get`，它代表读取、打开等标准概念）时，你通常可以猜出命令的动词，从而迈出发现命令的第一步。

当你为脚本命名时（*特别是*如果你打算共享它），请尽最大努力选择一个遵循这些约定的名称。配方 11.3 展示了一个有用的 cmdlet，可帮助你找到一个动词来正确命名你的脚本。作为其对脚本实用性的证明，请看看本书中包含的一些脚本：

```
PS > dir | select Name

Name
----
Compare-Property.ps1
Convert-TextObject.ps1
Get-AliasSuggestion.ps1
Get-Answer.ps1
Get-Characteristics.ps1
Get-OwnerReport.ps1
Get-PageUrls.ps1
Invoke-CmdScript.ps1
New-GenericObject.ps1
Select-FilteredObject.ps1
(...)
```

就像 PowerShell cmdlet 一样，这些脚本的名称清晰、易于理解，且使用了来自 PowerShell 标准动词列表中的动词。

## 另请参考

- 配方 1.2，“运行程序、脚本和现有工具”
- 配方 11.3，“为命令名称查找合适的动词”
- 附录 J，*标准 PowerShell 动词*

# 11.2 编写函数

## 问题

你的脚本中有想要多次调用的命令，或者有一个脚本片段，你认为它是脚本主要目的的“辅助”部分。

## 解决方案

将这段通用代码放入一个函数中，然后调用该函数。例如，脚本中的这段摄氏度转换代码：

```
param([double] $fahrenheit)

## 将其转换为摄氏度
$celsius = $fahrenheit - 32
$celsius = $celsius / 1.8

## 输出答案
"$fahrenheit degrees Fahrenheit is $celsius degrees Celsius."
```

可以放入一个函数中（该函数本身放在脚本里）：

```
param([double] $fahrenheit)

## 将华氏度转换为摄氏度
function ConvertFahrenheitToCelsius([double] $fahrenheit)
{
    $celsius = $fahrenheit - 32
    $celsius = $celsius / 1.8
    $celsius
}

$celsius = ConvertFahrenheitToCelsius $fahrenheit

## 输出答案
"$fahrenheit degrees Fahrenheit is $celsius degrees Celsius."
```

虽然使用函数可以说让这个特定脚本变得更长、更难理解，但这项技术极其宝贵（且被使用），几乎适用于所有非平凡的脚本。

## 讨论

一旦你定义了一个函数，该定义之后的任何命令都可以使用它。这意味着你必须在脚本中使用它的任何部分*之前*定义你的函数。如果你的脚本定义了许多函数，你可能会觉得这很笨重，因为函数定义遮盖了脚本的主要逻辑部分。如果是这种情况，你可以将主要逻辑放在一个 `Main` 函数中，如 配方 11.21 所述。

### 注意

习惯于在 `cmd.exe` 中进行批处理脚本编写的人常问：“PowerShell 中 `GOTO` 的等价物是什么？”在 `GOTO` 用于调用子例程或批处理文件中其他孤立的辅助部分的情况下，请使用 PowerShell 函数来完成该任务。如果 `GOTO` 用作循环遍历某事物的方式，PowerShell 的循环机制更为合适。

在 PowerShell 中，调用函数的设计初衷是让它感觉就像调用 cmdlet 或脚本一样。作为用户，你不必知道一个小的辅助例程是作为 cmdlet、脚本还是函数编写的。当你调用函数时，只需在函数名后添加参数，并用空格分隔每一个参数（如解决方案所示）。这与许多编程语言（如 C#）调用函数的方式形成对比，后者在函数名后使用括号，并用逗号分隔每个参数：

```
## 正确
ConvertFahrenheitToCelsius $fahrenheit

## 错误
ConvertFahrenheitToCelsius($fahrenheit)
```

此外，请注意，函数的返回值是该函数写入输出管道的任何内容（如解决方案中的 `$celsius`）。如果你愿意，可以写 `return $celsius`，但这没有必要。

有关编写函数的更多信息，请参阅“编写脚本，重用功能”。有关 PowerShell 循环语句的更多信息，请参阅配方 4.4。

## 另请参考

- 配方 4.4，“使用循环重复操作”
- “编写脚本，重用功能”

# 11.3 为命令名称查找合适的动词

## 问题

你正在编写一个新脚本或函数，并想为该命令选择一个合适的动词。

## 解决方案

查看 `Get-Verb` 命令的输出，以找到适合你命令的动词：

```
PS > Get-Verb In* | Format-Table -Auto

Verb       Group
----       -----
Initialize Data
Install    Lifecycle
Invoke     Lifecycle
```

## 讨论

命令名称的一致性是 PowerShell 最有益的功能之一，这主要归功于其标准动词集。虽然描述性的命令名称（如 `Stop-Process`）清楚地说明了命令的作用，但标准动词使命令更易于发现。

例如，许多技术都有自己创建事物的词汇：*new*、*create*、*instantiate*、*build* 等。当用户查找命令时（如果没有标准动词的好处），用户必须知道该操作的领域特定术语。如果用户不知道领域特定的动词，他们只能被迫翻阅冗长的命令列表，希望某个名称能引起共鸣。

然而，当命令使用 PowerShell 的标准动词时，发现变得容易得多。一旦用户学会了某个操作的*标准动词*，他们就不需要搜索其领域特定的替代词。最重要的是，他们投入的时间（主动或被动地学习标准 PowerShell 动词）提高了他们对*所有*命令的效率，而不仅仅是特定领域的命令。

### 注意

这个可发现性问题非常重要，以至于当模块定义一个带有非标准动词的命令时，PowerShell 会生成一条警告消息。为了支持你的命令的领域特定名称*除了*标准名称之外，只需定义一个别名。有关更多信息，请参阅配方 11.8。




为了让你在编写脚本或函数时更容易选择标准动词，PowerShell 提供了 `Get-Verb` 函数。你可以查看该函数的输出，以找到适合你命令的动词。有关标准动词的更详细说明，请参阅 附录 J。

## 另请参阅

配方 11.8，“从模块中选择性导出命令”

附录 J，*标准 PowerShell 动词*

# 11.4 编写脚本块

## 问题

你的脚本中有一个部分对所有输入的工作方式几乎相同，只有一点逻辑上的细微变化。

## 解决方案

如 示例 11-3 所示，将细微的逻辑差异放在脚本块中，然后将该脚本块作为参数传递给需要它的代码。使用调用运算符 (`&`) 来执行脚本块。

##### 示例 11-3. 将脚本块应用于管道中每个元素的脚本

```powershell
##############################################################################
##
## Invoke-ScriptBlock
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Apply the given mapping command to each element of the input. (Note that
PowerShell includes this command natively, and calls it Foreach-Object)

.EXAMPLE

PS > 1,2,3 | Invoke-ScriptBlock { $_ * 2 }

#>

param(
    ## The script block to apply to each incoming element
    [ScriptBlock] $MapCommand
)

begin
{
    Set-StrictMode -Version 3
}
process
{
    & $mapCommand
}
```

## 讨论

想象一个脚本，需要将列表中的所有元素乘以 2：

```powershell
function MultiplyInputByTwo
{
   process
   {
       $_ * 2
   }
}
```

但它也需要执行更复杂的计算：

```powershell
function MultiplyInputComplex
{
   process
   {
       ($_ + 2) * 3
   }
}
```

这两个函数非常相似，除了实际执行计算的那一行。随着我们添加更多计算，这变得越来越明显。每添加一个新的七行函数，只能得到一行独特的有价值代码！

```powershell
PS > 1,2,3 | MultiplyInputByTwo
2
4
6
PS > 1,2,3 | MultiplyInputComplex
9
12
15
```

如果我们改用脚本块来保存这个“未知”计算，就不需要不断添加新函数：

```powershell
PS > 1,2,3 | Invoke-ScriptBlock { $_ * 2 }
2
4
6
PS > 1,2,3 | Invoke-ScriptBlock { ($_ + 2) * 3 }
9
12
15
PS > 1,2,3 | Invoke-ScriptBlock { ($_ + 3) * $_ }
4
10
18
```

事实上，`Invoke-ScriptBlock` 提供的功能非常有用，以至于它成为了一个标准的 PowerShell cmdlet——称为 `ForEach-Object`。有关脚本块的更多信息，请参阅 “编写脚本，重用功能”。有关运行脚本的更多信息，请参阅 配方 1.2。

## 另请参阅

配方 1.2，“运行程序、脚本和现有工具”

“编写脚本，重用功能”

# 11.5 从脚本、函数或脚本块返回数据

## 问题

你希望你的脚本或函数向调用它的任何内容返回数据。

## 解决方案

要从脚本或函数返回数据，请将该数据写入输出管道：

```powershell
##############################################################################
## Get-Tomorrow
##
## Get the date that represents tomorrow
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

Set-StrictMode -Version 3

function GetDate
{
    Get-Date
}

$tomorrow = (GetDate).AddDays(1)
$tomorrow
```

## 讨论

在 PowerShell 中，你的函数或脚本生成的任何数据都会被发送到输出管道，除非有东西捕获了该输出。`GetDate` 函数生成数据（一个日期）且不捕获它，因此它成为函数的输出。调用 `GetDate` 函数的脚本部分捕获该输出并对其进行操作。

最后，脚本将 `$tomorrow` 变量写入管道而不捕获它，因此它成为脚本本身的返回值。

###### 注意

某些 .NET 方法（例如 `System.Collections.ArrayList` 类）会产生输出，即使你可能不期望它们这样做。为了防止这些方法将数据发送到输出管道，请捕获数据或将其转换为 [`void`]：

```powershell
PS > $collection = New-Object System.Collections.ArrayList
PS > $collection.Add("Hello")
0
PS > [void] $collection.Add("Hello")
```

即使有这种“管道输出即返回值”的理念，PowerShell 仍然支持传统的 `return` 关键字作为从函数或脚本返回的方式。如果你在关键字后指定任何内容（例如 `return "Hello"`），PowerShell 会将其视为一个 `"Hello"` 语句，后跟一个 `return` 语句。

###### 注意

如果你想让脚本的其他阅读者清楚你的意图，可以使用 `Write-Output` cmdlet 显式地将数据发送到管道。两者产生相同的结果，所以这只是个人偏好的问题。

如果你将集合（如数组或 `ArrayList`）写入输出管道，PowerShell 实际上会将该集合的每个元素写入管道。为了在集合沿管道传输时保持其完整，请在返回时在其前面加上逗号。这将返回一个集合（将被展开），其中包含一个元素：你想要保持完整的集合。

```powershell
function WritesObjects
{
    $arrayList = New-Object System.Collections.ArrayList
    [void] $arrayList.Add("Hello")
    [void] $arrayList.Add("World")

    $arrayList
}

function WritesArrayList
{
    $arrayList = New-Object System.Collections.ArrayList
    [void] $arrayList.Add("Hello")
    [void] $arrayList.Add("World")

    ,$arrayList
}

$objectOutput = WritesObjects

# The following command would generate an error
# $objectOutput.Add("Extra")

$arrayListOutput = WritesArrayList
$arrayListOutput.Add("Extra")
```

虽然在 PowerShell 完全结构化的数据世界中相对少见，但你有时可能想使用退出代码来指示脚本的成功或失败。为此，PowerShell 提供了 `exit` 关键字。

有关 `return` 和 `exit` 语句的更多信息，请参阅 “编写脚本，重用功能” 和 配方 15.1。

## 另请参阅

配方 15.1，“确定最后一条命令的状态”

“编写脚本，重用功能”

# 11.6 将常用命令打包在模块中

## 问题

你开发了一组有用的命令或函数。你想将它们提供给用户，或在多个脚本之间共享。

## 解决方案

首先，将这些通用函数定义单独放在一个扩展名为 *.psm1* 的文件中，如 示例 11-4 所示。

##### 示例 11-4. 温度命令模块



# Temperature.psm1

```powershell
##############################################################################
##
## Temperature.psm1
## 操作和转换温度的命令
##
## 摘自《PowerShell Cookbook》（O'Reilly）
## 作者：Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

## 将华氏度转换为摄氏度
function Convert-FahrenheitToCelsius([double] $fahrenheit)
{
    $celsius = $fahrenheit - 32
    $celsius = $celsius / 1.8
    $celsius
}

## 将摄氏度转换为华氏度
function Convert-CelsiusToFahrenheit([double] $celsius)
{
    $fahrenheit = $celsius * 1.8
    $fahrenheit = $fahrenheit + 32
    $fahrenheit
}
```

接下来，将该文件放置在你的 `Modules` 目录中（由 `PSModulePath` 环境变量定义），在一个同名的子目录下。例如，将 `Temperature.psm1` 放在 `<My Documents>\PowerShell\Modules\Temperature` 中。调用 `Import-Module` 命令将该模块（及其命令）导入到你的会话中，如 示例 11-5 所示。

### 示例 11-5. 导入模块

```powershell
PS > Import-Module Temperature
PS > Convert-FahrenheitToCelsius 81
27.2222222222222
```

## 讨论

PowerShell 模块为你提供了一种简单的方式来打包相关的命令和功能。正如解决方案所示，编写模块就像向文件中添加函数一样简单。

与核心命令的命名一样，模块中打包的命令的命名在给予用户一致且可发现的 PowerShell 体验方面起着关键作用。当你为模块中的命令命名时，请确保它们遵循 `Verb-Noun` 语法，并从 PowerShell 的标准动词集中选择动词。如果你的模块不遵循这些标准，用户在加载你的模块时将收到警告消息。有关如何使模块命令可发现（并根据需要特定于领域）的信息，请参阅 配方 11.8。

除了创建包含模块命令的 `.psm1` 文件外，你还应该创建一个*模块清单*来描述其内容和系统要求。模块清单允许你定义模块的作者、公司、版权信息等。有关更多信息，请参阅 `New-ModuleManifest` cmdlet。

编写模块后，最后一步是使其可供系统使用。当你调用 `Import-Module <模块名称>` 来加载模块时，PowerShell 会查看 `PSModulePath` 环境变量中列出的每个目录。

### 注意

`PSModulePath` 变量是一个环境变量，就像系统的 `PATH` 环境变量一样。有关如何查看和修改环境变量的更多信息，请参阅 配方 16.1。

如果 PowerShell 找到一个名为 `<模块名称>` 的目录，它会在该目录中查找同名的 `psm1` 文件。一旦找到 `psm1` 文件，它就会将该模块加载到你的会话中。除了 `psm1` 文件，PowerShell 还支持*模块清单* (`psd1`) 文件，它们允许你定义大量*关于*模块的信息：其作者、描述、嵌套模块、版本要求等等。有关更多信息，请输入 `Get-Help New-ModuleManifest`。

如果你只想让模块对你自己可用（或者如果你将模块作为安装过程的一部分进行安装，则为“当前用户”），请将其放在每用户模块文件夹中：`<My Documents>\PowerShell\Modules\<模块名称>`。如果你想让模块对系统的所有用户可用，请将模块放在 *Program Files* 目录下的专用目录中，然后将该目录添加到系统范围的 `PSModulePath` 环境变量中。

如果你不想永久安装模块，可以在加载模块时指定 `psm1` 文件的完整路径。例如：

```powershell
Import-Module c:\tools\Temperature.psm1
```

如果你想从脚本所在的同一目录加载模块，请参阅 配方 16.6。

当你从脚本加载模块时，PowerShell 会使该模块的命令在整个会话中可用。例如，如果你的脚本加载了 `Temperature` 模块，则该模块中的函数在脚本退出后仍将可用。为确保脚本在退出后不会意外影响用户的会话，你应该移除加载的任何模块：

```powershell
$moduleToRemove = $null
if(-not (Get-Module <模块名称>))
{
    $moduleToRemove = Import-Module <模块名称> -Passthru
}

######################
##
## 脚本内容放这里
##
######################

if($moduleToRemove)
{
    $moduleToRemove | Remove-Module
}
```

如果你有一个*模块*加载辅助模块（而不是*脚本*加载辅助模块），则不需要此步骤。由模块加载的模块仅影响加载它们的模块。

如果你想让用户在加载模块时配置它，可以在模块开头定义一个参数块。这些参数随后通过 `Import-Module` 命令的 `-ArgumentList` 参数填充。例如，一个接受“重试次数”和网站作为参数的模块：

```powershell
param(
    [int] $RetryCount,
    [URI] $Website
)

function Get-Page
{
    ....
}
```

用户将使用以下命令行加载模块：

```powershell
Import-Module <模块名称> -ArgumentList 10,"http://www.example.com"
Get-Page "/index.html"
```

关于 `-ArgumentList` 参数的一个重要点是，它对用户输入的支持比大多数脚本、函数和脚本块提供的支持要有限得多。PowerShell 允许你按名称、别名以及有序或无序的方式访问大多数 `param()` 语句中的参数。另一方面，提供给 `Import-Module` 命令的参数必须仅作为值提供，并且必须按照模块定义的确切顺序提供。

有关访问命令参数的更多信息，请参阅 配方 11.11。有关导入模块（以及可用的不同类型的模块）的更多信息，请参阅 配方 1.28。有关模块的更多信息，请输入 `Get-Help about_Modules`。

## 另请参阅

- 配方 1.28，“使用附加命令扩展 Shell”
- 配方 11.8，“从模块中选择性导出命令”
- 配方 11.11，“访问脚本、函数或脚本块的参数”
- 配方 16.1，“查看和修改环境变量”

# 11.7 编写维护状态的命令

## 问题

你有一个函数或脚本，需要在调用之间维护状态。

## 解决方案

将这些命令放在一个*模块*中。将要保留的任何信息存储在一个变量中，并给该变量一个 `SCRIPT` 作用域。请参阅 示例 11-6。

### 示例 11-6. 一个维护状态的模块



# PersistentState.psm1

演示通过模块作用域变量实现持久状态。

源自 *PowerShell Cookbook*（O'Reilly 出版），作者 Lee Holmes（http://www.leeholmes.com/guide）

```powershell
$SCRIPT:memory = $null

function Set-Memory
{
    param(
        [Parameter(ValueFromPipeline = $true)]
        $item
    )

    begin { $SCRIPT:memory = New-Object System.Collections.ArrayList }
    process { $null = $memory.Add($item) }
}

function Get-Memory
{
    $memory.ToArray()
}

Set-Alias remember Set-Memory
Set-Alias recall Get-Memory

Export-ModuleMember -Function Set-Memory,Get-Memory
Export-ModuleMember -Alias remember,recall
```

## 讨论

编写脚本或命令时，经常需要在命令调用之间维护状态。例如，命令可能会记住用户首选项、缓存配置数据或存储其他类型的模块状态。请参阅示例 11-7。

### 示例 11-7. 使用维护状态的命令

```powershell
PS > Import-Module PersistentState
PS > Get-Process -Name PowerShell | remember
PS > recall

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    527       6    32704      44140   172     2.13   2644 powershell
    517       7    23080      33328   154     1.81   2812 powershell
    357       6    31848      33760   165     1.42   3576 powershell
```

在基本脚本中，跨调用维护状态的唯一方法是将信息存储在全局变量中。但这会引入两个问题。

第一个问题是全局变量的影响远不止定义它们的脚本。一旦脚本将信息存储在全局变量中，就会污染用户的会话。如果用户有同名变量，脚本会覆盖其内容。第二个问题是这种污染的自然对应面。当脚本将信息存储在全局变量中时，用户和其他脚本都能访问它。由于意外或好奇，这些“内部”全局变量很容易被破坏或损坏。

可以通过使用模块来解决此问题。将命令放入模块后，PowerShell 会使具有*脚本*作用域的变量可供该模块中的所有命令使用。除了让脚本作用域变量可供所有命令使用外，PowerShell 还会在命令调用之间保持它们的值。

#### 注意

与变量一样，PowerShell 驱动器也遵循作用域的概念。在模块内使用 `New-PSDrive` cmdlet 时，该驱动器将保持为该模块私有。若要创建一个在模块外部也可见的新驱动器，请使用*全局*作用域创建它：

```powershell
New-PSDrive -Name Temp FileSystem -Root C:\Temp -Scope Global
```

有关变量及其作用域的详细信息，请参阅配方 3.6。有关定义模块的详细信息，请参阅配方 11.6。

## 另请参阅

- 配方 3.6，“控制变量和其他项目的访问与作用域”
- 配方 11.6，“在模块中打包通用命令”

# 11.8 从模块中选择性导出命令

## 问题

你有一个模块，并且只想从该模块导出特定命令。

## 解决方案

使用 `Export-ModuleMember` cmdlet 声明要导出的特定命令。所有其他命令将保留为模块内部命令。请参阅示例 11-8。

### 示例 11-8. 从模块导出特定命令

```powershell
##############################################################################
##
## SelectiveCommands.psm1
## 演示模块命令的选择性导出
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

## 一个内部辅助函数
function MyInternalHelperFunction
{
    "Result from my internal helper function"
}

## 从模块导出的命令
function Get-SelectiveCommandInfo
{
    "Getting information from the SelectiveCommands module"
    MyInternalHelperFunction
}

## 标准命令的替代名称
Set-Alias gsci Get-SelectiveCommandInfo
Set-Alias DomainSpecificVerb-Info Get-SelectiveCommandInfo

## 导出特定命令
Export-ModuleMember -Function Get-SelectiveCommandInfo
Export-ModuleMember -Alias gsci,DomainSpecificVerb-Info
```

## 讨论

默认情况下，当 PowerShell 导入模块时，它会导入该模块中定义的所有函数。这使得你（作为模块作者）能够非常简单地创建相关命令库。

随着模块命令变得更复杂，你通常会编写辅助函数和支持例程。由于这些命令不打算直接暴露给用户，因此你需要从模块中选择性地导出命令。`Export-ModuleMember` 命令正是为此而设计的。

一旦模块包含对 `Export-ModuleMember` 的调用，PowerShell 将不再导出模块中的所有函数。相反，它只导出你定义的命令。示例 11-8 中对 `Export-ModuleMember` 的第一次调用演示了如何从模块中选择性地导出函数。

由于命令名称的一致性是 PowerShell 最有益的功能之一，如果模块导出的函数（无论是显式还是默认）使用非标准动词，PowerShell 会生成警告消息。例如，假设你有一项技术，使用*重新生成配置*作为任务的高度特定短语。此外，它已经有一个 `regen` 命令来完成此任务。

你可能会自然地考虑将 `Regenerate-Configuration` 和 `regen` 作为要从模块导出的函数名称，但这样做会疏远那些没有你技术深厚背景的用户。如果没有相同的技术专长，他们就不会知道命令的名称，而是会根据现有的 PowerShell 知识本能地寻找 `Reset-Configuration`、`Restore-Configuration` 或 `Initialize-Configuration`。在这种情况下，解决方案是使用标准动词命名函数，*同时*使用命令别名来支持领域专家。

`Export-ModuleMember` cmdlet 也支持这种情况。除了允许你从模块中选择性地导出命令外，它还允许你为模块命令导出替代名称（*别名*）。示例 11-8 中对 `Export-ModuleMember` 的第二次调用（以及其前面的别名定义）演示了如何从模块导出别名。

有关命令命名的详细信息，请参阅配方 11.3。有关编写模块的详细信息，请参阅配方 11.6。

## 另请参阅

- 配方 3.6，“控制变量和其他项目的访问与作用域”
- 配方 11.3，“为命令名称查找合适的动词”
- 配方 11.6，“在模块中打包通用命令”

# 11.9 诊断并与内部模块状态交互

## 问题

你有一个模块，并且想要检查其内部变量和函数。

## 解决方案

使用 `Enter-Module` 脚本（示例 11-9）临时进入模块并在其作用域内调用命令。



## 示例 11-9. 在模块作用域内调用命令

```
##############################################################################
##
## Enter-Module
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Lets you examine internal module state and functions by executing user
input in the scope of the supplied module.

.EXAMPLE

PS > Import-Module PersistentState
PS > Get-Module PersistentState

ModuleType Name                      ExportedCommands
---------- ----                      ----------------
Script     PersistentState           {Set-Memory, Get-Memory}


PS > "Hello World" | Set-Memory
PS > $m = Get-Module PersistentState
PS > Enter-Module $m
PersistentState: dir variable:\mem*

Name                           Value
----                           -----
memory                         {Hello World}

PersistentState: exit
PS >

#>

param(
    ## The module to examine
    [System.Management.Automation.PSModuleInfo] $Module
)

Set-StrictMode -Version 3

$userInput = Read-Host $($module.Name)
while($userInput -ne "exit")
{
    $scriptblock = [ScriptBlock]::Create($userInput)
    & $module $scriptblock

    $userInput = Read-Host $($module.Name)
}
```

## 讨论

PowerShell 模块是创建共享私有状态的相关命令集的有效方式。虽然模块中的命令可以在彼此间共享私有状态，但 PowerShell 可防止该状态意外影响 PowerShell 会话的其余部分。

不过，在开发模块时，您有时可能需要出于诊断目的与该内部状态进行交互。为了支持这一点，PowerShell 允许您使用调用运算符 (`&`) 以特定模块为目标：

```
PS > $m = Get-Module PersistentState
PS > & $m { dir variable:\mem* }

Name                           Value
----                           -----
memory                         {Hello World}
```

对于更详细的调查任务，此语法显得笨重，因此 `Enter-Module` 会为您自动完成提示和调用。

有关编写模块的更多信息，请参阅食谱 11.6。

## 另请参考

食谱 11.6，“在模块中打包通用命令”

## 11.10 处理模块移除时的清理任务

## 问题

您有一个模块，并希望在该模块被移除时执行某些操作（如清理任务）。

## 解决方案

将脚本块赋值给 `$MyInvocation.MyCommand.ScriptBlock.Module.OnRemove` 事件。将任何清理命令放在该脚本块中。请参阅示例 11-10。

### 示例 11-10. 从模块内部处理清理任务

```
##############################################################################
##
## TidyModule.psm1
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS
Demonstrates how to handle cleanup tasks when a module is removed

.EXAMPLE

PS > Import-Module TidyModule
PS > $TidyModuleStatus
Initialized
PS > Remove-Module TidyModule
PS > $TidyModuleStatus
Cleaned Up

#>

## Perform some initialization tasks
$GLOBAL:TidyModuleStatus = "Initialized"

## Register for cleanup
$MyInvocation.MyCommand.ScriptBlock.Module.OnRemove = {
    $GLOBAL:TidyModuleStatus = "Cleaned Up"
}
```

## 讨论

PowerShell 模块有一种自然的方式来定义初始化要求（编写在模块主体中的任何脚本），但清理要求并不那么简单。

在模块创建期间，您可以使用 `$MyInvocation.MyCommand.ScriptBlock.Module` 属性访问您的模块。每个模块都有一个 `OnRemove` 事件，您可以通过向其分配一个脚本块来订阅该事件。当 PowerShell 卸载您的模块时，它会调用该脚本块。

请勿将此技术用于极其敏感的清理要求。如果用户只是关闭 PowerShell 窗口，则不会处理 `OnRemove` 事件。如果存在此顾虑，请注册 `PowerShell.Exiting` 引擎事件并从那里移除您的模块：

```
Register-EngineEvent PowerShell.Exiting { Remove-Module TidyModule }
```

这免去了用户必须记住调用 `Remove-Module` 的麻烦。

有关编写模块的更多信息，请参阅食谱 11.6。有关 PowerShell 事件的更多信息，请参阅食谱 31.2。

## 另请参考

食谱 11.6，“在模块中打包通用命令”

食谱 31.2，“创建并响应自定义事件”

## 11.11 访问脚本、函数或脚本块的参数

## 问题

您想要访问提供给脚本、函数或脚本块的参数。

## 解决方案

要按名称访问参数，请使用 `param` 语句：

```
param($firstNamedArgument, [int] $secondNamedArgument = 0)

"First named argument is: $firstNamedArgument"
"Second named argument is: $secondNamedArgument"
```

要按位置访问未命名参数，请使用 `$args` 数组：

```
"First positional argument is: " + $args[0]
"Second positional argument is: " + $args[1]
```

您可以在脚本、函数和脚本块中以完全相同的方式使用这些技术，如示例 11-11 所示。

### 示例 11-11. 在脚本、函数和脚本块中使用参数

```
##############################################################################
##
## Get-Arguments
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Uses command-line arguments

#>

param(
    ## The first named argument
    $FirstNamedArgument,

    ## The second named argument
    [int] $SecondNamedArgument = 0
)

Set-StrictMode -Version 3

## Display the arguments by name
"First named argument is: $firstNamedArgument"
"Second named argument is: $secondNamedArgument"

function GetArgumentsFunction
{
    ## We could use a param statement here, as well
    ## param($firstNamedArgument, [int] $secondNamedArgument = 0)

    ## Display the arguments by position
    "First positional function argument is: " + $args[0]
    "Second positional function argument is: " + $args[1]
}

GetArgumentsFunction One Two

$scriptBlock =
{
    param($firstNamedArgument, [int] $secondNamedArgument = 0)

    ## We could use $args here, as well
    "First named scriptblock argument is: $firstNamedArgument"
    "Second named scriptblock argument is: $secondNamedArgument"
}

& $scriptBlock -First One -Second 4.5
```

示例 11-11 生成以下输出：

```
PS > Get-Arguments First 2
First named argument is: First
Second named argument is: 2
First positional function argument is: One
Second positional function argument is: Two
First named scriptblock argument is: One
Second named scriptblock argument is: 4
```

## 讨论

虽然 PowerShell 同时支持 `param` 关键字和 `$args` 数组，但您通常更倾向于使用 `param` 关键字来定义和访问脚本、函数和脚本块的参数。

### 注意

在大多数语言中，通过 `$args` 数组访问参数最常见的原因是确定当前正在运行的脚本的名称。有关如何在 PowerShell 中执行此操作的信息，请参阅食谱 16.3。




# 11.11 在 PowerShell 中使用参数

当你使用 `param` 关键字定义参数时，PowerShell 会为你的脚本或函数提供许多有用的功能，让用户能够像使用 Cmdlet 一样使用你的脚本：

*   用户只需指定足以将其与其他参数区分开来的参数名称即可。
*   用户能更清晰地理解参数的含义。
*   你可以指定参数的类型，PowerShell 会在需要时用它来转换输入。
*   你可以为参数指定默认值。

## 支持 PowerShell 的通用参数

除了你定义的参数外，你可能还希望支持 PowerShell 的标准参数：`-Verbose`、`-Debug`、`-ErrorAction`、`-WarningAction`、`-InformationAction`、`-ErrorVariable`、`-WarningVariable`、`-InformationVariable`、`-OutVariable`、`-OutBuffer` 和 `-PipelineVariable`。

要获取这些额外参数，请在函数内部添加 `[CmdletBinding()]` 特性，或在脚本顶部声明它。即使你的函数或脚本未声明任何参数，`param()` 语句也是必需的。这些（及其他相关）额外功能现在使你的函数成为*高级函数*。请参阅示例 11-12。

### 示例 11-12. 声明高级函数

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding()]
    param()

    Write-Verbose "Verbose Message"
}
```

如果你的函数定义了具有高级*验证*的参数，则无需显式添加 `[CmdletBinding()]` 特性。在这种情况下，PowerShell 已经知道将你的命令视为高级函数。

#### 注意

在 PowerShell 的测试阶段，*高级函数*被称为*脚本 Cmdlet*。我们决定更改名称，因为*脚本 Cmdlet* 这一术语让人对未知领域产生恐惧感。用户习惯编写函数，但“没时间去学那些新的脚本 Cmdlet 东西”。因为脚本 Cmdlet 只是具有额外功能的普通函数，所以新名称更讲得通。

虽然 PowerShell 将所有通用参数添加到你的函数中，但你实际上不需要实现支持它们的代码。例如，对 `Write-Verbose` 的调用通常不产生输出。当用户向你的函数指定 `-Verbose` 参数时，PowerShell 会自动显示 `Write-Verbose` Cmdlet 的输出。

```
PS > Invoke-MyAdvancedFunction
PS > Invoke-MyAdvancedFunction -Verbose
VERBOSE: Verbose Message
```

如果你的 Cmdlet 修改系统状态，支持标准的 `-WhatIf` 和 `-Confirm` 参数将非常有帮助。有关如何实现这一点的信息，请参阅配方 11.15。

## 使用 $args 数组

尽管命名参数、通用参数和高级函数提供了强大的功能，但 `$args` 数组有时仍然很有用。例如，它提供了一种一次性处理所有参数的简洁方法：

```
function Reverse
{
    $argsEnd = $args.Length - 1
    $args[$argsEnd..0]
}
```

这将产生：

```
PS > Reverse 1 2 3 4
4
3
2
1
```

如果你在脚本中定义了参数，`$args` 数组代表未被这些参数捕获的任何参数：

```
function MyParamsAndArgs {
    param($MyArgument)

    "Got MyArgument: $MyArgument"
    "Got Args: $args"
}
```

```
PS > MyParamsAndArgs -MyArgument One Two Three
Got MyArgument: One
Got Args: Two Three
```

直到现在，本配方中的所有示例都展示了如何从命令内部访问命令参数。在某些情况下，你可能需要知道如果提供输入，其他命令*将会*如何处理它。对于此场景，你可以使用 PowerShell `static` 参数绑定器类。例如，这个高级函数允许位置参数，但第一个参数究竟被计为 `Id` 还是 `Name`，完全取决于你传入的内容：

```
function Invoke-ComplexFunction
{
     param(
         [Parameter(ParameterSetName = "ById", Position = 0)]
         [int] $Id,

         [Parameter(ParameterSetName = "ByName", Position = 0)]
         [string] $Name,

         [Parameter(Position = 1)]
         [string] $Extra
     )
}
```

如果我们使用 `static` 参数绑定器，我们可以看到 PowerShell 将如何处理该输入：

```
$script = { Invoke-ComplexFunction 1234 Hello }
$command = $script.Ast.Find(
    { param($Ast) $Ast -is [Management.Automation.Language.CommandAst] }, $false )
$results = [Management.Automation.Language.StaticParameterBinder]::BindCommand(
    $command )
```

当我们窥探 `$results` 时，可以看到 PowerShell 会选中 `Id` 和 `Extra` 参数：

```
PS > $results.BoundParameters

Key   Value
---   -----
Id    System.Management.Automation.Language.ParameterBindingResult
Extra System.Management.Automation.Language.ParameterBindingResult
```

甚至能看到它们将被赋予的值：

```
PS > $results.BoundParameters.Id

ConstantValue Value
------------- -----
          1234 1234


PS > $results.BoundParameters.Extra

ConstantValue Value
------------- -----
Hello         Hello
```

有关 `param` 语句的更多信息，请参阅“编写脚本，重用功能”。有关运行脚本的更多信息，请参阅配方 1.2。有关 PowerShell 引擎公开的功能（如 `-Whatif` 和 `-Confirm`）的更多信息，请参阅配方 11.15。

有关如何声明具有丰富验证和行为的参数的信息，请参阅配方 11.12。

## 另请参阅

*   配方 1.2，“运行程序、脚本和现有工具”
*   配方 11.12，“向参数添加验证”
*   配方 11.15，“提供 -WhatIf、-Confirm 和其他 Cmdlet 功能”
*   配方 16.3，“访问有关命令调用的信息”
*   “编写脚本，重用功能”

# 11.12 向参数添加验证

## 问题

你希望确保传递给参数的用户输入满足特定的限制或约束。

## 解决方案

使用 `[Parameter()]` 特性将参数声明为强制性、位置性、互斥参数集的一部分，或能够从管道接收输入。

```
param(
    [Parameter(
        Mandatory = $true,
        Position = 0,
        ValueFromPipeline = $true,
        ValueFromPipelineByPropertyName = $true)]
    [string[]] $Name
)
```

使用额外的验证特性来定义别名、支持 null 或空值、计数限制（针对集合）、长度限制（针对字符串）、正则表达式要求、范围要求（针对数字）、允许值要求，甚至任意脚本要求。

```
param(
    [ValidateLength(5,10)]
    [string] $Name
)

"Hello $Name"
```

## 讨论

传统 Shell 要求扩展（脚本和命令）手动编写参数支持，导致行为差异巨大。有些仅实现了最基本、令人困惑的支持。其他的实现了更复杂的功能，但方式与任何其他命令都不同。最基本、令人困惑的支持最为常见，因为编写功能完备的参数支持是一项复杂的工程。





幸运的是，PowerShell 引擎已经编写了所有复杂的参数处理支持，并为你管理所有这些细节。你无需编写代码来强制执行这些规则，只需将参数标记为强制性、位置性，或声明其验证要求即可。这种对参数行为和验证的内置支持构成了 PowerShell 独特一致性的核心基石。

参数验证是行为良好的脚本与不良脚本之间的主要区别之一。在运行新脚本（或你很久以前编写的脚本）时，查看参数定义和验证要求是快速熟悉该脚本行为的最佳途径之一。

从脚本作者的角度来看，验证要求能让你免于编写原本无论如何都需要编写的验证代码。

### 定义参数行为

`[Parameter()]` 特性的元素主要定义了参数与其他参数之间的行为关系。所有元素均为可选。

对于仅接受 `$true` 或 `$false` 值的元素，你可以省略 `` `= $true` `` 赋值：

`Mandatory` = `$true`

: 

将参数定义为强制性。如果用户未为此参数提供值，PowerShell 会自动提示用户输入。如果未指定，则该参数为可选。

`Position` = `` `position` ``

: 

定义此参数的位置。当用户在未指定参数名的情况下提供参数值时（例如 `Invoke-MyFunction -Param1 Argument1 Argument2` 中的 `` `Argument2` ``），此设置生效。PowerShell 会将这些值按位置编号从低到高分配给定义了 `Position` 的参数。如果未指定，用户必须提供该参数的名称。

`ParameterSetName` = `` `name` ``

: 

将此参数定义为一组相关参数中的成员。此后，此参数的行为将特定于该相关参数集，且该参数仅存在于定义了它的参数集中。例如，当用户只能提供 `Name` *或* `ID` 时会用到此功能。若要将某参数包含在两个或更多特定参数集中，请使用两个或更多 `[Parameter()]` 特性。如果未指定，该参数将属于所有参数集。要定义 cmdlet 的默认参数集名称，请在 `CmdletBinding` 特性中提供：`[CmdletBinding(DefaultParameterSetName = "Name")]`。

`ValueFromPipeline` = `$true`

: 

声明此参数可直接接受管道输入。如果用户将数据通过管道传入你的脚本或函数，PowerShell 会在命令的 `process {}` 代码块中将此输入赋值给你的参数。有关接受管道输入的更多信息，请参阅 Recipe 11.18。请勿将此参数应用于 `String` 类型的参数，因为几乎所有输入都可以转换为字符串——这往往会产生毫无意义的结果。如果未指定，该参数不直接接受管道输入。

`ValueFromPipelineByPropertyName` = `$true`

: 

声明此参数可接受管道输入，前提是传入对象的某个属性名称与其名称匹配。如果为 true，PowerShell 会在命令的 `process {}` 代码块中将该属性的值赋给你的参数。有关接受管道输入的更多信息，请参阅 Recipe 11.18。如果未指定，该参数不按属性名称接受管道输入。

`ValueFromRemainingArguments` = `$true`

: 

声明此参数接受所有未分配给位置参数或命名参数的剩余输入。只能有一个参数拥有此元素。如果没有参数声明支持此功能，PowerShell 会为无法分配的参数生成错误。

### 定义参数验证

除了 `[Parameter()]` 特性外，PowerShell 还允许你应用其他特性，为参数添加进一步的行为或验证约束。所有验证特性均为可选：

`[Alias("name")]`

: 

为此参数定义一个别名。这对于名称冗长但描述性强、却有更通俗常用称呼的参数特别有帮助。如果未指定，该参数只能通过你最初声明的名称来引用。你可以为一个参数提供多个别名。要了解命令参数的别名，请参阅 Recipe 1.20。

`[AllowNull()]`

: 

允许此参数接收 `$null` 作为其值。这仅对强制性参数有要求。如果未指定，强制性参数不能接收 `$null` 作为其值，但可选参数可以。

`[AllowEmptyString()]`

: 

允许此字符串参数接收空字符串作为其值。这仅对强制性参数有要求。如果未指定，强制性字符串参数不能接收空字符串作为其值，但可选字符串参数可以。你可以将其应用于非字符串参数，但不会产生任何影响。

`[AllowEmptyCollection()]`

: 

允许此集合参数接收空集合作为其值。这仅对强制性参数有要求。如果未指定，强制性集合参数不能接收空集合作为其值，但可选集合参数可以。你可以将其应用于非集合参数，但不会产生任何影响。

`[ValidateCount(lower limit, upper limit)]`

: 

限制传递给此参数的集合中元素的数量。如果未指定，强制性参数的下限为一个元素。可选参数无限制。你可以将其应用于非集合参数，但不会产生任何影响。

`[ValidateLength(lower limit, upper limit)]`

: 

限制此参数可接受的字符串长度。如果未指定，强制性参数的下限为一个字符。可选参数无限制。你可以将其应用于非字符串参数，但不会产生任何影响。

`[ValidatePattern("regular expression")]`

: 

强制传递给此字符串参数的输入必须匹配指定模式。如果未指定，字符串输入无模式要求。你可以将其应用于非字符串参数，但不会产生任何影响。

不过，如果你的参数有模式要求，在脚本或函数主体中验证参数可能更有效。PowerShell 在参数无法匹配此模式时生成的错误信息不太友好（“The argument…does not match the `` `<pattern>` `` pattern”）。相反，你可以生成一条解释该模式*意图*的消息：

```
if($EmailAddress -notmatch Pattern)
{
    throw "Please specify a valid email address."
}
```

`[ValidateRange(lower limit, upper limit)]`

: 

限制此参数可接受的数值参数的上下限。如果未指定，参数无范围限制。你可以将其应用于非数值参数，但不会产生任何影响。

`[ValidateScript( { script block } )]`

: 

确保传递给此参数的输入满足你在脚本块中提供的条件。PowerShell 将提议的输入赋值给 `$_`（或 `$PSItem`）变量，然后调用你的脚本块。如果脚本块返回 `$true`（或任何可转换为 `$true` 的值，如非空字符串），PowerShell 即视为验证成功。

`[ValidateSet("First Option", "Second Option", …, "Last Option")]`

: 




### `[ValidateNotNull()]`

确保提供给此参数的输入不为 null。这是强制参数的默认行为，此属性仅对可选参数有用。当应用于字符串参数时，`$null` 参数值会被转换为空字符串。

### `[ValidateNotNullOrEmpty()]`

确保提供给此参数的输入既不为 null 也不为空。这是强制参数的默认行为，此属性仅对可选参数有用。当应用于字符串参数时，输入必须是长度大于 1 的字符串。当应用于集合参数时，集合必须至少包含一个元素。当应用于其他类型的参数时，此属性等同于 `[ValidateNotNull()]` 属性。

有关高级参数验证的更多信息，请输入 `Get-Help about_functions_advanced_parameters`。

## 另请参阅

- 食谱 1.20，“程序：学习常用参数的别名”
- 食谱 11.18，“访问脚本的管道输入”
- “向命令提供输入”

# 11.13 接受带有局部变量的脚本块参数

## 问题

您的命令将脚本块作为参数。当您调用该脚本块时，您希望变量引用用户会话中的变量，而不是您的脚本中的变量。

## 解决方案

在提供的脚本块上调用 `GetNewClosure()` 方法，然后再定义您自己的变量或调用该脚本块。请参阅示例 11-13。

### 示例 11-13. 支持来自用户会话变量的命令

```
##############################################################################
##
## Invoke-ScriptBlockClosure
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

演示脚本块上 GetNewClosure() 方法，该方法从用户会话中引入变量
（如果已定义）。

.EXAMPLE

PS > $name = "Hello There"
PS > Invoke-ScriptBlockClosure { $name }
Hello There
Hello World
Hello There

#>

param(
    ## 要调用的脚本块
    [ScriptBlock] $ScriptBlock
)

Set-StrictMode -Version 3

## 创建一个新的脚本块，从用户的作用域引入变量
## （如果已定义。）
$closedScriptBlock = $scriptBlock.GetNewClosure()

## 正常调用脚本块。$name 变量的内容将来自用户的会话。
& $scriptBlock

## 定义一个新变量
$name = "Hello World"

## 正常调用脚本块。$name 变量的内容现在将是 "Hello World"，
## 来自我们的作用域。
& $scriptBlock

## 调用“封闭的”脚本块。$name 变量的内容仍将是用户会话中的内容
## （如果已定义。）
& $closedScriptBlock
```

## 讨论

每当您调用脚本块时（例如，用户作为参数值传递的脚本块），PowerShell 会处理该脚本块中的变量，就好像您自己输入的一样。例如，如果脚本块引用的变量在您的脚本或模块中定义，PowerShell 在计算该变量时将使用该值。

这通常是期望的行为，尽管其用法最终取决于您的脚本。例如，食谱 11.4 接受一个脚本块参数，旨在引用脚本*内部*定义的变量：具体来说是 `$_`（或 `$PSItem`）。

或者，这可能并不总是您想要的。有时，您可能希望变量名引用来自*用户会话*的变量，而不是可能来自您的脚本的变量。

在这种情况下，解决方案是调用 `GetNewClosure()` 方法。此方法使脚本块成为自包含的，或*封闭的*。变量保持调用 `GetNewClosure()` 方法时的值，即使创建了同名的新变量。

## 另请参阅

- 食谱 3.6，“控制变量和其他项目的访问与作用域”
- 食谱 11.4，“编写脚本块”

# 11.14 动态组合命令参数

## 问题

您想要指定即将调用的命令的参数，但事先不知道这些参数是什么。

## 解决方案

将参数及其值定义为哈希表的元素，然后使用 `@` 字符将该哈希表传递给命令：

```
PS > $parameters = @{
    Name = "PowerShell";
    WhatIf = $true
}

PS > Stop-Process @parameters
What if: Performing operation "Stop-Process" on Target "powershell (2380)".
What if: Performing operation "Stop-Process" on Target "powershell (2792)".
```

## 讨论

当您编写调用其他命令的命令时，常见的问题是不知道将要传递给目标命令的确切参数值。解决方法很简单，就是将参数值存储在变量中：

```
PS > function Stop-ProcessWhatIf($name)
{
    Stop-Process -Name $name -Whatif
}

PS > Stop-ProcessWhatIf PowerShell
What if: Performing operation "Stop-Process" on Target "powershell (2380)".
What if: Performing operation "Stop-Process" on Target "powershell (2792)".
```

当您使用这种方法时，如果您事先不知道要传递哪些参数*名称*，事情似乎会变得更加困难。PowerShell 通过一种称为*展开*的技术显著改善了这种情况，该技术允许您传递参数值*和*名称。

第一步是定义一个变量——例如 `parameters`。在该变量中，存储参数名称及其值的哈希表。调用命令时，您可以使用 `@` 字符和存储它们的变量名来传递参数名称和值的哈希表。请注意，您使用 `@` 字符来表示变量，而不是通常的 `$` 字符：

```
Stop-Process @parameters
```

当您编写旨在增强或扩展现有命令的命令时，这是一个常见需求。在这种情况下，您只想将用户的所有输入（参数值*和*名称）传递给现有命令，即使您不知道他们确切提供了什么。

为了进一步简化这种情况，*高级函数*可以访问一个名为 `PSBoundParameters` 的自动变量。此自动变量是一个哈希表，存储传递给当前命令的所有参数，适用于调整和展开。有关此方法的示例，请参阅食谱 11.23。

除了支持 `PSBoundParameters` 自动变量的展开外，PowerShell 还支持 `$args` 数组的展开，用于极其轻量的命令包装器：

```
PS > function rsls { dir -rec | Select-String @args }
PS > rsls -SimpleMatch '["Pattern"]'
```

有关高级函数的更多信息，请参阅食谱 11.11。

## 另请参阅

- 食谱 11.11，“访问脚本、函数或脚本块的参数”
- 食谱 11.23，“程序：增强或扩展现有 Cmdlet”

# 11.15 提供 -WhatIf、-Confirm 和其他 Cmdlet 功能

## 问题



# 11.15 支持 `-WhatIf` 和 `-Confirm` 参数

## 问题

你想要支持标准的 `-WhatIf` 和 `-Confirm` 参数，并访问 PowerShell 引擎中以 cmdlet 为中心的支持。

## 解决方案

确保你的脚本或函数声明了 `[CmdletBinding()]` 特性，然后通过 `$psCmdlet` 自动变量访问引擎功能。

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding(SupportsShouldProcess = $true)]
    param()

    if($psCmdlet.ShouldProcess("test.txt", "Remove Item"))
    {
        "Removing test.txt"
    }

    Write-Verbose "Verbose Message"
}
```

## 讨论

当脚本或函数进阶为*高级函数*时，PowerShell 会定义一个额外的 `$psCmdlet` 自动变量。此自动变量公开了对 `-WhatIf` 和 `-Confirm` 自动参数的支持。如果你的命令定义了参数集，它还会根据用户选择的参数公开 PowerShell 选定的参数集名称。有关高级函数的更多信息，请参阅食谱 11.11。

要支持 `-WhatIf` 和 `-Confirm` 参数，请在脚本或函数内部添加 `[CmdletBinding(SupportsShouldProcess = $true)]` 特性。你应该在任何修改系统状态的脚本或函数上支持此特性，因为它们允许用户在实际执行前调查脚本将执行的操作。然后，你只需用 `if($psCmdlet.ShouldProcess(...) ) { }` 块包围更改系统的脚本部分即可。示例 11-14 演示了此方法。

### 示例 11-14. 添加对 `-WhatIf` 和 `-Confirm` 的支持

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding(SupportsShouldProcess = $true)]
    param()

    if($psCmdlet.ShouldProcess("test.txt", "Remove Item"))
    {
        "Removing test.txt"
    }

    Write-Verbose "Verbose Message"
}
```

现在你的高级函数表现得和内置的 PowerShell cmdlet 一样规范！

```
PS > Invoke-MyAdvancedFunction -WhatIf
What if: Performing operation "Remove Item" on Target "test.txt".
```

如果你的命令会导致需要谨慎评估的高影响结果，请调用 `$psCmdlet.ShouldContinue()` 方法。这会为用户生成警告，但请务必支持一个 `-Force` 参数，让他们可以绕过此消息。

```
function Invoke-MyDangerousFunction
{
    [CmdletBinding()]
    param(
        [Switch] $Force
    )

    if($Force -or $psCmdlet.ShouldContinue(
        "Do you wish to invoke this dangerous operation?
         Changes can not be undone.",
        "Invoke dangerous action?"))
    {
        "Invoking dangerous action"
    }
}
```

这将生成标准的 PowerShell 确认消息：

```
PS > Invoke-MyDangerousFunction

Invoke dangerous action?
Do you wish to invoke this dangerous operation? Changes can not be undone.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Invoking dangerous action

PS > Invoke-MyDangerousFunction -Force
Invoking dangerous action
```

要进一步探索 `$psCmdlet` 自动变量，可以使用示例 11-15。此命令创建最基本的高级函数，然后在其中调用你提供的任何脚本块。

### 示例 11-15. Invoke-AdvancedFunction.ps1

```
param(
    [Parameter(Mandatory = $true)]
    [ScriptBlock] $Scriptblock
    )

## Invoke the script block supplied by the user.
& $scriptblock
```

为了开放式探索，请使用 `$host.EnterNestedPrompt()` 作为脚本块：

```
PS > Invoke-AdvancedFunction { $host.EnterNestedPrompt() }
PS > $psCmdlet | Get-Member


   TypeName: System.Management.Automation.PSScriptCmdlet

Name                                MemberType Definition
----                                ---------- ----------
(...)
WriteDebug                          Method     System.Void WriteDebug(s...
WriteError                          Method     System.Void WriteError(S...
WriteObject                         Method     System.Void WriteObject(...
WriteProgress                       Method     System.Void WriteProgres...
WriteVerbose                        Method     System.Void WriteVerbose...
WriteWarning                        Method     System.Void WriteWarning...
(...)
ParameterSetName                    Property   System.String ParameterS...

(Now at a nested prompt)
PS >> exit

(Now back to the regular prompt)
PS >
```

有关 PowerShell 引擎中 cmdlet 支持的更多信息，请参阅 [PowerShell 程序员指南](https://oreil.ly/jJrgp) 中的开发人员参考。

## 另请参阅

食谱 11.11，「访问脚本、函数或脚本块的参数」

# 11.16 为脚本或函数添加帮助

## 问题

你想要让你的命令和使用信息可通过 `Get-Help` 命令获取。

## 解决方案

在脚本开头添加描述性帮助注释（包含特定于帮助的特殊标记，如 `.SYNOPSIS`、`.EXAMPLE` 和 `.OUTPUTS`），用于提供摘要、说明、示例、注释等。在参数前添加描述性帮助注释以描述其含义和行为：

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

## 讨论

正如食谱 11.12 中讨论的参数验证一样，丰富的帮助传统上仅在最高端的命令中受支持。对于大多数命令，如果你能弄清楚如何获取某种形式的使用消息，那已经很幸运了。

与 PowerShell 易于定义的高级参数验证支持一样，为命令和函数添加帮助极其简单。尽管简单，但基于注释的帮助提供了你期望的完全成熟的 PowerShell 命令的所有功能：概述、说明、示例、特定于参数的详细信息等。

PowerShell 通过查看脚本或函数的注释来创建帮助。如果注释包含任何受支持的帮助标记（如 `.SYNOPSIS`、`.EXAMPLE`、`.OUTPUTS`），PowerShell 会将它们添加到命令的帮助中。

### 注意

为了加快这些帮助注释的处理速度，PowerShell 对其出现的位置施加了限制。此外，如果它遇到的注释*不是*基于帮助的注释，它将停止搜索该注释块中的帮助标记。如果你习惯在脚本开头放置标题或版权信息，这可能会让你感到惊讶。解决方案演示了如何通过将标题和基于注释的帮助放在单独的注释块中来避免此问题。有关这些准则的更多信息，请输入 `Get-Help about_Comment_Based_Help`。



您可以将帮助标记放在单行注释或多行（块）注释中。您可能会发现多行注释更易于处理，因为您可以在支持拼写和语法检查的编辑器中编写它们，然后只需将其粘贴到脚本中。此外，当您不必修复行首的注释标记时，调整注释的自动换行也会更容易。从用户的角度来看，多行注释为 *`.EXAMPLES`* 部分提供了显著优势，因为在尝试之前它们需要的修改少得多。

有关最常用帮助标记的列表，请参阅 “帮助注释”。

## 另请参阅

- 食谱 11.12，“为参数添加验证”
- “帮助注释”

## 11.17 为函数或脚本块添加自定义标记

### 问题

您想要标记函数或脚本块，或向其中添加您自己的自定义信息。

### 解决方案

如果您希望自定义信息始终与函数或脚本块关联，请在该函数内部声明一个 `System.ComponentModel.Description` 属性：

```
function TestFunction
{
    [System.ComponentModel.Description("Information I care about")]
    param()

    "Some function with metadata"
}
```

如果您不控制函数的源代码，请创建一个新的 `System.ComponentModel.Description` 属性，并手动将其添加到脚本块的 `Attributes` 集合中：

```
$testFunction = Get-Command TestFunction
$newAttribute =
  New-Object ComponentModel.DescriptionAttribute "More information I care about"
$testFunction.ScriptBlock.Attributes.Add($newAttribute)
```

要检索与函数或脚本块关联的任何属性，请访问 `ScriptBlock.Attributes` 属性：

```
PS > $testFunction = Get-Command TestFunction
PS > $testFunction.ScriptBlock.Attributes

Description                           TypeId
-----------                           ------
Information I care about              System.ComponentModel.Description...
```

### 讨论

虽然这肯定是一个特殊的需求，但有时向函数或脚本块添加您自己的自定义信息会很有帮助。例如，一旦您构建了一大组函数，其中许多仅在特定上下文中才真正有用。某些函数可能仅适用于您的某个客户，而其他函数则是为您正在开发的自定义网站编写的。如果您忘记了函数的名称，可能很难浏览所有函数以找到适用于您当前上下文的那些。

您可能会发现编写一个新函数 `Get-CommandForContext` 很有帮助，该函数接受一个上下文（例如，*website*）并仅返回适用于该上下文的命令。

```
function Get-CommandForContext($context)
{
    Get-Command -CommandType Function |
        Where-Object { $_.ScriptBlock.Attributes |
            Where-Object { $_.Description -eq "Context=$context" } }
}
```

然后编写一些适用于特定上下文的函数：

```
function WebsiteFunction
{
    [System.ComponentModel.Description("Context=Website")]
    param()

    "Some function I use with my website"
}

function ExchangeFunction
{
    [System.ComponentModel.Description("Context=Exchange")]
    param()

    "Some function I use with Exchange"
}
```

然后，基于这两个函数，我们得到了一个相当于 `Get-Command` 的上下文敏感版本：

```
PS > Get-CommandForContext Website

CommandType     Name                          Definition
-----------     ----                          ----------
Function        WebsiteFunction               ...

PS > Get-CommandForContext Exchange

CommandType     Name                          Definition
-----------     ----                          ----------
Function        ExchangeFunction              ...
```

虽然 `System.ComponentModel.Description` 属性是最通用的，但 PowerShell 允许您在函数中放置任何属性。您可以定义自己的属性（通过在 .NET Framework 中派生自 `System.Attribute` 类）或使用 .NET Framework 中包含的任何其他属性。示例 11-16 展示了用于查找所有具有接受单个字符串作为参数的构造函数的属性的 PowerShell 命令。这些属性很可能是通用的。

#### 示例 11-16. 查找所有有用的属性

```
$types = [Appdomain]::CurrentDomain.GetAssemblies() |
    ForEach-Object { $_.GetTypes() }

foreach($type in $types)
{
    if($type.BaseType -eq [System.Attribute])
    {
        foreach($constructor in $type.GetConstructors())
        {
            if($constructor.ToString() -match "\(System.String\)")
            {
                $type
            }
        }
    }
}
```

有关使用 .NET 对象的更多信息，请参阅 食谱 3.8。

### 另请参阅

- 食谱 3.8，“使用 .NET 对象”

## 11.18 访问脚本的管道输入

### 问题

您想要与用户通过管道发送到您的函数、脚本或脚本块的输入进行交互。

### 解决方案

要从脚本内部访问来自用户的管道输入，请使用 `$input` 变量，如 示例 11-17 所示。

#### 示例 11-17. 访问管道输入

```
function InputCounter
{
    $count = 0
    ## 遍历管道中的每个元素，并累加
    ## 元素的数量。
    foreach($element in $input)
    {
        $count++
    }

    $count
}
```

当针对您的 Windows 系统目录运行时，此函数会产生以下（或类似）输出：

```
PS > dir $env:WINDIR | InputCounter
295
```

### 讨论

在两个命令之间传递管道输入时，您可能见过 `Foreach-Object` cmdlet 使用 `$_` 变量。

当您编写自己的脚本时，还有另一种选择：`$input` 变量。

在您的脚本、函数和脚本块中，`$input` 变量代表用户提供的管道输入的*枚举器*（而不是简单的数组）。枚举器允许您使用 `foreach` 语句高效地扫描输入元素（如 示例 11-17 所示），但不允许您直接访问特定项（例如输入中的第五个元素）。

##### 注意

枚举器只允许您向前扫描其内容。一旦您访问了一个元素，PowerShell 会自动移动到下一个。如果您需要访问已访问过的项，必须调用 `$input.Reset()` 以从头开始再次扫描列表，或者将输入存储在数组中。

如果您需要访问输入中的特定元素（或多次访问项），最好的方法是将输入存储在数组中。这会阻止您的脚本利用 `$input` 枚举器的流式行为，但有时这是唯一的选择。要将输入存储在数组中，请使用 PowerShell 的列表求值语法（ `@()` ）强制 PowerShell 将其解释为数组：

```
function ReverseInput
{
    $inputArray = @($input)
    $inputEnd = $inputArray.Count - 1

    $inputArray[$inputEnd..0]
}
```

这将产生：

```
PS > 1,2,3,4 | ReverseInput
4
3
2
1
```

如果处理管道输入在您的脚本、函数或脚本块中起主要作用，PowerShell 提供了一种处理管道输入的替代方法，这可能会使您的脚本更易于编写和理解。有关更多信息，请参阅 食谱 11.19。

### 另请参阅

- 食谱 11.19，“使用 Cmdlet 关键字编写面向管道的脚本”

## 11.19 使用 Cmdlet 关键字编写面向管道的脚本

### 问题




您的脚本、函数或脚本块主要从管道获取输入，您希望以一种既易于实现又易于阅读的方式来编写它。

## 解决方案

要将脚本清晰地分离为处理初始化、逐记录处理和清理部分的区域，请分别使用 `begin`、`process` 和 `end` 关键字。例如，示例 11.18 中解决方案的面向管道转换如示例 11-18 所示。

### 示例 11-18. 使用 Cmdlet 关键字的面向管道脚本

```powershell
function InputCounter
{
    begin
    {
        $count = 0
    }

    ## 遍历管道中的每个元素，并累加元素总数。
    process
    {
        Write-Debug "Processing element $_"
        $count++
    }

    end
    {
        $count
    }
}
```

这将产生以下输出：

```
PS > $debugPreference = "Continue"
PS > dir | InputCounter
DEBUG: Processing element Compare-Property.ps1
DEBUG: Processing element Convert-TextObject.ps1
DEBUG: Processing element ConvertFrom-FahrenheitWithFunction.ps1
DEBUG: Processing element ConvertFrom-FahrenheitWithoutFunction.ps1
DEBUG: Processing element Get-AliasSuggestion.ps1
(...)
DEBUG: Processing element Select-FilteredObject.ps1
DEBUG: Processing element Set-ConsoleProperties.ps1
20
```

## 讨论

如果您的脚本、函数或脚本块主要处理来自管道的输入，`begin`、`process` 和 `end` 关键字能让您最清晰地表达解决方案。您的脚本的读者（包括您自己！）可以轻松看出脚本的哪些部分处理初始化、逐记录处理和清理。此外，将代码分离到这些块中，可以让您的脚本在上一个脚本生成元素后立即从管道中使用它们。

例如，示例 11-19 中所示的 `Get-InputWithForeach` 和 `Get-InputWithKeyword` 函数。第一个函数使用 `foreach` 语句遍历其输入中的每个元素，而第二个函数使用 `begin`、`process` 和 `end` 关键字。

### 示例 11-19. 采用不同方法处理管道输入的两个函数

```powershell
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)

Set-StrictMode -Version 3

## 使用 foreach 语句遍历 $input 中的每个元素来处理管道中的每个元素
function Get-InputWithForeach($identifier)
{
    Write-Host "Beginning InputWithForeach (ID: $identifier)"

    foreach($element in $input)
    {
        Write-Host "Processing element $element (ID: $identifier)"
        $element
    }

    Write-Host "Ending InputWithForeach (ID: $identifier)"
}

## 使用 cmdlet 风格的关键字遍历 $input 中的每个元素来处理管道中的每个元素
function Get-InputWithKeyword($identifier)
{
    begin
    {
        Write-Host "Beginning InputWithKeyword (ID: $identifier)"
    }

    process
    {
        Write-Host "Processing element $_ (ID: $identifier)"
        $_
    }

    end
    {
        Write-Host "Ending InputWithKeyword (ID: $identifier)"
    }
}
```

单独运行时，这两个函数的行为相同，但当我们将它们与其他接受管道输入的脚本或函数结合使用时，差异就变得明显了。当脚本使用 `$input` 变量时，它必须等待前一个脚本完成输出生成后才能开始。如果前一个脚本生成所有记录需要很长时间（例如，大型目录列表），那么您的用户必须等到整个目录列表完成才能看到任何结果，而不是在脚本生成每个项目时就看到结果。

#### 注意

如果脚本、函数或脚本块使用 cmdlet 风格的关键字，则必须将其所有代码（除了注释或其 `param` 语句（如果使用的话））放在三个块中的一个内。如果您的代码需要定义和初始化变量或定义函数，请将它们放在 `begin` 块中。与包含在大括号内的大多数代码块不同，`begin`、`process` 和 `end` 块中的代码可以访问在其之前的块中定义的变量和函数。

当我们将两个使用 `begin`、`process` 和 `end` 关键字处理输入的脚本链接在一起时，第二个脚本可以在第一个脚本生成输入后立即处理它。

```
PS > 1,2,3 | Get-InputWithKeyword 1 | Get-InputWithKeyword 2
Starting InputWithKeyword (ID: 1)
Starting InputWithKeyword (ID: 2)
Processing element 1 (ID: 1)
Processing element 1 (ID: 2)
1
Processing element 2 (ID: 1)
Processing element 2 (ID: 2)
2
Processing element 3 (ID: 1)
Processing element 3 (ID: 2)
3
Stopping InputWithKeyword (ID: 1)
Stopping InputWithKeyword (ID: 2)
```

当我们将两个使用 `$input` 变量处理输入的脚本链接在一起时，第二个脚本要等到第一个脚本完成后才能开始。

```
PS > 1,2,3 | Get-InputWithForeach 1 | Get-InputWithForeach 2
Starting InputWithForeach (ID: 1)
Processing element 1 (ID: 1)
Processing element 2 (ID: 1)
Processing element 3 (ID: 1)
Stopping InputWithForeach (ID: 1)
Starting InputWithForeach (ID: 2)
Processing element 1 (ID: 2)
1
Processing element 2 (ID: 2)
2
Processing element 3 (ID: 2)
3
Stopping InputWithForeach (ID: 2)
```

当第一个脚本使用 cmdlet 风格的关键字，而第二个脚本使用 `$input` 变量时，第二个脚本要等到第一个脚本完成后才能开始。

```
PS > 1,2,3 | Get-InputWithKeyword 1 | Get-InputWithForeach 2
Starting InputWithKeyword (ID: 1)
Processing element 1 (ID: 1)
Processing element 2 (ID: 1)
Processing element 3 (ID: 1)
Stopping InputWithKeyword (ID: 1)
Starting InputWithForeach (ID: 2)
Processing element 1 (ID: 2)
1
Processing element 2 (ID: 2)
2
Processing element 3 (ID: 2)
3
Stopping InputWithForeach (ID: 2)
```

当第一个脚本使用 `$input` 变量，而第二个脚本使用 cmdlet 风格的关键字时，第二个脚本可以在第一个脚本生成输入后立即处理它。但请注意，`InputWithKeyword` 在 `InputWithForeach` 之前启动。这是因为没有显式 `begin`、`process` 或 `end` 块的函数，默认会将其所有代码放在 `end` 块中。

```
PS > 1,2,3 | Get-InputWithForeach 1 | Get-InputWithKeyword 2
Starting InputWithKeyword (ID: 2)
Starting InputWithForeach (ID: 1)
Processing element 1 (ID: 1)
Processing element 1 (ID: 2)
1
Processing element 2 (ID: 1)
Processing element 2 (ID: 2)
2
Processing element 3 (ID: 1)
Processing element 3 (ID: 2)
3
Stopping InputWithForeach (ID: 1)
Stopping InputWithKeyword (ID: 2)
```

有关处理管道输入的更多信息，请参阅“编写脚本，重用功能”。

## 另请参阅

示例 11.18，“访问脚本的管道输入”

“编写脚本，重用功能”

# 11.20 编写面向管道的函数

## 问题

您的函数主要从管道获取输入，并且您希望它对该输入的每个元素执行相同的步骤。

## 解决方案

要编写面向管道的函数，请使用 `filter` 关键字而不是 `function` 关键字来定义您的函数。PowerShell 将当前管道对象作为 `$_`（或 `$PSItem`）变量提供：

```powershell
filter Get-PropertyValue($property)
{
    $_.$property
}
```

## 讨论

筛选器等同于一个使用 cmdlet 风格关键字且将其所有代码都放在 `process` 部分的函数。

该解决方案演示了一个极其有用的筛选器：它返回管道中每个项目的属性值：




```
PS > Get-Process | Get-PropertyValue Name
audiodg
avgamsvr
avgemc
avgrssvc
avgrssvc
avgupsvc
(...)
```

有关 cmdlet 风格关键字的更多信息，请参阅 Recipe 11.19。

## 另请参阅

Recipe 11.19，“使用 Cmdlet 关键字编写面向管道的脚本”

# 11.21 组织脚本以提高可读性

## 问题

你有一个包含辅助函数的长脚本，但这些辅助函数掩盖了脚本的主要意图。

## 解决方案

将脚本的主要逻辑放在一个名为 `Main` 的函数中，并将该函数放在脚本的顶部。在脚本的底部（在所有辅助函数也定义之后），点源 `Main` 函数：

```
## LongScript.ps1

function Main
{
    "Invoking the main logic of the script"
    CallHelperFunction1
    CallHelperFunction2
}

function CallHelperFunction1
{
    "Calling the first helper function"
}

function CallHelperFunction2
{
    "Calling the second helper function"
}

. Main
```

## 讨论

当 PowerShell 调用脚本时，它会从头到尾按顺序执行。就像在控制台中输入命令一样，如果你尝试调用一个尚未定义的函数，PowerShell 会生成错误。

编写包含大量辅助函数的长脚本时，这通常会导致这些辅助函数移至脚本顶部，以便在主逻辑最终执行它们时它们都已定义。因此，在阅读脚本时，你不得不翻阅大量看似无关的辅助函数，才能到达脚本的主要逻辑。

###### 注意

你可能会想知道为什么 PowerShell 要求函数定义及其调用有这种严格的顺序。毕竟，脚本是自包含的，PowerShell 完全可以在调用脚本之前处理所有函数定义。

原因是为了与交互式环境保持一致。将脚本粘贴到控制台窗口是一种常见的诊断或实验技术，在 Visual Studio Code 中高亮脚本部分并选择“运行选择内容”也是如此。如果 PowerShell 在假想的*脚本模式*中做一些特殊处理，这些技术就无法实现。

要解决此问题，可以将主脚本逻辑放在其自己的函数中。名称无关紧要，但 `Main` 是一个传统名称。如果将此函数放在脚本顶部，主逻辑将立即可见。

函数不会自动执行，因此最后一步是调用 `Main` 函数。将此调用放在脚本末尾，可以确保所有必需的辅助函数都已定义。点源此函数可确保它在*脚本作用域*中处理，而不是在通常为其创建的隔离函数作用域中。

有关点源和脚本作用域的更多信息，请参阅 Recipe 3.6。

## 另请参阅

Recipe 3.6，“控制变量和其他项的访问与作用域”

# 11.22 调用动态命名的命令

## 问题

你想根据命令名称的*模式*采取行动，而不是根据命令名称本身。

## 解决方案

添加一个 `$executionContext.SessionState.InvokeCommand.CommandNotFoundAction`，它会拦截 PowerShell 的 `CommandNotFound` 错误，并根据未找到的 `CommandName` 采取行动。

示例 11-20 通过支持相对路径导航（无需显式调用 `Set-Location`）演示了此技术。

##### 示例 11-20. Add-RelativePathCapture.ps1

```
##############################################################################
##
## Add-RelativePathCapture
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

添加一个新的 CommandNotFound 处理程序，无需显式调用 'Set-Location' 即可捕获相对路径导航

.EXAMPLE

PS C:\Users\Lee\Documents>..
PS C:\Users\Lee>...
PS C:\>

#>

Set-StrictMode -Version 3

$executionContext.SessionState.InvokeCommand.CommandNotFoundAction = {
    param($CommandName, $CommandLookupEventArgs)

    ## 如果命令仅由点组成
    if($CommandName -match '^\.+$')
    {
        ## 关联一个应改为调用的新命令
        $CommandLookupEventArgs.CommandScriptBlock = {

            ## 计算点的数量，并运行 "Set-Location .." 比点数少一次。
            for($counter = 0; $counter -lt $CommandName.Length - 1; $counter++)
            {
                Set-Location ..
            }

        ## 我们调用 GetNewClosure()，以便在新命令中可以使用对 $CommandName 的引用。
        }.GetNewClosure()

        ## 停止命令解析过程。这在 CommandNotFoundAction 中并非严格必需。
        $CommandLookupEventArgs.StopSearch = $true
    }
}
```

## 讨论

PowerShell 支持多种有用的命名命令形式（cmdlet、函数和别名），但你可能会想编写扩展，根据名称的*形式*而不是传递给它的参数来改变其行为。例如，你可能只想通过输入 URL 就自动启动它们，或通过输入相对路径位置来浏览提供程序。

虽然相对路径导航不是 PowerShell 的内置功能，但通过自定义 PowerShell 的 `CommandNotFoundAction` 可以获得一个非常合理的替代方案。有关自定义 PowerShell 命令解析行为的更多信息，请参阅 Recipe 1.11。

## 另请参阅

Recipe 1.11，“自定义 PowerShell 的命令解析行为”

# 11.23 程序：增强或扩展现有 Cmdlet

虽然 PowerShell 的内置命令很有用，但有时你可能希望它们包含额外的参数或支持其功能的微小更改。这通常是一个困难的提议：除了解析参数并仅传递正确参数的复杂性外，包装命令还应能从 PowerShell 管道的流式特性中受益。

PowerShell 通过结合三个功能显著改善了这一情况：

`*可步进管道*`

: 

给定一个包含单个管道的脚本块，`GetSteppablePipeline()` 方法返回一个 `SteppablePipeline` 对象，使你能够控制管道的 `Begin`、`Process` 和 `End` 阶段。

`*参数展开*`

: 

给定一个包含名称和值的哈希表，PowerShell 允许你将整个哈希表传递给命令。如果你使用 `@` 符号标识哈希表变量名（而不是 `$` 符号），PowerShell 会将哈希表的每个元素视为命令的参数。

`*代理命令 API*`

: 

凭借对可步进管道、展开和参数验证的足够了解，你可以编写自己的函数来有效包装另一个命令。代理命令 API 通过自动生成大量所需的样板脚本，使这变得容易得多。

这三个功能最终实现了强大的命令扩展，但将它们组合在一起仍需要相当多的技术专长。为了简化操作，请使用 `New-CommandWrapper` 脚本（示例 11-21）轻松创建包装（并扩展）现有命令的命令。

##### 示例 11-21. New-CommandWrapper.ps1



# New-CommandWrapper

出自 Lee Holmes 编著的《PowerShell Cookbook》（O'Reilly 出版）（http://www.leeholmes.com/guide）

## 概要

为现有的 `cmdlets` 和 `functions` 添加参数和功能。

## 示例 1

```powershell
New-CommandWrapper Get-Process `
      -AddParameter @{
          SortBy = {
              $newPipeline = {
                  __ORIGINAL_COMMAND__ | Sort-Object -Property $SortBy
              }
          }
      }
```

此示例向 `Get-Process` 添加了一个 'SortBy' 参数。它通过向管道添加 `Sort-Object` 命令来实现这一点。

## 示例 2

```powershell
$parameterAttributes = @'
          [Parameter(Mandatory = $true)]
          [ValidateRange(50,75)]
          [Int]
'@

New-CommandWrapper Clear-Host `
      -AddParameter @{
          @{
              Name = 'MyMandatoryInt';
              Attributes = $parameterAttributes
          } = {
              Write-Host $MyMandatoryInt
              Read-Host "Press ENTER"
         }
      }
```

此示例向 `Clear-Host` 添加了一个新的强制性 'MyMandatoryInt' 参数。该参数还会被验证以确保其值在 50 到 75 之间。它不会改变管道，但在处理原始管道之前会在屏幕上显示一些信息。

## 参数

```powershell
param(
    ## The name of the command to extend
    [Parameter(Mandatory = $true)]
    $Name,

    ## Script to invoke before the command begins
    [ScriptBlock] $Begin,

    ## Script to invoke for each input element
    [ScriptBlock] $Process,

    ## Script to invoke at the end of the command
    [ScriptBlock] $End,

    ## Parameters to add, and their functionality.
    ##
    ## The Key of the hashtable can be either a simple parameter name,
    ## or a more advanced parameter description.
    ##
    ## If you want to add additional parameter validation (such as a
    ## parameter type,) then the key can itself be a hashtable with the keys
    ## 'Name' and 'Attributes'. 'Attributes' is the text you would use when
    ## defining this parameter as part of a function.
    ##
    ## The value of each hashtable entry is a script block to invoke
    ## when this parameter is selected. To customize the pipeline,
    ## assign a new script block to the $newPipeline variable. Use the
    ## special text, __ORIGINAL_COMMAND__, to represent the original
    ## command. The $targetParameters variable represents a hashtable
    ## containing the parameters that will be passed to the original
    ## command.
    [HashTable] $AddParameter
)
```

## 脚本主体

```powershell
Set-StrictMode -Version 3

## Store the target command we are wrapping, and its command type
$target = $Name
$commandType = "Cmdlet"

## If a function already exists with this name (perhaps it's already been
## wrapped,) rename the other function and chain to its new name.
if (Test-Path function:\$Name)
{
    $target = "$Name" + "-" + [Guid]::NewGuid().ToString().Replace("-","")
    Rename-Item function:\GLOBAL:$Name GLOBAL:$target
    $commandType = "Function"
}

## The template we use for generating a command proxy
$proxy = @'

__CMDLET_BINDING_ATTRIBUTE__
param(
__PARAMETERS__
)
begin
{
    try {
        __CUSTOM_BEGIN__

        ## Access the REAL Foreach-Object command, so that command
        ## wrappers do not interfere with this script
        $foreachObject = $executionContext.InvokeCommand.GetCmdlet(
            "Microsoft.PowerShell.Core\Foreach-Object")

        $wrappedCmd = $ExecutionContext.InvokeCommand.GetCommand(
            '__COMMAND_NAME__',
            [System.Management.Automation.CommandTypes]::__COMMAND_TYPE__)

        ## TargetParameters represents the hashtable of parameters that
        ## we will pass along to the wrapped command
        $targetParameters = @{}
        $PSBoundParameters.GetEnumerator() |
            & $foreachObject {
                if($command.Parameters.ContainsKey($_.Key))
                {
                    $targetParameters.Add($_.Key, $_.Value)
                }
            }

        ## finalPipeline represents the pipeline we wil ultimately run
        $newPipeline = { & $wrappedCmd @targetParameters }
        $finalPipeline = $newPipeline.ToString()

        __CUSTOM_PARAMETER_PROCESSING__

        $steppablePipeline = [ScriptBlock]::Create(
            $finalPipeline).GetSteppablePipeline()
        $steppablePipeline.Begin($PSCmdlet)
    } catch {
        throw
    }
}

process
{
    try {
        __CUSTOM_PROCESS__
        $steppablePipeline.Process($_)
    } catch {
        throw
    }
}

end
{
    try {
        __CUSTOM_END__
        $steppablePipeline.End()
    } catch {
        throw
    }
}

dynamicparam
{
    ## Access the REAL Get-Command, Foreach-Object, and Where-Object
    ## commands, so that command wrappers do not interfere with this script
    $getCommand = $executionContext.InvokeCommand.GetCmdlet(
        "Microsoft.PowerShell.Core\Get-Command")
    $foreachObject = $executionContext.InvokeCommand.GetCmdlet(
        "Microsoft.PowerShell.Core\Foreach-Object")
    $whereObject = $executionContext.InvokeCommand.GetCmdlet(
        "Microsoft.PowerShell.Core\Where-Object")

    ## Find the parameters of the original command, and remove everything
    ## else from the bound parameter list so we hide parameters the wrapped
    ## command does not recognize.
    $command = & $getCommand __COMMAND_NAME__ -Type __COMMAND_TYPE__
    $targetParameters = @{}
    $PSBoundParameters.GetEnumerator() |
        & $foreachObject {
            if($command.Parameters.ContainsKey($_.Key))
            {
                $targetParameters.Add($_.Key, $_.Value)
            }
        }

    ## Get the argument list as it would be passed to the target command
    $argList = @($targetParameters.GetEnumerator() |
        Foreach-Object { "-$($_.Key)"; $_.Value })

    ## Get the dynamic parameters of the wrapped command, based on the
    ## arguments to this command
    $command = $null
    try
    {
        $command = & $getCommand __COMMAND_NAME__ -Type __COMMAND_TYPE__ `
            -ArgumentList $argList
    }
    catch
    {

    }

    $dynamicParams = @($command.Parameters.GetEnumerator() |
        & $whereObject { $_.Value.IsDynamic })

    ## For each of the dynamic parameters, add them to the dynamic
    ## parameters that we return.
    if ($dynamicParams.Length -gt 0)
    {
        $paramDictionary = `
            New-Object Management.Automation.RuntimeDefinedParameterDictionary
        foreach ($param in $dynamicParams)
        {
            $param = $param.Value
            $arguments = $param.Name, $param.ParameterType, $param.Attributes
            $newParameter = `
                New-Object Management.Automation.RuntimeDefinedParameter `
                $arguments
            $paramDictionary.Add($param.Name, $newParameter)
        }
        return $paramDictionary
    }
}

<#

.ForwardHelpTargetName __COMMAND_NAME__
.ForwardHelpCategory __COMMAND_TYPE__

#>

'@

## Get the information about the original command
$originalCommand = Get-Command $target
$metaData = New-Object System.Management.Automation.CommandMetaData `
    $originalCommand
$proxyCommandType = [System.Management.Automation.ProxyCommand]

## Generate the cmdlet binding attribute, and replace information
## about the target
$proxy = $proxy.Replace("__CMDLET_BINDING_ATTRIBUTE__",
    $proxyCommandType::GetCmdletBindingAttribute($metaData))
$proxy = $proxy.Replace("__COMMAND_NAME__", $target)
$proxy = $proxy.Replace("__COMMAND_TYPE__", $commandType)

## Stores new text we'll be putting in the param() block
$newParamBlockCode = ""

## Stores new text we'll be putting in the begin block
## (mostly due to parameter processing)
$beginAdditions = ""

## If the user wants to add a parameter
$currentParameter = $originalCommand.Parameters.Count
if ($AddParameter)
{
    foreach ($parameter in $AddParameter.Keys)
    {
        ## Get the code associated with this parameter
        $parameterCode = $AddParameter[$parameter]

        ## If it's an advanced parameter declaration, the hashtable
        ## holds the validation and / or type restrictions
        if ($parameter -is [Hashtable])
        {
            ## Add their attributes and other information to
            ## the variable holding the parameter block additions
            if ($currentParameter -gt 0)
            {
                $newParamBlockCode += ","
            }

            $newParamBlockCode += "`n`n    " +
                $parameter.Attributes + "`n" +
                '    $' + $parameter.Name

            $parameter = $parameter.Name
        }
        else
        {
            ## If this is a simple parameter name, add it to the list of
            ## parameters. The proxy generation APIs will take care of
            ## adding it to the param() block.
            $newParameter =
                New-Object System.Management.Automation.ParameterMetadata `
                    $parameter
            $metaData.Parameters.Add($parameter, $newParameter)
        }

        $parameterCode = $parameterCode.ToString()

        ## Create the template code that invokes their parameter code if
        ## the parameter is selected.
        $templateCode = @"

        if (`$PSBoundParameters['$parameter'])
        {
            $parameterCode

            ## Replace the __ORIGINAL_COMMAND__ tag with the code
            ## that represents the original command
            `$alteredPipeline = `$newPipeline.ToString()
            `$finalPipeline = `$alteredPipeline.Replace(
                '__ORIGINAL_COMMAND__', `$finalPipeline)
        }
"@

        ## Add the template code to the list of changes we're making
        ## to the begin() section.
        $beginAdditions += $templateCode
        $currentParameter++
    }
}

## Generate the param() block
$parameters = $proxyCommandType::GetParamBlock($metaData)
if ($newParamBlockCode) { $parameters += $newParamBlockCode }
$proxy = $proxy.Replace('__PARAMETERS__', $parameters)

## Update the begin, process, and end sections
$proxy = $proxy.Replace('__CUSTOM_BEGIN__', $Begin)
$proxy = $proxy.Replace('__CUSTOM_PARAMETER_PROCESSING__', $beginAdditions)
$proxy = $proxy.Replace('__CUSTOM_PROCESS__', $Process)
$proxy = $proxy.Replace('__CUSTOM_END__', $End)

## Save the function wrapper
Write-Verbose $proxy
Set-Content function:\GLOBAL:$NAME $proxy

## If we were wrapping a cmdlet, hide it so that it doesn't conflict with
## Get-Help and Get-Command
if ($commandType -eq "Cmdlet")
{
    $originalCommand.Visibility = "Private"
}
```



## 另请参考

- 食谱 1.2，“运行程序、脚本和现有工具”
