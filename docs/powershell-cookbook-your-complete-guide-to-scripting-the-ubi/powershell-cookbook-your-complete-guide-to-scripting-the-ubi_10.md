# 第 6 章 计算与数学

## 6.0 简介

数学是任何脚本语言的重要特性。语言的数学支持当然包括加、减、乘、除，但也延伸至更高级的数学运算。因此，PowerShell 提供了一套强大的数学和面向计算的功能，这不足为奇。

由于 PowerShell 从命令行提供了对其脚本语言的完全访问，这让一个强大且实用的命令行计算器始终触手可及！除了支持传统的数学运算外，PowerShell 还通过原生支持兆字节和吉字节等概念、简单统计（如求和与平均值）以及进制间的转换，来满足系统管理员的需求。

## 6.1 执行简单算术运算

### 问题

你想使用 PowerShell 计算简单的数学结果。

### 解决方案

使用 PowerShell 的算术运算符：

| 运算符 | 说明 |
| --- | --- |
| + | 加法 |
| - | 减法 |
| * | 乘法 |
| / | 除法 |
| % | 取模 |
| +=, -=, *=, /=, %= | 上述运算符的赋值变体 |
| () | 优先级/运算顺序 |

有关这些数学运算符的详细说明，请参阅“简单运算符”。

### 讨论

许多编程语言的一个难点在于它们处理变量中数据的方式。例如，这段 C# 代码片段将值 1 存储在 `result` 变量中，而用户可能希望结果保存浮点值 1.5：

```
double result = 0;
result = 3/2;
```

这是因为 C#（以及许多其他语言）根据除法中使用的数据类型来决定除法的结果。在前面的示例中，它判定你想要一个整数答案，因为你在除法中使用了两个整数。

另一方面，PowerShell 避免了这个问题。即使你在除法中使用两个整数，如果需要，PowerShell 也会将结果作为浮点数返回。这称为*扩展*。

```
PS > $result = 0
PS > $result = 3/2
PS > $result
1.5
```

这一自动扩展的一个例外是当你显式告诉 PowerShell 你想要的结果类型时。例如，你可能会使用整数转换（`[int]`）来表示你毕竟想要结果为整数：

```
PS > $result = [int] (3/2)
PS > $result
2
```

许多编程语言在将浮点数转换为整数时，会丢弃小数点后的部分。这称为*截断*。另一方面，PowerShell 对此转换使用*银行家舍入法*。它将浮点数转换为最接近的整数，遇到中间值时舍入到最近的偶数。




不过，有几种编程技术使用截断，因此脚本语言以某种方式支持它仍然很重要。PowerShell 没有内置的执行截断式除法的运算符，但它确实通过 .NET Framework 中的 `[Math]::Truncate()` 方法支持它：

```
PS > $result = 3/2
PS > [Math]::Truncate($result)
1
```

如果该语法显得繁琐，以下示例定义了一个 `trunc` 函数来截断其输入：

```
PS > function trunc($number) { [Math]::Truncate($number) }
PS > $result = 3/2
PS > trunc $result
1
```

## 另请参阅

“简单运算符”

# 6.2 执行复杂算术运算

## 问题

你想要使用 PowerShell 计算更复杂或高级的数学结果。

## 解决方案

PowerShell 主要通过其对 .NET Framework 中 `System.Math` 类的支持来支持更高级的数学任务。

要查找数字的绝对值，请使用 `[Math]::Abs()` 方法：

```
PS > [Math]::Abs(-10.6)
10.6
```

要查找数字的幂（如平方或立方），请使用 `[Math]::Pow()` 方法。在本例中，该方法计算 123 的平方：

```
PS > [Math]::Pow(123, 2)
15129
```

要查找数字的平方根，请使用 `[Math]::Sqrt()` 方法：

```
PS > [Math]::Sqrt(100)
10
```

要查找角度（以弧度为单位）的正弦、余弦或正切，请使用 `[Math]::Sin()`、`[Math]::Cos()` 或 `[Math]::Tan()` 方法：

