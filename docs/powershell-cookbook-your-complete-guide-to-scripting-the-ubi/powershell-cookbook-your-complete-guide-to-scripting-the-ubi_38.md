# 第五部分 参考资料

*   附录 A，*PowerShell 语言和环境*
*   附录 B，*正则表达式参考*
*   附录 C，*XPath 快速参考*
*   附录 D，*.NET 字符串格式设置*
*   附录 E，*.NET DateTime 格式设置*
*   附录 F，*精选 .NET 类及其用途*
*   附录 G，*WMI 参考*
*   附录 H，*精选 COM 对象及其用途*
*   附录 I，*精选事件及其用途*
*   附录 J，*标准 PowerShell 动词*

# 附录 A. PowerShell 语言和环境

## 命令和表达式

PowerShell 将你输入的任何行拆分为各个单元（*令牌*），然后以两种方式之一解释每个 token：作为命令或作为表达式。两者的区别很微妙：表达式支持逻辑和流程控制语句（如 `if`、`foreach` 和 `throw`），而命令则不支持。



# PowerShell 评估控制

您通常希望控制 PowerShell 解释语句的方式，因此 表 A-1 列出了可供您使用的选项。

## 表 A-1. PowerShell 评估控制

### 优先级控制：`()`

强制对命令或表达式进行求值，这类似于在数学表达式中使用括号来强制求值顺序。

例如：

```
PS > 5 * (1 + 2)
15

PS > (dir).Count
227
```

### 表达式子解析：`$()`

强制对命令或表达式进行求值，这类似于在数学表达式中使用括号来强制求值顺序。

然而，子解析功能强大如子程序，仅当子程序包含逻辑或流控制语句时才需要它。

此语句也用于在字符串内部展开动态信息。

例如：

```
PS > "The answer is (2+2)"
The answer is (2+2)

PS > "The answer is $(2+2)"
The answer is 4

PS > $value = 10
PS > $result = $(
   if($value -gt 0) { $true }
   else { $false })
PS > $result
True
```

### 列表求值：`@()`

强制将表达式作为列表进行求值。如果它已经是列表，则保持为列表。如果不是，PowerShell 会暂时将其视为列表。

例如：

```
PS > "Hello".Length
5

PS > @("Hello").Length
1

PS > ([PSCustomObject] @{
    Property1 = "Hello"
    Count = 100 }).Count
100

PS > @([PSCustomObject] @{
    Property1 = "Hello"
    Count = 100 }).Count
1
```

### DATA 求值：`DATA { }`

在 PowerShell 数据语言的上下文中求值给定的脚本块。数据语言仅支持 PowerShell 语言中以数据为中心的功能。

例如：

```
PS > DATA { 1 + 1 }
2

PS > DATA { $myVariable = "Test" }
Assignment statements are not
allowed in restricted language
mode or a Data section.
```

# 注释

要创建单行注释，请以 `#` 字符开头。要创建块（或多行）注释，请用 `<#` 和 `#>` 字符括起该区域。

```
# This is a regular comment

<# This is a block comment

function MyTest
{
    "This should not be considered a function"
}

$myVariable = 10;

Block comment ends
#>

# This is regular script again
```

# 帮助注释

PowerShell 通过查看脚本或函数的注释来为其创建帮助。如果注释包含任何受支持的帮助标记，PowerShell 会将它们添加到命令的帮助中。

基于注释的帮助支持以下标记，这些标记均不区分大小写：

`.SYNOPSIS`

：命令的简短摘要，理想情况下为单个句子。

`.DESCRIPTION`

：命令的更详细说明。

`.PARAMETER` *`name`*

：参数 *`name`* 的说明，每个您想要描述的参数对应一个。虽然您可以为每个参数编写一个 `.PARAMETER` 注释，但 PowerShell 也支持直接写在参数上方的注释。将参数帮助与实际参数放在一起使其更易于阅读和维护。

`.EXAMPLE`

：此命令的使用示例，每个您想要提供的示例对应一个。PowerShell 将 `.EXAMPLE` 标记正下方的行视为示例命令。如果该行不包含任何看起来像提示符的文本，PowerShell 会在其前面添加提示符。它将紧随初始行之后的行视为额外的输出和示例说明。

`.INPUTS`

：此命令支持的管道输入的简短摘要。对于每种输入类型，PowerShell 内置帮助遵循以下约定：

```
System.String
    You can pipe a string that contains a path to Get-ChildItem.
```

`.OUTPUTS`

：此命令生成的项目的简短摘要。对于每种输出类型，PowerShell 内置帮助遵循以下约定：

```
System.ServiceProcess.ServiceController
    This cmdlet returns objects that represent the services on the computer.
```

`.NOTES`

：关于此命令的任何其他注释或备注。

`.LINK`

：指向相关帮助主题或命令的链接，每个链接对应一个 `.LINK` 标记。如果相关帮助主题是 URL，则当用户为您的命令向 `Get-Help` 提供 `-Online` 参数时，PowerShell 会启动该 URL。

虽然这些是您可能会用到的所有受支持帮助标记，但基于注释的帮助还支持 `Get-Help` 一些不太常用功能的标记：

* `.COMPONENT`
* `.ROLE`
* `.FUNCTIONALITY`
* `.FORWARDHELPTARGETNAME`
* `.FORWARDHELPCATEGORY`
* `.REMOTEHELPRUNSPACE`
* `.EXTERNALHELP`

有关这些标记的详细信息，请键入 **`Get-Help about_Comment_Based_Help`**。

# 变量

PowerShell 提供了多种定义和访问变量的方式，汇总见 表 A-2。

## 表 A-2. PowerShell 变量语法

### 简单变量：`$simpleVariable = "Value"`

简单的变量名。变量名必须由字母数字字符组成。变量名不区分大小写。

### 多变量赋值：`$variable1, $variable2 = "Value1", "Value2"`

多变量赋值。PowerShell 根据右侧对应位置的值填充每个变量。多余的值将作为列表赋给最后一个列出的变量。

### 任意变量名：`${arbitrary!@#@\#{var}iable } = "Value"`

