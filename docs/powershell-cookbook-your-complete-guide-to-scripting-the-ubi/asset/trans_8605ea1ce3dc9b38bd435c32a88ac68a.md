
虽然 PowerShell 完全支持 Unicode，但托管 `pwsh.exe` 的 Windows 控制台由于 Windows 控制台系统的限制，无法正确输出某些字符。然而，图形化 PowerShell 主机（如 Visual Studio Code 和许多第三方 PowerShell IDE）不受这些限制的影响。

如果在处理用户输入时使用 PowerShell 的标准功能，你不必担心其复杂性。但如果你想处理输入中的单个字符或单词，则需要采取特殊预防措施。`System.Globalization.StringInfo` 类允许你以区分区域性的方式执行此操作。有关使用 `StringInfo` 类的更多信息，请参阅 [Microsoft 文档](https://oreil.ly/jRnWp)。

因此，为了确保脚本在用户输入方面保持区分区域性，只需在可能的情况下使用 PowerShell 对字符串操作的支持即可。

## 大小写规则

脚本中的常见需求是将用户输入与某些预定义文本（如菜单选择）进行比较。通常你希望这种比较不区分大小写，这样 `"QUIT"` 和 `"qUiT"` 就表示相同的含义。

实现这一点的传统方法是将用户输入转换为大写或小写：

```
## $text comes from the user, and contains the value "quit"
if($text.ToUpper() -eq "QUIT") { ... }
```

不幸的是，在不同区域性中运行时，显式更改字符串的大小写会以微妙的方式失败，因为许多区域性有不同的大小写和比较规则。例如，土耳其语包含两种字母 `I`：一种带点，一种不带点。小写字母 `i` 的大写版本对应带点的大写 `I`，而不是 `QUIT` 中使用的大写 `I`。该示例会导致前面的字符串比较在土耳其系统上失败。

[食谱 13.8](#ch13.xhtml#program_invoke_a_script_block_with_alter) 让我们非常清楚地看到这一点：

```
PS > Use-Culture tr-TR { "quit".ToUpper() -eq "QUIT" }
False
PS > Use-Culture tr-TR { "quIt".ToUpper() -eq "QUIT" }
True
PS > Use-Culture tr-TR { "quit".ToUpper() }
QUİT
```

要以不区分大小写的方式将某些输入与硬编码字符串进行比较，更好的解决方案是使用 PowerShell 的 `-eq` 运算符，而不必自己更改任何大小写。`-eq` 运算符默认不区分大小写且区域性中立：

```
PS > $text1 = "Hello"
PS > $text2 = "HELLO"
PS > $text1 -eq $text2
True
```

因此，为了确保脚本在大小写规则方面保持区分区域性，只需在可能的情况下使用 PowerShell 的不区分大小写的比较运算符即可。

## 排序规则

排序规则在不同区域性之间经常变化。例如，使用 [食谱 13.8](#ch13.xhtml#program_invoke_a_script_block_with_alter) 中给出的脚本比较英语和丹麦语：

```
PS > Use-Culture en-US { "Apple","Æble" | Sort-Object }
Æble
Apple
PS > Use-Culture da-DK { "Apple","Æble" | Sort-Object }
Apple
Æble
```

为了确保脚本在排序规则方面保持区分区域性，假设排序后输出是正确排序的，但不要依赖排序输出的实际顺序。

## 其他指南

有关编写区分区域性程序的其他资源，请参阅 [关于全球化和本地化 .NET 应用程序的 Microsoft 文档](https://oreil.ly/Qxz6i)。

## 另请参阅

[食谱 13.8，“程序：使用替代区域性设置调用脚本块”](#ch13.xhtml#program_invoke_a_script_block_with_alter)

# 13.7 在脚本输出中支持其他语言

## 问题

你正在向用户显示文本消息，并希望支持国际语言。

## 解决方案

使用 `Import-LocalizedData` cmdlet，如 [示例 13-6](#ch13.xhtml#import_localizeddataps1) 所示。

### 示例 13-6. 为脚本或模块导入特定区域性的字符串

```
## Create some default messages for English cultures, and
## when culture-specific messages are not available.
$messages = DATA {
    @{
        Greeting = "Hello, {0}"
        Goodbye = "So long."
    }
}

## Import localized messages for the current culture.
Import-LocalizedData messages -ErrorAction SilentlyContinue

## Output the localized messages
$messages.Greeting -f "World"
$messages.Goodbye
```

## 讨论

`Import-LocalizedData` cmdlet 让你轻松编写为不同语言显示不同消息的脚本。

这种本地化支持的核心来自 `消息表` 的概念：消息 ID（如 `Greeting` 或 `Goodbye` 消息）到其实际表示消息的简单映射。你不直接向用户输出字符串，而是从消息表中检索字符串并输出它。脚本的本地化来自于将消息表替换为包含适合当前语言的消息的表。

PowerShell 使用标准哈希表来定义消息表。哈希表中的键和值分别代表消息 ID 及其对应的字符串。

### 注意

解决方案在 `DATA` 部分中定义默认消息表。与从 `*.psd1` 文件加载消息一样，这会将 PowerShell 置于完整 PowerShell 语言的以数据为中心的子集中。虽然不是必需的，但这对于错误检测和一致性都是一种有用的做法。

在脚本中定义默认消息表后，下一步是创建本地化版本并将它们放在脚本旁边的特定语言目录中。`Import-LocalizedData` cmdlet 的真正魔力在于它在加载适当的消息文件时应用的智能。

作为背景，引用区域性（用于本地化目的）的标准方式是一个结合 `culture` 和 `region` 的标识符。例如，德国使用的德语由标识符 `de-DE` 定义。美国使用的英语由标识符 `en-US` 定义，而加拿大使用的英语由标识符 `en-CA` 定义。大多数语言在许多地区都有使用。

当你调用 `Import-LocalizedData` cmdlet 时，PowerShell 会转到与脚本相同的目录，并首先尝试从名称与当前区域性全名匹配的目录（例如 `en-CA` 或 `en-GB`）加载消息。如果失败，它会回退到区域中性目录（如 `en` 或 `de`），再回退到操作系统定义的其他备选语言。

为了让你的工作可用于尽可能广泛的语言，请将本地化消息放在最通用的适用目录中。例如，将法语消息（首先）放在 `fr` 目录中，以便所有法语区域都能受益。如果之后想针对特定区域自定义消息，请将它们放在特定区域的目录中。

与其在脚本文件（如主脚本）中定义这些消息表，不如将它们放在与脚本同名的 `*.psd1` 文件中。例如，[示例 13-6](#ch13.xhtml#import_localizeddataps1) 将其本地化消息放在 `Import-LocalizedData.psd1` 中。PowerShell 的 `psd1` 文件代表完整 PowerShell 语言的以数据为中心的子集，非常适合本地化。在 `*.psd1` 文件中定义一个哈希表（[示例 13-7](#ch13.xhtml#import_localizeddatapsd1)），但不要像默认消息表那样将其存储在变量中。

### 示例 13-7. 定义消息表的本地化 .psd1 文件

```
@{
    Greeting = "Guten Tag, {0}"
    Goodbye = "Auf Wiedersehen."
}
```

如果你已经使用一组工具来帮助管理软件本地化过程，它们可能不理解 PowerShell `*.psd1` 文件格式。另一种标准消息格式是简单的名称-值映射，因此 PowerShell 通过 `ConvertFrom-StringData` cmdlet 支持该格式：

```
ConvertFrom-StringData @'
Greeting = Guten Tag, {0}
Goodbye = Auf Wiedersehen
'@
```