```
PS > [Math]::Sin( [Math]::PI / 2 )
1
```

要查找正弦、余弦或正切值对应的角度（以弧度为单位），请使用 `[Math]::ASin()`、`[Math]::ACos()` 或 `[Math]::ATan()` 方法：

```
PS > [Math]::ASin(1)
1.5707963267949
```

请参阅 食谱 3.12 以了解如何发现 `System.Math` 类提供的其他功能。

## 讨论

一旦你开始使用 `System.Math` 类，可能会觉得其设计者遗漏了重要功能。该类支持数字的平方根，但不支持其他根（如立方根）。它支持以弧度为单位的正弦、余弦和正切（及其反函数），但不支持更常用的度数单位。

### 处理任意根

要确定数字的任意根（如立方根），可以使用 示例 6-1 中给出的函数。

#### 示例 6-1. 根函数及一些示例计算

```
PS > function root($number, $root) { [Math]::Pow($number, 1 / $root) }
PS > root 64 3
4
PS > root 25 5
1.90365393871588
PS > [Math]::Pow(1.90365393871588, 5)
25.0000000000001
PS > [Math]::Pow( $(root 25 5), 5)
25
```

此函数应用了一个数学事实：数字的平方根等同于将该数字提升到 1/2 次幂，立方根等同于提升到 1/3 次幂，以此类推。

该示例还阐释了计算机数学中一个非常重要的观点。当你使用此函数（或任何操作浮点数的代码）时，请始终注意浮点运算的结果仅仅是实际结果的近似值。如果你在同一语句中组合多个计算（或将中间结果存储到变量中），编程和脚本语言有时能保持准确答案（如第二个 `[Math]::Pow()` 尝试所示），但这种情况很少见。

一些数学系统通过将方程和计算作为符号（而非数字）来处理，从而避免了这个问题。像人类一样，这些系统知道对一个刚开过平方根的数字进行平方会得到原来的数字——所以它们根本不必执行这两种运算。然而，这些系统极其专业化，通常也非常昂贵。

### 使用度数而非弧度

将弧度（数学家常用的角度单位）转换为度数（大多数人常用的角度单位）比根函数简单得多。如果以弧度测量，圆周为 `2 * Pi` 弧度；如果以度数测量，则为 `360` 度。这给出了以下两个函数：

```
function Convert-RadiansToDegrees($angle) { $angle / (2 * [Math]::Pi) * 360 }
function Convert-DegreesToRadians($angle) { $angle / 360 * (2 * [Math]::Pi) }
```

及其用法：

```
PS > Convert-RadiansToDegrees ([Math]::Pi)
180
PS > Convert-RadiansToDegrees ([Math]::Pi / 2)
90
PS > Convert-DegreesToRadians 360
6.28318530717959
PS > Convert-DegreesToRadians 45
0.785398163397448
PS > [Math]::Tan( (Convert-DegreesToRadians 45) )
1
```

### 处理大数字

除了支持所有标准 .NET 数据类型（字节、整数、浮点数和十进制数）外，PowerShell 还允许你处理这些标准数据类型无法处理的极大数字：

```
PS > [Math]::Pow(12345, 123)
Infinity

PS > [BigInt]::Pow(12345, 123)
17922747853679707527695216231943419712992696443062340535140391466684
40953031931423861053031289352606613314821666096691426463815891552569
61299625923906846736377224598990446854741893321648522851663303862851
16587975372427272838604280411617304001701448802369380754772495091658
80584554994292720483269340987503673640044881128194397555564034430275
23561951313385041616743787240003466700321402142800004483416756392021
35945746171990585436418152506177298295938033884123488041067995268917
9117442108690738677978515625
```

除了 `BigInt` 类提供的静态方法外，你还可以使用 `n` 数字字面量后缀直接对大整数进行标准数学运算（加、减、乘、除）：

```
PS > $num1 = 962822088399213984108510902933777372323n
PS > $num2 = 986516486816816168176871687167106806788n
PS > $num1 * $num2
949839864077222593647087206583370147511597229917261205272142276616785899728524
```

作为一个重要提示，在使用 `BigInt` 数字时，请务必始终使用 `n` 数字字面量后缀（或将 `BigInt` 数字放在字符串中，然后将其转换为 `BigInt` 类型）。如果不这样做，PowerShell 会认为你试图提供 `Double` 类型的数字（这会导致极大数字丢失数据），然后将该数字转换为大整数。

```
PS > $r = 962822088399213984108510902933777372323
PS > $r
9.62822088399214E+38

PS > [BigInt] $r
962822088399213912109618944997163270144

PS > [BigInt] 962822088399213984108510902933777372323
962822088399213912109618944997163270144

PS > [BigInt] "962822088399213984108510902933777372323"
962822088399213984108510902933777372323
```

### 处理虚数和复数

当你需要处理涉及 -1 的平方根的计算时，`System.Numerics.Complex` 类提供了大量支持：

```
PS > [System.Numerics.Complex]::ImaginaryOne | Format-List

Real      : 0
Imaginary : 1
Magnitude : 1
Phase     : 1.5707963267949
```

除了 `Complex` 类提供的静态方法外，你还可以直接对复数进行标准数学运算（加、减、乘、除）：

```
PS > [System.Numerics.Complex]::ImaginaryOne *
    [System.Numerics.Complex]::ImaginaryOne | Format-List

Real      : -1
Imaginary : 0
Magnitude : 1
Phase     : 3.14159265358979
```

## 另请参阅

食谱 3.12，“了解类型和对象”

# 6.3 测量列表的统计属性

## 问题

你想要测量对象列表的数值（最小值、最大值、总和、平均值）或文本（字符、单词、行数）特征。

## 解决方案

使用 `Measure-Object` cmdlet 来测量列表的这些统计属性。

要测量对象流的数值特征，请将这些对象通过管道传递给 `Measure-Object` cmdlet：



# 6.4 以二进制方式处理数字

## 问题

你想要处理数字的各个位，或者处理一个通过组合一系列标志构建的数字。

## 解决方案

要直接输入十六进制数字，请使用 `0x` 前缀：

```
PS > $hexNumber = 0x1234
PS > $hexNumber
4660
```

要将数字转换为其二进制表示形式，请向 `[Convert]::ToString()` 方法提供以 `2` 为基数：

```
PS > [Convert]::ToString(1234, 2)
10011010010
```

要将二进制数字转换为其十进制表示形式，请使用二进制前缀 `0b`：

```
$myBinary = 0b10011010010
```

如果你拥有字符串形式的值，请向 `[Convert]::ToInt32()` 方法提供以 `2` 为基数：

```
PS > [Convert]::ToInt32("10011010010", 2)
1234
```

要管理数字的各个位，请使用 PowerShell 的二元运算符。在本例中，`Archive` 标志只是给定文件可能为真的众多属性之一：

```
PS > $archive = [System.IO.FileAttributes] "Archive"
PS > attrib +a test.txt
PS > Get-ChildItem | Where { $_.Attributes -band $archive } | Select Name

Name
----
test.txt
PS > attrib -a test.txt
PS > Get-ChildItem | Where { $_.Attributes -band $archive } | Select Name
PS >
```

## 讨论

在某些系统管理任务中，遇到本身似乎毫无意义的数字是很常见的。文件的属性就是一个完美的例子：

```
PS > (Get-Item test.txt).Encrypt()
PS > (Get-Item test.txt).IsReadOnly = $true
PS > [int] (Get-Item test.txt -force).Attributes
16417
PS > (Get-Item test.txt -force).IsReadOnly = $false
PS > (Get-Item test.txt).Decrypt()
PS > [int] (Get-Item test.txt).Attributes
32
```

数字 `16417` 和 `32` 可能告诉我们关于该文件的什么信息？

答案在于从另一个角度审视这些属性——将它们视为一组可以为真或为假的特性。例如，示例 6-3 所示为目录中的项目可能的属性。

### 示例 6-3. 文件的可能属性

