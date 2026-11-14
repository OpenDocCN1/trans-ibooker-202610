# 附录 B. 正则表达式参考

正则表达式在大多数文本解析和文本匹配任务中发挥着重要作用。它们构成了 `-split` 和 `-match` 运算符、`switch` 语句、`Select-String` cmdlet 等的重要基础。表 B-1 到 B-9 列出了常用的正则表达式。

### 表 B-1. 字符类：表示字符集的模式

| 模式 | 说明 | 示例 |
| --- | --- | --- |
| `.` | 除换行符外的任何字符。如果正则表达式使用 `SingleLine` 选项，则匹配任何字符。 | `PS > "T" -match '.'`<br>`True` |
| `[`*`characters`*`]` | 方括号内的任何字符。例如：`[aeiou]`。 | `PS > "Test" -match '[Tes]'`<br>`True` |
| `[^`*`characters`*`]` | 方括号内以外的任何字符。例如：`[^aeiou]`。 | `PS > "Test" -match '[^Tes]'`<br>`False` |
| `[`*`start-end`*`]` | `start` 和 `end` 字符之间（含）的任何字符。您可以在方括号内包含多个字符范围。例如：`[a-eh-j]`。 | `PS > "Test" -match '[e-t]'`<br>`True` |
| `[^`*`start-end`*`]` | 不在任何 `start` 到 `end` 字符范围（含）内的任何字符。您可以在方括号内包含多个字符范围。例如：`[^a-eh-j]`。 | `PS > "Test" -match '[^e-t]'`<br>`False` |
| `\p{`*`character class`*`}` | `{`*`character class`*`}` 指定的 Unicode 组或块范围内的任何字符。 | `PS > "+" -match '\p{Sm}'`<br>`True` |
| `\P{`*`character class`*`}` | 不在 `{`*`character class`*`}` 指定的 Unicode 组或块范围内的任何字符。 | `PS > "+" -match '\P{Sm}'`<br>`False` |
| `\w` | 任何单词字符。请注意，这是单词字符的 *Unicode* 定义，包括数字以及许多数学符号和各种其他符号。 | `PS > "a" -match '\w'`<br>`True` |
| `\W` | 任何非单词字符。 | `PS > "!" -match '\W'`<br>`True` |
| `\s` | 任何空白字符。 | `PS > "`t" -match '\s'`<br>`True` |
| `\S` | 任何非空白字符。 | `PS > " `t" -match '\S'`<br>`False` |
| `\d` | 任何十进制数字。 | `PS > "5" -match '\d'`<br>`True` |
| `\D` | 任何非十进制数字的字符。 | `PS > "!" -match '\D'`<br>`True` |

### 表 B-2. 量词：对前一个表达式强制指定数量的表达式

| 量词 | 含义 | 示例 |
| --- | --- | --- |
| <none> | 匹配一次。 | `PS > "T" -match 'T'`<br>`True` |
| `*` | 零次或多次匹配，匹配尽可能多的内容。 | `PS > "A" -match 'T*'`<br>`True`<br><br>`PS > "TTTTT" -match '^T*$'`<br>`True`<br><br>`PS > 'ATTT' -match 'AT*'; $matches[0]`<br>`True`<br>`ATTT` |
| `+` | 一次或多次匹配，匹配尽可能多的内容。 | `PS > "A" -match 'T+'`<br>`False`<br><br>`PS > "TTTTT" -match '^T+$'`<br>`True`<br><br>`PS > 'ATTT' -match 'AT+'; $matches[0]`<br>`True`<br>`ATTT` |
| `?` | 零次或一次匹配，匹配尽可能多的内容。 | `PS > "TTTTT" -match '^T?$'`<br>`False`<br><br>`PS > 'ATTT' -match 'AT?'; $matches[0]`<br>`True`<br>`AT` |
| `{`*`n`*`}` | 精确匹配 `n` 次。 | `PS > "TTTTT" -match '^T{5}$'`<br>`True` |
| `{`*`n,`*`}` | 匹配 `n` 次或更多，匹配尽可能多的内容。 | `PS > "TTTTT" -match '^T{4,}$'`<br>`True` |
| `{`*`n,m`*`}` | 匹配 `n` 到 `m` 次（含），匹配尽可能多的内容。 | `PS > "TTTTT" -match '^T{4,6}$'`<br>`True` |
| `*?` | 零次或多次匹配，匹配尽可能少的内容。 | `PS > "A" -match '^AT*?$'`<br>`True`<br><br>`PS > 'ATTT' -match 'AT*?'; $matches[0]`<br>`True`<br>`A` |
| `+?` | 一次或多次匹配，匹配尽可能少的内容。 | `PS > "A" -match '^AT+?$'`<br>`False`<br><br>`PS > 'ATTT' -match 'AT+?'; $matches[0]`<br>`True`<br>`AT` |
| `??` | 零次或一次匹配，匹配尽可能少的内容。 | `PS > "A" -match '^AT??$'`<br>`True`<br><br>`PS > 'ATTT' -match 'AT??'; $matches[0]`<br>`True`<br>`A` |
| `{`*`n`*`}`? | 精确匹配 `n` 次。 | `PS > "TTTTT" -match '^T{5}?$'`<br>`True` |
| `{`*`n,`*`}`? | 匹配 `n` 次或更多，匹配尽可能少的内容。 | `PS > "TTTTT" -match '^T{4,}?$'`<br>`True` |
| `{`*`n,m`*`}`? | 匹配 `n` 到 `m` 次（含），匹配尽可能少的内容。 | `PS > "TTTTT" -match '^T{4,6}?$'`<br>`True` |

### 表 B-3. 分组构造：允许您对字符、模式和其他表达式进行分组的表达式

| 分组构造 | 说明 | 示例 |
| --- | --- | --- |
| `(`*`text`*`)` | 捕获括号内匹配的文本。这些捕获根据左括号的顺序按编号命名（从一开始）。 | `PS > "Hello" -match '^(.*)llo$'; $matches[1]`<br>`True`<br>`He` |
| `(?<`*`name`*`>)` | 捕获括号内匹配的文本。这些捕获由 `name` 中给出的名称命名。 | `PS > "Hello" -match '^(?<One>.*)llo$'; $matches.One`<br>`True`<br>`He` |
| `(?<`*`name1-name2`*`>)` | 平衡组定义。这是一种高级正则表达式构造，但可让您匹配均匀平衡的术语对。 |  |
| `(?:)` | 非捕获组。 | `PS > "A1" -match '((A|B)\d)'; $matches`<br>`True`<br><br>`Name                              Value`<br>`----                              -----`<br>`2                                 A`<br>`1                                 A1`<br>`0                                 A1`<br><br>`PS > "A1" -match '((?:A|B)\d)'; $matches`<br>`True`<br><br>`Name                              Value`<br>`----                              -----`<br>`1                                 A1`<br>`0                                 A1` |
| `(?imnsx-imnsx:)` | 为此组应用或禁用给定选项。支持的选项包括：<br>`i` 不区分大小写<br>`m` 多行<br>`n` 显式捕获<br>`s` 单行<br>`x` 忽略空白 | `PS > "Te`nst" -match '(T e.st)'`<br>`False`<br><br>`PS > "Te`nst" -match '(?sx:T e.st)'`<br>`True` |
| `(?=)` |  |  |





# 正则表达式零宽断言

## 正向/反向预查与后查

### `(?=...)`

零宽正向预查断言。确保给定模式向右匹配，但不实际执行匹配。

```
PS > "555-1212" -match '(?=...-)(.*)'; $matches[1]
True
555-1212
```

### `(?!...)`

零宽负向预查断言。确保给定模式向右不匹配，但不实际执行匹配。

```
PS > "friendly" -match '(?!friendly)friend'
False
```

### `(?<=...)`

零宽正向后查断言。确保给定模式向左匹配，但不实际执行匹配。

```
PS > "public int X" -match '^.*(?<=public )int .*$'
True
```

### `(?<!...)`

零宽负向后查断言。确保给定模式向左不匹配，但不实际执行匹配。

```
PS > "private int X" -match '^.*(?<!private )int .*$'
False
```

### `(?>...)`

非回溯子表达式。仅当该子表达式能完全匹配时才匹配。

```
PS > "Hello World" -match '(Hello.*)orld'
True

PS > "Hello World" -match '(?>Hello.*)orld'
False
```

非回溯版本的子表达式匹配失败，因为其完全匹配将是“Hello World”。

## 表 B-4. 原子零宽断言：限制匹配位置的模式

| 断言 | 限制 |
|------|------|
| `^` | 匹配必须出现在字符串开头（或行首，若 `Multiline` 选项生效）。 |
| `$` | 匹配必须出现在字符串结尾（或行尾，若 `Multiline` 选项生效）。 |
| `\A` | 匹配必须出现在字符串开头。 |
| `\Z` | 匹配必须出现在字符串结尾，或字符串末尾的 `\n` 之前。 |
| `\z` | 匹配必须出现在字符串结尾。 |
| `\G` | 匹配必须出现在上一次匹配结束的位置。配合 `System.Text.RegularExpressions.Match.NextMatch()` 使用。 |
| `\b` | 匹配必须出现在单词边界：由非字母数字字符分隔的单词的首尾字符。 |
| `\B` | 匹配不得出现在单词边界。 |

**示例：**

```
PS > "Test" -match '^est'
False
```

```
PS > "Test" -match 'Tes$'
False
```

```
PS > "The`nTest" -match '(?m:^Test)'
True

PS > "The`nTest" -match '(?m:\ATest)'
False
```

```
PS > "The`nTest`n" -match '(?m:The$)'
True

PS > "The`nTest`n" -match '(?m:The\Z)'
False

PS > "The`nTest`n" -match 'Test\Z'
True
```

```
PS > "The`nTest`n" -match 'Test\z'
False
```

```
PS > "Testing" -match 'ing\b'
True
```

```
PS > "Testing" -match 'ing\B'
False
```

## 表 B-5. 替换模式：正则表达式替换操作中使用的模式

| 模式 | 替换内容 |
|------|----------|
| `$`*`number`* | 编号为 *`number`* 的组匹配的文本。 |
| `${`*`name`*`}` | 名为 *`name`* 的组匹配的文本。 |
| `$$` | 字面量 `$`。 |
| `$&` | 整个匹配的副本。 |
| `` $` `` | 输入字符串中匹配之前的文本。 |
| `$'` | 输入字符串中匹配之后的文本。 |
| `$+` | 最后捕获的组。 |
| `$_` | 整个输入字符串。 |

**示例：**

```
PS > "Test" -replace "(.*)st",'$1ar'
Tear
```

```
PS > "Test" -replace "(?<pre>.*)st",'${pre}ar'
Tear
```

```
PS > "Test" -replace ".",'$$'
$$$$
```

```
PS > "Test" -replace "^.*$",'Found: $&'
Found: Test
```

```
PS > "Test" -replace "est$",'Te$`'
TTeT
```

```
PS > "Test" -replace "^Tes",'Res$'''
Restt
```

```
PS > "Testing" -replace "(.*)ing",'$+ed'
Tested
```

```
PS > "Testing" -replace "(.*)ing",'String: $_'
String: Testing
```

## 表 B-6. 选择构造：允许执行二选一逻辑的表达式

| 选择构造 | 说明 |
|----------|------|
| `|` | 匹配由竖线字符分隔的任意项。 |
| `(?(expression)yes\|no)` | 若 `expression` 在此处匹配，则匹配 *yes 项*；否则匹配 *no 项*。*no 项* 可选。 |
| `(?(name)yes\|no)` | 若名为 *`name`* 的捕获组在此处有捕获，则匹配 *yes 项*；否则匹配 *no 项*。*no 项* 可选。 |

**示例：**

```
PS > "Test" -match '(B|T)est'
True
```

```
PS > "3.14" -match '(?(\d)3.14|Pi)'
True

PS > "Pi" -match '(?(\d)3.14|Pi)'
True

PS > "2.71" -match '(?(\d)3.14|Pi)'
False
```

```
PS > "123" -match '(?<one>1)?(?(one)23|234)'
True

PS > "23" -match '(?<one>1)?(?(one)23|234)'
False

PS > "234" -match '(?<one>1)?(?(one)23|234)'
True
```

## 表 B-7. 反向引用构造：引用表达式内捕获组的表达式

| 反向引用构造 | 指向 |
|--------------|------|
| *`\number`* | 表达式中编号为 *`number`* 的组。 |
| `\k<`*`name`*`>` | 表达式中名为 *`name`* 的组。 |

**示例：**

```
PS > "|Text|" -match '(.)Text\1'
True

PS > "|Text+" -match '(.)Text\1'
False
```

```
PS > "|Text|" -match '(?<Symbol>.)Text\k<Symbol>'
True

PS > "|Text+" -match '(?<Symbol>.)Text\k<Symbol>'
False
```

## 表 B-8. 其他构造：修改正则表达式的其他表达式

| 构造 | 说明 |
|------|------|
| `(?imnsx-imnsx)` | 对表达式其余部分应用或禁用给定选项。支持的选项有：<br>`i` 大小写不敏感<br>`m` 多行<br>`n` 显式捕获<br>`s` 单行<br>`x` 忽略空白 |
| `(?# )` | 行内注释。在第一个右括号处终止。 |
| `#` *\[到行尾\]* | 启用 `IgnoreWhitespace` 选项时允许的注释形式。 |

**示例：**

```
PS > "Te`nst" -match '(?sx)T e.st'
True
```

```
PS > "Test" -match '(?# Match "Test")Test'
True
```

```
PS > "Test" -match '(?x)Test # Matches Test'
True
```

## 表 B-9. 字符转义：表示其他字符的字符序列

| 转义字符 | 匹配 |
|----------|------|
| `<普通字符>` | 除 `. $ ^ { [ ( | ) * + ? \` 以外的字符匹配其自身。 |
| `\a` | 响铃（警报） `u0007`。 |
| `\b` | 在 `[]` 字符类中为退格符 `u0008`。在正则表达式中，`\b` 表示单词边界（`\w` 与 `\W` 字符之间），除非在 `[]` 字符类内，此时 `\b` 指代退格字符。在替换模式中，`\b` 始终表示退格。 |
| `\t` | 制表符 `u0009`。 |
| `\r` | 回车符 `u000D`。 |
| `\v` | 垂直制表符 `u000B`。 |
| `\f` | 换页符 `u000C`。 |
| `\n` | 换行符 `u000A`。 |
| `\e` | 转义符 `u001B`。 |
| `\ddd` | 八进制表示的 ASCII 字符（最多三位数字）。无前导零的数字若仅有一位，或对应捕获组编号，则视为反向引用。 |
| `\xdd` | 十六进制表示的 ASCII 字符（恰好两位数字）。 |
| `\cC` | ASCII 控制字符；例如 `\cC` 为 Control-C。 |
| `\udddd` | 十六进制表示的 Unicode 字符（恰好四位数字）。 |
| `\` | 后跟未被识别为转义字符的字符时，匹配该字符。例如 `\*` 匹配字面字符 `*`。 |

# 附录 C. XPath 快速参考

正如正则表达式是与纯文本交互的标准方式，XPath 是与 XML 交互的标准方式。因此，你在工作中很可能会遇到 XPath。多个 cmdlet 支持 XPath 查询：`Select-Xml`、`Get-WinEvent` 等。表 C-1 和 C-2 简要概述了 XPath 概念。

以下示例使用此示例 XML：





# 附录 C. XPath 导航与选择

## 示例 XML 文档

```xml
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
```

## 表 C-1. 导航与选择

| 语法 | 含义 |
|--------|---------|
| `/` | 表示 XML 树的根节点。 |
| `/Node` | 从 XML 树的根节点导航到名为 `Node` 的节点。 |
| `/Node/*/Node2` | 通过 `Node` 导航到名为 `Node2` 的节点，中间允许有任意单个节点。 |
| `//Node` | 查找 XML 树中任何位置名为 `Node` 的所有节点。 |
| `..` | 检索给定节点的父节点。 |
| `@Attribute` | 访问名为 `Attribute` 的属性的值。 |

### 表 C-1 示例

**根节点 (`/`)**
```powershell
PS > $xml | Select-Xml "/" | Select -Expand Node

AddressBook
-----------
AddressBook
```

**子节点 (`/AddressBook`)**
```powershell
PS > $xml | Select-Xml "/AddressBook" | Select -Expand Node

Person
------
{Lee, Ariel}
```

**通过通配符获取孙节点 (`/AddressBook/*/Name`)**
```powershell
PS > $xml | Select-Xml "/AddressBook/*/Name" | Select -Expand Node

#text
-----
Lee
Ariel
```

**后代节点 (`//Phone`)**
```powershell
PS > $xml | Select-Xml "//Phone" | Select -Expand Node

type                                   #text
----                                   -----
home                                   555-1212
work                                   555-1213
                                       555-1234
```

**父节点 (`..`)**
```powershell
PS > $xml | Select-Xml "//Phone" | Select -Expand Node

type                                  #text
----                                  -----
home                                  555-1212
work                                  555-1213
                                      555-1234

PS > $xml | Select-Xml "//Phone/.." | Select -Expand Node

contactType              Name             Phone
-----------              ----             -----
Personal                 Lee              {Phone, Phone}
Business                 Ariel            555-1234
```

**属性 (`@type`)**
```powershell
PS > $xml | Select-Xml "//Phone/@type" | Select -Expand Node

#text
-----
home
work
```

## 表 C-2. 比较

| 语法 | 含义 |
|--------|---------|
| `[ ]` | 筛选，类似于 `Where-Object` cmdlet。 |
| `and` | 逻辑*与*。 |
| `or` | 逻辑*或*。 |
| `not()` | 逻辑*非*。 |
| `=` | *相等*。 |
| `!=` | *不等*。 |

### 表 C-2 示例

**按属性筛选 (`//Person[@contactType = 'Personal']`)**
```powershell
PS > $xml | Select-Xml "//Person[@contactType = 'Personal']" |
    Select -Expand Node

contactType               Name              Phone
-----------               ----              -----
Personal                  Lee               {Phone, Phone}
```

**按元素值筛选 (`//Person[Name = 'Lee']`)**
```powershell
PS > $xml | Select-Xml "//Person[Name = 'Lee']" |
    Select -Expand Node

contactType               Name              Phone
-----------               ----              -----
Personal                  Lee               {Phone, Phone}
```

# 附录 D. .NET 字符串格式化

## 字符串格式化语法

格式（`-f`）运算符支持的格式字符串是一个包含格式项的字符串。每个格式项采用以下形式：

```
{index[,alignment][:formatString]}
```

- `index` 表示格式运算符后面的对象数组中项的从零开始的索引。
- `alignment` 为可选项，表示项的对齐方式。正数将项向右对齐到指定宽度的字段中。负数将项向左对齐到指定宽度的字段中。

```powershell
PS > ("{0,6}" -f 4.99), ("{0,6:##.00}" -f 15.9)
  4.99
 15.90
```

- `formatString` 为可选项，使用该类型特定的格式字符串语法对项进行格式化（如表 D-1 和表 D-2 所示）。

## 标准数字格式字符串

<a id="table-d-1-standard-numeric-format-strings"></a>
**表 D-1. 标准数字格式字符串**

| 格式说明符 | 名称 | 说明 |
|------------------|------|-------------|
| `C` or `c` | 货币 | 货币金额： |
| `D` or `d` | 十进制 | 十进制数值（用于整数类型）。精度说明符控制结果中的最小位数： |
| `E` or `e` | 科学计数法 | 科学（指数）计数法。精度说明符控制小数点后的位数： |
| `F` or `f` | 定点 | 定点计数法。精度说明符控制小数点后的位数： |
| `G` or `g` | 常规 | 数字最紧凑的表示形式（在定点和科学计数法之间）。精度说明符控制有效数字的位数： |
| `N` or `n` | 数字 | 数字的易读形式，包含数字组之间的分隔符。精度说明符控制小数点后的位数： |
| `P` or `p` | 百分比 | 数字（通常在 0 和 1 之间）表示为百分比。精度说明符控制小数点后的位数： |
| `R` or `r` | 往返 | 使用精度对 Single 或 Double 数字进行格式化，以保证字符串（在解析时）将再次生成原始数字： |
| `X` or `x` | 十六进制 | 将数字转换为十六进制数字字符串。说明符的大小写控制生成的十六进制数字的大小写。精度说明符控制生成字符串中的最小位数： |

### 表 D-1 示例

**货币 (`C`)**
```powershell
PS > "{0:C}" -f 1.23
$1.23
```

**十进制 (`D`)**
```powershell
PS > "{0:D4}" -f 2
0002
```

**科学计数法 (`E`)**
```powershell
PS > "{0:E3}" -f [Math]::Pi
3.142E+000
```

**定点 (`F`)**
```powershell
PS > "{0:F3}" -f [Math]::Pi
3.142
```

**常规 (`G`)**
```powershell
PS > "{0:G3}" -f [Math]::Pi
3.14

PS > "{0:G3}" -f 1mb
1.05E+06
```

**数字 (`N`)**
```powershell
PS > "{0:N4}" -f 1mb
1,048,576.0000
```

**百分比 (`P`)**
```powershell
PS > "{0:P4}" -f 0.67
67.0000 %
```

**往返 (`R`)**
```powershell
PS > "{0:R}" -f (1mb/2.0)
524288

PS > "{0:R}" -f (1mb/9.0)
116508.44444444444
```

**十六进制 (`X`)**
```powershell
PS > "{0:X4}" -f 1324
052C
```

## 自定义数字格式字符串

您可以使用表 D-2 中列出的自定义数字字符串，以标准格式字符串不支持的方式格式化数字。

<a id="table-d-2-custom-numeric-format-strings"></a>
**表 D-2. 自定义数字格式字符串**

| 格式说明符 | 名称 | 说明 |
|------------------|------|-------------|
| `0` | 零占位符 | 指定数字字符串的精度和宽度。原始数字中未匹配数字的零将作为零输出： |
| `#` | 数字占位符 | 指定数字字符串的精度和宽度。输入数字中未匹配数字的 `#` 符号将不输出： |
| `.` | 小数点 | 确定小数点的位置： |
| `,` | 千位分隔符 | 当放在格式字符串中小数点前的零或数字占位符之间时，在数字组之间添加分隔符字符： |
| `,` | 数字缩放 | 当放在格式字符串中字面（或隐式）小数点之前时，将输入值除以 1,000。您可以多次应用此格式说明符： |
| `%` | 百分比占位符 | 将输入值乘以 100，并在格式说明符中显示的位置插入百分号： |
| `E0`<br>`E+0`<br>`E-0`<br>`e0`<br>`e+0`<br>`e-0` | 科学计数法 | |

### 表 D-2 示例

**零占位符 (`0`)**
```powershell
PS > "{0:00.0}" -f 4.12341234
04.1
```

**数字占位符 (`#`)**
```powershell
PS > "{0:##.#}" -f 4.12341234
4.1
```

**小数点 (`.`)**
```powershell
PS > "{0:##.#}" -f 4.12341234
4.1
```

**千位分隔符 (`,`) **
```powershell
PS > "{0:#,#.#}" -f 1234.121234
1,234.1
```

**数字缩放 (`,`) **
```powershell
PS > "{0:##,,.000}" -f 1048576
1.049
```

**百分比占位符 (`%`)**
```powershell
PS > "{0:%##.000}" -f .68
%68.000
```




# 科学计数法和字面量字符串

以科学计数法显示输入。紧随 `E` 之后的零的数量定义了指数字段的最小长度：

```
PS > "{0:##.#E000}" -f 2.71828
27.2E-001
```

| 格式说明符 | 名称 | 说明 | 示例 |
|------------------|------|-------------|---------|
| `'`*`text`*`'` | 字面量字符串 | 将提供的文本按字面量插入输出，不影响格式设置 | `PS > "{0:#.00'##'}" -f 2.71828`<br>`2.72##` |
| `"`*`text`*`"` | 字面量字符串 | 将提供的文本按字面量插入输出，不影响格式设置 | `PS > "{0:#.00'##'}" -f 2.71828`<br>`2.72##` |
| `;` | 部分配置分隔符 | 允许条件格式设置。<br><br>如果格式说明符不包含部分配置分隔符，则格式化语句应用于所有输入。<br><br>如果格式说明符包含一个分隔符（创建两个部分），第一部分应用于正数和零，第二部分应用于负数。<br><br>如果格式说明符包含两个分隔符（创建三个部分），这些部分分别应用于正数、负数和零： | `PS > "{0:POS;NEG;ZERO}" -f -14`<br>`NEG` |
| `Other` | 其他字符 | 将提供的文本按字面量插入输出，不影响格式设置 | `PS > "{0:$## Please}" -f 14`<br>`$14 Please` |

# 附录 E .NET DateTime 格式化

`DateTime` 格式字符串将 `DateTime` 对象转换为几种标准格式之一，如 表 E-1 所列。

## 表 E-1. 标准 DateTime 格式字符串

| 格式说明符 | 名称 | 说明 | 示例 |
|------------------|------|-------------|---------|
| `d` | 短日期 | 区域性的短日期格式 | `PS > "{0:d}" -f [DateTime] "01/23/4567"`<br>`1/23/4567` |
| `D` | 长日期 | 区域性的长日期格式 | `PS > "{0:D}" -f [DateTime] "01/23/4567"`<br>`Friday, January 23, 4567` |
| `f` | 完整日期/短时间 | 组合长日期和短时间格式模式 | `PS > "{0:f}" -f [DateTime] "01/23/4567"`<br>`Friday, January 23, 4567 12:00 AM` |
| `F` | 完整日期/长时间 | 组合长日期和长时间格式模式 | `PS > "{0:F}" -f [DateTime] "01/23/4567"`<br>`Friday, January 23, 4567 12:00:00 AM` |
| `g` | 常规日期/短时间 | 组合短日期和短时间格式模式 | `PS > "{0:g}" -f [DateTime] "01/23/4567"`<br>`1/23/4567 12:00 AM` |
| `G` | 常规日期/长时间 | 组合短日期和长时间格式模式 | `PS > "{0:G}" -f [DateTime] "01/23/4567"`<br>`1/23/4567 12:00:00 AM` |
| `M` or `m` | 月日 | 区域性的 `MonthDay` 格式 | `PS > "{0:M}" -f [DateTime] "01/23/4567"`<br>`January 23` |
| `o` | 往返日期/时间 | 使用一种模式格式化日期，该模式保证字符串（在解析时）将再次生成原始 `DateTime` | `PS > "{0:o}" -f [DateTime] "01/23/4567"`<br>`4567-01-23T00:00:00.0000000` |
| `R` or `r` | RFC1123 | 标准 RFC1123 格式模式 | `PS > "{0:R}" -f [DateTime] "01/23/4567"`<br>`Fri, 23 Jan 4567 00:00:00 GMT` |
| `s` | 可排序 | 可排序格式模式。符合 ISO 8601 并提供适合排序的输出 | `PS > "{0:s}" -f [DateTime] "01/23/4567"`<br>`4567-01-23T00:00:00` |
| `t` | 短时间 | 区域性的 `ShortTime` 格式 | `PS > "{0:t}" -f [DateTime] "01/23/4567"`<br>`12:00 AM` |
| `T` | 长时间 | 区域性的 `LongTime` 格式 | `PS > "{0:T}" -f [DateTime] "01/23/4567"`<br>`12:00:00 AM` |
| `u` | 通用可排序 | 将区域性的 `UniversalSortable DateTime` 格式应用于输入的 UTC 等效值 | `PS > "{0:u}" -f [DateTime] "01/23/4567"`<br>`4567-01-23 00:00:00Z` |
| `U` | 通用 | 将区域性的 `FullDateTime` 格式应用于输入的 UTC 等效值 | `PS > "{0:U}" -f [DateTime] "01/23/4567"`<br>`Friday, January 23, 4567 8:00:00 AM` |
| `Y` or `y` | 年月 | 区域性的 `YearMonth` 格式 | `PS > "{0:Y}" -f [DateTime] "01/23/4567"`<br>`January, 4567` |

# 自定义 DateTime 格式字符串

您可以使用 表 E-2 中列出的自定义 `DateTime` 格式字符串，以标准格式字符串不支持的方式设置日期格式。

> **注意**
>
> 单字符格式说明符默认被解释为标准 `DateTime` 格式字符串，除非它们与其他格式说明符一起使用。在它们前面添加 `%` 字符，以使它们被解释为自定义格式说明符。

## 表 E-2. 自定义 DateTime 格式字符串

| 格式说明符 | 说明 | 示例 |
|------------------|-------------|---------|
| `d` | 月中的某天，介于 1 和 31 之间的数字。表示个位数日期时不带前导零 | `PS > "{0:%d}" -f [DateTime] "01/02/4567"`<br>`2` |
| `dd` | 月中的某天，介于 1 和 31 之间的数字。表示个位数日期时带前导零 | `PS > "{0:dd}" -f [DateTime] "01/02/4567"`<br>`02` |
| `ddd` | 星期几的缩写名称 | `PS > "{0:ddd}" -f [DateTime] "01/02/4567"`<br>`Fri` |
| `dddd` | 星期几的完整名称 | `PS > "{0:dddd}" -f [DateTime] "01/02/4567"`<br>`Friday` |
| `f` | 秒的小数部分（毫秒）的最高有效位数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:%f}" -f $date`<br>`0` |
| `ff` | 秒的小数部分（毫秒）的最高两位有效数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:ff}" -f $date`<br>`09` |
| `fff` | 秒的小数部分（毫秒）的最高三位有效数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:fff}" -f $date`<br>`093` |
| `ffff` | 秒的小数部分（毫秒）的最高四位有效数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:ffff}" -f $date`<br>`0937` |
| `fffff` | 秒的小数部分（毫秒）的最高五位有效数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:fffff}" -f $date`<br>`09375` |
| `ffffff` | 秒的小数部分（毫秒）的最高六位有效数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:ffffff}" -f $date`<br>`093750` |
| `fffffff` | 秒的小数部分（毫秒）的最高七位有效数字 | `PS > $date = Get-Date`<br>`PS > $date.Millisecond`<br>`93`<br>`PS > "{0:fffffff}" -f $date`<br>`0937500` |
| `F`<br>`FF`<br>`FFF`<br>`...`<br>`FFFFFFF` | 秒的小数部分（毫秒）的最高有效位数字。与小写 `f` 说明符系列相比，如果数字为零则不显示任何内容 | `PS > "{0:|F FF FFF FFFF|}" -f [DateTime] "01/02/4567"`<br>`|   |----` |
| `%g` or `gg` | 纪元（例如 A.D.） | `PS > "{0:gg}" -f [DateTime] "01/02/4567"`<br>`A.D.` |
| `%h` | 小时，介于 1 和 12 之间的数字。个位数不包含前导零 | `PS > "{0:%h}" -f [DateTime] "01/02/4567 4:00pm"`<br>`4` |
| `hh` | 小时，介于 01 和 12 之间的数字。个位数包含前导零。注意：除非与其他格式说明符一起使用，否则这会被解释为标准 `DateTime` 格式字符串 | `PS > "{0:hh}" -f [DateTime] "01/02/4567 4:00pm"`<br>`04` |
| `%H` | 小时，介于 0 和 23 之间的数字。个位数不包含前导零 | `PS > "{0:%H}" -f [DateTime] "01/02/4567 4:00pm"`<br>`16` |
| `HH` | 小时，介于 00 和 23 之间的数字。个位数包含前导零 | `PS > "{0:HH}" -f [DateTime] "01/02/4567 4:00am"`<br>`04` |
| `K` | 对应输入日期种类（即本地、UTC 或未指定）的 `DateTime.Kind` 说明符 | `PS > "{0:%K}" -f [DateTime]::Now.ToUniversalTime()`<br>`Z` |
| `m` | 分钟，介于 0 和 59 之间的数字。个位数不包含前导零 | `PS > "{0:%m}" -f [DateTime]::Now`<br>`7` |
| `mm` | 分钟，介于 00 和 59 之间的数字。个位数包含前导零 | *(示例继续)* |




# 日期和时间格式说明符

下表介绍了 PowerShell/.NET 中可用的自定义日期和时间格式说明符。

| 说明符 | 说明 | 示例 |
|-----------|-------------|---------|
| `M` | 月份，介于 1 和 12 之间的数字。个位数不包含前导零。 | ```PS > "{0:%M}" -f [DateTime] "01/02/4567" 1``` |
| `MM` | 月份，介于 01 和 12 之间的数字。个位数包含前导零。 | ```PS > "{0:MM}" -f [DateTime] "01/02/4567" 01``` |
| `MMM` | 缩写的月份名称。 | ```PS > "{0:MMM}" -f [DateTime] "01/02/4567" Jan``` |
| `MMMM` | 完整的月份名称。 | ```PS > "{0:MMMM}" -f [DateTime] "01/02/4567" January``` |
| `s` | 秒，介于 0 和 59 之间的数字。个位数不包含前导零。 | ```PS > $date = Get-Date PS > "{0:%s}" -f $date 7``` |
| `ss` | 秒，介于 00 和 59 之间的数字。个位数包含前导零。 | ```PS > $date = Get-Date PS > "{0:ss}" -f $date 07``` |
| `t` | 上午/下午指示符的第一个字符。 | ```PS > $date = Get-Date PS > "{0:%t}" -f $date P``` |
| `tt` | 上午/下午指示符。 | ```PS > $date = Get-Date PS > "{0:tt}" -f $date PM``` |
| `y` | 年份，最多两位数字。 | ```PS > "{0:%y}" -f [DateTime] "01/02/4567" 67``` |
| `yy` | 年份，最多两位数字。 | ```PS > "{0:yy}" -f [DateTime] "01/02/4567" 67``` |
| `yyy` | 年份，最多四位数字。 | ```PS > "{0:yyy}" -f [DateTime] "01/02/4567" 4567``` |
| `yyyy` | 年份，最多四位数字。 | ```PS > "{0:yyyy}" -f [DateTime] "01/02/4567" 4567``` |
| `yyyyy` | 年份，最多五位数字。 | ```PS > "{0:yyyyy}" -f [DateTime] "01/02/4567" 04567``` |
| `z` | 相对于 GMT 的带符号时区偏移量。不包含前导零。 | ```PS > "{0:%z}" -f [DateTime]::Now -8``` |
| `zz` | 相对于 GMT 的带符号时区偏移量。包含前导零。 | ```PS > "{0:zz}" -f [DateTime]::Now -08``` |
| `zzz` | 相对于 GMT 的带符号时区偏移量，以小时和分钟为单位。 | ```PS > "{0:zzz}" -f [DateTime]::Now -08:00``` |
| `:` | 时间分隔符。 | ```PS > "{0:y/m/d h:m:s}" -f [DateTime] "01/02/4567 4:00pm" 67/0/2 4:0:0``` |
| `/` | 日期分隔符。 | ```PS > "{0:y/m/d h:m:s}" -f [DateTime] "01/02/4567 4:00pm" 67/0/2 4:0:0``` |
| `"`text`"` / `'`text`'` | 将提供的文本按原样插入输出中，不影响格式设置。 | ```PS > "{0:'Day: 'dddd}" -f [DateTime]::Now Day: Monday``` |
| `%c` | 允许使用单字符自定义格式说明符的语法。`%` 符号不会添加到输出中。 | ```PS > "{0:%h}" -f [DateTime] "01/02/4567 4:00pm" 4``` |
| `Other` | 将提供的文本按原样插入输出中，不影响格式设置。 | ```PS > "{0:dddd!}" -f [DateTime]::Now Monday!``` |

# 附录 F. 精选 .NET 类及其用途

表 F-1 到 F-6 提供了 .NET Framework 中类型的指针，这些类型有效地补充了 PowerShell 提供的功能。有关详细说明和文档，请参阅 [官方文档](https://oreil.ly/UJrRW)。

## 表 F-1. PowerShell

| 类 | 说明 |
|-------|-------------|
| `System.Management.Automation.PSObject` | 表示一个 PowerShell 对象，您可以向其添加注释、属性等。 |

## 表 F-2. 实用程序

| 类 | 说明 |
|-------|-------------|
| `System.DateTime` | 表示一个时间点，通常表示为日期和一天中的时间。 |
| `System.Guid` | 表示全局唯一标识符 (GUID)。 |
| `System.Math` | 提供三角函数、对数函数和其他常用数学函数的常量和静态方法。 |
| `System.Random` | 表示伪随机数生成器，一种生成满足随机性特定统计要求的数字序列的设备。 |
| `System.Convert` | 将一种基础数据类型转换为另一种基础数据类型。 |
| `System.Environment` | 提供有关当前环境和平台的信息及操作手段。 |
| `System.Console` | 表示控制台应用程序的标准输入、输出和错误流。 |
| `System.Text.RegularExpressions.Regex` | 表示一个不可变的正则表达式。 |
| `System.Diagnostics.Debug` | 提供一组帮助调试代码的方法和属性。 |
| `System.Diagnostics.EventLog` | 提供与 Windows 事件日志的交互。 |
| `System.Diagnostics.Process` | 提供对本地和远程进程的访问，并允许启动和停止本地系统进程。 |
| `System.Diagnostics.Stopwatch` | 提供一组方法和属性，可用于精确测量经过的时间。 |
| `System.Media.SoundPlayer` | 控制从 `.wav` 文件播放声音。 |

## 表 F-3. 集合和对象实用程序

| 类 | 说明 |
|-------|-------------|
| `System.Array` | 提供用于创建、操作、搜索和排序数组的方法，从而作为公共语言运行时中所有数组的基类。 |
| `System.Enum` | 为枚举提供基类。 |
| `System.String` | 将文本表示为一系列 Unicode 字符。 |
| `System.Text.StringBuilder` | 表示一个可变的字符字符串。 |
| `System.Collections.Specialized.OrderedDictionary` | 表示可通过键或索引访问的键/值对集合。 |
| `System.Collections.ArrayList` | 使用数组实现 `IList` 接口，其大小会根据需要动态增加。 |

## 表 F-4. .NET Framework

| 类 | 说明 |
|-------|-------------|
| `System.AppDomain` | 表示应用程序域，即应用程序在其中执行的隔离环境。 |
| `System.Reflection.Assembly` | 定义程序集，它是公共语言运行时应用程序中可重用、可版本控制且自我描述的构建基块。 |
| `System.Type` | 表示类型声明：类类型、接口类型、数组类型、值类型、枚举类型、类型参数、泛型类型定义以及开放或封闭的构造泛型类型。 |
| `System.Threading.Thread` | 创建并控制线程，设置其优先级，并获取其状态。 |
| `System.Runtime.InteropServices.Marshal` | 提供一组方法，用于分配非托管内存、复制非托管内存块、将托管类型转换为非托管类型，以及与非托管代码交互时使用的其他各种方法。 |
| `Microsoft.CSharp.CSharpCodeProvider` | 提供对 C# 代码生成器和代码编译器实例的访问。 |

## 表 F-5. 注册表

| 类 | 说明 |
|-------|-------------|
| `Microsoft.Win32.Registry` | 提供表示本地和远程 Windows 注册表中根项的 `RegistryKey` 对象，以及用于访问键/值对的静态方法。 |
| `Microsoft.Win32.RegistryKey` | 表示 Windows 注册表中的项级节点。 |

## 表 F-6. 输入和输出

| 类 | 说明 |
|-------|-------------|
| `System.IO.Stream` | 提供字节序列的通用视图。 |
| `System.IO.BinaryReader` | 将基元数据类型作为二进制值读取。 |
| `System.IO.BinaryWriter` | 将基元类型以二进制形式写入流。 |
| `System.IO.BufferedStream` | 为另一个流上的读取和写入操作添加缓冲层。 |
| `System.IO.Directory` | 公开用于创建、移动和枚举目录及子目录的静态方法。 |
| `System.IO.FileInfo` | 提供用于创建、复制、删除、移动和打开文件的实例方法，并协助创建 `FileStream` 对象。 |
| `System.IO.DirectoryInfo` | 公开用于创建、移动和枚举目录及子目录的实例方法。 |





### 表 F-6. I/O

| 类 | 说明 |
| --- | --- |
| `System.IO.File` | 提供用于创建、复制、删除、移动和打开文件的静态方法，并有助于创建 `FileStream` 对象。 |
| `System.IO.MemoryStream` | 创建其后备存储区为内存的流。 |
| `System.IO.Path` | 对包含文件或目录路径信息的 `String` 实例执行操作。这些操作以跨平台方式执行。 |
| `System.IO.TextReader` | 表示可读取字符序列的读取器。 |
| `System.IO.StreamReader` | 实现一个 `TextReader`，以特定编码从字节流中读取字符。 |
| `System.IO.TextWriter` | 表示可写入字符序列的写入器。 |
| `System.IO.StreamWriter` | 实现一个 `TextWriter`，以特定编码向流中写入字符。 |
| `System.IO.StringReader` | 实现一个从字符串读取的 `TextReader`。 |
| `System.IO.StringWriter` | 实现一个用于向字符串写入信息的 `TextWriter`。 |
| `System.IO.Compression.DeflateStream` | 提供使用 Deflate 算法压缩和解压流的方法和属性。 |
| `System.IO.Compression.GZipStream` | 提供使用 GZip 算法压缩和解压流的方法和属性。 |
| `System.IO.FileSystemWatcher` | 监听文件系统更改通知，并在目录或目录中的文件发生更改时引发事件。 |

### 表 F-7. 安全性

| 类 | 说明 |
| --- | --- |
| `System.Security.Principal.WindowsIdentity` | 表示一个 Windows 用户。 |
| `System.Security.Principal.WindowsPrincipal` | 允许代码检查 Windows 用户的 Windows 组成员身份。 |
| `System.Security.Principal.WellKnownSidType` | 定义一组常用的安全标识符 (SID)。 |
| `System.Security.Principal.WindowsBuiltInRole` | 指定用于 `IsInRole()` 的常见角色。 |
| `System.Security.SecureString` | 表示应保密的文本。使用时为隐私加密文本，不再需要时从计算机内存中删除。 |
| `System.Security.Cryptography.TripleDESCrypto​Servi⁠ceProvider` | 定义一个包装对象，用于访问 TripleDES 算法的加密服务提供程序 (CSP) 版本。 |
| `System.Security.Cryptography.PasswordDeriveBytes` | 使用 PBKDF1 算法的扩展从密码派生密钥。 |
| `System.Security.Cryptography.SHA1` | 计算输入数据的 SHA1 哈希值。 |
| `System.Security.AccessControl.FileSystemSecurity` | 表示文件或目录的访问控制和审核安全性。 |
| `System.Security.AccessControl.RegistrySecurity` | 表示注册表项的 Windows 访问控制安全性。 |

### 表 F-8. UI

| 类 | 说明 |
| --- | --- |
| `System.Windows.Forms.Form` | 表示构成应用程序 UI 的窗口或对话框。 |
| `System.Windows.Forms.FlowLayoutPanel` | 表示动态布局其内容的面板。 |

### 表 F-9. 图像处理

| 类 | 说明 |
| --- | --- |
| `System.Drawing.Image` | 为 `Bitmap` 和 `Metafile` 类提供功能的类。 |
| `System.Drawing.Bitmap` | 封装 GDI+ 位图，由图形图像的像素数据及其属性组成。位图是用于处理由像素数据定义的图像的对象。 |

### 表 F-10. 网络

| 类 | 说明 |
| --- | --- |
| `System.Uri` | 提供统一资源标识符 (URI) 的对象表示形式，并可轻松访问 URI 的各个部分。 |
| `System.Net.NetworkCredential` | 为基于密码的身份验证方案（如基本、摘要、Kerberos 身份验证和 NTLM）提供凭据。 |
| `System.Net.Dns` | 提供简单的域名解析功能。 |
| `System.Net.FtpWebRequest` | 实现文件传输协议 (FTP) 客户端。 |
| `System.Net.HttpWebRequest` | 提供 `WebRequest` 类的特定于 HTTP 的实现。 |
| `System.Net.WebClient` | 提供用于向 URI 标识的资源发送和接收数据的常用方法。 |
| `System.Net.Sockets.TcpClient` | 为 TCP 网络服务提供客户端连接。 |
| `System.Net.Mail.MailAddress` | 表示电子邮件发件人或收件人的地址。 |
| `System.Net.Mail.MailMessage` | 表示可使用 `SmtpClient` 类发送的电子邮件。 |
| `System.Net.Mail.SmtpClient` | 允许应用程序使用简单邮件传输协议 (SMTP) 发送电子邮件。 |
| `System.IO.Ports.SerialPort` | 表示串口资源。 |
| `System.Web.HttpUtility` | 在处理 Web 请求时，提供对 URL 进行编码和解码的方法。 |

### 表 F-11. XML

| 类 | 说明 |
| --- | --- |
| `System.Xml.XmlTextWriter` | 表示一个写入器，它提供了一种快速、非缓存、仅向前的方式，生成包含符合 W3C 可扩展标记语言 (XML) 1.0 和 XML 命名空间建议的 XML 数据的流或文件。 |
| `System.Xml.XmlDocument` | 表示一个 XML 文档。 |

### 表 F-12. Windows Management Instrumentation (WMI)

| 类 | 说明 |
| --- | --- |
| `System.Management.ManagementObject` | 表示一个 WMI 实例。 |
| `System.Management.ManagementClass` | 表示一个管理类。管理类是一个 WMI 类，例如 `Win32_LogicalDisk`（可表示磁盘驱动器）或 `Win32_Process`（表示进程，如 `Notepad.exe` 的实例）。该类的成员使你能够使用特定的 WMI 类路径访问 WMI 数据。有关详细信息，请参阅官方 [Windows Management Instrumentation 文档](https://aka.ms/wmi) 中的“Win32 Classes”。 |
| `System.Management.ManagementObjectSearcher` | 根据指定查询检索 WMI 管理对象的集合。此类是检索管理信息的更常用入口点之一。例如，可用于枚举系统上的所有磁盘驱动器、网络适配器、进程等管理对象，或查询所有处于启动状态的网络连接、暂停的服务等。实例化时，该类的实例将采用以 `ObjectQuery` 或其派生类表示的 WMI 查询作为输入，并可选择采用表示用于执行查询的 WMI 命名空间的 `ManagementScope`。它还可以在 `EnumerationOptions` 中采用其他高级选项。调用该对象上的 `Get()` 方法时，`ManagementObjectSearcher` 将在指定范围内执行给定查询，并在 `ManagementObjectCollection` 中返回与查询匹配的管理对象集合。 |
| `System.Management.ManagementDateTimeConverter` | 提供将 DMTF 日期时间和时间间隔转换为符合 CLR 规范的 `DateTime` 和 `TimeSpan` 格式（反之亦然）的方法。 |
| `System.Management.ManagementEventWatcher` | 根据指定的事件查询订阅临时事件通知。 |

### 表 F-13. Active Directory

| 类 | 说明 |
| --- | --- |
| `System.DirectoryServices.DirectorySearcher` | 对 Active Directory 执行查询。 |
| `System.DirectoryServices.DirectoryEntry` | `DirectoryEntry` 类封装 Active Directory 层次结构中的节点或对象。 |

### 表 F-14. 数据库

| 类 | 说明 |
| --- | --- |
| `System.Data.DataSet` | 表示数据的内存缓存。 |
| `System.Data.DataTable` | 表示一张内存数据表。 |
| `System.Data.SqlClient.SqlCommand` | 表示要在 SQL Server 数据库上执行的 `Transact-SQL` 语句或存储过程。 |




# 附录 G. WMI 参考

Windows 中的 Windows Management Instrumentation (WMI) 设施提供了数千个类，这些类提供了管理员感兴趣的信息。表 G-1 列出了 WMI 涵盖的类别和子类别，可用于大致了解 WMI 类的范围。表 G-2 提供了最有用的 WMI 类的精选子集。有关类别的详细信息，请搜索官方 [WMI 文档](https://aka.ms/wmi)。

## 表 F-15. 消息队列

| 类 | 说明 |
| --- | --- |
| `System.Messaging.MessageQueue` | 提供对消息队列服务器上队列的访问。 |

## 表 F-16. 事务

| 类 | 说明 |
| --- | --- |
| `System.Transactions.Transaction` | 表示一个事务。 |

## 表 G-1. WMI 类类别和子类别

| 类别 | 子类别 |
| --- | --- |
| 计算机系统硬件 | 冷却设备、输入设备、大容量存储、主板、控制器和端口、网络设备、电源、打印、电话、视频和显示器 |
| 操作系统 | COM、桌面、驱动程序、文件系统、作业对象、内存和页面文件、多媒体音频/视频、网络、操作系统事件、操作系统设置、进程、注册表、计划作业、安全性、服务、共享、开始菜单、存储、用户、Windows NT 事件日志、Windows 产品激活 |
| WMI 服务管理 | WMI 配置、WMI 管理 |
| 常规 | 已安装的应用程序、性能计数器、安全描述符 |

## 表 G-2. 精选 WMI 类

| 类 | 说明 |
| --- | --- |
| `CIM_DataFile` | 表示数据或可执行代码的命名集合。目前，提供程序返回固定和映射逻辑磁盘上的文件。将来，将仅返回本地固定磁盘上的文件实例。 |
| `Win32_BaseBoard` | 表示基板，也称为主板或系统板。 |
| `Win32_BIOS` | 表示安装在计算机上的计算机系统基本输入/输出服务 (BIOS) 的属性。 |
| `Win32_BootConfiguration` | 表示 Windows 系统的启动配置。 |
| `Win32_CacheMemory` | 表示计算机系统上的内部和外部缓存内存。 |
| `Win32_CDROMDrive` | 表示 Windows 计算机系统上的 CD-ROM 驱动器。请注意，驱动器名称与分配给设备的逻辑驱动器号不对应。 |
| `Win32_ComputerSystem` | 表示 Windows 环境中的计算机系统。 |
| `Win32_ComputerSystemProduct` | 表示一个产品。这包括在此计算机系统上使用的软件和硬件。 |
| `Win32_DCOMApplication` | 表示 DCOM 应用程序的属性。 |
| `Win32_Desktop` | 表示用户桌面的通用特征。可以修改此类的属性以自定义桌面。 |
| `Win32_DesktopMonitor` | 表示连接到计算机系统的监视器或显示设备的类型。 |
| `Win32_DeviceMemoryAddress` | 表示 Windows 系统上的设备内存地址。 |
| `Win32_Directory` | 表示 Windows 计算机系统上的目录项。*目录* 是一种文件类型，可对数据文件进行逻辑分组，并为分组文件提供路径信息。`Win32_Directory` 不包含网络驱动器的目录。 |
| `Win32_DiskDrive` | 表示运行 Windows 操作系统的计算机所看到的物理磁盘驱动器。与 Windows 物理磁盘驱动器交互的任何接口都是此类的后代（或成员）。通过此对象看到的磁盘驱动器功能对应于驱动器的逻辑和管理特征。在某些情况下，这可能不反映设备的实际物理特征。基于另一个逻辑设备的任何对象都不是此类的成员。 |
| `Win32_DiskPartition` | 表示 Windows 系统上物理磁盘分区区域的功能和管理容量（例如，磁盘 #0，分区 #1）。 |
| `Win32_DiskQuota` | 跟踪 NTFS 文件系统卷的磁盘空间使用情况。系统管理员可以配置 Windows，以便在用户超过指定的磁盘空间限制时阻止进一步使用磁盘空间并记录事件。管理员也可以在用户超过指定的磁盘空间警告级别时记录事件。此类是在 Windows XP 中新增的。 |
| `Win32_DMAChannel` | 表示 Windows 计算机系统上的直接内存访问 (DMA) 通道。DMA 是一种在设备和内存之间移动数据（反之亦然）的方法，无需微处理器的帮助。系统板使用 DMA 控制器来处理固定数量的通道，每个通道一次只能由一个（且仅一个）设备使用。 |
| `Win32_Environment` | 表示 Windows 计算机系统上的环境或系统环境设置。查询此类将返回在 `HKLM\System\CurrentControlSet\Control\Sessionmanager\Environment` 以及 `HKEY_USERS\<user sid>\Environment` 中找到的环境变量。 |
| `Win32_Group` | 表示组帐户的数据。组帐户允许更改用户列表的访问权限（例如，Administrators）。 |
| `Win32_IDEController` | 管理集成设备电子 (IDE) 控制器设备的功能。 |
| `Win32_IRQResource` | 表示 Windows 计算机系统上的中断请求线 (IRQ) 号。中断请求是设备或程序为处理时间关键型事件而发送给 CPU 的信号。IRQ 可以是基于硬件或软件的。 |
| `Win32_LoadOrderGroup` | 表示定义执行依赖关系的一组系统服务。必须按照加载顺序组指定的顺序启动这些服务，因为这些服务相互依赖。这些依赖服务需要前置服务的存在才能正常运行。此类中的数据由提供程序从注册表项 `System\CurrentControlSet\Control\GroupOrderList` 派生。 |
| `Win32_LogicalDisk` | 表示解析为 Windows 系统上实际本地存储设备的数据源。 |
| `Win32_LogonSession` | 描述与登录到 Windows NT 或 Windows 2000 的用户关联的登录会话。 |
| `Win32_NetworkAdapter` | 表示运行 Windows 操作系统的计算机的网络适配器。 |
| `Win32_NetworkAdapterConfiguration` | 表示网络适配器的属性和行为。此类包含额外的属性和方法，支持独立于网络适配器的 TCP/IP 和 Internetwork Packet Exchange (IPX) 协议的管理。 |
| `WIN32_NetworkClient` | 表示 Windows 系统上的网络客户端。网络上与该系统具有客户端关系的任何计算机系统都是此类的后代（或成员）（例如，作为 Windows 2000 域一部分的运行 Windows 2000 Workstation 或 Windows 98 的计算机）。 |
| `Win32_NetworkConnection` | 表示 Windows 环境中的活动网络连接。 |




| Class | Description |
|-------|-------------|
| `Win32_NetworkLoginProfile` | 表示 Windows 系统上特定用户的网络登录信息。这包括但不限于密码状态、访问权限、磁盘配额和登录目录路径。 |
| `Win32_NetworkProtocol` | 表示 Win32 计算机系统上的协议及其网络特征。 |
| `Win32_NTDomain` | 表示 Windows NT 域。 |
| `Win32_NTEventlogFile` | 表示 Windows NT 事件的逻辑文件或目录。该文件也称为事件日志。 |
| `Win32_NTLogEvent` | 用于转换来自 Windows NT 事件日志的实例。应用程序必须具有 `SeSecurityPrivilege` 才能从安全事件日志接收事件；否则，将向应用程序返回“拒绝访问”。 |
| `Win32_OnBoardDevice` | 表示内置在主板（系统板）上的常见适配器设备。 |
| `Win32_OperatingSystem` | 表示安装在运行 Windows 操作系统的计算机上的操作系统。任何可安装在 Windows 系统上的操作系统都是该类的后代或成员。`Win32_OperatingSystem` 是一个单例类。要获取单个实例，请对键使用 `@`。Windows Server 2003、Windows XP、Windows 2000 和 Windows NT 4.0：如果计算机安装了多个操作系统，此类仅返回当前活动操作系统的实例。 |
| `Win32_OSRecoveryConfiguration` | 表示操作系统故障时将从内存中收集的信息类型。这包括启动失败和系统崩溃。 |
| `Win32_PageFileSetting` | 表示页面文件的设置。从此类实例化的对象中包含的信息指定了在系统启动时创建文件时使用的页面文件参数。此类中的属性可以被修改并延迟到启动时生效。这些设置不同于通过关联类 `Win32_PageFileUsage` 表示的页面文件运行时状态。 |
| `Win32_PageFileUsage` | 表示用于处理 Win32 系统上虚拟内存文件交换的文件。从此类实例化的对象中包含的信息指定了页面文件的运行时状态。 |
| `Win32_PerfRawData_PerfNet_Server` | 提供监视使用 WINS 服务器服务的通信的性能计数器的原始数据。 |
| `Win32_PhysicalMemoryArray` | 表示计算机系统物理内存的详细信息。这包括内存设备数量、可用内存容量和内存类型（例如系统内存或视频内存）。 |
| `Win32_PortConnector` | 表示物理连接端口，例如 DB-25 针公头、Centronics 或 PS/2。 |
| `Win32_PortResource` | 表示 Windows 计算机系统上的 I/O 端口。 |
| `Win32_Printer` | 表示连接到运行 Microsoft Windows 操作系统的计算机上的设备，该设备可以在纸张或其他介质上生成打印图像或文本。 |
| `Win32_PrinterConfiguration` | 表示打印机设备的配置。这包括分辨率、颜色、字体和方向等功能。 |
| `Win32_PrintJob` | 表示由 Windows 应用程序生成的打印作业。在运行 Windows 操作系统的计算机上运行的应用程序的“打印”命令生成的任何工作单元都是该类的后代或成员。 |
| `Win32_Process` | 表示操作系统上的进程。 |
| `Win32_Processor` | 表示在运行 Windows 操作系统的计算机上能够解释指令序列的设备。在多处理器计算机上，每个处理器存在一个 `Win32_Processor` 类实例。 |
| `Win32_Product` | 表示由 Windows Installer 安装的产品。产品通常与一个安装包相关联。有关特定操作系统的安装支持或要求的信息，请访问 [Microsoft 开发人员文档站点](https://aka.ms/wmi) 并搜索“WMI 组件的操作系统可用性”。 |
| `Win32_QuickFixEngineering` | 表示已应用于当前操作系统的系统范围的 Quick Fix Engineering (QFE) 或更新。 |
| `Win32_QuotaSetting` | 包含卷上磁盘配额的设置信息。 |
| `Win32_Registry` | 表示 Windows 计算机系统上的系统注册表。 |
| `Win32_ScheduledJob` | 表示使用 `AT` 命令创建的作业。`Win32_ScheduledJob` 类不表示使用控制面板中的“计划任务向导”创建的作业。您无法在“计划任务”UI 中更改由 WMI 创建的任务。Windows 2000 和 Windows NT 4.0：您可以使用“计划任务”UI 修改最初用 WMI 创建的任务。但是，尽管任务已成功修改，但您无法再使用 WMI 访问该任务。针对计划服务安排的每个作业都会持久存储（计划程序可以在重新启动后启动作业），并在指定的时间和星期几或月份执行。如果计算机在指定的作业时间未处于活动状态，或者计划服务未运行，则计划服务会在第二天的指定时间运行指定的作业。作业根据协调世界时 (UTC) 并带有相对于格林威治标准时间 (GMT) 的偏移进行安排，这意味着可以使用任何时区来指定作业。枚举对象时，`Win32_ScheduledJob` 类返回带有 UTC 偏移的本地时间，创建新作业时转换为本地时间。例如，指定在波士顿的计算机上于周一晚上 10:30 PST 运行的作业，将安排在周二凌晨 1:30 EST 本地运行。请注意，客户端必须考虑本地计算机上是否实行夏令时，如果是，则从 UTC 偏移中减去 60 分钟的偏差。 |
| `Win32_SCSIController` | 表示 Windows 系统上的 SCSI 控制器。 |
| `Win32_Service` | 表示运行 Microsoft Windows 操作系统的计算机上的服务。服务应用程序符合服务控制管理器 (SCM) 的接口规则，可以通过“服务”控制面板实用程序由用户在系统启动时自动启动，或由使用 Windows API 中包含的服务函数的应用程序启动。服务可以在没有用户登录计算机时启动。 |
| `Win32_Share` | 表示 Windows 系统上的共享资源。这可能是磁盘驱动器、打印机、进程间通信或其他可共享设备。 |
| `Win32_SoftwareElement` | 表示软件元素，它是软件功能的一部分（产品的一个不同子集，可能包含一个或多个元素）。每个软件元素都在一个 `Win32_SoftwareElement` 实例中定义，功能与其 `Win32_SoftwareFeature` 实例之间的关联在 `Win32_SoftwareFeatureSoftwareElements` 关联类中定义。有关在特定操作系统上安装的支持或要求的信息，请访问 [Microsoft 开发人员文档站点](https://aka.ms/wmi) 并搜索“WMI 组件的操作系统可用性”。 |
| `Win32_SoftwareFeature` | 表示由一个或多个软件元素组成的产品的一个不同子集。每个软件元素都在一个 `Win32_SoftwareElement` 实例中定义，功能与其 `Win32_SoftwareFeature` 实例之间的关联在 `Win32_SoftwareFeatureSoftwareElements` 关联类中定义。有关在特定操作系统上安装的支持或要求的信息，请访问 [Microsoft 开发人员文档站点](https://aka.ms/wmi) 并搜索“WMI 组件的操作系统可用性”。 |
| `WIN32_SoundDevice` | 表示 Windows 计算机系统上声音设备的属性。 |




# WMI 类

| 类 | 说明 |
|-------|-------------|
| `Win32_StartupCommand` | 表示用户登录到计算机系统时自动运行的命令。 |
| `Win32_SystemAccount` | 表示系统账户。操作系统以及在 Windows NT 下运行的服务使用系统账户。Windows NT 中有许多服务和进程需要内部登录的能力——例如，在 Windows NT 安装期间。系统账户就是为此目的而设计的。 |
| `Win32_SystemDriver` | 表示基础服务的系统驱动程序。 |
| `Win32_SystemEnclosure` | 表示与物理系统机箱关联的属性。 |
| `Win32_SystemSlot` | 表示物理连接点，包括端口、主板插槽和外设以及专有连接点。 |
| `Win32_TapeDrive` | 表示 Windows 计算机上的磁带驱动器。磁带驱动器主要通过只能按顺序访问这一事实来区分。 |
| `Win32_TemperatureProbe` | 表示温度传感器的属性（例如电子温度计）。 |
| `Win32_TimeZone` | 表示 Windows 系统的时区信息，其中包括夏令时转换所需的更改。 |
| `Win32_UserAccount` | 包含有关运行 Windows 操作系统的计算机上的用户账户的信息。由于 `Name` 和 `Domain` 都是关键属性，在大型网络上枚举 `Win32_UserAccount` 可能会对性能产生负面影响。调用 `GetObject` 或查询特定实例的影响较小。 |
| `Win32_VoltageProbe` | 表示电压传感器的属性（电子电压表）。 |
| `Win32_VolumeQuotaSetting` | 将磁盘配额设置与特定磁盘卷关联。Windows 2000/NT：此类不可用。 |
| `Win32_WMISetting` | 包含 WMI 服务的操作参数。此类只能有一个实例，该实例始终存在于每个 Windows 系统中且无法删除。无法创建其他实例。 |

# 附录 H. 选定的 COM 对象及其用途

作为扩展性和管理接口，许多应用程序通过 COM 对象公开有用的功能。尽管 PowerShell 直接处理许多此类任务，但许多 COM 对象仍然提供重要价值。

表 H-1. COM 标识符和说明

| 标识符 | 说明 |
|------------|-------------|
| `Access.Application` | 允许与 Microsoft Access 进行交互和自动化。 |
| `Agent.Control` | 允许控制 Microsoft Agent 3D 动画角色。 |
| `AutoItX3.Control` | (非默认) 通过 `AutoIt` 管理工具提供对 Windows 自动化的访问。 |
| `CEnroll.CEnroll` | 提供对证书注册服务的访问。 |
| `CertificateAuthority.Request` | 提供对证书颁发机构请求的访问。 |
| `COMAdmin.COMAdminCatalog` | 提供对 Windows COM+ 目录的访问和管理。 |
| `Excel.Application` | 允许与 Microsoft Excel 进行交互和自动化。 |
| `Excel.Sheet` | 允许与 Microsoft Excel 工作表进行交互。 |
| `HNetCfg.FwMgr` | 提供对 Windows 防火墙管理功能的访问。 |
| `HNetCfg.HNetShare` | 提供对 Windows 连接共享管理功能的访问。 |
| `HTMLFile` | 允许与新的 Internet Explorer 文档进行交互和创作。 |
| `InfoPath.Application` | 允许与 Microsoft InfoPath 进行交互和自动化。 |
| `InternetExplorer.Application` | 允许与 Internet Explorer 进行交互和自动化。 |
| `IXSSO.Query` | 允许与 Microsoft Index Server 进行交互。 |
| `IXSSO.Util` | 提供与 `IXSSO.Query` 对象一起使用的实用程序的访问。 |
| `LegitCheckControl.LegitCheck` | 提供对当前计算机上 Windows 正版优势状态信息的访问。 |
| `MakeCab.MakeCab` | 提供创建和管理 Cabinet (*.cab*) 文件的功能。 |
| `MAPI.Session` | 提供对消息应用程序编程接口 (MAPI) 会话的访问，如文件夹、消息和通讯簿。 |
| `Messenger.MessengerApp` | 允许与 Messenger 进行交互和自动化。 |
| `Microsoft.FeedsManager` | 允许与 Microsoft RSS 订阅平台进行交互。 |
| `Microsoft.ISAdm` | 提供对 Microsoft Index Server 的管理。 |
| `Microsoft.Update.AutoUpdate` | 提供对 Microsoft Update 自动更新计划的管理。 |
| `Microsoft.Update.Installer` | 允许从 Microsoft Update 安装更新。 |
| `Microsoft.Update.Searcher` | 提供对来自 Microsoft Update 的更新的搜索功能。 |
| `Microsoft.Update.Session` | 提供对 Microsoft Update 历史记录本地信息的访问。 |
| `Microsoft.Update.SystemInfo` | 提供对当前系统与 Microsoft Update 相关信息的访问。 |
| `MMC20.Application` | 允许与 Microsoft 管理控制台 (MMC) 进行交互和自动化。 |
| `MSScriptControl.ScriptControl` | 允许对 WSH 脚本进行评估和控制。 |
| `Msxml2.XSLTemplate` | 允许处理 XSL 转换。 |
| `Outlook.Application` | 允许通过 Microsoft Outlook 与您的电子邮件、日历、联系人、任务等进行交互和自动化。 |
| `OutlookExpress.MessageList` | 允许通过 Microsoft Outlook Express 与您的电子邮件进行交互和自动化。 |
| `PowerPoint.Application` | 允许与 Microsoft PowerPoint 进行交互和自动化。 |
| `Publisher.Application` | 允许与 Microsoft Publisher 进行交互和自动化。 |
| `RDS.DataSpace` | 提供对远程 DataSpace 业务对象代理的访问。 |
| `SAPI.SpVoice` | 提供对 Microsoft 语音 API 的访问。 |
| `Scripting.FileSystemObject` | 提供对计算机文件系统的访问。大多数功能可通过 PowerShell 或 PowerShell 对 .NET Framework 的支持更直接地获得。 |
| `Scripting.Signer` | 提供对 WSH 文件数字签名的管理。 |
| `Scriptlet.TypeLib` | 允许动态创建脚本类型库 (*.tlb*) 文件。 |
| `ScriptPW.Password` | 允许以掩码方式输入明文密码。如果可能，应避免使用此方法，而应首选带有 `-AsSecureString` 参数的 `Read-Host` cmdlet。 |
| `SharePoint.OpenDocuments` | 允许与 Microsoft SharePoint Services 进行交互。 |
| `Shell.Application` | 提供对 Windows 资源管理器 Shell 应用程序各方面的访问，如管理窗口、文件和文件夹以及当前会话。 |
| `Shell.LocalMachine` | 提供与 Windows Shell 相关的当前计算机信息的访问。 |
| `Shell.User` | 提供对当前用户的 Windows 会话和配置文件各方面的访问。 |
| `SQLDMO.SQLServer` | 提供对 Microsoft SQL Server 管理功能的访问。 |
| `Vim.Application` | (非默认) 允许与 VIM 编辑器进行交互和自动化。 |
| `WIA.CommonDialog` | 通过 Windows 图像获取功能提供对图像捕获的访问。 |
| `WMPlayer.OCX` | 允许与 Windows Media Player 进行交互和自动化。 |
| `Word.Application` | 允许与 Microsoft Word 进行交互和自动化。 |
| `Word.Document` | 允许与 Microsoft Word 文档进行交互。 |
| `WScript.Network` | 提供对网络 Windows 环境各方面的访问，如打印机和网络驱动器，以及计算机和域信息。 |
| `WScript.Shell` | 提供对 Windows Shell 各方面的访问，如应用程序、快捷方式、环境变量、注册表和操作环境。 |





| 类 | 说明 |
|-------|-------------|
| `WSHController` | 允许在远程计算机上执行 WSH 脚本。 |

# 附录 I. 选定事件及其用途

PowerShell 的事件命令使您能够访问来自 .NET Framework 的事件，以及由 Windows Management Instrumentation (WMI) 暴露的事件。表 I-1 列出了一系列 .NET 事件。表 I-2 列出了一系列 WMI 事件。

## 表 I-1. 选定的 .NET 事件

| 类型 | 事件 | 说明 |
|------|-------|-------------|
| `System.AppDomain` | `AssemblyLoad` | 当加载程序集时发生。 |
| `System.AppDomain` | `TypeResolve` | 当类型的解析失败时发生。 |
| `System.AppDomain` | `ResourceResolve` | 当资源的解析失败，因为该资源不是程序集中有效的链接资源或嵌入资源时发生。 |
| `System.AppDomain` | `AssemblyResolve` | 当程序集的解析失败时发生。 |
| `System.AppDomain` | `ReflectionOnlyAssemblyResolve` | 当在反射上下文中程序集的解析失败时发生。 |
| `System.AppDomain` | `UnhandledException` | 当异常未被捕获时发生。 |
| `System.Console` | `CancelKeyPress` | 当同时按下 Control 修饰键 (Ctrl) 和 C 控制台键 (C) 时发生 (Ctrl+C)。 |
| `Microsoft.Win32.SystemEvents` | `DisplaySettingsChanging` | 当显示设置正在更改时发生。 |
| `Microsoft.Win32.SystemEvents` | `DisplaySettingsChanged` | 当用户更改显示设置时发生。 |
| `Microsoft.Win32.SystemEvents` | `InstalledFontsChanged` | 当用户向系统添加或移除字体时发生。 |
| `Microsoft.Win32.SystemEvents` | `LowMemory` | 当系统可用内存 (RAM) 不足时发生。 |
| `Microsoft.Win32.SystemEvents` | `PaletteChanged` | 当用户切换到使用不同调色板的应用程序时发生。 |
| `Microsoft.Win32.SystemEvents` | `PowerModeChanged` | 当用户挂起或恢复系统时发生。 |
| `Microsoft.Win32.SystemEvents` | `SessionEnded` | 当用户正在注销或关闭系统时发生。 |
| `Microsoft.Win32.SystemEvents` | `SessionEnding` | 当用户尝试注销或关闭系统时发生。 |
| `Microsoft.Win32.SystemEvents` | `SessionSwitch` | 当前登录用户发生更改时发生。 |
| `Microsoft.Win32.SystemEvents` | `TimeChanged` | 当用户更改系统时钟上的时间时发生。 |
| `Microsoft.Win32.SystemEvents` | `UserPreferenceChanged` | 当用户首选项已更改时发生。 |
| `Microsoft.Win32.SystemEvents` | `UserPreferenceChanging` | 当用户首选项正在更改时发生。 |
| `System.Net.WebClient` | `OpenReadCompleted` | 当打开包含资源的流的异步操作完成时发生。 |
| `System.Net.WebClient` | `OpenWriteCompleted` | 当打开用于向资源写入数据的流的异步操作完成时发生。 |
| `System.Net.WebClient` | `DownloadStringCompleted` | 当异步资源下载操作完成时发生。 |
| `System.Net.WebClient` | `DownloadDataCompleted` | 当异步数据下载操作完成时发生。 |
| `System.Net.WebClient` | `DownloadFileCompleted` | 当异步文件下载操作完成时发生。 |
| `System.Net.WebClient` | `UploadStringCompleted` | 当异步字符串上传操作完成时发生。 |
| `System.Net.WebClient` | `UploadDataCompleted` | 当异步数据上传操作完成时发生。 |
| `System.Net.WebClient` | `UploadFileCompleted` | 当异步文件上传操作完成时发生。 |
| `System.Net.WebClient` | `UploadValuesCompleted` | 当异步名称/值集合上传完成时发生。 |
| `System.Net.WebClient` | `DownloadProgressChanged` | 当异步下载操作成功传输部分或全部数据时发生。 |
| `System.Net.WebClient` | `UploadProgressChanged` | 当异步上传操作成功传输部分或全部数据时发生。 |
| `System.Net.Sockets.SocketAsyncEventArgs` | `Completed` | 用于完成异步操作的事件。 |
| `System.Net.NetworkInformation.NetworkChange` | `NetworkAvailabilityChanged` | 当网络可用性发生更改时发生。 |
| `System.Net.NetworkInformation.NetworkChange` | `NetworkAddressChanged` | 当网络接口的 IP 地址发生更改时发生。 |
| `System.IO.FileSystemWatcher` | `Changed` | 当指定路径中的文件或目录被更改时发生。 |
| `System.IO.FileSystemWatcher` | `Created` | 当指定路径中创建文件或目录时发生。 |
| `System.IO.FileSystemWatcher` | `Deleted` | 当指定路径中删除文件或目录时发生。 |
| `System.IO.FileSystemWatcher` | `Renamed` | 当指定路径中重命名文件或目录时发生。 |
| `System.Timers.Timer` | `Elapsed` | 当间隔期满时发生。 |
| `System.Diagnostics.EventLog` | `EntryWritten` | 当条目被写入本地计算机上的事件日志时发生。 |
| `System.Diagnostics.Process` | `OutputDataReceived` | 当应用程序写入其重定向的 StandardOutput 流时发生。 |
| `System.Diagnostics.Process` | `ErrorDataReceived` | 当应用程序写入其重定向的 StandardError 流时发生。 |
| `System.Diagnostics.Process` | `Exited` | 当进程退出时发生。 |
| `System.IO.Ports.SerialPort` | `ErrorReceived` | 表示处理 `SerialPort` 对象错误事件的方法。 |
| `System.IO.Ports.SerialPort` | `PinChanged` | 表示将处理 `SerialPort` 对象串口引脚更改事件的方法。 |
| `System.IO.Ports.SerialPort` | `DataReceived` | 表示将处理 `SerialPort` 对象数据接收事件的方法。 |
| `System.Management.Automation.Job` | `StateChanged` | 当作业状态更改时触发该事件，例如当作业在所有运行空间中完成或在任何一个运行空间中失败时。 |
| `System.Management.Automation.Debugger` | `DebuggerStop` | 当 PowerShell 停止执行脚本并因遇到断点或执行单步命令而进入调试器时引发该事件。 |
| `System.Management.Automation.Debugger` | `BreakpointUpdated` | 当断点更新时引发该事件，例如启用或禁用断点时。 |
| `System.Management.Automation.Runspaces.Runspace` | `StateChanged` | 当运行空间状态更改时引发该事件。 |
| `System.Management.Automation.Runspaces.Runspace` | `AvailabilityChanged` | 当运行空间可用性更改时引发该事件，例如当运行空间变为可用或忙碌时。 |
| `System.Management.Automation.Runspaces.Pipeline` | `StateChanged` | 当管道状态更改时引发该事件。 |
| `System.Management.Automation.PowerShell` | `InvocationStateChanged` | 当 PowerShell 对象的管道状态更改时引发该事件。 |
| `System.Management.Automation.PSDataCollection[T]` | `DataAdded` | 向集合添加数据后触发该事件。 |
| `System.Management.Automation.PSDataCollection[T]` | `Completed` | 调用 `Complete` 方法以指示不再向集合添加数据时触发该事件。 |
| `System.Management.Automation.Runspaces.RunspacePool` | `StateChanged` | 当运行空间池状态更改时引发该事件。 |
| `System.Management.Automation.Runspaces.PipelineReader[T]` | `DataReady` | 当数据被添加到缓冲区时触发该事件。 |





# 附录 I. 精选 .NET 和 WMI 事件

## 表 I-1. 精选 .NET 事件

| 类 | 事件 | 说明 |
|-------|-------|-------------|
| `System.Diagnostics.​Event⁠ing.Reader.​EventLogWatcher` | `EventRecordWritten` | 允许设置一个委托（事件处理程序方法），每当发布的事件与此对象的事件查询中指定的标准匹配时，就会调用该委托。 |
| `System.Data.Common.​DbCon⁠nec⁠tion` | `StateChange` | 当事件的状态发生变化时发生。 |
| `System.Data.SqlClient.​Sql⁠Bulk⁠Copy` | `SqlRowsCopied` | 每当处理的行数达到 `NotifyAfter` 属性指定的数量时发生。 |
| `System.Data.SqlClient.​Sql⁠Com⁠mand` | `StatementCompleted` | 当 Transact-SQL 语句执行完成时发生。 |
| `System.Data.SqlClient.​Sql⁠Con⁠nection` | `InfoMessage` | 当 SQL Server 返回警告或信息性消息时发生。 |
| `System.Data.SqlClient.​Sql⁠Con⁠nection` | `StateChange` | 当事件的状态发生变化时发生。 |
| `System.Data.SqlClient.​Sql⁠Data⁠Adapter` | `RowUpdated` | 在 `Update` 期间，命令针对数据源执行后发生。已尝试更新，因此会触发该事件。 |
| `System.Data.SqlClient.​Sql⁠Data⁠Adapter` | `RowUpdating` | 在 `Update` 期间，命令针对数据源执行前发生。已尝试更新，因此会触发该事件。 |
| `System.Data.SqlClient.​Sql⁠Data⁠Adapter` | `FillError` | 当填充操作期间发生错误时返回。 |
| `System.Data.SqlClient.​Sql⁠De⁠pendency` | `OnChange` | 当接收到与此 `SqlDependency` 对象关联的任何命令的通知时发生。 |

## 表 I-2. 精选 WMI 事件

### `__InstanceCreationEvent`

此事件类通用地表示 WMI 提供程序中实例的创建，例如进程、服务、文件等。此通用事件的注册如下所示：

```
$query = "SELECT * FROM __InstanceCreationEvent " +
            "WITHIN 5 " +
            "WHERE targetinstance is a
            'Win32_UserAccount'
Register-CimIndicationEvent -Query $query
```

### `__InstanceDeletionEvent`

此事件类通用地表示 WMI 提供程序中实例的删除，例如进程、服务、文件等。此通用事件的注册如下所示：

```
$query = "SELECT * FROM __InstanceDeletionEvent " +
            "WITHIN 5 " +
            "WHERE targetinstance is a
            'Win32_UserAccount'
Register-CimIndicationEvent -Query $query
```

### `__InstanceModificationEvent`

此事件类通用地表示 WMI 提供程序中实例的修改，例如进程、服务、文件等。此通用事件的注册如下所示：

```
$query = "SELECT * FROM __InstanceModificationEvent "
            + "WITHIN 5 " +
            "WHERE targetinstance is a
            'Win32_UserAccount'
Register-CimIndicationEvent -Query $query
```

### `Msft_WmiProvider_OperationEvent`

`Msft_WmiProvider_OperationEvent` 事件类是所有 WMI 提供程序事件的根定义。提供程序操作被定义为代表客户端通过 WMI 执行的某些操作，其结果是对提供程序可执行文件进行一次或多次调用。该类的属性定义了与正在执行的操作相关联的提供程序的标识，并与 `Msft_Providers` 类的实例唯一关联。在内部，WMI 可以包含任意数量的引用特定 `__Win32Provider` 实例的对象，因为它根据提供程序是否支持每用户或每区域设置实例化，以及提供程序托管在何处来区分每个对象。目前 `TransactionIdentifier` 始终为空字符串。

### `Win32_ComputerSystemEvent`

此事件类表示与计算机系统相关的事件。

### `Win32_ComputerShutdownEvent`

此事件类表示计算机已开始关机过程时的事件。

### `Win32_IP4RouteTableEvent`

`Win32_IP4RouteTableEvent` 类表示因计算机系统上 IP 路由的添加、删除或修改而导致的 IP 路由更改事件。

### `RegistryEvent`

注册表事件类允许您订阅涉及配置单元子树、键和特定值更改的事件。

### `RegistryKeyChangeEvent`

`RegistryKeyChangeEvent` 类表示特定键的更改。这些更改仅适用于该键，不适用于其子键。

### `RegistryTreeChangeEvent`

`RegistryTreeChangeEvent` 类表示某个键及其子键的更改。

### `RegistryValueChangeEvent`

`RegistryValueChangeEvent` 类表示特定键的单个值的更改。

### `Win32_SystemTrace`

`SystemTrace` 类是所有系统跟踪事件的基类。系统跟踪事件由内核记录器通过事件跟踪 API 引发。

### `Win32_ProcessTrace`

此事件是进程事件的基础事件。

### `Win32_ProcessStartTrace`

`ProcessStartTrace` 事件类指示已启动新进程。

### `Win32_ProcessStopTrace`

`ProcessStopTrace` 事件类指示进程已终止。

### `Win32_ModuleTrace`

`ModuleTrace` 事件类是模块事件的基础事件。

### `Win32_ModuleLoadTrace`

`ModuleLoadTrace` 事件类指示进程已加载新模块。

### `Win32_ThreadTrace`

`ThreadTrace` 事件类是线程事件的基础事件。

### `Win32_ThreadStartTrace`

`ThreadStartTrace` 事件类指示已启动新线程。

### `Win32_ThreadStopTrace`

`ThreadStopTrace` 事件类指示线程已终止。

### `Win32_PowerManagementEvent`

`Win32_PowerManagementEvent` 类表示因电源状态更改而导致的电源管理事件。这些状态更改与高级电源管理 (APM) 或高级配置与电源接口 (ACPI) 系统管理协议相关联。

### `Win32_DeviceChangeEvent`

`Win32_DeviceChangeEvent` 类表示因计算机系统上设备的添加、删除或修改而导致的设备更改事件。这包括硬件配置更改（扩展坞连接和断开）、硬件状态或新映射的设备（网络驱动器映射）。例如，当发送 `WM_DEVICECHANGE` 消息时，设备已发生更改。

### `Win32_SystemConfigurationChangeEvent`

`Win32_SystemConfigurationChangeEvent` 是一个事件类，指示系统上的设备列表已刷新，意味着已添加或删除了设备，或配置已更改。当发送 Windows 消息 `DevMgrRefreshOn<ComputerName>` 时，将引发此事件。确切的设备列表更改不包含在消息中，因此需要进行设备刷新以获取当前系统设置。受影响的配置更改示例包括 IRQ 设置、COM 端口和 BIOS 版本等。

### `Win32_VolumeChangeEvent`

`Win32_VolumeChangeEvent` 类表示因计算机系统上添加了驱动器号或装载的驱动器（例如 CD-ROM）而导致的本地驱动器事件。目前不支持网络驱动器。

# 附录 J. 标准 PowerShell 动词

Cmdlet 和脚本应使用 `Verb-Noun` 语法命名——例如 `Get-ChildItem`。官方指导原则是，除极少数例外情况外，cmdlet 应使用标准 PowerShell 动词。它们应避免任何可以映射到标准动词的同义词或概念。这使得管理员能够快速理解使用新名词的一组 cmdlet。

**注意：** 要快速访问此列表（不含定义），请输入 `Get-Verb`。




# 标准 PowerShell 动词

动词应使用现在时措辞，名词应使用单数形式。表 J-1 到 J-6 列出了标准 PowerShell 动词的不同类别。

## 表 J-1. 标准 PowerShell 常用动词

| 动词 | 含义 | 同义词 |
| --- | --- | --- |
| `Add` | 将资源添加到容器中，或将元素附加到另一个元素 | 追加、附加、连接、插入 |
| `Clear` | 从容器中移除所有元素 | 刷新、擦除、释放、取消标记、取消设置、置空 |
| `Close` | 移除对资源的访问 | 关闭、密封 |
| `Copy` | 将资源复制到另一个名称或容器 | 复制、克隆、复制 |
| `Enter` | 将资源设置为上下文 | 进入、Telnet、打开 |
| `Exit` | 返回到进入新上下文之前存在的上下文 | 退出、断开连接 |
| `Find` | 在未知上下文中搜索所需项目 | 挖掘、发现 |
| `Format` | 将项目转换为指定的结构或布局 | 布局、排列 |
| `Get` | 检索数据 | 读取、打开、类型、目录、获取、转储、获得、检查、查找、搜索 |
| `Hide` | 使显示不可见 | 抑制 |
| `Join` | 连接资源 | 组合、联合、连接、关联 |
| `Lock` | 锁定资源 | 限制、阻止 |
| `Move` | 移动资源 | 传输、重命名、迁移 |
| `New` | 创建新资源 | 创建、生成、构建、制作、分配 |
| `Open` | 启用对资源的访问 | 释放、解封 |
| `Optimize` | 提高资源的有效性 | 改进、修复 |
| `Pop` | 从堆栈顶部移除项目 | 移除、粘贴 |
| `Push` | 将项目放到堆栈顶部 | 放置、添加、复制 |
| `Redo` | 重复某个操作或撤销 Undo 操作 | 重复、重试、恢复 |
| `Resize` | 更改资源的大小 | 更改、增大、缩小 |
| `Remove` | 从容器中移除资源 | 删除、剪切 |
| `Rename` | 给资源一个新名称 | 重命名、交换 |
| `Reset` | 将资源恢复到预定义或原始状态 | 恢复、还原 |
| `Select` | 从较大的数据集创建数据子集 | 选取、Grep、筛选 |
| `Search` | 在集合中查找资源（或关于该资源的摘要信息）（不实际检索资源，而是提供检索时使用的信息） | 查找、获取、Grep、选择 |
| `Set` | 放置数据 | 写入、分配、配置 |
| `Show` | 检索、格式化并显示信息 | 显示、报告 |
| `Skip` | 在搜索或导航中跳过元素 | 绕过、跳转 |
| `Split` | 将数据分离成较小的元素 | 分割、切分、解析 |
| `Step` | 将进程或导航向前移动一个单位 | 下一个、迭代 |
| `Switch` | 在不同的替代方案或选项之间切换资源的状态 | 切换、更改、翻转 |
| `Undo` | 将资源设置为其之前的状态 | 还原、放弃 |
| `Unlock` | 解锁资源 | 释放、取消限制 |
| `Use` | 将资源应用于上下文或与之关联 | 使用、拥有 |
| `Watch` | 持续监视项目 | 监视、轮询 |

## 表 J-2. 标准 PowerShell 通信动词

| 动词 | 含义 | 同义词 |
| --- | --- | --- |
| `Connect` | 将源连接到目标 | 加入、Telnet |
| `Disconnect` | 将源与目标断开连接 | 中断、注销 |
| `Read` | 从非连接源获取信息 | 提示、获取 |
| `Receive` | 从连接的源获取信息 | 读取、接受、窥视 |
| `Send` | 将信息写入连接的目标 | 放置、广播、邮件 |
| `Write` | 将信息写入非连接的目标 | 输出、打印 |

## 表 J-3. 标准 PowerShell 数据动词

| 动词 | 含义 | 同义词 |
| --- | --- | --- |
| `Backup` | 备份数据 | 保存、刻录 |
| `Checkpoint` | 创建数据或其配置当前状态的快照 | 差异、开始事务 |
| `Compare` | 将资源与另一个资源进行比较 | 差异、Bc |
| `Compress` | 减小项目的大小或资源使用量 | 压缩、挤压、归档 |
| `Convert` | 当 cmdlet 支持双向转换或多种数据类型转换时，将一种表示形式更改为另一种 | 更改、调整大小、重采样 |
| `ConvertFrom` | 从一种主要输入转换为多种支持的输出 | 导出、输出、Out |
| `ConvertTo` | 从多种支持的输入转换为一种主要输出 | 导入、输入、In |
| `Dismount` | 从命名空间中的某个位置分离命名实体 | 卸载、取消链接 |
| `Edit` | 就地修改项目 | 更改、修改、变更 |
| `Expand` | 增加项目的大小或资源使用量 | 展开、解压 |
| `Export` | 将主要输入资源存储到后备存储或交换格式中 | 提取、备份 |
| `Group` | 将项目与其他相关项目组合 | 合并、组合、映射 |
| `Import` | 从后备存储或交换格式创建主要输出资源 | 加载、读取 |
| `Initialize` | 准备资源以供使用并将其初始化为默认状态 | 设置、更新、重建 |
| `Limit` | 对资源应用约束 | 配额、强制执行 |
| `Merge` | 从多个数据集创建单个数据实例 | 组合、联接 |
| `Mount` | 将命名实体附加到命名空间中的某个位置 | 附加、链接 |
| `Out` | 将数据发送到终端位置 | 打印、格式化、发送 |
| `Publish` | 使资源为他人所知或可见 | 部署、发布、安装 |
| `Restore` | 将资源恢复到预定义的一组条件或由检查点设置的条件 | 修复、返回、修正 |
| `Save` | 将挂起的更改存储到可恢复的存储中 | 写入、保留、提交 |
| `Sync` | 使两个资源相互同步 | 推送、更新 |
| `Unpublish` | 从公共可见性中移除资源 | 卸载、还原 |
| `Update` | 更新或刷新资源 | 刷新、更新、编制索引 |

## 表 J-4. 标准 PowerShell 诊断动词

| 动词 | 含义 | 同义词 |
| --- | --- | --- |
| `Debug` | 检查资源，诊断操作问题 | 附加、诊断 |
| `Measure` | 识别操作消耗的资源或检索关于资源的统计信息 | 计算、确定、分析 |
| `Ping` | 确定资源是否处于活动且响应状态（在大多数情况下，应由动词 `Test` 替代） | 连接、调试 |
| `Repair` | 从损坏或故障状态恢复项目 | 修复、恢复、重建 |
| `Resolve` | 将简写表示映射到更完整的表示 | 展开、确定 |
| `Test` | 验证资源的有效性或一致性 | 诊断、验证、分析 |
| `Trace` | 跟踪资源的活动 | 检查、挖掘 |

## 表 J-5. 标准 PowerShell 生命周期动词

| 动词 | 含义 | 同义词 |
| --- | --- | --- |
| `Approve` | 批准或允许某个项目或资源 | 允许、许可 |
| `Assert` | 声明某个项目或事实的状态 | 验证、检查 |
| `Build` | 从某组输入文件（通常是源代码或声明性文档）创建构件（通常是二进制文件或文档） | 编译、生成 |
| `Complete` | 完成挂起的操作 | 完成、结束 |
| `Confirm` | 批准或确认资源或进程 | 检查、验证 |
| `Deny` | 拒绝或不允许资源或进程 | 失败、停止 |
| `Deploy` | 将应用程序、网站或解决方案发送到远程目标，以便该解决方案的使用者在部署完成后可以访问它 | 发布、发行 |
| `Disable` | 将项目配置为不可用 | 停止、隐藏 |




## 标准 PowerShell 动词

| 动词 | 含义 | 同义词 |
|------|---------|----------|
| `Enable` | 将某项配置为可用 | Allow, Permit |
| `Install` | 将资源放置在指定位置并可选择性地对其进行初始化 | Setup, Configure |
| `Invoke` | 调用或启动无法停止的活动 | Run, Call, Perform |
| `Register` | 将某项添加到受监视或发布的资源中 | Record, Submit, Journal, Subscribe |
| `Request` | 提交以供考虑或批准 | Ask, Query |
| `Restart` | 停止操作并再次启动 | Recycle, Hup |
| `Resume` | 在操作挂起后开始执行 | Continue |
| `Start` | 开始某项活动 | Launch, Initiate |
| `Stop` | 终止某项活动 | Halt, End, Discontinue |
| `Submit` | 添加到待处理操作列表或发送以供批准 | Send, Post |
| `Suspend` | 暂停操作，但不终止 | Pause, Sleep, Break |
| `Uninstall` | 从指定位置移除资源 | Remove, Clear, Clean |
| `Unregister` | 从受监视或发布的资源中移除某项 | Unsubscribe, Erase, Remove |
| `Wait` | 暂停直到预期事件发生 | Sleep, Pause, Join |

## 表 J-6. 标准 PowerShell 安全动词

| 动词 | 含义 | 同义词 |
|------|---------|----------|
| `Block` | 限制对资源的访问 | Prevent, Limit, Deny |
| `Grant` | 授予对资源的访问权限 | Allow, Enable |
| `Protect` | 限制对资源的访问 | Encrypt, Seal |
| `Revoke` | 移除对资源的访问权限 | Remove, Disable |
| `Unblock` | 移除对资源访问的限制 | Clear, Allow |
| `Unprotect` | 从受保护资源中移除限制 | Decrypt, Decode |
