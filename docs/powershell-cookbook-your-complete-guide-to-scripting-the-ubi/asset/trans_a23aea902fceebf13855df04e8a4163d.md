

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

- `formatString` 为可选项，使用该类型特定的格式字符串语法对项进行格式化（如[表 D-1](#table-d-1-standard-numeric-format-strings)和[表 D-2](#table-d-2-custom-numeric-format-strings)所示）。

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

您可以使用[表 D-2](#table-d-2-custom-numeric-format-strings)中列出的自定义数字字符串，以标准格式字符串不支持的方式格式化数字。

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