```
PS > [Enum]::GetNames([System.IO.FileAttributes])
ReadOnly
Hidden
System
Directory
Archive
Device
Normal
Temporary
SparseFile
ReparsePoint
Compressed
Offline
NotContentIndexed
Encrypted
IntegrityStream
NoScrubData
```

如果一个文件是 `ReadOnly`（只读）、`Archive`（存档）和 `Encrypted`（加密），那么你可以将以下内容视为该文件属性的简明描述：

```
ReadOnly = True
Archive = True
Encrypted = True
```

计算机恰好有一种极其简洁的方式来表示真/假值的集合——一种称为 *二进制* 的表示法。要以二进制表示目录项目的属性，只需将它们放入表格中。如果属性适用于该项目，我们给它一个 1，否则给 0（请参见表 6-1）。

**表 6-1. 目录项目的属性**

| 属性 | 真 (1) 或假 (0) |
| --- | --- |
| `Encrypted` | 1 |
| `NotContentIndexed` | 0 |
| `Offline` | 0 |
| `Compressed` | 0 |
| `ReparsePoint` | 0 |
| `SparseFile` | 0 |
| `Temporary` | 0 |
| `Normal` | 0 |
| `Device` | 0 |
| `Archive` | 1 |
| `Directory` | 0 |
| <Unused> | 0 |
| `System` | 0 |
| `Hidden` | 0 |
| `ReadOnly` | 1 |

如果我们将这些特性视为数字中的各个二进制位，就会得到数字 `100000000100001`。如果我们将该数字转换为十进制形式，就能清楚地看出数字 `16417` 的来源：

```
PS > 0b100000000100001
16417
```

这项技术是许多可以表示为特性或标志组合的属性的核心。不过，文档通常不是在表格中列出这些特性，而是描述仅当该特性处于激活状态时所产生的数字——例如 `FILE_ATTRIBUTE_REPARSEPOINT = 0x400`。示例 6-4 展示了这些文件属性的各种表示形式。

### 示例 6-4. 可能的文件属性的整数、十六进制和二进制表示



```
PS > $attributes = [Enum]::GetValues([System.IO.FileAttributes])
PS > $attributes | Select-Object `
   @{"Name"="Property";
       "Expression"= { $_ } },
   @{"Name"="Integer";
       "Expression"= { [int] $_ } },
   @{"Name"="Hexadecimal";
       "Expression"= { [Convert]::ToString([int] $_, 16) } },
   @{"Name"="Binary";
       "Expression"= { [Convert]::ToString([int] $_, 2) } } |
   Format-Table -auto


         Property Integer Hexadecimal Binary
         -------- ------- ----------- ------
         ReadOnly       1 1           1
           Hidden       2 2           10
           System       4 4           100
        Directory      16 10          10000
          Archive      32 20          100000
           Device      64 40          1000000
           Normal     128 80          10000000
        Temporary     256 100         100000000
       SparseFile     512 200         1000000000
     ReparsePoint    1024 400         10000000000
       Compressed    2048 800         100000000000
          Offline    4096 1000        1000000000000
NotContentIndexed    8192 2000        10000000000000
        Encrypted   16384 4000        100000000000000
  IntegrityStream   32768 8000        1000000000000000
      NoScrubData  131072 20000       100000000000000000
```

知道 `16417` 这个数字是如何形成的后，你就可以用有意义的方式使用这些属性。例如，PowerShell 的 `-band` 运算符允许你检查某个位是否已被设置（假设你已通过资源管理器 UI 或其他方式将 `test.txt` 设置为加密）：

```
PS > $encrypted = 16384
PS > $attributes = (Get-Item test.txt -force).Attributes
PS > ($attributes -band $encrypted) -eq $encrypted
True

