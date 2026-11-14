
# 算术运算符

## 乘法运算符 (`*`)

用于数字时，返回其乘积。

用于字符串时（`"=" * 80`），返回一个新字符串，该字符串通过将字符串自身追加指定次数而创建。

用于数组时（`1..3 * 7`），返回一个新数组，该数组通过将数组自身追加指定次数而创建。

此运算符不适用于哈希表。

用于任何其他类型时，如果该类型实现了乘法运算符（`op_Multiply`），PowerShell 将使用该类型的乘法运算符。

## 除法运算符 (`/`)

```
$leftValue / $rightValue
```

用于数字时，返回其商。

此运算符不适用于字符串、数组或哈希表。

用于任何其他类型时，如果该类型实现了除法运算符（`op_Division`），PowerShell 将使用该类型的除法运算符。

## 取模运算符 (`%`)

```
$leftValue % $rightValue
```

用于数字时，返回其除法的余数。

此运算符不适用于字符串、数组或哈希表。

用于任何其他类型时，如果该类型实现了取模运算符（`op_Modulus`），PowerShell 将使用该类型的取模运算符。

## 赋值运算符 (`+=`, `-=`, `*=`, `/=`, `%=`)

*赋值运算符*：

```
$variable operator= value
```

这些运算符与简单的算术运算符（`+`、`-`、`*`、`/` 和 `%`）相匹配，但将结果存储在运算符左侧的变量中。这是以下形式的简写：

```
$variable = $variable operator value
```

# 逻辑运算符

逻辑运算符允许您比较布尔值，如[表 A-6](#app01.xhtml#windows_powershell_logical_operators)所示。

## 逻辑与 (`-and`)

*逻辑与*：

```
$leftValue -and $rightValue
```

如果左侧和右侧参数都评估为 `$true`，则返回 `$true`。否则返回 `$false`。

您可以在同一个表达式中组合多个 `-and` 运算符：

```
$value1 -and $value2 -and $value3 ...
```

PowerShell 将 `-and` 运算符实现为短路运算符，仅当其前面的所有参数都评估为 `$true` 时才会评估后续参数。

## 逻辑或 (`-or`)

*逻辑或*：

```
$leftValue -or $rightValue
```

如果左侧或右侧参数评估为 `$true`，则返回 `$true`。否则返回 `$false`。

您可以在同一个表达式中组合多个 `-or` 运算符：

```
$value1 -or $value2 -or $value3 ...
```

PowerShell 将 `-or` 运算符实现为短路运算符，仅当其前面的所有参数都评估为 `$false` 时才会评估后续参数。

## 逻辑异或 (`-xor`)

*逻辑异或*：

```
$leftValue -xor $rightValue
```

如果左侧或右侧参数（但不同时）评估为 `$true`，则返回 `$true`。

否则返回 `$false`。

## 逻辑非 (`-not`, `!`)

*逻辑非*：

```
-not $value
```

如果其右侧（唯一）参数评估为 `$false`，则返回 `$true`。否则返回 `$false`。

# 二进制运算符

二进制运算符（如[表 A-7](#app01.xhtml#windows_powershell_binary_operators)所示）允许您将布尔逻辑运算符逐位应用于运算符的参数。比较位时，1 代表 `$true`，而 0 代表 `$false`。

## 二进制与 (`-band`)

*二进制与*：

```
$leftValue -band $rightValue
```

返回一个数字，如果左侧和右侧参数在该位置的位均为 1，则该位置的位设置为 1。所有其他位设置为 0。

例如：

```
PS > $int1 = 0b110110110
PS > $int2 = 0b010010010
PS > $result = $int1 -band $int2
PS > [Convert]::ToString($result, 2)
10010010
```

## 二进制或 (`-bor`)

*二进制或*：

```
$leftValue -bor $rightValue
```

返回一个数字，如果左侧和右侧参数在该位置的任一位为 1，则该位置的位设置为 1。所有其他位设置为 0。

例如：

```
PS > $int1 = 0b110110110
PS > $int2 = 0b010010010
PS > $result = $int1 -bor $int2
PS > [Convert]::ToString($result, 2)
110110110
```

## 二进制异或 (`-bxor`)

*二进制异或*：

```
$leftValue -bxor $rightValue
```

返回一个数字，如果左侧和右侧参数在该位置的任一位为 1 但不同时为 1，则该位置的位设置为 1。所有其他位设置为 0。

例如：

```
PS > $int1 = 0b110110110
PS > $int2 = 0b010010010
PS > $result = $int1 -bxor $int2
PS > [Convert]::ToString($result, 2)
100100100
```

## 二进制非 (`-bnot`)

*二进制非*：

```
-bnot $value
```

返回一个数字，如果右侧（唯一）参数在该位置的位设置为 1，则该位置的位设置为 1。所有其他位设置为 0。

例如：

```
PS > $int1 = 0b110110110
PS > $result = -bnot $int1
PS > [Convert]::ToString($result, 2)
11111111111111111111111001001001
```

## 二进制左移 (`-shl`)

*二进制左移*：

```
$value -shl $count
```

将数字的位向左移动 `$count` 位。右侧的位设置为 0。

例如：

```
PS > $int1 = 438
PS > [Convert]::ToString($int1, 2)
110110110

PS > $result = $int1 -shl 5
PS > [Convert]::ToString($result, 2)
11011011000000
```

## 二进制右移 (`-shr`)

*二进制右移*：

```
$value -shr $count
```

将数字的位向右移动 `$count` 位。对于有符号值，左侧的位保持其符号不变。

例如：

```
PS > $int1 = -2345
PS > [Convert]::ToString($int1, 2)
11111111111111111111011011010111

PS > $result = $int1 -shr 3
PS > [Convert]::ToString($result, 2)
11111111111111111111111011011010
```

# 其他运算符

PowerShell 支持其他几个简单运算符，如[表 A-8](#app01.xhtml#other_windows_powershell_operators)所示。

## 替换运算符 (`-replace`)

*替换运算符*：

```
"target" -replace "pattern","replacement"
```

返回一个新字符串，其中 `"target"` 中与正则表达式 `"pattern"` 匹配的文本已被替换为替换文本 `"replacement"`。

```
"target" -replace "pattern",{ scriptblock }
```

返回一个新字符串，其中 `"target"` 中与正则表达式 `"pattern"` 匹配的文本已被替换为所提供脚本块的输出值。在脚本块中，`$_` 变量代表当前的 `System.Text.RegularExpressions.Match`。

默认情况下，PowerShell 执行不区分大小写的比较。`-ireplace` 运算符明确指定不区分大小写，而 `-creplace` 运算符执行区分大小写的比较。

如果正则表达式模式包含命名捕获或捕获组，替换字符串也可以引用它们。

例如：

```
PS > "Hello World" -replace "(.*) (.*)",'$2 $1'
World Hello
```

如果 `"target"` 表示一个数组，则 `-replace` 运算符将作用于该数组的每个元素。

有关正则表达式详细信息，请参阅[附录 B](#app02.xhtml#regular_expression_reference)。

## 格式化运算符 (`-f`)

*格式化运算符*：

```
"Format String" -f values
```

返回一个字符串，其中格式字符串中的格式项已被替换为值数组中值的文本等效项。

例如：

```
PS > "{0:n0}" -f 1000000000
1,000,000,000
```

格式化运算符的格式字符串完全等同于 .NET `String.Format` 方法支持的格式字符串。

有关格式字符串语法的更多详细信息，请参阅[附录 D](#app04.xhtml#net_string_formatting)。

## 类型转换运算符 (`-as`)

*类型转换运算符*：

```
$value -as [Type]
```

返回转换为给定 .NET 类型的 `$value`。如果无法进行此转换，PowerShell 返回 `$null`。

例如：

```
PS > 3/2 -as [int]
2
PS > $result = "Hello" -as [int]
PS > $result -eq $null
True
```

## 一元拆分运算符 (`-split`)

*一元拆分运算符*：

```
-split "Input String"
```