任意变量名。变量名必须用大括号括起来，但可以包含任何字符。变量名中的大括号必须用反引号 (\`) 转义。

### 变量“获取和设置内容”语法：`${c:\filename.extension}`

变量“获取和设置内容”语法。这类似于任意变量名语法。如果该名称对应有效的 PowerShell 路径，则可以通过读取和写入变量来获取和设置该位置项目的内容。

### 强类型变量：`[datatype] $variable = "Value"`

强类型变量。确保变量只能包含您声明的类型的数据。如果在赋值时无法将数据强制转换为该类型，PowerShell 会抛出错误。

### 受约束变量：`[constraint] $variable = "Value"`

受约束变量。确保变量只能包含通过所提供验证约束的数据。

```
PS > [ValidateLength(4, 10)] $a = "Hello"
```

支持的验证约束与作为参数验证属性支持的相同。

### 作用域变量：`$SCOPE:variable`

获取或设置该特定作用域中的变量。有效的作用域名称为 `global`（使变量在整个 Shell 中可用）、`script`（使变量仅在当前脚本中可用或在模块命令期间持久化）、`local`（使变量仅在当前作用域及子作用域中可用）和 `private`（使变量仅在当前作用域中可用）。默认作用域为*当前*作用域：在 Shell 中交互式定义时为 `global`，在脚本中任何函数或脚本块之外定义时为 `script`，其他情况下为 `local`。

### New-Item 变量提供程序：`New-Item Variable:\variable -Value value`

使用变量提供程序创建新变量。

### 获取变量：`Get-Item Variable:\variable` / `Get-Variable variable`

使用变量提供程序或 `Get-Variable` cmdlet 获取变量。这使您可以访问关于变量的额外信息，例如其选项和描述。

### New-Variable cmdlet：`New-Variable variable -Option option -Value value`

使用 `New-Variable` cmdlet 创建变量。这使您可以提供关于变量的额外信息，例如其选项和描述。

## 注意

与某些语言不同，PowerShell 在将数字转换为 `[int]` 数据类型时会进行**舍入**（而非截断）：

```
PS > (3/2)
1.5
PS > [int] (3/2)
2
```



要让 PowerShell 截断数字，请参阅 第 6 章。

# 布尔值

布尔（true 或 false）变量最常初始化为其字面值 `$true` 和 `$false`。不过，当 PowerShell 将变量作为布尔表达式（例如 `if` 语句）的一部分进行求值时，它会将它们映射为合适的布尔表示，如 表 A-3 所列。

表 A-3. PowerShell 布尔值解释

| 结果 | 布尔表示 |
| --- | --- |
| `$true` | True |
| `$false` | False |
| `$null` | False |
| 非零数字 | True |
| 零 | False |
| 非空字符串 | True |
| 空字符串 | False |
| 空数组 | False |
| 单元素数组 | 其单个元素的布尔表示 |
| 多元素数组 | True |
| 哈希表（无论是否为空） | True |

# 字符串

PowerShell 为处理纯文本数据提供了多种工具。

## 字面量字符串和可扩展字符串

要定义字面量字符串（即不发生变量或转义扩展的字符串），请用单引号将其括起来：

```
$myString = 'hello `t $ENV:SystemRoot'
```

`$myString` 获得的实际值为 ``hello `t $ENV:SystemRoot``。

要定义可扩展字符串（即发生变量和转义扩展的字符串），请用双引号将其括起来：

```
$myString = "hello `t $ENV:SystemRoot"
```

`$myString` 获得的值类似于 `hello C:\WINDOWS`。

要在单引号字符串中包含单引号，或在双引号字符串中包含双引号，请连续输入两个引号字符：

```
PS > "Hello ""There""!"
Hello "There"!
PS > 'Hello ''There''!'
Hello 'There'!
```

###### 注

要在可扩展字符串中包含复杂表达式，请使用子表达式。例如：

```
$prompt = "$(get-location) >"
```

`$prompt` 获得的值类似于 `c:\temp >`。

访问对象的属性需要子表达式：

```
$version = "Current PowerShell version is:"
    $PSVersionTable.PSVersion.Major
```

`$version` 获得的值类似于：

```
Current PowerShell version is: 3
```

## Here 字符串

要定义 *Here 字符串*（即可能跨越多行的字符串），请在开头放置两个字符 `@"`，在结尾单独一行放置两个字符 `"@`。

例如：

```
$myHereString = @"
This text may span multiple lines, and may
contain "quotes."
"@
```

Here 字符串可以是字面量（单引号）或可扩展（双引号）两种类型。

## 转义序列

PowerShell 支持字符串内的转义序列，如 表 A-4 所列。

表 A-4. PowerShell 转义序列

| 序列 | 含义 |
| --- | --- |
| `` `0 `` | *空* 字符。常用作记录分隔符。 |
| `` `a `` | *响铃* 字符。在控制台显示时发出蜂鸣声。 |
| `` `b `` | *退格* 字符。该字符仍保留在字符串中，但在控制台显示时会被覆盖。 |
| `` `e `` | *转义* 字符。标记 ANSI 转义序列的开始，例如 "`` `e[2J ``"。 |
| `` `f `` | *换页符*。在大多数打印机上打印时创建分页符。 |
| `` `n `` | *换行符*。 |
| `` `r `` | *回车符*。PowerShell 中的换行完全由 `` `n `` 字符指示，因此很少需要此字符。 |
| `` `t `` | *制表符*。 |
| `` `u{hex-code} `` | *Unicode 字符字面量*。创建由指定的十六进制 Unicode 代码点表示的字符，例如 "`` `u{2265} ``" (≥)。 |
| `` `v `` | *垂直制表符*。 |
| `''`（两个单引号） | *单引号*，用于字面量字符串中。 |
| `""`（两个双引号） | *双引号*，用于可扩展字符串中。 |
| `` `any other character` `` | 该字符，按字面意思理解。 |

# 数字

PowerShell 为与数字和数值数据交互提供了多种选项。

## 简单赋值

要定义一个保存数值数据的变量，只需像赋值其他变量一样为其赋值。PowerShell 会自动以足以准确保存数据的格式存储您的数据：

```
$myInt = 10

$myUnsignedInt = 10u
$myUnsignedInt = [uint] 10
```

`$myInt` 获得值 `10`，类型为（32 位）整数。`$myUnsignedInt` 获得值 `10`，类型为无符号整数。

```
$myDouble = 3.14
```

`$myDouble` 获得值 `3.14`，类型为（53 位，9 位精度）双精度浮点数。

要显式将数字作为字节（8 位）或短整数（16 位）赋值，请使用 `y` 或 `s` 后缀。在任一后缀前加上 `u` 可创建该数据类型的无符号版本。您也可以使用 `[byte]`、`[int16]` 和 `[short]` 强制转换：

```
$myByte = 127y
$myByte = [byte] 127
$myUnsignedByte = 127uy

$myShort = 32767s
$myShort = [int16] 32767
$myShort = [short] 32767

$myUnsignedShort = 32767us
$myUnsignedShort = [ushort] 32767
```

要显式将数字作为长整数（64 位）或十进制数（96 位，96 位精度）赋值，请使用长整数（`l`）和十进制（`d`）后缀。您也可以使用 `[long]` 强制转换：

```
$myLong = 2147483648l
$myLong = [long] 2147483648

$myUnsignedLong = 2147483648ul
$myUnsignedLong = [ulong] 2147483648

$myDecimal = 0.999d
```

要显式将数字作为 BigInteger（任意大整数，无上下界）赋值，请使用 BigInteger（`n`）后缀：

```
$myBigInt = 99999999999999999999999999999n
```

PowerShell 还支持科学计数法，其中 `e`<`number`> 表示将原始数字乘以 10 的 <`number`> 次幂：

```
$myPi = 3141592653e-9
```

`$myPi` 获得值 `3.141592653`。

PowerShell 中的数据类型（整数、长整数、双精度浮点数和十进制数）构建在同名的 .NET 数据类型之上。

## 管理用数值常量

由于计算机管理员很少有机会使用 10 的整数次幂的数字，PowerShell 提供了数值常量 `pb`、`tb`、`gb`、`mb` 和 `kb`，分别表示拍字节 (1,125,899,906,842,624)、太字节 (1,099,511,627,776)、吉字节 (1,073,741,824)、兆字节 (1,048,576) 和千字节 (1,024)：

```
PS > $downloadTime = (1gb + 250mb) / 120kb
PS > $downloadTime
10871.4666666667
```

只要结果适合该数据类型，您就可以将这些数值乘数与数据类型结合使用，例如 `250ngb`。

## 十六进制和其他数制

要直接输入十六进制数字，请使用十六进制前缀 `0x`：

```
$myErrorCode = 0xFE4A
```

`$myErrorCode` 获得整数值 `65098`。

要直接输入二进制数字，请使用二进制前缀 `0b`：

```
$myBinary = 0b101101010101
```

`$myBinary` 获得整数值 `2901`。

如果您不知道十六进制或二进制常量值，或需要转换为八进制，请使用 .NET Framework 的 `[Convert]` 类。第一个参数是要转换的值，第二个参数是基数（2、8、10 或 16）：

```
$myOctal = [Convert]::ToInt32("1234567", 8)
```

`$myOctal` 获得整数值 `342391`。

```
$myHexString = [Convert]::ToString(65098, 16)
```

`$myHexString` 获得字符串值 `fe4a`。

```
$myBinaryString = [Convert]::ToString(12345, 2)
```

`$myBinaryString` 获得字符串值 `11000000111001`。

###### 注

请参阅 “使用 .NET Framework” 以了解有关使用 PowerShell 与 .NET Framework 交互的更多信息。

## 大数字

要处理极大数字，请使用 `BigInt` 类：

```
[BigInt]::Pow(12345, 123)
```

要对多个大数字进行数学运算，请对所有操作数使用 `[BigInt]` 强制转换（或 `n` BigInt 数据类型）：

```
PS > 98123498123498123894n * 98123498123498123894n
9628220883992139841085109029337773723236

PS > $val = "98123498123498123894"
PS > ([BigInt] $val) * ([BigInt] $val)
9628220883992139841085109029337773723236
```

## 虚数和复数





要处理虚数和复数，请使用 `System.Numerics.Complex` 类：

```
PS > [System.Numerics.Complex]::ImaginaryOne *
    [System.Numerics.Complex]::ImaginaryOne | Format-List

Real      : -1
Imaginary : 0
Magnitude : 1
Phase     : 3.14159265358979
```

# 数组和列表

## 数组定义

PowerShell 数组用于保存数据列表。`@()`（`数组转换`）语法告诉 PowerShell 将括号内的内容视为数组。要创建一个空数组，请输入：

```
$myArray = @()
```

要定义一个非空数组，请使用逗号分隔其元素：

```
$mySimpleArray = 1,"Two",3.14
```

数组也可以选择仅包含单个元素：

```
$myList = ,"Hello"
```

或者，也可以使用数组转换语法：

```
$myList = @("Hello")
```

除非将其声明为强类型数组，否则数组的元素不需要全部具有相同的数据类型。在下面的示例中，外部方括号定义了一个强类型变量（如“变量”中所述），而 `int[]` 表示一个整数数组：

```
[int[]] $myArray = 1,2,3.14
```

在此模式下，如果 PowerShell 无法将列表中的任何元素转换为所需的数据类型，则会生成错误。在本例中，它将 `3.14` 四舍五入为整数值 3：

```
PS > $myArray[2]
3
```

### 注意

为了确保 PowerShell 将长度不确定的集合（如历史记录列表或目录列表）视为列表，请使用“命令和表达式”中描述的列表求值语法 `@(...)`。

数组也可以是多维 *交错* 数组（数组中的数组）：

```
$multiDimensional = @(
      (1,2,3,4),
      (5,6,7,8)
   )
```

`$multiDimensional[0][1]` 返回 2，来自第 0 行、第 1 列。

`$multiDimensional[1][3]` 返回 8，来自第 1 行、第 3 列。

要定义一个非交错的多维数组，请创建 .NET 类型的多维实例。对于整数，那将是一个 `System.Int32` 数组：

```
$multidimensional = New-Object "Int32[,]" 2,4
$multidimensional[0,1] = 2
$multidimensional[1,3] = 8
```

## 数组访问

要访问数组中的特定元素，请使用 `[]` 运算符。PowerShell 从零开始为数组元素编号。以 `$myArray = 1,2,3,4,5,6` 为例：

```
$myArray[0]
```

返回 1，即数组中的第一个元素。

```
$myArray[2]
```

返回 3，即数组中的第三个元素。

```
$myArray[-1]
```

返回 6，即数组的最后一个元素。

```
$myArray[-2]
```

返回 5，即数组的倒数第二个元素。

您还可以访问数组中的元素范围：

```
PS > $myArray[0..2]
1
2
3
```

返回第 0 个到第 2 个元素（含）。

```
PS > $myArray[-1..2]
6
1
2
3
```

返回最后一个元素，然后循环返回第 0 个到第 2 个元素（含）。PowerShell 之所以循环，是因为范围中的第一个数字是负数，而第二个数字是正数。

```
PS > $myArray[-1..-3]
6
5
4
```

按降序返回从数组的最后一个元素到倒数第三个元素。PowerShell 不会循环（因此在这种情况下会向后扫描），因为范围中的两个数字具有相同的符号。

如果被访问的数组可能为 *null*，则可以使用空条件数组访问运算符 (`?[]`)。如果被访问的数组不存在，表达式的结果将为 *null*；否则，结果将为指定索引处的元素：

```
(Get-Process -id 0).Modules?[0]
```

## 数组切片

您可以将上一节中的多个语句一次性组合起来，从数组中提取更复杂的范围。使用 + 号将数组范围与显式索引分隔开：

```
$myArray[0,2,4]
```

返回索引 0、2 和 4 处的元素。

```
$myArray[0,2+4..5]
```

返回索引 0、2 以及 4 到 5（含）处的元素。

```
$myArray[,0+2..3+0,0]
```

返回索引 0、2 到 3（含）、0 以及再次 0 处的元素。

### 注意

您可以使用数组切片语法来创建数组：

```
$myArray = ,0+2..3+0,0
```

# 哈希表（关联数组）

## 哈希表定义

PowerShell *哈希表*（也称为 *关联数组*）允许您将键与值关联起来。要定义哈希表，请使用以下语法：

```
$myHashtable = @{}
```

您可以在创建哈希表时使用其键/值对对其进行初始化。PowerShell 假定键为字符串，但值可以是任何数据类型。

```
$myHashtable = @{ Key1 = "Value1"; "Key 2" = 1,2,3; 3.14 = "Pi" }
```

要定义一个保留插入顺序的哈希表，请使用 `[ordered]` 转换：

```
$orderedHash = [ordered] @{}
$orderedHash["NewKey"] = "Value"
```

## 哈希表访问

要在关联数组中访问或修改特定元素，可以使用数组访问语法或属性访问语法：

```
$myHashtable["Key1"]
```

返回 `"Value1"`。

```
$myHashtable."Key 2"
```

返回数组 `1,2,3`。

```
$myHashtable["New Item"] = 5
```

将 `"New Item"` 添加到哈希表中。

```
$myHashtable."New Item" = 5
```

同样将 `"New Item"` 添加到哈希表中。

# XML

PowerShell 支持 XML 作为原生数据类型。要创建 XML 变量，请将字符串转换为 `[xml]` 类型：

```
$myXml = [xml] @"
<AddressBook>
   <Person contactType="Personal">
      <Name>Lee</Name>
      <Phone type="home">555-1212</Phone>
      <Phone type="work">555-1213</Phone>
   </Person>
   <Person contactType="Business">
      <Name>Ariel</Name>
      <Phone>555-1234</Phone>
   </Person>
</AddressBook>
"@
```

PowerShell 将所有子节点和属性作为属性公开。在此过程中，PowerShell 会自动将共享相同节点类型的子节点分组：

```
$myXml.AddressBook
```

返回一个包含 `Person` 属性的对象。

```
$myXml.AddressBook.Person
```

返回 `Person` 节点的列表。每个 Person 节点将 `contactType`、`Name` 和 `Phone` 作为属性公开。

```
$myXml.AddressBook.Person[0]
```

返回第一个 `Person` 节点。

```
$myXml.AddressBook.Person[0].ContactType
```

返回 `Personal` 作为第一个 `Person` 节点的联系人类型。

# 简单运算符

定义好数据后，下一步就是处理它。

## 算术运算符

算术运算符允许您对数据执行数学运算，如表 A-5 所示。

### 注意

.NET Framework 中的 `System.Math` 类除了 PowerShell 原生支持的运算符外，还提供了许多强大的运算：

```
PS > [Math]::Pow([Math]::E, [Math]::Pi)
23.1406926327793
```

请参阅“使用 .NET Framework”，了解有关使用 PowerShell 与 .NET Framework 交互的更多信息。

**表 A-5. PowerShell 算术运算符**

| 运算符 | 说明 |
| --- | --- |
| `+` | `加法运算符`：`$leftValue + $rightValue`。用于数字时，返回其和。用于字符串时，返回通过将第二个字符串追加到第一个字符串而创建的新字符串。用于数组时，返回通过将第二个数组追加到第一个数组而创建的新数组。用于哈希表时，返回通过合并两个哈希表而创建的新哈希表（重复键会报错）。用于其他类型时，如果已实现则使用 `op_Addition`。 |
| `-` | `减法运算符`：`$leftValue - $rightValue`。用于数字时，返回其差。不适用于字符串、数组或哈希表。用于其他类型时，如果已实现则使用 `op_Subtraction`。 |
| `*` | `乘法运算符`：`$leftValue * $rightValue`。 |




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

逻辑运算符允许您比较布尔值，如表 A-6 所示。

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

二进制运算符（如表 A-7 所示）允许您将布尔逻辑运算符逐位应用于运算符的参数。比较位时，1 代表 `$true`，而 0 代表 `$false`。

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

PowerShell 支持其他几个简单运算符，如表 A-8 所示。

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

有关正则表达式详细信息，请参阅附录 B。

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

有关格式字符串语法的更多详细信息，请参阅附录 D。

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



# PowerShell 运算符

## 拆分和联接运算符

### `-split`

将给定的输入字符串拆分为数组，使用空白字符 (`\s+`) 来识别元素之间的边界。它还会修剪结果。

例如：

```
PS > -split "  Hello    World   "
Hello
World
```

*二元拆分运算符*：

```
"Input String" -split "delimiter",maximum,options
"Input String" -split { Scriptblock },maximum
```

将给定的输入字符串拆分为数组，使用给定的 *`delimiter`* 或 *`script block`* 来识别元素之间的边界。

*`Delimiter`* 被解释为正则表达式匹配。对于输入中的每个字符都会调用 *`Scriptblock`*，当它返回 `$true` 时会引入拆分。

*`Maximum`* 定义要返回的最大元素数量，将未拆分的元素作为最后一项。此项为可选。如果你想提供选项但不更改最大值，请使用 "`0`" 表示无限制。

*`Options`* 定义要应用于拆分行为的特殊行为。可能的枚举值包括：

- `SimpleMatch`：按字面字符串拆分，而非它们可能代表的正则表达式。
- `RegexMatch`：按正则表达式拆分。此选项为默认值。
- `CultureInvariant`：在执行不区分大小写的拆分时，不使用特定区域性的大小写规则。
- `IgnorePatternWhitespace`：忽略拆分模式中的空格和正则表达式注释。
- `Multiline`：允许 `^` 和 `$` 字符匹配行边界，而不仅仅是内容的开头和结尾。
- `Singleline`：将 `^` 和 `$` 字符视为内容的开头和结尾。此选项为默认值。
- `IgnoreCase`：搜索匹配项时忽略内容的大小写。
- `ExplicitCapture`：在正则表达式匹配中，仅捕获命名组。此选项对 `-split` 运算符无影响。

例如：

```
PS > "1a2B3" -split "[a-z]+",0,"IgnoreCase"
1
2
3
```

### `-join`

*一元联接运算符*：

```
-join ("item1","item2",...,"item_n")
```

将提供的项目合并为单个字符串，不使用分隔符。例如：

```
PS > -join ("a","b")
ab
```

*二元联接运算符*：

```
("item1","item2",...,"item_n") -join Delimiter
```

将提供的项目合并为单个字符串，使用 *`Delimiter`* 作为分隔符。例如：

```
PS > ("a","b") -join ", "
a, b
```

## 比较运算符

PowerShell 比较运算符（列于 表 A-9 中）允许你比较表达式。默认情况下，PowerShell 的比较运算符不区分大小写。对于所有适用大小写敏感性的运算符，`-i` 前缀使这种不区分大小写的行为显式化，而 `-c` 前缀则执行区分大小写的比较。

### 表 A-9. PowerShell 比较运算符

| 运算符 | 含义 |
| --- | --- |
| `-eq` | *相等运算符*：`$leftValue -eq $rightValue` |
| `-ne` | *非相等运算符*：`$leftValue -ne $rightValue` |
| `-ge` | *大于或等于运算符*：`$leftValue -ge $rightValue` |
| `-gt` | *大于运算符*：`$leftValue -gt $rightValue` |
| `-in` | *包含运算符*：`$value -in $list` |
| `-notin` | *非包含运算符* |
| `-lt` | *小于运算符*：`$leftValue -lt $rightValue` |
| `-le` | *小于或等于运算符*：`$leftValue -le $rightValue` |
| `-like` | *模糊匹配运算符*：`$leftValue -like Pattern` |
| `-notlike` | *非模糊匹配运算符* |
| `-match` | *正则匹配运算符*：`"Target" -match Regular Expression` |
| `-notmatch` | *非正则匹配运算符* |
| `-contains` | *包含运算符*：`$list -contains $value` |

### 运算符详情

#### `-eq`

*相等运算符*：

```
$leftValue -eq $rightValue
```

对于所有基元类型，如果 *`$leftValue`* 和 *`$rightValue`* 相等，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中等于 *`$rightValue`* 的所有元素。

用于任何其他类型时，如果该类型实现了 `Equals()` 方法，PowerShell 将使用该方法。

#### `-ne`

*非相等运算符*：

```
$leftValue -ne $rightValue
```

对于所有基元类型，如果 *`$leftValue`* 和 *`$rightValue`* 不相等，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中不等于 *`$rightValue`* 的所有元素。

用于任何其他类型时，如果该类型实现了 `Equals()` 方法，PowerShell 将返回该方法的否定结果。

#### `-ge`

*大于或等于运算符*：

```
$leftValue -ge $rightValue
```

对于所有基元类型，如果 *`$leftValue`* 大于或等于 *`$rightValue`*，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中大于或等于 *`$rightValue`* 的所有元素。

用于任何其他类型时，如果该对象实现了 `Compare()` 方法，PowerShell 将返回该方法的结果。如果该方法返回的数字大于或等于零，则运算符返回 `$true`。

#### `-gt`

*大于运算符*：

```
$leftValue -gt $rightValue
```

对于所有基元类型，如果 *`$leftValue`* 大于 *`$rightValue`*，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中大于 *`$rightValue`* 的所有元素。

用于任何其他类型时，如果该对象实现了 `Compare()` 方法，PowerShell 将返回该方法的结果。如果该方法返回的数字大于零，则运算符返回 `$true`。

#### `-in`

*包含运算符*：

```
$value -in $list
```

如果值 *$value* 包含在列表 `$list` 中，则返回 `$true`。即，如果列表中至少有一个项目使 `$item -eq $value` 返回 `$true`。这等同于操作数颠倒的 `-contains` 运算符。

#### `-notin`

*非包含运算符*：

当 `-in` 运算符返回 `$false` 时，返回 `$true`。

#### `-lt`

*小于运算符*：

```
$leftValue -lt $rightValue
```

对于所有基元类型，如果 *`$leftValue`* 小于 *`$rightValue`*，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中小于 *`$rightValue`* 的所有元素。

用于任何其他类型时，如果该对象实现了 `Compare()` 方法，PowerShell 将返回该方法的结果。如果该方法返回的数字小于零，则运算符返回 `$true`。

#### `-le`

*小于或等于运算符*：

```
$leftValue -le $rightValue
```

对于所有基元类型，如果 *`$leftValue`* 小于或等于 *`$rightValue`*，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中小于或等于 *`$rightValue`* 的所有元素。

用于任何其他类型时，如果该对象实现了 `Compare()` 方法，PowerShell 将返回该方法的结果。如果该方法返回的数字小于或等于零，则运算符返回 `$true`。

#### `-like`

*模糊匹配运算符*：

```
$leftValue -like Pattern
```

根据目标评估模式，如果简单匹配成功，则返回 `$true`。

用于数组时，返回 *`$leftValue`* 中与 *`Pattern`* 匹配的所有元素。

`-like` 运算符支持以下简单通配符：

- `?`：任意单个未指定字符
- `*`：零个或多个未指定字符
- `[a-b]`：a–b 范围内的任意字符
- `[ab]`：指定的字符 a 或 b

例如：

```
PS > "Test" -like "[A-Z]e?[tr]"
True
```

#### `-notlike`

*非模糊匹配运算符*：

当 `-like` 运算符返回 `$false` 时，返回 `$true`。

#### `-match`

*正则匹配运算符*：

```
"Target" -match Regular Expression
```

根据目标评估正则表达式，如果匹配成功，则返回 `$true`。完成后，PowerShell 将成功的匹配项放入 `$matches` 变量中。

用于数组时，返回 *`Target`* 中与 *`Regular Expression`* 匹配的所有元素。

`$matches` 变量是一个哈希表，将各个匹配项映射到它们匹配的文本。0 是匹配的整个文本，1 及以上包含正则表达式中任何未命名捕获组的文本，字符串值包含正则表达式中任何命名捕获组的文本。

例如：

```
PS > "Hello World" -match "(.*) (.*)"
True
PS > $matches[1]
Hello
```

有关正则表达式详细信息，请参阅 附录 B。

#### `-notmatch`

*非正则匹配运算符*：

当 `-match` 运算符返回 `$false` 时，返回 `$true`。

`-notmatch` 运算符仍会用 `match` 的结果填充 `$matches` 变量。

#### `-contains`

*包含运算符*：

```
$list -contains $value
```




# PowerShell 运算符和控制语句

## 包含运算符

### `-contains`

如果 `$list` 指定的列表包含值 `$value`，则返回 `$true`——即，如果列表中至少有一个项目使 `$item -eq $value` 返回 `$true`。这等同于将操作数颠倒的 `-in` 运算符。

### `-notcontains`

*否定包含运算符*：

当 `-contains` 运算符会返回 `$false` 时，返回 `$true`。

## 类型运算符

### `-is`

*类型运算符*：

```
$leftValue -is [type]
```

如果 `$value` 是（或继承自）指定的 .NET 类型，则返回 `$true`。

### `-isnot`

*否定类型运算符*：

当 `-is` 运算符会返回 `$false` 时，返回 `$true`。

# 条件语句

PowerShell 中的条件语句允许你更改脚本中的执行流程。

## if、elseif 和 else 语句

```
if (condition)
{
   statement block
}
elseif (condition)
{
   statement block
}
else
{
   statement block
}
```

如果 `condition` 计算结果为 `$true`，PowerShell 将执行你提供的语句块。然后，它在 `if/elseif/else` 语句列表的末尾恢复执行。即使语句块只包含一条语句，PowerShell 也要求用大括号将语句块括起来。

> **注意**
>
> 有关 PowerShell 如何将表达式作为条件进行求值的讨论，请参阅“简单运算符”和“比较运算符”。

如果 `condition` 计算结果为 `$false`，PowerShell 将对后续任何（可选的）`elseif` 条件求值，直到有一个匹配为止。如果匹配，PowerShell 将执行与该条件关联的语句块，然后在 `if/elseif/else` 语句列表的末尾恢复执行。

例如：

```
$textToMatch = Read-Host "Enter some text"
$matchType = Read-Host "Apply Simple or Regex matching?"
$pattern = Read-Host "Match pattern"
if ($matchType -eq "Simple")
{
    $textToMatch -like $pattern
}
elseif ($matchType -eq "Regex")
{
    $textToMatch -match $pattern
}
else
{
    Write-Host "Match type must be Simple or Regex"
}
```

如果没有条件计算结果为 `$true`，PowerShell 将执行与（可选的）`else` 子句关联的语句块，然后在 `if/elseif/else` 语句列表的末尾恢复执行。

要将 `if` 语句应用于列表的每个元素并对其进行筛选以仅返回与提供的条件匹配的结果，请使用 `Where-Object` cmdlet 或 `.where()` 方法：

```
Get-Process | Where-Object { $_.Handles -gt 500 }

(Get-Process).where({ $_.Handles -gt 500 })
```

## 三元运算符

```
$result = condition ? true value : false value
```

if/else 语句的简短形式。如果 `condition` 计算结果为 `$true`，表达式的结果为 *true value* 子句的值。否则，表达式的结果为 *false value* 子句的值。例如：

```
(Get-Random) % 2 -eq 0 ? "Even number" : "Odd number"
```

## 空合并和赋值运算符

```
$result = nullable value ?? default value
```

赋值版本：

```
$result = nullable value
$result ??= default value
```

三元运算符的简短形式，仅检查表达式是否为 *null*。如果为 null，表达式的结果为 *default value* 子句的值。例如：

```
Get-Process | ForEach-Object { $_.CPU ?? "<Unavailable>" }
```

或

```
$cpu = (Get-Process -id 0).CPU
$cpu ??= "Unavailable"
```

## switch 语句

```
switch options expression
{
   comparison value           { statement block }
   -or-
   { comparison expression }  { statement block }
   (...)
   default                    { statement block }
}
```

或：

```
switch options -file filename
{
   comparison value           { statement block }
   -or
   { comparison expression }  { statement block }
   (...)
   default                    { statement block }
}
```

当 PowerShell 对 `switch` 语句求值时，它将 `expression` 与 switch 主体中的语句进行比较。如果 `expression` 是一个值列表，PowerShell 将把每个项与 switch 主体中的语句进行比较。如果指定了 `-file` 选项，PowerShell 会将文件中的行视为 `expression` 中的项目列表。

`comparison value` 语句允许你将当前输入项与 `comparison value` 指定的模式进行匹配。默认情况下，PowerShell 将其视为不区分大小写的精确匹配，但你提供给 `switch` 语句的选项可以更改此行为，如表 A-10 所示。

### 表 A-10. PowerShell switch 语句支持的选项

| 选项 | 含义 |
| --- | --- |
| `-casesensitive`, `-c` | *区分大小写的匹配*。启用此选项时，仅当当前输入项与 `comparison value` 指定的值完全匹配时，PowerShell 才会执行关联的语句块。如果当前输入对象是字符串，则匹配区分大小写。 |
| `-exact`, `-e` | *精确匹配*。启用此选项时，仅当当前输入项与 `comparison value` 指定的值完全匹配时，PowerShell 才会执行关联的语句块。此匹配不区分大小写。这是默认操作模式。 |
| `-regex`, `-r` | *正则表达式匹配*。启用此选项时，仅当当前输入项与 `comparison value` 指定的正则表达式匹配时，PowerShell 才会执行关联的语句块。此匹配不区分大小写。 |
| `-wildcard`, `-w` | *通配符匹配*。启用此选项时，仅当当前输入项与 `comparison value` 指定的通配符匹配时，PowerShell 才会执行关联的语句块。通配符匹配支持以下简单通配符字符：`?`（任意单个未指定字符）、`*`（零个或多个未指定字符）、`[a-b]`（a–b 范围内的任意字符）、`[ab]`（指定的字符 a 或 b）。此匹配不区分大小写。 |

`{ comparison expression }` 语句允许你在任意脚本块中处理存储在 `$_`（或 `$PSItem`）变量中的当前输入项。当它处理 `{ comparison expression }` 语句时，仅当 `{ comparison expression }` 计算结果为 `$true` 时，PowerShell 才会执行关联的语句块。

如果 `switch` 主体中的任何其他语句都不匹配，PowerShell 将执行与（可选的）`default` 语句关联的语句块。

在处理 `switch` 语句时，PowerShell 会尝试将当前输入对象与 `switch` 主体中的每个语句进行匹配，即使已经有一个或多个匹配，也会继续向下匹配下一个语句。要让 PowerShell 停止当前比较（但使用下一个输入对象重试 switch 语句），请在语句块中将 `continue` 语句作为最后一条语句。要让 PowerShell 在处理完一个匹配后完全退出 `switch` 语句，请在语句块中将 `break` 语句作为最后一条语句。

例如：

```
$myPhones = "(555) 555-1212","555-1234"

switch -regex ($myPhones)
{
  { $_.Length -le 8 }  { "Area code was not specified"; break }
  { $_.Length -gt 8 }  { "Area code was specified" }
  "\((555)\).*"        { "In the $($matches[1]) area code" }
}
```

生成以下输出：

```
Area code was specified
In the 555 area code
Area code was not specified
```

> **注意**
>
> 有关 `break` 语句的更多信息，请参阅下一节关于循环语句的内容。

默认情况下，PowerShell 将其视为不区分大小写的精确匹配，但你提供给 `switch` 语句的选项可以更改此行为。

# 循环语句

PowerShell 中的循环语句允许你多次执行语句组。

## for 语句

```
:loop_label for (initialization; condition; increment)
{
   statement block
}
```





# PowerShell 循环语句

## for 语句

当 PowerShell 执行 `for` 语句时，它首先执行由 `initialization` 给出的表达式。接下来它对 `condition` 求值。如果 `condition` 求值为 `$true`，PowerShell 将执行给定的语句块。然后它执行由 `increment` 给出的表达式。只要 `condition` 求值为 `$true`，PowerShell 就会继续执行语句块和 `increment` 语句。

例如：

```
for($counter = 0; $counter -lt 10; $counter++)
{
    Write-Host "Processing item $counter"
}
```

`break` 和 `continue` 语句（本附录稍后讨论）可以将任何外层循环语句的 `loop_label` 指定为其目标。

## foreach 语句

```
:loop_label foreach(variable in expression)
{
   statement block
}
```

当 PowerShell 执行 `foreach` 语句时，它执行由 `expression` 给出的管道——例如，`Get-Process | Where-Object {$_.Handles -gt 500}` 或 `1..10`。对于表达式生成的每个项目，它将该项目赋值给由 `variable` 指定的变量，然后执行给定的语句块。例如：

```
$handleSum = 0
foreach($process in Get-Process |
    Where-Object { $_.Handles -gt 500 })
{
   $handleSum += $process.Handles
}
$handleSum
```

除了 `foreach` 语句，你还可以直接在集合上使用 `foreach` 方法：

```
$handleSum = 0
(Get-Process).foreach( { $handleSum += $_.Handles } )
```

`break` 和 `continue` 语句（本附录稍后讨论）可以将任何外层循环语句的 `loop_label` 指定为其目标。除了 `foreach` 语句，PowerShell 还提供了具有类似功能的 `ForEach-Object` cmdlet。有关更多信息，请参阅 食谱 4.4。

## while 语句

```
:loop_label while(condition)
{
   statement block
}
```

当 PowerShell 执行 `while` 语句时，它首先对由 `condition` 给出的表达式求值。如果该表达式求值为 `$true`，PowerShell 将执行给定的语句块。只要 `condition` 求值为 `$true`，PowerShell 就会继续执行该语句块。例如：

```
$command = "";
while($command -notmatch "quit")
{
   $command = Read-Host "Enter your command"
}
```

`break` 和 `continue` 语句（本附录稍后讨论）可以将任何外层循环语句的 `loop_label` 指定为其目标。

## do … while 语句 / do … until 语句

```
:loop_label do
{
   statement block
} while(condition)
```

或

```
:loop_label do
{
   statement block
} until(condition)
```

当 PowerShell 执行 `do … while` 或 `do … until` 语句时，它首先执行给定的语句块。在 `do … while` 语句中，只要 `condition` 求值为 `$true`，PowerShell 就会继续执行该语句块。在 `do … until` 语句中，只要 `condition` 求值为 `$false`，PowerShell 就会继续执行该语句。例如：

```
$validResponses = "Yes","No"
$response = ""
do
{
   $response = Read-Host "Yes or No?"
} while($validResponses -notcontains $response)
"Got it."

$response = ""
do
{
   $response = Read-Host "Yes or No?"
} until($validResponses -contains $response)
"Got it."
```

`break` 和 `continue` 语句（本附录稍后讨论）可以将任何外层循环语句的 `loop_label` 指定为其目标。

## 流程控制语句

PowerShell 支持两个语句来帮助你控制循环内的流程：`break` 和 `continue`。

### break

`break` 语句停止当前循环的执行。然后 PowerShell 在当前循环语句的末尾恢复执行，就好像循环语句已自然完成一样。例如：

```
for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            break
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

产生如下输出（注意第二列永远不会达到值 `2`）：

```
Processing item 0,0
Processing item 0,1
Processing item 1,0
Processing item 1,1
Processing item 2,0
Processing item 2,1
Processing item 3,0
Processing item 3,1
Processing item 4,0
Processing item 4,1
```

如果你在 `break` 语句中指定了标签——例如，`break outer_loop`——PowerShell 将停止执行该循环。例如：

```
:outer_loop for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            break outer_loop
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

产生如下输出：

```
Processing item 0,0
Processing item 0,1
```

### continue

`continue` 语句跳过当前语句块其余部分的执行。然后 PowerShell 继续当前循环语句的下一次迭代，就好像语句块已自然完成一样。例如：

```
for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            continue
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

产生如下输出：

```
Processing item 0,0
Processing item 0,1
Processing item 0,3
Processing item 0,4
Processing item 1,0
Processing item 1,1
Processing item 1,3
Processing item 1,4
Processing item 2,0
Processing item 2,1
Processing item 2,3
Processing item 2,4
Processing item 3,0
Processing item 3,1
Processing item 3,3
Processing item 3,4
Processing item 4,0
Processing item 4,1
Processing item 4,3
Processing item 4,4
```

如果你在 `continue` 语句中指定了标签——例如，`continue outer_loop`——PowerShell 将继续该循环的下一次迭代。

例如：

```
:outer_loop for($counter = 0; $counter -lt 5; $counter++)
{
    for($counter2 = 0; $counter2 -lt 5; $counter2++)
    {
        if($counter2 -eq 2)
        {
            continue outer_loop
        }

        Write-Host "Processing item $counter,$counter2"
    }
}
```

产生如下输出：

```
Processing item 0,0
Processing item 0,1
Processing item 1,0
Processing item 1,1
Processing item 2,0
Processing item 2,1
Processing item 3,0
Processing item 3,1
Processing item 4,0
Processing item 4,1
```

## 类

```
## 一个名为 "Example" 的类，继承自 "BaseClass"
## 并实现了 "ImplementedInterface" 接口
class Example : BaseClass, ImplementedInterface
{
    ## 默认构造函数，同时也调用了基类的构造函数。
    Example() : base()
    {
        [Example]::lastInstantiated = Get-Date
    }

    ## 带参数的构造函数
    Example([string] $Name)
    {
        $this.Name = $Name
        [Example]::lastInstantiated = Get-Date
    }

    ## 一个带有验证特性的公开可见属性
    [ValidateLength(2,20)]
    [string] $Name

    ## 一个从默认视图中隐藏的属性
    static hidden [DateTime] $lastInstantiated

    ## 一个返回值的公开可见方法
    [string] ToString()
    {
        ## 需要 Return 语句。隐式/管道输出
        ## 不像函数那样被视为输出。
        return $this.ToString( [Int32]::MaxValue )
    }

    ## 一个返回值的公开可见方法
    [string] ToString([int] $MaxLength)
    {
        $output = "Name = $($this.Name);" +
            "LastInstantiated = $([Example]::lastInstantiated)"
        $outputLength = [Math]::Min($MaxLength, $output.Length)
        return $output.Substring(0, $outputLength)
    }

}
```

### 基类和接口





# 类定义

要定义一个继承自基类或实现接口的类，请在类名后提供基类和/或接口名称，用冒号分隔。继承自基类或实现任意接口均为可选项。

```
class Example [: BaseClass, ImplementedInterface]
```

## 构造函数

要定义类构造函数，请创建一个与类同名的方法。您可以定义多个构造函数，包括带参数的构造函数。要自动调用基类的构造函数，请在方法名末尾添加 `: base()`。

```
Example() [: base()]

Example([int] $Parameter1, [string] $Parameter2) [: base()]
```

## 属性

要定义公开可见的属性，请在类中定义一个 PowerShell 变量。与常规 PowerShell 变量一样，您可以选择性地添加验证特性或为属性声明类型约束。

```
[ValidateLength(2,20)]
[string] $Name
```

要从默认视图中隐藏属性（类似于其他语言中的成员变量），请使用 `hidden` 关键字。用户若需要仍可访问隐藏属性：它们只是从默认视图中移除了。如果您希望属性在当前进程中由类的所有实例共享，可以将其设为 `static`。

```
static hidden [DateTime] $lastInstantiated
```

## 方法

定义方法的方式类似于定义 PowerShell 函数，但不需要 `function` 关键字和 `param()` 语句。方法支持参数、参数验证，并且只要参数不同，也可以拥有相同的名称。

```
[string] ToString() { ... }

[string] ToString([int] $MaxLength) { ... }
```

# 自定义枚举

要定义自定义枚举，请使用 `enum` 关键字：

```
enum MyColor {
  Red = 1
  Green = 2
  Blue = 3
}
```

如果枚举值旨在通过位运算符进行组合，请使用 `[Flags()]` 特性。如果您要求枚举值派生自特定的整型数据类型（`byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`），请在冒号字符后提供该数据类型。

```
[Flags()] enum MyColor : uint {
  Red = 1
  Green = 2
  Blue = 4
}
```

# 工作流专用语句

在工作流中，PowerShell 支持三种传统 PowerShell 脚本不支持的语句：`InlineScript`、`Parallel` 和 `Sequence`。

> **注意**
>
> PowerShell 不再支持工作流。本节的存在是为了帮助您理解和处理已经编写的工作流。

## InlineScript

`InlineScript` 关键字定义一个 PowerShell 脚本岛，它将作为一个单元被调用，并使用传统的 PowerShell 脚本语义。例如：

```
workflow MyWorkflow
{
    ## 方法调用在工作流中不受支持
    ## [Math]::Sqrt(100)

    InlineScript
    {
        ## 在 InlineScript 中受支持
        [Math]::Sqrt(100)
    }
}
```

## Parallel/Sequence

`Parallel` 关键字指定语句块内的所有语句应并行运行。要将应作为一个单元运行的语句分组，请使用 `Sequence` 关键字：

```
workflow MyWorkflow
{
    Parallel
    {
        InlineScript { Start-Sleep -Seconds 2; "One thing run in parallel" }
        InlineScript { Start-Sleep -Seconds 4; "Another thing run in parallel" }
        InlineScript { Start-Sleep -Seconds 3; "A third thing run in parallel" }

        Sequence
        {
            Start-Sleep -Seconds 1
            "A fourth"
            "and fifth thing run as a unit, in parallel"
        }
    }
}
```

请注意，您不应仅为并行语句而使用 PowerShell 工作流——`ForEach-Object` cmdlet 的 `-Parallel` 参数效率高得多。

# 使用 .NET Framework

赋予 PowerShell 在系统管理和应用程序开发方面惊人触达能力的一个特性，是其利用 Microsoft 庞大且广泛的 .NET Framework 的能力。

在 PowerShell 中使用 .NET Framework 主要通过两项任务之一实现：调用方法或访问属性。

## 静态方法

要在类上调用静态方法，请输入：

```
[ClassName]::MethodName(parameter list)
```

例如：

```
PS > [System.Diagnostics.Process]::GetProcessById(0)
```

获取 ID 为 0 的进程并显示以下输出：

```
Handles  NPM(K)    PM(K)      WS(K) VM(M)  CPU(s)     Id ProcessName
-------  ------    -----      ----- -----  ------     -- -----------
      0       0        0         16     0              0 Idle
```

## 实例方法

要在对象实例上调用方法，请输入：

```
$objectReference.MethodName(parameter list)
```

例如：

```
PS > $process = [System.Diagnostics.Process]::GetProcessById(0)
PS > $process.Refresh()
```

这将 ID 为 0 的进程存储在 `$process` 变量中。然后在该特定进程上调用 `Refresh()` 实例方法。

## 显式实现的接口方法

要在显式实现的接口上调用方法：

```
([Interface] $objectReference).MethodName(parameter list)
```

例如：

```
PS > ([IConvertible] 123).ToUint16($null)
```

## 静态属性

要访问类上的静态属性，请输入：

```
[ClassName]::PropertyName
```

或：

```
[ClassName]::PropertyName = value
```

例如，`[System.DateTime]` 类提供了一个返回当前时间的 `Now` 静态属性：

```
PS > [System.DateTime]::Now
Sunday, July 16, 2006 2:07:20 PM
```

虽然这很少见，但某些类型允许您设置某些静态属性的值。

## 实例属性

要访问对象上的实例属性，请输入：

```
$objectReference.PropertyName
```

或：

```
$objectReference.PropertyName = value
```

例如：

```
PS > $today = [System.DateTime]::Now
PS > $today.DayOfWeek
Sunday
```

这将当前日期存储在 `$today` 变量中。然后在该特定日期上调用 `DayOfWeek` 实例属性。

如果属性的值可能为 *null*，则可以使用空条件属性访问运算符 (`?.`)。如果链中的任何属性不存在，表达式的结果将为 *null*；否则将为最终属性的值：

```
(Get-Process -id 0)?.MainModule?.Filename
```

## 了解类型

了解类和类型的两个主要途径是 `Get-Member` cmdlet 和 .NET Framework 的文档。

### Get-Member Cmdlet

要了解给定类型支持哪些方法和属性，请将其通过管道传递给 `Get-Member` cmdlet，如表 A-11 所示。

**表 A-11. 使用 `Get-Member` Cmdlet**

| 操作 | 结果 |
| --- | --- |
| `[typename] \| Get-Member -Static` | 给定类型的所有静态方法和属性。 |
| `$objectReference \| Get-Member -Static` | `$objectReference` 中类型提供的所有静态方法和属性。 |
| `$objectReference \| Get-Member` | `$objectReference` 中类型提供的所有实例方法和属性。如果 `$objectReference` 代表一个项目集合，PowerShell 将返回该集合所包含类型的实例和属性。要查看集合本身的实例和属性，请使用 `Get-Member` 的 `-InputObject` 参数：`Get-Member -InputObject $objectReference` |
| `[typename] \| Get-Member` | 代表此类型的 `System.RuntimeType` 对象的所有实例方法和属性。 |

### .NET Framework 文档

了解 .NET Framework 中类的另一个信息来源是文档本身，可通过 [Microsoft 的开发人员文档站点](https://docs.microsoft.com) 的搜索功能获取。




典型的类文档首先从总体概述开始，然后提供指向该类成员（即它支持的方法和属性列表）的超链接。

### 注意

要快速访问成员文档，请在 MSDN 搜索词中添加 "members" 术语以更明确地搜索它们：

```
classname members
```

类成员的文档列出了它们的构造函数、方法、属性等。它使用 `S` 图标表示静态方法和属性。单击成员名称可获取有关该成员的详细信息，包括该成员生成的对象类型。

## 类型快捷方式

当你指定类型名称时，PowerShell 允许你对某些最常用的类型使用简短形式，如 表 A-12 所示。

**表 A-12. PowerShell 类型快捷方式**

| 类型快捷方式 | 完整类名 |
| --- | --- |
| `[Adsi]` | `[System.DirectoryServices.DirectoryEntry]` |
| `[AdsiSearcher]` | `[System.DirectoryServices.DirectorySearcher]` |
| `[Float]` | `[System.Single]` |
| `[Hashtable]` | `[System.Collections.Hashtable]` |
| `[Int]` | `[System.Int32]` |
| `[IPAddress]` | `[System.Net.IPAddress]` |
| `[Long]` | `[System.Int64]` |
| `[PowerShell]` | `[System.Management.Automation.PowerShell]` |
| `[PSCustomObject]` | `[System.Management.Automation.PSObject]` |
| `[PSModuleInfo]` | `[System.Management.Automation.PSModuleInfo]` |
| `[PSObject]` | `[System.Management.Automation.PSObject]` |
| `[Ref]` | `[System.Management.Automation.PSReference]` |
| `[Regex]` | `[System.Text.RegularExpressions.Regex]` |
| `[Runspace]` | `[System.Management.Automation.Runspaces.Runspace]` |
| `[RunspaceFactory]` | `[System.Management.Automation.Runspaces.RunspaceFactory]` |
| `[ScriptBlock]` | `[System.Management.Automation.ScriptBlock]` |
| `[Switch]` | `[System.Management.Automation.SwitchParameter]` |
| `[Wmi]` | `[System.Management.ManagementObject]` |
| `[WmiClass]` | `[System.Management.ManagementClass]` |
| `[WmiSearcher]` | `[System.Management.ManagementObjectSearcher]` |
| `[Xml]` | `[System.Xml.XmlDocument]` |
| `[TypeName]` | `[System.TypeName]` |

## 创建类型实例

```
$objectReference = New-Object TypeName parameters
$objectReference = [TypeName]::new(parameters)
```

虽然类的静态方法和属性会生成对象，但你通常需要显式地自己创建它们。PowerShell 的 `New-Object` cmdlet 允许你创建指定类型的实例。参数列表必须与该类型的某个构造函数接受的参数列表相匹配，这在 SDK 文档中有说明。

例如：

```
$webClient = New-Object Net.WebClient
$webClient.DownloadString("http://search.msn.com")
```

如果该类型代表泛型类型，请将其类型参数放在方括号中：

```
PS > $hashtable = New-Object "System.Collections.Generic.Dictionary[String,Bool]"
PS > $hashtable["Test"] = $true
```

大多数常用类型默认可用。但是，许多类型只有在加载定义它们的库（称为 *程序集*）后才可用。类的 Microsoft 文档包含定义它的程序集。

若要加载程序集，请使用 `Add-Type` cmdlet 的 `-AssemblyName` 参数：

```
PS > Add-Type -AssemblyName System.Web
PS > [System.Web.HttpUtility]::UrlEncode("http://www.bing.com")
http%3a%2f%2fwww.bing.com
```

若要更新 PowerShell 默认搜索的命名空间列表，请在 `using` 语句中指定该命名空间：

```
PS > using namespace System.Web
PS > [HttpUtility]::UrlEncode("http://www.bing.com")
```

## 与 COM 对象交互

PowerShell 允许你以与 .NET Framework 对象交互相同的方式访问 COM 对象的方法和属性。要与 COM 对象交互，请在 `New-Object` 上使用其 `ProgId` 和 `-ComObject` 参数（通常简写为 `-Com`）：

```
PS > $shell = New-Object -Com Shell.Application
PS > $shell.Windows() | Select-Object LocationName,LocationUrl
```

有关系统管理员最常用的 COM 对象的详细信息，请参阅 附录 H。

## 扩展类型

PowerShell 支持两种向任何类型添加自定义方法和属性的方式：`Add-Member` cmdlet 和自定义类型扩展文件。

### Add-Member cmdlet

`Add-Member` cmdlet 允许你动态地向对象添加方法、属性等。它支持 表 A-13 中所示的扩展。

**表 A-13. `Add-Member` cmdlet 支持的选定成员类型**

| 成员类型 | 含义 |
| --- | --- |
| `AliasProperty` | 一个定义为另一个属性别名的属性： |
| | ```
PS > $testObject = [PsObject] "Test"
PS > $testObject | Add-Member "AliasProperty" Count Length
PS > $testObject.Count
4
``` |
| `CodeProperty` | 由 `System.Reflection.MethodInfo` 定义的属性。此方法必须是公共的、静态的、返回结果（非 void）且接受一个 `PsObject` 类型的参数。 |
| `NoteProperty` | 由你提供的初始值定义的属性： |
| | ```
PS > $testObject = [PsObject] "Test"
PS > $testObject | Add-Member NoteProperty Reversed tseT
PS > $testObject.Reversed
tseT
``` |
| `ScriptProperty` | 由你提供的脚本块定义的属性。在该脚本块中，`$this` 指代当前实例： |
| | ```
PS > $testObject = [PsObject] ("Hi" * 100)
PS > $testObject | Add-Member ScriptProperty IsLong {
      $this.Length -gt 100
   }
PS > $testObject.IsLong
True
``` |
| `PropertySet` | 定义为一组属性快捷方式的属性。用于 `Select-Object` 等 cmdlet： |
| | ```
$testObject = [PsObject] [DateTime]::Now
$collection = New-Object `
       Collections.ObjectModel.Collection``1[System.String]
$collection.Add("Month")
$collection.Add("Year")
$testObject | Add-Member PropertySet MonthYear $collection

PS > $testObject | select MonthYear

Month                                   Year
-----                                   ----
    3                                   2010
``` |
| `CodeMethod` | 由 `System.Reflection.MethodInfo` 定义的方法。此方法必须是公共的、静态的，且接受一个 `PsObject` 类型的参数。 |
| `ScriptMethod` | 由你提供的脚本块定义的方法。在该脚本块中，`$this` 指代当前实例，`$args` 指代输入参数： |
| | ```
PS > $testObject = [PsObject] "Hello"
PS > $testObject | Add-Member ScriptMethod IsLong {
      $this.Length -gt $args[0]
   }
PS > $testObject.IsLong(3)
True

PS > $testObject.IsLong(100)
False
``` |

### 自定义类型扩展文件

虽然 `Add-Member` cmdlet 允许你自定义单个对象，但 PowerShell 还支持配置文件，让你自定义给定类型的所有对象。例如，你可能想要向所有字符串添加 `Reverse()` 方法，或向所有类型添加 `HelpUrl` 属性（基于 MSDN URL 别名）。

PowerShell 将多个类型扩展添加到 PowerShell 安装目录下的 `types.ps1xml` 文件中。此文件可作为示例来源，但你不应直接修改它。相反，请创建一个新文件，并使用 `Update-TypeData` cmdlet 加载你的自定义项。以下命令从与你的配置文件相同的目录加载 `Types.custom.ps1xml`：

```
$typesFile = Join-Path (Split-Path $profile) "Types.Custom.Ps1Xml"
Update-TypeData -PrependPath $typesFile
```

有关自定义类型扩展文件的详细信息，请参阅 配方 3.16。

# 编写脚本，重用功能



当你想要开始打包并复用命令时，最佳的放置位置是 `scripts`、`functions` 和 `script blocks`。`script`（脚本）是一个包含一系列 PowerShell 命令的文本文件。`function`（函数）也是一系列 PowerShell 命令，但通常放置在脚本内部，以将其分解为更小、更易理解的片段。`script block`（脚本块）是一个没有名称的函数。这三者支持相同的功能，仅定义方式不同。

# 编写命令

## 编写脚本

要编写 `script`，请在文本编辑器中编写 PowerShell 命令，并使用 `*.ps1` 扩展名保存文件。

## 编写函数

函数允许你将紧密相关的命令块打包成一个可通过名称访问的单元。

```
function SCOPE:name(parameters)
{
   statement block
}
```

或：

```
filter SCOPE:name(parameters)
{
   statement block
}
```

有效的作用域名称包括 `global`（创建一个在整个 Shell 中可用的函数）、`script`（创建一个仅在当前脚本中可用的函数）、`local`（创建一个仅在当前作用域及子作用域中可用的函数）和 `private`（创建一个仅在当前作用域中可用的函数）。默认作用域为 `local` 作用域，其遵循与默认变量作用域相同的规则。

函数语句块的内容遵循与脚本内容相同的规则。函数支持 `$args` 数组、形式参数、`$input` 枚举器、Cmdlet 关键字、管道输出和等效的返回语义。

#### 注意

一个常见的错误是像调用方法一样调用函数：

```
$result = GetMyResults($item1, $item2)
```

PowerShell 对待函数的方式与对待脚本和其他命令相同，因此正确的写法应为：

```
$result = GetMyResults $item1 $item2
```

第一个命令将一个包含 `$item1` 和 `$item2` 的数组传递给 `GetMyResults` 函数。

筛选器只是一个函数，其语句被视为包含在 `process` 语句块中。有关 `process` 语句块的更多信息，请参阅“命令中的 Cmdlet 关键字”。

#### 注意

脚本中的命令只能访问已经定义的函数。当脚本开头完全由辅助函数组成时，这往往会使大型脚本难以理解。以下面这种方式构建脚本通常会使其更清晰：

```
function Main
{
   (...)
   HelperFunction
   (...)
}

function HelperFunction
{
   (...)
}

. Main
```

## 编写脚本块

```
$objectReference =
{
   statement block
}
```

PowerShell 支持 `script blocks`，其行为与未命名的函数和脚本完全相同。与脚本和函数一样，`script block` 语句块的内容遵循与函数或脚本内容相同的规则。脚本块支持 `$args` 数组、形式参数、`$input` 枚举器、Cmdlet 关键字、管道输出和等效的返回语义。

与脚本和函数一样，你可以调用或点源引用 `script block`。由于 `script block` 没有名称，你可以直接调用它（ `&` { `"Hello"`}）或调用包含它的变量（`&` `$objectReference`）。

# 运行命令

执行命令（`script`、`function` 或 `script block`）有两种方式：调用它或点源引用它。

## 调用

调用命令会运行其内部的命令。除非显式使用 `GLOBAL` 作用域关键字定义，否则脚本中定义的变量和函数在脚本退出后将不再保留。

#### 注意

默认情况下，PowerShell 中的一项称为执行策略的安全功能会阻止脚本运行。当你想要在 PowerShell 中启用脚本时，必须更改此设置。要了解可用的不同执行策略，请输入 `Get-Help about_signing`。选择执行策略后，使用 `Set-ExecutionPolicy` cmdlet 进行配置：

```
Set-ExecutionPolicy RemoteSigned
```

如果命令名称不包含空格，只需输入其名称：

```
c:\temp\Invoke-Commands.ps1 parameter1 parameter2 ...
Invoke-MyFunction parameter1 parameter2 ...
```

要作为后台作业运行命令，请使用后台运算符（`&`）：

```
c:\temp\Invoke-Commands.ps1 parameter1 parameter2 ... &
```

你可以使用完全限定路径或相对于当前位置的相对路径。如果脚本在当前目录中，必须显式指明：

```
.\Invoke-Commands.ps1 parameter1 parameter2 ...
```

如果命令名称包含空格（或命令没有名称，如 `script block` 的情况），则通过使用调用/调用运算符（`&`）并将命令名称作为参数来调用该命令。

```
& "C:\Script Directory\Invoke-Commands.ps1" parameter1 parameter2 ...
```

`Script blocks` 没有名称，因此将保存它们的变量放在调用运算符后面：

```
$scriptBlock = { "Hello World" }
& $scriptBlock parameter1 parameter2 ...
```

如果要在模块的上下文中调用命令，请提供对该模块的引用作为调用的一部分：

```
$module = Get-Module PowerShellCookbook
& $module Invoke-MyFunction parameter1 parameter2 ...
& $module $scriptBlock parameter1 parameter2 ...
```

## 点源引用

点源引用命令会运行其内部的命令。与简单调用命令不同，脚本中定义的变量和函数在脚本退出后*确实*会保留。

你可以通过使用点运算符（`.`）并提供命令名称作为参数来调用脚本：

```
. "C:\Script Directory\Invoke-Commands.ps1" Parameters
. Invoke-MyFunction parameters
. $scriptBlock parameters
```

点源引用脚本时，可以使用完全限定路径或相对于当前位置的相对路径。如果脚本在当前目录中，必须显式指明：

```
. .\Invoke-Commands.ps1 Parameters
```

如果要在模块的上下文中点源引用命令，请提供对该模块的引用作为调用的一部分：

```
$module = Get-Module PowerShellCookbook
. $module Invoke-MyFunction parameters
. $module $scriptBlock parameters
```

## 参数

需要或支持用户输入的命令通过参数实现。你可以使用 `Get-Command` cmdlet 查看命令支持的参数：

```
PS > Get-Command Stop-Process -Syntax

Stop-Process [-Id] <int[]> [-PassThru] [-Force] [-WhatIf] [-Confirm] [...]
Stop-Process -Name <string[]> [-PassThru] [-Force] [-WhatIf] [-Confirm] [...]
Stop-Process [-InputObject] <Process[]> [-PassThru] [-Force] [-WhatIf] [...]
```

在这种情况下，`Stop-Process` 命令支持的参数有 `Id`、`Name`、`InputObject`、`PassThru`、`Force`、`WhatIf` 和 `Confirm`。

要为参数提供值，请使用短划线字符，后跟参数名称，然后是一个空格，最后是参数值。

```
Stop-Process -Id 1234
```

如果参数值包含空格，请用引号将其括起来：

```
Stop-Process -Name "Process With Spaces"
```

如果变量包含你想用作参数值的值，请通过 PowerShell 的常规变量引用语法提供：

```
$name = "Process With Spaces"
Stop-Process -Name $name
```

如果你想使用其他 PowerShell 语言元素作为参数值，请用括号将该值括起来：

```
Get-Process -Name ("Power" + "Shell")
```

你只需提供足以将其与其他参数区分开来的参数名称部分即可。

```
Stop-Process -N "Process With Spaces"
```

如果命令的语法显示参数名在方括号中（如 `[-Id]`），则该参数为*位置参数*，你可以省略参数名仅提供值。PowerShell 会根据位置顺序将这些未命名的值提供给参数。

```
Stop-Process 1234
```




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





# 参数验证特性

## ValidatePattern

`[ValidatePattern("regular expression")]`

强制要求传入此字符串参数的输入必须匹配某个模式。如果未指定，字符串输入则没有模式要求。你可以将其应用于非字符串参数，但不会产生任何影响。

## ValidateRange

`[ValidateRange(lower limit, upper limit)]`

限制此参数可接受的数值参数的上限和下限。如果未指定，参数则没有范围限制。你可以将其应用于非数字参数，但不会产生任何影响。

## ValidateScript

`[ValidateScript({ script block })]`

确保传递给此参数的输入满足你在脚本块中提供的条件。PowerShell 将提议的输入赋值给 `$_`（或 `$PSItem`）变量，然后调用你的脚本块。如果脚本块返回 `$true`（或任何可转换为 `$true` 的值，例如非空字符串），PowerShell 会认为验证成功。

## ValidateSet

`[ValidateSet("First Option", "Second Option", …, "Last Option")]`

确保传递给此参数的输入等于集合中的某个选项。PowerShell 在此比较过程中使用其标准的相等含义：即 `-eq` 运算符使用的相同规则。如果你的验证需要非标准规则（例如字符串的区分大小写比较），可以改在脚本或函数主体中编写验证逻辑。

## ValidateNotNull

`[ValidateNotNull()]`

确保传递给此参数的输入不为 null。这是强制参数的默认行为，因此此特性仅对可选参数有用。当应用于字符串参数时，`$null` 参数值会被转换为空字符串。

## ValidateNotNullOrEmpty

`[ValidateNotNullOrEmpty()]`

确保传递给此参数的输入不为 null 或空。这是强制参数的默认行为，因此此特性仅对可选参数有用。当应用于字符串参数时，输入必须是长度大于一的字符串。当应用于集合参数时，集合必须至少包含一个元素。当应用于其他类型的参数时，此特性等同于 `[ValidateNotNull()]` 特性。

# 管道输入

要访问通过管道传递给命令的数据，请使用 PowerShell 放置在 `$input` 特殊变量中的输入枚举器：

```
foreach($element in $input)
{
   "Input was: $element"
}
```

`$input` 变量是管道输入的 .NET 枚举器。枚举器能非常高效地支持流式处理场景，但不像数组那样允许你访问任意元素。如果想再次处理这些元素，到达末尾后必须在 `$input` 枚举器上调用 `Reset()` 方法。

如果你需要以非结构化方式访问管道输入，请使用以下命令将输入枚举器转换为数组：

```
$inputArray = @($input)
```

# 命令中的 Cmdlet 关键字

当管道输入是命令的核心场景时，可以包含标记为 `begin`、`process` 和 `end` 的语句块：

```
param(...)

begin
{
   ...
}
process
{
   ...
}
end
{
   ...
}
```

PowerShell 在加载命令时执行 `begin` 语句，为管道传递的每个项目执行 `process` 语句，在处理完所有管道输入后执行 `end` 语句。在 `process` 语句块中，`$_`（或 `$PSItem`）变量代表当前的管道对象。

编写包含这些关键字的命令时，脚本中的所有命令都必须包含在语句块内。

# $MyInvocation 自动变量

`$MyInvocation` 自动变量包含脚本运行时的上下文信息，包括关于命令（`MyCommand`）、定义它的脚本（`ScriptName`）等的详细信息。

# 从命令检索输出

PowerShell 提供了三种从命令检索输出的主要方式。

## 管道输出

```
any command
```

脚本的返回值/输出是它生成但未捕获的任何数据。如果命令包含：

```
"Text Output"
5*5
```

然后将该命令的输出赋值给变量，将创建一个包含 `Text Output` 和 `25` 这两个值的数组。

## Return 语句

```
return value
```

以下语句：

```
return $false
```

只是管道输出的简写形式：

```
$false
return
```

## Exit 语句

```
exit errorlevel
```

`exit` 语句从当前命令或 PowerShell 实例返回错误代码。如果在脚本中的任何位置调用（内联、在函数中或在脚本块中），它将退出脚本。如果在脚本外部调用（例如函数），它将退出 PowerShell。`exit` 语句将 `$LastExitCode` 自动变量设置为 `errorLevel`。反过来，如果 `errorLevel` 不为零，则将 `$?` 自动变量设置为 `$false`。

> **注意**
>
> 输入 `Get-Help about_automatic_variables` 以获取有关自动变量的详细信息。

# 错误管理

PowerShell 支持两类错误：*非终止性* 和 *终止性*。它将这两类错误作为列表收集在 `$error` 自动变量中。

## 非终止性错误

大多数错误都是 *非终止性错误*，即它们不会停止当前 cmdlet、脚本、函数或管道的执行。当命令输出错误（通过 PowerShell 的错误输出机制）时，PowerShell 会将该错误写入称为 *错误输出流* 的流中。

可以使用 `Write-Error` cmdlet（编写 cmdlet 时使用 `WriteError()` API）输出非终止性错误。

`$ErrorActionPreference` 自动变量让你控制 PowerShell 如何处理非终止性错误。它支持以下值，如表 A-14 所示。

**表 A-14. `ErrorActionPreference` 自动变量值**

| 值 | 含义 |
| --- | --- |
| `Ignore` | 不显示错误，也不将其添加到 `$error` 集合中。仅在提供给命令的 `ErrorAction` 参数时支持。 |
| `SilentlyContinue` | 不显示错误，但将其添加到 `$error` 集合中。 |
| `Stop` | 将非终止性错误视为终止性错误。 |
| `Continue` | 显示错误，但继续执行当前 cmdlet、脚本、函数或管道。这是默认值。 |
| `Inquire` | 显示提示，询问 PowerShell 应如何处理此错误。 |

大多数 cmdlet 允许你通过将这些值之一传递给 `ErrorAction` 参数来显式配置此行为。

## 终止性错误

*终止性错误* 会停止当前 cmdlet、脚本、函数或管道的执行。如果命令（如 cmdlet 或 .NET 方法调用）生成结构化异常（例如，向方法提供超出有效范围的参数），PowerShell 会将其作为终止性错误呈现。如果 PowerShell 无法解析脚本、函数或管道的某个元素，也会生成终止性错误。

可以在脚本中使用 `throw` 关键字生成终止性错误：

```
throw message
```

> **注意**
>
> 在你自己的脚本和 cmdlet 中，仅当操作的根本意图无法实现时才生成终止性错误。例如，无法在远程服务器上执行命令应视为非终止性错误，而完全无法连接到远程服务器则应视为终止性错误。

可以通过 `try`、`catch` 和 `finally` 语句拦截终止性错误，这与许多其他编程语言支持的方式相同。




# 错误处理：`try` / `catch` / `finally` 和 `trap`

```powershell
try
{
    statement block
}
catch [exception type]
{
    error handling block
}
catch [alternate exception type]
{
    alternate error handling block
}
finally
{
    cleanup block
}
```

在 `try` 语句之后，必须提供 `catch` 语句、`finally` 语句或两者兼有。如果指定了异常类型（这是可选的），则可以指定多个 `catch` 语句来处理不同类型的异常。如果指定了异常类型，则 `catch` 块仅适用于该类型的终止错误。

如果在 PowerShell 遇到终止错误之前定义了 `trap` 语句，PowerShell 还允许您拦截终止错误：

```powershell
trap [exception type]
{
   statement block
   [continue or break]
}
```

如果指定了异常类型，则 `trap` 语句仅适用于该类型的终止错误。

在 `catch` 块或 `trap` 语句中，`$_`（或 `$PSItem`）变量代表正在处理的当前异常或错误。

如果指定了 `continue` 关键字，则会告诉 PowerShell 在遇到终止错误的位置之后继续处理脚本、函数或管道。

如果指定了 `break` 关键字，则会告诉 PowerShell 在遇到终止错误的位置之后停止处理脚本、函数或管道的其余部分。默认模式为 `break`，如果既未指定 `break` 也未指定 `continue`，则应用此模式。

# 格式化输出

```text
Pipeline | Formatting Command
```

当对象到达输出管道的末尾时，PowerShell 会将它们转换为文本，以使其适合人类阅读。PowerShell 支持多种选项来帮助您控制此格式化过程，如表 A-15 所列。

## 表 A-15. PowerShell 格式化命令

### `Format-Table`

将输入对象的属性格式化为表格，仅包含您指定的对象属性。如果未指定属性列表，PowerShell 会选取一组默认属性。

除了提供对象属性外，您还可以提供高级格式化语句：

```powershell
PS > Get-Process | `
   Format-Table -Auto Name,`
   @{Label="HexId";
     Expression={ "{0:x}" -f $_.Id}
     Width=4
     Align="Right"
    }
```

高级格式化语句是一个包含 `Label` 和 `Expression` 键（或其任意简写形式）的哈希表。Expression 键的值应为一个脚本块，该脚本块为当前对象（由 `$_` 变量表示）返回结果。

有关 `Format-Table` cmdlet 的详细信息，请输入 `Get-Help Format-Table`。

### `Format-List`

将输入对象的属性格式化为列表，仅包含您指定的对象属性。如果未指定属性列表，PowerShell 会选取一组默认属性。

`Format-List` cmdlet 支持 `Format-Table` cmdlet 所使用的高级格式化语句。

`Format-List` cmdlet 是您最常用来获取对象属性详细摘要的命令。

命令 `Format-List *` 返回所有属性，但不包括 PowerShell 默认隐藏的属性。命令 `Format-List * -Force` 返回所有属性。

有关 `Format-List` cmdlet 的详细信息，请输入 `Get-Help Format-List`。

### `Format-Wide`

以极其简洁的摘要视图格式化输入对象的属性。如果未指定属性，PowerShell 会选取一个默认属性。

除了提供对象属性外，您还可以提供高级格式化语句：

```powershell
PS > Get-Process | `
   Format-Wide -Auto `
   @{ Expression={ "{0:x}" -f $_.Id} }
```

高级格式化语句是一个包含 `Expression` 键（或其任意简写形式）的哈希表。Expression 键的值应为一个脚本块，该脚本块为当前对象（由 `$_` 变量表示）返回结果。

有关 `Format-Wide` cmdlet 的详细信息，请输入 `Get-Help Format-Wide`。

## 自定义格式化文件

PowerShell 中的所有格式化默认值（例如，当您未指定格式化命令或未指定格式化属性时）均由安装目录中的 `*.Format.Ps1Xml` 文件驱动，其方式类似于配方 3.16 中提到的类型扩展文件。

要创建自己的格式化自定义项，请将这些文件作为示例来源，但不要直接修改它们。相反，请创建一个新文件，并使用 `Update-FormatData` cmdlet 加载您的自定义项。`Update-FormatData` cmdlet 将更改应用于当前 PowerShell 实例。如果您希望每次启动 PowerShell 时都加载它们，请在配置文件脚本中调用 `Update-FormatData`。以下命令从与配置文件相同的目录加载 `Format.custom.ps1xml`：

```powershell
$formatFile = Join-Path (Split-Path $profile) "Format.Custom.Ps1Xml"
Update-FormatData -PrependPath $typesFile
```

要在不使用格式化文件的情况下添加格式化信息，请参阅配方 3.17。

# 捕获输出

在 PowerShell 中有多种方法可以捕获命令的输出，如表 A-16 所列。

## 表 A-16. 在 PowerShell 中捕获输出

| 命令 | 结果 |
| --- | --- |
| `$variable = Command` | 将 PowerShell 命令生成的对象存储到 `$variable` 中。 |
| `$variable = Command \| Out-String` | 将 PowerShell 命令的可视化表现存储到 `$variable` 中。这是转换为人类可读输出后的 PowerShell 命令。 |
| `$variable = NativeCommand` | 将本机命令的（字符串）输出存储到 `$variable` 中。PowerShell 将其作为字符串列表存储——每行输出对应一个字符串。 |
| `Command -OutVariable variable` | 对于大多数命令，将 PowerShell 命令生成的对象存储到 `$variable` 中。参数 `-OutVariable` 也可以写作 `-Ov`。 |
| `Command > File` | 将 PowerShell 的可视化表现（或本机命令的标准输出）重定向到 `File`，如果 `File` 存在则覆盖。错误不会被此重定向捕获。 |
| `Command >> File` | 将 PowerShell 的可视化表现（或本机命令的标准输出）重定向到 `File`，如果 `File` 存在则追加。错误不会被此重定向捕获。 |
| `Command 2> File` | 将 PowerShell 或本机命令的错误重定向到 `File`，如果 `File` 存在则覆盖。 |
| `Command n> File` | 将流编号 `n` 重定向到 `File`，如果 `File` 存在则覆盖。支持的流包括：`2` 代表错误，`3` 代表警告，`4` 代表详细，`5` 代表调试，`6` 代表结构化信息流，`*` 代表所有。 |
| `Command 2>> File` | 将 PowerShell 或本机命令的错误重定向到 `File`，如果 `File` 存在则追加。 |
| `Command n>> File` | 将流编号 `n` 重定向到 `File`，如果 `File` 存在则追加。支持的流包括：`2` 代表错误，`3` 代表警告，`4` 代表详细，`5` 代表调试，`6` 代表结构化信息流，`*` 代表所有。 |
| `Command > File 2>&1` | 将 PowerShell 或本机命令的错误流和标准输出流都重定向到 `File`，如果 `File` 存在则覆盖。 |
| `Command >> File 2>&1` | 将 PowerShell 或本机命令的错误流和标准输出流都重定向到 `File`，如果 `File` 存在则追加。 |

虽然 `Write-Host` cmdlet 的输出通常直接发送到屏幕，但您可以使用结构化信息流将其捕获到变量中。




```powershell
PS > function HostWriter { Write-Host "Console Output" }
PS > $a = HostWriter
Console Output
PS > $a
PS > $a = HostWriter 6>&1
PS > $a
Console Output
```

# 常见自定义点

尽管开箱即用非常有用，但 PowerShell 还提供了多种自定义和个性化的途径。

## 控制台设置

Windows PowerShell UI 提供了多项功能，使您的 Shell 体验更加高效。

### 调整字体大小

Windows Terminal 应用程序和默认的 Windows 控制台都允许您调整字体大小。

要临时更改字体大小，请按住 Ctrl 键并使用鼠标向上或向下滚动。在 Windows Terminal 应用程序中，您也可以使用 Ctrl+Plus 或 Ctrl+Minus 热键。在 Windows Terminal 应用程序中，Ctrl+0 可将字体大小重置回默认值。

要在默认的 Windows 控制台中更改默认字体大小，请打开系统菜单（右键单击控制台窗口左上角的标题栏），选择“属性”→“字体”。如果从“开始”菜单启动 Windows PowerShell，它将以对字体和窗口大小的一些默认修改启动。要在 Windows Terminal 应用程序中更改默认字体大小，请向任意终端配置文件添加 `fontSize` 设置：

```json
{
    "guid": "...",
    "name": "PowerShell (Demos)",
    "fontSize": 18,
    "colorScheme": "Campbell Powershell",
    "source": "Windows.Terminal.PowershellCore"
}
```

### 调整其他 Windows Terminal 设置

Windows Terminal 应用程序包含大量配置设置。其中包括：

*   配置可用 Shell 和应用程序的列表（如 `bash.exe`）
*   配色方案和 UI 主题
*   将操作绑定到热键
*   文本选择行为
*   窗口透明度
*   背景图像

有关这些设置的完整列表，请参阅 Windows Terminal 中 [全局设置](https://aka.ms/terminal-global-settings) 和 [常规配置文件设置](https://aka.ms/terminal-profile-settings) 的文档。

### 使用热键更高效地操作 Shell

PowerShell 控制台支持许多热键，有助于提高控制台操作效率，如 **表 A-17** 所示。

**表 A-17. PowerShell 热键**

| 热键 | 含义 |
| --- | --- |
| 按下并释放 Windows 键，然后输入 `pwsh` 或 `powershell` | 启动 PowerShell 或 Windows PowerShell。`Win+X` 热键也提供了一种快速启动 Windows PowerShell 的方式。 |
| 上箭头 | 向后浏览命令历史记录。 |
| 下箭头 | 向前浏览命令历史记录。 |
| 左箭头 | 将光标在命令行上向左移动一个字符。 |
| 右箭头 | 将光标在命令行上向右移动一个字符。如果在行尾，则在该位置插入上一条命令文本中的一个字符。 |
| Ctrl+左箭头 | 将光标在命令行上向左移动一个单词。 |
| Ctrl+右箭头 | 将光标在命令行上向右移动一个单词。 |
| Home 键 | 将光标移至命令行开头。 |
| End 键 | 将光标移至命令行末尾。 |
| Ctrl+Shift+PgUp, Ctrl+Shift+PgDn | 在 Windows Terminal 应用程序中，滚动屏幕缓冲区。在 Windows 控制台中，可使用 PgUp 和 PgDn。 |
| Ctrl+Shift+F | 在 Windows Terminal 应用程序中，搜索屏幕缓冲区中的文本。在 Windows 控制台中，可使用 Alt+Space E F。 |
| Alt+Space E K | 在 Windows 控制台中，选择要从屏幕缓冲区复制的文本。有关另一种方法，请参阅 配方 1.10。 |
| Ctrl+C | 取消当前操作。如果选中了文本，Ctrl+C 会将该文本复制到剪贴板。 |
| Ctrl+V | 粘贴剪贴板内容。 |
| Ctrl+Shift+T | 在 Windows Terminal 应用程序中，打开新标签页。也可使用 Ctrl+Shift+1、Ctrl+Shift+2 等打开对应编号的配置文件标签页（如 `bash.exe`）。 |
| Ctrl+Shift+W, Alt+F4 | 在 Windows Terminal 应用程序中，关闭当前标签页或整个应用程序。在 Windows 控制台中，可使用 Alt+Space C 关闭整个应用程序。 |
| Ctrl+Break | 在 Windows 控制台中，中断 PowerShell 调试器进入当前正在运行的脚本。 |
| Ctrl+Home | 删除从当前命令行开头到（但不包括）当前光标位置的字符。 |
| Ctrl+End | 删除从当前光标位置（包括该位置）到当前命令行末尾的字符。 |
| Ctrl+Z, Ctrl+Y | 撤销和重做。 |
| F8 | 向后浏览命令历史，仅显示与命令行上已输入文本匹配的命令。 |
| Ctrl+R | 根据交互式输入的文本，开始在命令历史中交互式地向后搜索。 |

> **注意**
>
> PowerShell 通过 `PSReadLine` 模块提供的命令行编辑体验比此表列出的要丰富得多。它包含 Emacs 和 Vi 按键绑定，以及定义自己的功能——您可以通过输入 `Get-PSReadLineKeyHandler` 查看完整的默认列表。有关详细信息，请参阅 配方 1.10。

## 配置文件

PowerShell 在启动期间会自动运行 **表 A-18** 中列出的四个脚本。如果存在，每个脚本都允许您自定义执行环境。PowerShell 运行您放入这些文件中的任何内容，就如同您在命令行中手动输入一样。

**表 A-18. PowerShell 配置文件**

| 配置文件用途 | 配置文件位置 |
| --- | --- |
| 自定义所有 PowerShell 会话，包括系统上所有用户的 PowerShell 宿主应用程序 | `InstallationDirectory\profile.ps1` |
| 自定义系统上所有用户的 `pwsh.exe` 会话 | `InstallationDirectory\Microsoft.PowerShell_profile.ps1` |
| 自定义所有 PowerShell 会话，包括 PowerShell 宿主应用程序 | `<My Documents>\PowerShell\profile.ps1` |
| `pwsh.exe` 会话的典型自定义 | `<My Documents>\PowerShell\Microsoft.PowerShell_profile.ps1` |

在 Windows PowerShell 中，其中一些位置会有所不同。

PowerShell 通过定义自动变量 `$profile` 使编辑配置文件脚本变得简单。单独使用时，它指向“当前用户、 `pwsh.exe`”配置文件。此外，`$profile` 变量定义了指向其他配置文件位置的附加属性：

```powershell
PS > $profile | Format-List -Force

AllUsersAllHosts       : C:\...Microsoft.PowerShell...\profile.ps1
AllUsersCurrentHost    : C:\...Microsoft.PowerShell...\Microsoft.PowerShell_profile.ps1
CurrentUserAllHosts    : D:\Lee\PowerShell\profile.ps1
CurrentUserCurrentHost : D:\Lee\PowerShell\Microsoft.PowerShell_profile.ps1
```

要创建新配置文件，请输入：

```powershell
New-Item -Type file -Force $profile
```

要编辑此配置文件，请输入：

```powershell
notepad $profile
```

## 提示符

要自定义提示符，请将 `prompt` 函数添加到您的配置文件中。此函数返回一个字符串。例如：

```powershell
function prompt
{
    "PS [$env:COMPUTERNAME] >"
}
```

有关自定义提示符的详细信息，另请参阅 配方 1.9。

## 制表符补全

您可以定义 `TabExpansion2` 函数来自定义按 Tab 键时 PowerShell 补全属性、变量、参数和文件的方式。

不过，您的 `TabExpansion` 函数会覆盖 PowerShell 默认定义的函数，因此您可能想要以其定义为起点：

```powershell
Get-Content function:\TabExpansion2
```

有关自定义制表符扩展的详细信息，请参阅 配方 1.18。

## 用户输入




您可以定义 `PSConsoleHostReadLine` 函数，以自定义 PowerShell 控制台主机（而非集成脚本环境 [ISE]）读取用户输入的方式。此函数负责处理用户的所有按键，并最终返回 PowerShell 应调用的命令。

有关重写用户输入的详细信息，请参阅 食谱 1.10。

## 命令解析

您可以通过将脚本块分配给 `$executionContext.SessionState.InvokeCommand` 的 `PreCommandLookupAction`、`PostCommandLookupAction` 或 `CommandNotFoundAction` 属性（可以是其中一个或全部），在三个位置拦截 PowerShell 的命令解析行为。

PowerShell 在用户键入命令名称后、但在尝试解析命令之前调用 `PreCommandLookupAction`。一旦解析了命令、但在执行命令之前，它会调用 `PostCommandLookupAction`。当找不到命令、但在生成错误消息之前，它会调用 `CommandNotFoundAction`。每个脚本块接收两个参数：`CommandName` 和 `CommandLookupEventArgs`。

```
$executionContext.SessionState.InvokeCommand.CommandNotFoundAction = {
    param($CommandName, $CommandLookupEventArgs)

    (...)
}
```

如果您的脚本块将脚本块分配给 `CommandLookupEventArgs` 的 `CommandScriptBlock` 属性，或将 `CommandInfo` 分配给 `CommandLookupEventArgs` 的 `Command` 属性，PowerShell 将分别使用该脚本块或命令。如果您的脚本块将 `StopSearch` 属性设置为 `true`，PowerShell 将不再进行进一步的命令解析。

有关重写用户输入的详细信息，请参阅 食谱 1.11。
