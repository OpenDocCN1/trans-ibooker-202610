

## 1.24 程序：在格式化输出中搜索模式

如果您输入一个数字，Tab 补全将在您的命令历史记录中查找具有该 ID 的条目，然后用该历史记录条目的文本替换您的命令行。当您想要稍微修改之前的历史记录条目时，这特别有用，因为 `Invoke-History` 本身不支持此操作。

### 模式补全

如果您输入其他任何内容，Tab 补全将搜索包含该文本的命令历史记录条目。在底层，PowerShell 使用 `-like` 运算符来匹配您的命令条目，因此您可以使用该运算符支持的所有通配符。有关搜索文本模式的更多信息，请参阅 [食谱 5.7](#ch05.xhtml#search_a_string_for_text_or_a_pattern)。

PowerShell 的 Tab 补全主要由完全可自定义的 `TabExpansion2` 函数驱动。您可以轻松更改此函数以包含更高级的功能，甚至只需自定义特定行为以适应您的个人偏好。有关更多信息，请参阅 [食谱 1.18](#ch01.xhtml#customize_tab_expansion)。

### 另请参阅

- [食谱 1.9，“自定义 Shell、配置文件和提示符”](#ch01.xhtml#customize_your_shell_profile_and_prompt)
- [食谱 1.18，“理解并自定义 PowerShell 的 Tab 补全”](#ch01.xhtml#customize_tab_expansion)
- [食谱 1.31，“在会话间保存状态”](#ch01.xhtml#save-state-exiting)
- [食谱 5.7，“在字符串中搜索文本或模式”](#ch05.xhtml#search_a_string_for_text_or_a_pattern)

虽然 PowerShell 内置的过滤功能非常灵活（例如 `Where-Object` cmdlet），但它们通常针对传入对象的特定属性进行操作。如果您正在对象的格式化输出中搜索文本，或者不知道哪个属性包含您要查找的文本，简单的基于文本的过滤有时会很有帮助。

要解决此问题，可以在将输出传递给 `Select-String` cmdlet 之前，先通过管道传递给 `Out-String` cmdlet：

```
Get-Service | Out-String -Stream | Select-String audio
```

或者，使用内置别名：

```
Get-Service | oss | sls audio
```

在脚本形式中，`Select-TextOutput`（如 [示例 1-17](#ch01.xhtml#select-textoutput.ps1) 所示）正是执行此操作，它允许您在命令输出的可视化表示中搜索模式。

#### 示例 1-17. Select-TextOutput.ps1

```
##############################################################################
##
## Select-TextOutput
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

搜索命令的文本输出以查找模式。

.EXAMPLE

PS > Get-Service | Select-TextOutput audio
在 Get-Service 的输出中查找所有对 "Audio" 的引用

#>

param(
    ## 要搜索的模式
    $Pattern
)

Set-StrictMode -Version 3
$input | Out-String -Stream | Select-String $pattern
```

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

### 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

## 1.25 以交互方式查看和处理命令输出

### 问题

您想要以图形方式探索和分析命令的输出。

### 解决方案

使用 `Out-GridView` cmdlet 以交互方式探索命令的输出。

### 讨论

`Out-GridView` cmdlet 是少数显示图形用户界面的 PowerShell cmdlet 之一。虽然 `Where-Object` 和 `Sort-Object` cmdlet 是排序和筛选项目列表的最常用方法，但 `Out-GridView` cmdlet 在重复细化的风格方面非常有效，这种风格有时有助于您开发复杂查询。[图 1-4](#ch01.xhtml#out_gridview_1) 展示了 `Out-GridView` cmdlet 的实际运行情况。

![](Images/wps4_0104.png)

`Out-GridView` 主要允许您通过两种方式筛选命令输出：*快速筛选*表达式和*条件筛选*。

*快速筛选*相当简单。当您在最顶部的“筛选”窗口中输入文本时，`Out-GridView` 会筛选列表，使其仅包含与该文本匹配的项目。如果要将此文本筛选限制在特定列中，只需在搜索字符串前提供列名，并用冒号分隔两者。您可以提供多个搜索字符串，在这种情况下，`Out-GridView` 仅返回与所有必需字符串匹配的行。

##### 注意

与 PowerShell 中的大多数筛选 cmdlet 不同，`Out-GridView` cmdlet 中的快速筛选不支持通配符或正则表达式。对于这种类型的高级查询，基于条件的筛选会有所帮助。

*条件筛选*可对 `Out-GridView` cmdlet 使用的筛选进行细粒度控制。要应用条件筛选，请单击“添加条件”按钮并选择要筛选的属性。`Out-GridView` 会在快速筛选字段下方添加一行，并允许您选择几种操作之一应用于此属性：

- 小于或等于
- 大于或等于
- 介于
- 等于
- 不等于
- 包含
- 不包含

除了这些筛选选项外，`Out-GridView` 还允许您单击并重新排列标题列以按其排序。

### 处理输出

一旦您对命令输出进行了切片和切块，您可以选择要保留的任意行，然后按 `Ctrl+C` 将它们复制到剪贴板。`Out-GridView` 将项目作为制表符分隔的数据复制到剪贴板，因此您可以轻松将信息粘贴到电子表格或其他文件中以进行进一步处理。

除了支持剪贴板输出外，如果使用其 `-PassThru` 参数，`Out-GridView` cmdlet 还支持全保真对象筛选。有关此全保真筛选的示例，请参阅 [食谱 2.4](#ch02.xhtml#program_interactively_filter_lists_of_ob)。

### 另请参阅

- [食谱 2.4，“以交互方式筛选对象列表”](#ch02.xhtml#program_interactively_filter_lists_of_ob)

## 1.26 程序：以交互方式查看和探索对象

在 PowerShell 中处理不熟悉的对象时，您大部分时间都花在 `Get-Member` 和 `Format-List` 命令上——浏览属性、查看成员等。

对于临时调查，图形界面通常很有用。

要解决此问题，[示例 1-18](#ch01.xhtml#show-object-ps1) 提供了一个交互式树视图，您可以使用它来探索和导航对象。例如，要检查 PowerShell 所见脚本的结构（其*抽象语法树*）：

```
$ps = { Get-Process -ID $pid }.Ast
Show-Object $ps
```

有关解析和分析 PowerShell 脚本结构的更多信息，请参阅 [食谱 10.10](#ch10.xhtml#parse_and_interpret_powershell_scripts)。

#### 示例 1-18. Show-Object.ps1

