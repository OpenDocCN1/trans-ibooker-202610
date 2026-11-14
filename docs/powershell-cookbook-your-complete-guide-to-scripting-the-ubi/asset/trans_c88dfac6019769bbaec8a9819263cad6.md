

# 从非 PowerShell 应用程序调用 PowerShell 命令或脚本

## 问题

你想要从批处理文件、登录脚本、计划任务或任何其他非 PowerShell 应用程序中调用 PowerShell 命令或脚本。

## 解决方案

要调用 PowerShell 命令，请使用 `-Command` 参数：

```
pwsh -Command Get-Process; Read-Host
```

要启动 PowerShell 脚本，请使用 `-File` 参数：

```
pwsh -File '脚本的完整路径' 参数
```

例如：

```
pwsh -File 'c:\shared scripts\Get-Report.ps1' Hello World
```

## 讨论

默认情况下，传递给 `pwsh.exe` 的任何参数都会被解释为要运行的脚本。如果你使用 `-Command` 参数，PowerShell 会像你在交互式 Shell 中输入命令一样运行它，然后退出。你可以通过向 `pwsh.exe` 提供其他参数（如 `-NoExit`、`-NoProfile` 等）来自定义此行为。

### 注意

如果你是需要运行 PowerShell 脚本或命令的程序的作者，PowerShell 允许你比调用其命令行界面更轻松地调用这些脚本和命令。有关此方法的更多信息，请参阅 [食谱 17.10](#ch17.xhtml#add_powershell_scripting_to_your_own_pro)。

由于启动脚本非常常见，PowerShell 提供了 `-File` 参数，以消除必须从 `-Command` 参数调用脚本时产生的复杂性。此技术允许你将 PowerShell 脚本作为登录脚本、高级文件关联、计划任务等的目标来调用。

### 注意

当 PowerShell 检测到其输入或输出流已被重定向时，它会抑制可能正常显示的任何提示。如果你想在另一个应用程序（如 Emacs）中托管一个交互式 PowerShell 提示符，请将 `-` 作为 `-File` 参数的参数。在 PowerShell 中（就像传统的 Unix Shell 一样），这意味着“从标准输入获取”。

```
pwsh -File -
```

如果脚本用于后台自动化或计划任务，这些脚本有时会干扰（或受到）用户环境的影响。对于这些情况，有三个参数非常有用：

`-NoProfile`
: 在不加载用户配置文件脚本的情况下运行命令或脚本。这会使脚本启动得更快，但其主要作用是防止用户首选项（例如别名和首选项变量）干扰脚本的工作环境。

`-WindowStyle`
: 使用指定的窗口样式运行命令或脚本——最常用的是 `Hidden`。当使用 `Hidden` 窗口样式运行时，PowerShell 会立即隐藏其主窗口。有关从 PowerShell *内部* 控制窗口样式的更多方法，请参阅 [食谱 24.3](#ch24.xhtml#launch_a_process)。

`-ExecutionPolicy`
: 使用指定的执行策略运行命令或脚本，该策略仅应用于此 PowerShell 实例。这让你可以编写 PowerShell 脚本来管理系统，而无需更改系统范围的执行策略。有关作用域执行策略的更多信息，请参阅 [食谱 18.1](#ch18.xhtml#enable_scripting_through_an_execution_po)。

如果传递给 `-Command` 参数的参数变得复杂，调用 PowerShell 的应用程序（如 `cmd.exe`）中的特殊字符处理可能会干扰你想要发送给 PowerShell 的命令。对于这种情况，PowerShell 支持 `EncodedCommand` 参数：即你想要运行的 Unicode 字符串的 Base64 编码表示。[示例 1-10](#ch01.xhtml#converting_powershell_commands_into_a_ba) 演示了如何将包含 PowerShell 命令的字符串转换为 Base64 编码形式。

#### 示例 1-10. 将 PowerShell 命令转换为 Base64 编码形式

```
$commands = '1..10 | % { "PowerShell Rocks" }'
$bytes = [System.Text.Encoding]::Unicode.GetBytes($commands)
$encodedString = [Convert]::ToBase64String($bytes)
```

获得编码字符串后，你可以将其用作 `EncodedCommand` 参数的值，如 [示例 1-11](#ch01.xhtml#launching_powershell_with_an_encoded_com) 所示。

#### 示例 1-11. 从 cmd.exe 使用编码命令启动 PowerShell

```
Microsoft Windows [Version 10.0.19041.685]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\Users\Lee>PowerShell -EncodedCommand MQAuAC4AMQAwACAAfAAgACUAIAB7ACAAIgBQAG8AdwBlAHIAUwBoAGUAbABsACAAUgBvAGMAawBzACIAIAB9AA==
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
PowerShell Rocks
```

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [食谱 17.10，“向你自己的程序添加 PowerShell 脚本功能”](#ch17.xhtml#add_powershell_scripting_to_your_own_pro)

# 1.18 理解并自定义 PowerShell 的 Tab 补全

## 问题

你想要自定义 PowerShell 对 `Tab` 键和 `Ctrl+Space` 按下的反应。

## 解决方案

创建一个名为 `TabExpansion2` 的自定义函数。当你在控制台或 Visual Studio Code 中按下 `Tab` 或 `Ctrl+Space` 时，PowerShell 会调用此函数。

## 讨论

当你按下 `Tab` 时，PowerShell 会调用一个称为 *Tab 扩展* 的功能：将你迄今为止输入的内容替换为其扩展版本（如果适用）。例如，如果你输入 `Set-Location C:\` 然后按下 `Tab`，PowerShell 会开始循环遍历 `C:\` 下的目录供你导航。

PowerShell 内置 Tab 扩展提供的功能非常丰富，如 [表 1-2](#ch01.xhtml#tab_expansion_features) 所示。

**表 1-2. PowerShell 中的 Tab 扩展功能**

| 说明 | 示例 |
| --- | --- |
| *命令补全*。当当前文本似乎代表命令调用时，补全命令名称。 | `Get-Ch` **`<Tab>`** |
| *参数补全*。补全当前命令的命令参数。 | `Get-ChildItem -Pat` **`<Tab>`** |
| *参数值补全*。补全当前命令参数的命令参数值。这适用于任何接受固定值集合（枚举或定义了 `ValidateSet` 特性的参数）的命令参数。此外，PowerShell 还包含针对模块名称、帮助主题、CIM/WMI 类、事件日志名称、作业 ID 和名称、进程 ID 和名称、提供程序名称、驱动器名称、服务名称和显示名称以及跟踪源名称的扩展参数值补全。 | `Set-ExecutionPolicy -ExecutionPolicy` **`<Tab>`** |
| *历史文本补全*。用匹配 `#` 字符后文本的命令历史记录项替换当前输入。 | `#Process` **`<Tab>`** |
| *历史 ID 补全*。用命令历史记录中编号为 `ID` 的项的命令行替换当前输入。 | `#12` **`<Tab>`** |
| *文件名补全*。用匹配你迄今为止输入内容的文件名替换当前参数值。当应用于 `Set-Location` cmdlet 时，PowerShell 会进一步筛选结果，仅显示目录。 | `Set-Location` `C:\Windows\S` **`<Tab>`** |
| *运算符补全*。用匹配的运算符替换当前文本。这包括提供给 `switch` 语句的标志。 | `"Hello World" -rep`**`<Tab>`**`switch - c` **`<Tab>`** |
| *变量补全*。用可用的 PowerShell 变量替换当前文本。PowerShell 甚至包含从未被调用过的脚本内容中的变量。 | `$myGreeting = "Hello World"; $myGr` **`<Tab>`** |
| *成员补全*。替换当前引用的变量或类型的成员名称。当 PowerShell 可以从管道中的前几条命令推断成员时，它甚至支持脚本块内的成员补全。 | `[Console]::Ba` **`<Tab>`**`Get-Process | Where-Object { $_.Ha` **`<Tab>`** } |
| *类型补全*。用其命名空间限定名替换缩写的类型名称。 | `[PSSer` **`<Tab>`**`$l = New-Object List[Stri` **`<Tab>`** ] |

