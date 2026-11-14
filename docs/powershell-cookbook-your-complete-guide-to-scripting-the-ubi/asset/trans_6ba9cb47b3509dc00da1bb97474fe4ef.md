

# 11.11 在 PowerShell 中使用参数

当你使用 `param` 关键字定义参数时，PowerShell 会为你的脚本或函数提供许多有用的功能，让用户能够像使用 Cmdlet 一样使用你的脚本：

*   用户只需指定足以将其与其他参数区分开来的参数名称即可。
*   用户能更清晰地理解参数的含义。
*   你可以指定参数的类型，PowerShell 会在需要时用它来转换输入。
*   你可以为参数指定默认值。

## 支持 PowerShell 的通用参数

除了你定义的参数外，你可能还希望支持 PowerShell 的标准参数：`-Verbose`、`-Debug`、`-ErrorAction`、`-WarningAction`、`-InformationAction`、`-ErrorVariable`、`-WarningVariable`、`-InformationVariable`、`-OutVariable`、`-OutBuffer` 和 `-PipelineVariable`。

要获取这些额外参数，请在函数内部添加 `[CmdletBinding()]` 特性，或在脚本顶部声明它。即使你的函数或脚本未声明任何参数，`param()` 语句也是必需的。这些（及其他相关）额外功能现在使你的函数成为*高级函数*。请参阅[示例 11-12](#ch11.xhtml#declaring_an_advanced_function)。

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

如果你的 Cmdlet 修改系统状态，支持标准的 `-WhatIf` 和 `-Confirm` 参数将非常有帮助。有关如何实现这一点的信息，请参阅[配方 11.15](#ch11.xhtml#support_whatif_confirm)。

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

有关 `param` 语句的更多信息，请参阅[“编写脚本，重用功能”](#app01.xhtml#writing_scripts_reusing_functionality)。有关运行脚本的更多信息，请参阅[配方 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。有关 PowerShell 引擎公开的功能（如 `-Whatif` 和 `-Confirm`）的更多信息，请参阅[配方 11.15](#ch11.xhtml#support_whatif_confirm)。

有关如何声明具有丰富验证和行为的参数的信息，请参阅[配方 11.12](#ch11.xhtml#add_validation_to_parameters)。

## 另请参阅

*   [配方 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
*   [配方 11.12，“向参数添加验证”](#ch11.xhtml#add_validation_to_parameters)
*   [配方 11.15，“提供 -WhatIf、-Confirm 和其他 Cmdlet 功能”](#ch11.xhtml#support_whatif_confirm)
*   [配方 16.3，“访问有关命令调用的信息”](#ch16.xhtml#access_information_about_your_commands_i)
*   [“编写脚本，重用功能”](#app01.xhtml#writing_scripts_reusing_functionality)

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

