

## 3.2 管道中的格式化 Cmdlet

在 PowerShell 的格式化 cmdlet 中，一个常见的绊脚石源于将它们置于脚本或管道的中间：

```
PS > Get-Process | Format-Table | Sort-Object Name
out-lineoutput : 类型为 "Microsoft.PowerShell.Commands.Internal.
Format.FormatEntryData" 的对象无效或序列不正确。这很可能是由用户指定的 "format-*"
命令与默认格式化发生冲突导致的。
```

在内部，PowerShell 的格式化命令会生成一种新类型的对象：`Microsoft.PowerShell.Commands.Internal.Format.*`。当这些对象到达管道末尾时，PowerShell 会自动将它们发送给一个输出 cmdlet：默认为 `Out-Default`。这些 `Out-*` cmdlet 假设对象按特定顺序到达，因此对格式化命令的输出执行任何操作都会导致输出系统报错。

要解决此问题，请尝试避免在脚本或管道中间调用格式化 cmdlet。如果这样做，脚本的输出将不再适合进行对象式操作，而这正是 PowerShell 的代名词。

如果您想直接使用格式化输请通过 `Out-String` cmdlet 发送输出，如 [示例 1.24](#ch01.xhtml#select-textoutput) 所述。

有关 `Format-Table` cmdlet 的详细信息，请输入 `Get-Help Format-Table`。有关哈希表的详细信息，请参阅 [示例 7.13](#ch07.xhtml#create_a_hashtable_or_associative_array)。有关脚本块的详细信息，请参阅 [示例 11.4](#ch11.xhtml#write_a_script_block)。

### 另请参阅

[示例 1.24，“程序：在格式化输出中搜索模式”](#ch01.xhtml#select-textoutput)

[示例 7.13，“创建哈希表或关联数组”](#ch07.xhtml#create_a_hashtable_or_associative_array)

[示例 11.4，“编写脚本块”](#ch11.xhtml#write_a_script_block)

## 3.3 在变量中存储信息

### 问题

您想要存储管道或命令的输出，以便稍后使用或更详细地处理它。

### 解决方案

要存储输出以供稍后使用，请将命令的输出存储在变量中。您稍后可以访问此信息，甚至可以将其传递到管道中，就像它是原始命令的输出一样：

```
PS > $result = 2 + 2
PS > $result
4

PS > $output = ipconfig
PS > $output | Select-String "Default Gateway" | Select -First 1

   Default Gateway . . . . . . . . . : 192.168.11.1

PS > $processes = Get-Process
PS > $processes.Count
85
PS > $processes | Where-Object { $_.ID -eq 0 }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)    Id ProcessName
-------  ------    -----      ----- -----   -----     -- -----------
      0       0        0         16     0              0 Idle
```

### 讨论

PowerShell 中的变量（以及所有其他脚本和编程语言中的变量）允许您存储某物的输出，以便稍后使用。变量名以美元符号（`$`）开头，后面可以跟几乎任何字符。一小部分字符在 PowerShell 中具有特殊含义，因此 PowerShell 提供了一种方法来创建包含甚至这些特殊字符的变量名。

有关 PowerShell 变量语法和类型的详细信息，请参阅 [“变量”](#app01.xhtml#variables-id001)。

您可以将任何管道或命令的结果存储在变量中以供稍后使用。如果该命令生成简单数据（如数字或字符串），则变量包含简单数据。如果该命令生成丰富数据（如 `Get-Process` cmdlet 返回的代表系统进程的对象），则变量包含该丰富数据列表。如果该命令生成纯文本（如传统可执行文件的输出），则变量包含纯文本。

#### 注意

如果您已将大量数据存储到变量中但不再需要该数据，请为该变量赋一个新值（如 `$null`）。这将允许 PowerShell 释放用于存储该数据的内存。

除了您创建的变量外，PowerShell 还会自动定义若干变量，它们代表诸如配置文件位置、PowerShell 进程 ID 等信息。要获取这些自动变量的完整列表，请输入 `Get-Help about_Automatic_Variables`。

### 另请参阅

[“变量”](#app01.xhtml#variables-id001)

## 3.4 访问环境变量

### 问题

您想要在脚本或交互式会话中使用环境变量（如系统路径或当前用户名）。

### 解决方案

PowerShell 提供了多种访问环境变量的方法。

要列出所有环境变量，请列出 `env` 驱动器的子项：

```
Get-ChildItem env:
```

要使用更简洁的语法获取环境变量，请在其名称前加上 `$env:`

```
$env:variablename
```

（例如，`$env:username`。）

要使用其提供程序路径获取环境变量，请将 `env:` 或 `Environment::` 提供给 `Get-ChildItem` cmdlet：

```
Get-ChildItem env:variablename
Get-ChildItem Environment::variablename
```

### 讨论

PowerShell 通过其*环境提供程序*提供对环境变量的访问。提供程序让您处理数据存储（如注册表、环境变量和别名）的方式非常类似于访问文件系统。

默认情况下，PowerShell 会创建一个名为 `env` 的驱动器，它与*环境提供程序*配合工作，让您访问环境变量。环境提供程序允许您像访问其他驱动器一样访问 `env:` 驱动器中的项目：`dir env:\variablename` 或 `dir env:variablename`。如果您想直接访问提供程序（而不是通过其驱动器），也可以输入 `dir Environment::variablename`。

然而，处理环境变量最常用（也最简单）的方法是输入 `$env:variablename`。这适用于任何提供程序，但最典型的是用于环境变量。

这是因为环境提供程序与其他几个提供程序共享一项功能——即支持核心 cmdlet 的 `*-Content` 集合（请参阅 [示例 3-1](#ch03.xhtml#working_with_content_on_different_provid)）。

#### 示例 3-1. 在不同提供程序上处理内容

```
PS > "hello world" > test
PS > Get-Content test
hello world

PS > Get-Content c:test
hello world

PS > Get-Content variable:ErrorActionPreference
Continue

PS > Get-Content function:prompt
"PS $($executionContext.SessionState.Path.CurrentLocation)
    $('>' * ($nestedPromptLevel + 1)) ";
(...)

PS > Get-Content env:systemroot
C:\WINDOWS
```

#### 示例 3-2. 使用 PowerShell 的特殊变量语法访问内容

```
PS > $function:prompt
"PS $($executionContext.SessionState.Path.CurrentLocation)
    $('>' * ($nestedPromptLevel + 1)) ";

PS > $variable:ErrorActionPreference
Continue
PS > $c:test
hello world
PS > $env:systemroot
C:\WINDOWS
```

这种用于内容管理的变量语法允许您同时获取和设置内容：

```
PS > $function:prompt = { "PS > " }
PS > $function:prompt
 "PS > "
```

现在，当使用此方法访问复杂的提供程序路径时，您会很快遇到命名问题（即使底层文件存在）：

```
PS > $c:\temp\test.txt
Unexpected token '\temp\test.txt' in expression or statement.
At line:1 char:17
+ $c:\temp\test.txt <<<<
```

解决方法在于 PowerShell 对复杂变量名的转义支持。要定义复杂变量名，请将其括在大括号中：

```
PS > ${1234123!@#$!@#$12$!@#$@!} = "Crazy Variable!"
PS > ${1234123!@#$!@#$12$!@#$@!}
Crazy Variable!
PS > dir variable:\1*

Name                              Value
----                              -----
1234123!@#$!@#$12$!@#$@!          Crazy Variable!
```