PS > $compressed = 2048
PS > ($attributes -band $compressed) -eq $compressed
False
```

虽然该示例显式使用了数值，但更常见的是按名称输入数字：

```
PS > $archive = [System.IO.FileAttributes] "Archive"
PS > ($attributes -band $archive) -eq $archive
True
```

有关 PowerShell 二元运算符的更多信息，请参阅“简单运算符”。

## 另请参阅

“简单运算符”

# 6.5 使用管理常量简化数学运算

## 问题

你想要处理常见的管理数字（即千字节、兆字节、吉字节、太字节和拍字节），而不必记住或计算这些数字。

## 解决方案

使用 PowerShell 的管理常量 `KB`、`MB`、`GB`、`TB` 和 `PB` 来帮助处理这些常见数字。

例如，我们可以计算以每秒 215 千字节的速度下载 10.18 兆字节文件所需的时间（以秒为单位）：

```
PS > 10.18mb / 215kb
48.4852093023256
```

## 讨论

PowerShell 的管理常量基于 2 的幂，因为这是处理计算机时最常用的类型。每个常量比前一个大 1,024 倍：

```
1kb = 1024
1mb = 1024 * 1 kb
1gb = 1024 * 1 mb
1tb = 1024 * 1 gb
1pb = 1024 * 1 tb
```

有些人（如硬盘制造商）倾向于将基于 2 的幂的数字称为“千字节”、“兆字节”和“吉字节”。他们用“千字节”、“兆字节”和“吉字节”表示基于 10 的幂的数字——即每个比前一个大 1,000 倍的数字。

虽然管理常量不表示这些数字，但 PowerShell 仍然可以轻松处理基于 10 的幂的数字——例如，计算 Windows 报告的“300 GB”硬盘实际有多大。为此，请使用科学（指数）表示法：

```
PS > $kilobyte = 1e3
PS > $kilobyte
1000

PS > $megabyte = 1e6
PS > $megabyte
1000000

PS > $gigabyte = 1e9
PS > $gigabyte
1000000000

PS > (300 * $gigabyte) / 1GB
279.396772384644
```

## 另请参阅

“简单赋值”

# 6.6 在不同进制间转换数字

## 问题

你想要将数字转换为不同的进制。

## 解决方案

PowerShell 脚本语言允许你直接输入十进制和十六进制数字。它不原生支持其他进制，但其对 .NET Framework 交互的支持可实现二进制、八进制、十进制和十六进制之间的双向转换。

要将十六进制数字转换为十进制表示，请在数字前加上 `0x` 前缀：

```
PS > $myErrorCode = 0xFE4A
PS > $myErrorCode
65098
```

要将二进制数字转换为十进制表示，请在前面加上 `0b` 前缀：

```
PS > 0b10011010010
1234
```

如果你有字符串形式的值，可以向 `[Convert]::ToInt32()` 方法提供 `2` 作为基数：

```
PS > [Convert]::ToInt32("10011010010", 2)
1234
```

要将八进制数字转换为十进制表示，请向 `[Convert]::ToInt32()` 方法提供 `8` 作为基数：

```
PS > [Convert]::ToInt32("1234", 8)
668
```

要将数字转换为十六进制表示，请使用 `[Convert]` 类或 PowerShell 的格式运算符：

```
PS > ## 使用 [Convert] 类
PS > [Convert]::ToString(1234, 16)
4d2

PS > ## 使用格式运算符
PS > "{0:X4}" -f 1234
04D2
```

如果你有一个想要转换为十六进制表示的大字节数组，可以使用 `BitConverter` 类：

```
PS > $bytes = Get-Content hello_world.txt -AsByteStream
PS > [System.BitConverter]::ToString($bytes).Replace("-","")
FFFE480065006C006C006F00200057006F0072006C006400200031000D000A00
```

要将数字转换为二进制表示，请向 `[Convert]::ToString()` 方法提供 `2` 作为基数：

```
PS > [Convert]::ToString(1234, 2)
10011010010
```

要将数字转换为八进制表示，请向 `[Convert]::ToString()` 方法提供 `8` 作为基数：

```
PS > [Convert]::ToString(1234, 8)
2322
```

## 讨论

当你处理表示数据二进制组合的数字（如文件属性）时，最常需要在不同进制间转换数字。有关如何处理此类二进制数据的更多信息，请参阅食谱 6.4。

## 另请参阅

食谱 6.4，“将数字作为二进制处理”
