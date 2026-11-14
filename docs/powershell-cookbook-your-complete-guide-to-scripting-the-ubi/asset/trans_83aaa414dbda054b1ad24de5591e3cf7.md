

# 为命令提供输入

## 套用 (Splatting)

您可以提供一个定义参数名称和值的哈希表，并使用 `splatting operator`（套用运算符），而无需显式提供参数名称和值：

```
$parameters = @{
   Path = "c:\temp"
   Recurse = $true
}

Get-ChildItem @parameters
```

## 默认参数值 (PSDefaultParameterValues)

要定义命令参数的默认值（如果未直接指定参数值），请向 `PSDefaultParameterValues` 哈希表赋值。此哈希表的键是命令名称和参数名称，以冒号分隔。两者均可使用通配符。此哈希表的值可以是简单的参数值，也可以是将动态求值的脚本块。

```
PS > $PSDefaultParameterValues["Get-Process:ID"] = $pid
PS > Get-Process

PS > $PSDefaultParameterValues["Get-Service:Name"] = {
    Get-Service -Name * | ForEach-Object Name | Get-Random }
PS > Get-Service
```

## 参数数组

要按位置访问命令行参数，请使用 PowerShell 放在 `$args` 特殊变量中的参数数组：

```
$firstArgument = $args[0]
$secondArgument = $args[1]
$argumentCount = $args.Count
```

## 形式参数

要定义一个具有简单参数支持的命令：

```
param(
    [TypeName] $VariableName = Default,
    ...
)
```

要定义一个支持高级功能的命令：

```
[CmdletBinding(cmdlet behavior customizations)]
param(
    [Parameter(Mandatory = $true, Position = 1, ...)]
    [Alias("MyParameterAlias"]
    [...]
    [TypeName] $VariableName = Default,
    ...
)
```

形式参数让您能受益于 PowerShell 一致的命令行解析引擎的诸多优势。

PowerShell 公开您的参数名称（例如 `$VariableName`）的方式，与其公开 cmdlet 中参数的方式相同。用户只需输入足以将您的参数名称与其余参数区分开的字符即可。

如果您定义的命令仅具有简单参数支持，当用户未键入参数名称时，PowerShell 会尝试按位置将输入分配给您的参数。

当您添加 `[CmdletBinding()]` 特性、`[Parameter()]` 特性或任何验证特性时，PowerShell 会添加对高级参数验证的支持。

## 命令行为自定义

`[CmdletBinding()]` 特性的元素描述了您的脚本或函数如何与系统交互。

`SupportsShouldProcess = $true`

: 如果为 `$true`，则启用 `-WhatIf` 和 `-Confirm` 参数，这会告知用户您的命令会修改系统，并可在两种实验模式之一中运行。指定此项时，您还必须在修改系统状态之前调用 `$psCmdlet.ShouldProcess()` 方法。如果未指定，默认值为 `$false`。

`DefaultParameterSetName =` `name`

: 定义此命令的默认参数集名称。当参数声明多组参数且用户输入未提供足够信息以在可用参数集之间进行选择时，此项用于消除歧义。如果未指定，则该命令没有默认参数集名称。

`ConfirmImpact =` `High`

: 将此命令定义为默认应显示其确认消息（由 `$psCmdlet.ShouldProcess()` 方法生成）的命令。更具体地说，PowerShell 定义了三种确认影响级别：`Low`、`Medium` 和 `High`。每当 cmdlet 的影响级别大于首选项变量时，PowerShell 会自动生成 cmdlet 的确认消息。如果未指定，则命令的影响级别为 `Medium`。

## 参数特性自定义

`[Parameter()]` 特性的元素主要定义您的参数如何与其他参数相关联。所有元素均为可选。

`Mandatory = $true`

: 将参数定义为强制性。如果用户未为此参数提供值，PowerShell 会自动提示他们输入。如果未指定，则该参数为可选。

`Position =` `position`

: 定义此参数的位置。当用户提供参数值但未指定其适用的参数时（例如 `Invoke-MyFunction -Param1 Argument1 Argument2` 中的 `Argument2`），此项适用。PowerShell 会从低到高将这些值提供给定义了 `Position` 的参数。如果未指定，则用户必须提供此参数的名称。

`ParameterSetName =` `name`

: 将此参数定义为一组其他相关参数的成员。然后，此参数的行为特定于这组相关参数，并且该参数仅存在于其定义所在的参数集中。例如，当用户可能仅提供 `Name` 或 `ID` 时，会使用此功能。要在两个或更多特定参数集中包含某个参数，请使用两个或更多 `[Parameter()]` 特性。如果未指定，则此参数是所有参数集的成员。

`ValueFromPipeline = $true`

: 声明此参数为直接接受管道输入的参数。如果用户将数据通过管道传入您的脚本或函数，PowerShell 会在命令的 `process {}` 块中将此输入分配给您的参数。如果未指定，则此参数不直接接受管道输入。

`ValueFromPipelineByPropertyName = $true`

: 声明此参数为在传入对象的某个属性与其名称匹配时接受管道输入的参数。如果为 true，PowerShell 会在命令的 `process {}` 块中将该属性的值分配给您的参数。如果未指定，则此参数不按属性名称接受管道输入。

`ValueFromRemainingArguments = $true`

: 声明此参数为接受所有未分配给位置参数或命名参数的剩余输入的参数。只能有一个参数具有此元素。如果没有参数声明支持此功能，PowerShell 会为无法分配的参数生成错误。

## 参数验证特性

除了 `[Parameter()]` 特性外，PowerShell 还允许您应用其他特性，以向参数添加行为或验证约束。所有验证特性均为可选。

`[Alias("name")]`

: 为此参数定义一个别名。这对于描述性强但有更常见通俗术语的长参数名称特别有帮助。如果未指定，则该参数只能通过您最初声明的名称来引用。

`[AllowNull()]`

: 允许此参数接收 `$null` 作为其值。这仅对强制参数是必需的。如果未指定，强制参数不能接收 `$null` 作为其值，尽管可选参数可以。

`[AllowEmptyString()]`

: 允许此字符串参数接收空字符串作为其值。这仅对强制参数是必需的。如果未指定，强制字符串参数不能接收空字符串作为其值，尽管可选字符串参数可以。您可以将其应用于非字符串参数，但不会产生任何影响。

`[AllowEmptyCollection()]`

: 允许此集合参数接收空集合作为其值。这仅对强制参数是必需的。如果未指定，强制集合参数不能接收空集合作为其值，尽管可选集合参数可以。您可以将其应用于非集合参数，但不会产生任何影响。

`[ValidateCount(lower limit, upper limit)]`

: 限制可提供给此参数的集合中的元素数量。如果未指定，强制参数的下限为一个元素。可选参数没有限制。您可以将其应用于非集合参数，但不会产生任何影响。

`[ValidateLength(lower limit, upper limit)]`

:

