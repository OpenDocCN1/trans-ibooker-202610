# 第 7 章 列表、数组和哈希表

# 7.0 简介

大多数脚本处理的不止一件事——服务器列表、文件列表、查找代码等。为了实现这一点，PowerShell 通过其语言特性和实用 cmdlet 提供了许多功能来帮助你完成这些任务。

PowerShell 让处理数组和列表就像处理其他数据类型一样：你可以轻松创建数组或列表，然后添加或删除元素。你也可以同样轻松地对其进行排序、搜索或与另一个数组合并。当你想要存储一条数据到另一条数据的映射时，哈希表完美满足该需求。

# 7.1 创建项目数组或列表

## 问题

你想要创建一个项目数组或列表。

## 解决方案

要创建一个包含给定项目集的数组，请用逗号分隔这些项目：

```
PS > $myArray = 1,2,"Hello World"
PS > $myArray
1
2
Hello World
```

要创建特定大小的数组，请使用 `New-Object` cmdlet：

```
PS > $myArray = New-Object string[] 10
PS > $myArray[5] = "Hello"
PS > $myArray[5]
Hello
```

要创建特定类型的数组，请使用强类型集合：

```
PS > $list = New-Object Collections.Generic.List[Int]
PS > $list.Add(10)
PS > $list.Add("Hello")
无法转换参数 "0"，其值为: "Hello"，用于 "Add" 类型 "System.Int32": "无法将值 "Hello" 转换为类型 "System.Int32"。错误: "输入字符串的格式不正确。""
```

要存储生成列表的命令的输出，请使用变量赋值：



```
PS > $myArray = Get-Process
PS > $myArray

Handles  NPM(K)    PM(K)      WS(K) VM(M)  CPU(s)     Id ProcessName
-------  ------    -----      ----- -----  ------     -- -----------
    274       6     1316       3908    33           3164 alg
    983       7     3636       7472    30            688 csrss
     69       4      924       3332    30    0.69   2232 ctfmon
    180       5     2220       6116    37           2816 dllhost
(...)
```

要创建一个您计划频繁修改的数组，请使用 `ArrayList`，如 示例 7-1 所示。

### 示例 7-1. 使用 ArrayList 管理动态项目集合

```
PS > $myArray = New-Object System.Collections.ArrayList
PS > [void] $myArray.Add("Hello")
PS > [void] $myArray.AddRange( ("World","How","Are","You") )
PS > $myArray
Hello
World
How
Are
You
PS > $myArray.RemoveAt(1)
PS > $myArray
Hello
How
Are
You
```

## 讨论

除了基元数据类型（如字符串、整数和十进制数）外，项目列表也是您编写的脚本和命令中的常见概念。大多数命令都会生成数据列表：`Get-Content` cmdlet 生成文件中的字符串列表，`Get-Process` cmdlet 生成系统上正在运行的进程列表，`Get-Command` cmdlet 生成命令列表，仅举几例。

#### 注意

解决方案展示了如何存储生成列表的命令的输出。如果命令仅输出一项（例如文件中的单行、单个进程或单个命令），则该输出不再是列表。如果您希望即使在输出不是列表时也将其视为列表，请使用列表求值语法 `` `@()` `` 强制 PowerShell 将其解释为数组：

```
$myArray = @(Get-Process Explorer)
```

当您想要创建特定类型的列表时，解决方案演示了如何使用 `System.Collections.Generic.List` 集合来实现。在类型名称后，您可以在方括号中定义列表的类型，例如 `[Int]`、`[String]`，或您想要限制集合使用的任何类型。这些类型的专用对象称为 *泛型对象*。有关创建泛型对象的详细信息，请参阅 “创建类型的实例”。

有关 PowerShell 中列表和数组的详细信息，请参阅 “数组和列表”。

## 另请参阅

“数组和列表”

“创建类型的实例”

# 7.2 创建交错数组或多维数组

## 问题

您想要创建一个数组的数组或一个多维数组。

## 解决方案

要创建数组的数组（*交错* 数组），请使用 `` `@()` `` 数组语法：

```
PS > $jagged = @(
      (1,2,3,4),
      (5,6,7,8)
   )

PS > $jagged[0][1]
2
PS > $jagged[1][3]
8
```

要创建（非交错）多维数组，请使用 `New-Object` cmdlet：

```
PS > $multidimensional = New-Object "int32[,]" 2,4
PS > $multidimensional[0,1] = 2
PS > $multidimensional[1,3] = 8
PS >
PS > $multidimensional[0,1]
2
PS > $multidimensional[1,3]
8
```

## 讨论

交错数组和多维数组可用于保存列表的列表和数组的数组。交错数组是数组的数组，其中每个数组仅包含其所需的元素数量。非交错数组更像是一个网格或矩阵，其中每个数组都需要具有相同的大小。交错数组更易于使用（且占用内存较少），但非交错多维数组在处理大型数据网格时有时很有用。

由于交错数组是数组的数组，在交错数组中创建项目遵循与在常规数组中创建项目相同的规则。如果任何数组是单元素数组，请使用一元逗号运算符。例如，要创建一个包含一个单元素嵌套数组的交错数组：

```
PS > $oneByOneJagged = @(
 ,(,1)
)

PS > $oneByOneJagged[0][0]
1
```

有关 PowerShell 中列表和数组的详细信息，请参阅 “数组和列表”。

## 另请参阅

“数组和列表”

# 7.3 访问数组元素

## 问题

您想要访问数组的元素。

## 解决方案

要访问数组的特定元素，请使用 PowerShell 的数组访问机制：

```
PS > $myArray = 1,2,"Hello World"
PS > $myArray[1]
2
```

要访问一系列数组元素，请使用数组范围和数组切片：

```
PS > $myArray = 1,2,"Hello World"
PS > $myArray[1..2 + 0]
2
Hello World
1
```

## 讨论

PowerShell 的数组访问机制提供了一种便捷的方式，用于访问数组的特定元素或该数组中更复杂的元素组合。在 PowerShell 中（与大多数其他脚本和编程语言一样），索引 0 处的项目表示数组中的第一个项目。

对于长项目列表，知道元素的索引有时会构成问题。有关此问题的解决方案，请参阅随本书代码示例附带的 `Add-FormatTableIndexParameter` 脚本。此脚本向 `Format-Table` cmdlet 添加了一个新的 `-IncludeIndex` 参数：

```
PS > $items = Get-Process outlook,powershell,emacs,notepad
PS > $items

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    163       6    17660      24136   576     7.63   7136 emacs
     74       4     1252       6184    56     0.19  11820 notepad
   3262      48    46664      88280   376    20.98   8572 OUTLOOK
    285      11    31328      21952   171   613.71   4716 powershell
    767      14    56568      66032   227   104.10  11368 powershell


PS > $items | Format-Table -IncludeIndex

PSIndex Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
------- -------  ------    -----      ----- -----   ------     -- -----------
0           163       6    17660      24136   576     7.63   7136 emacs
1            74       4     1252       6184    56     0.19  11820 notepad
2          3262      48    46664      88280   376    20.98   8572 OUTLOOK
3           285      11    31328      21952   171   613.71   4716 powershell
4           767      14    56568      66032   227   104.15  11368 powershell


PS > $items[2]

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   3262      48    46664      88280   376    20.98   8572 OUTLOOK
```

虽然通过数字索引处理数组元素很有帮助，但您可能会发现通过其他方式引用它们很有用——例如通过它们的名称，甚至自定义标签。这种类型的数组称为 *关联数组*（或 *哈希表*）。有关使用哈希表和关联数组的详细信息，请参阅 配方 7.13。

有关 PowerShell 中列表和数组的详细信息（包括数组范围和切片语法），请参阅 “数组和列表”。有关获取本书代码示例的详细信息，请参阅 “使用代码示例”。

## 另请参阅

配方 7.13，“创建哈希表或关联数组”

“数组和列表”

# 7.4 遍历数组的每个元素

## 问题

您想要处理数组的每个元素。

## 解决方案

要逐个访问数组中的每个项目，请使用 `ForEach-Object` cmdlet：

```
PS > $myArray = 1,2,3
PS > $sum = 0
PS > $myArray | ForEach-Object { $sum += $_ }
PS > $sum
6
```

要以更像脚本的方式访问数组中的每个项目，请使用 `foreach` 脚本关键字：

```
PS > $myArray = 1,2,3
PS > $sum = 0
PS > foreach($element in $myArray) { $sum += $element }
PS > $sum
6
```

要按位置访问数组中的项目，请使用 `for` 循环：




# PowerShell 数组操作

## 使用 `for` 循环遍历数组

```
PS > $myArray = 1,2,3
PS > $sum = 0
PS > for($counter = 0; $counter -lt $myArray.Count; $counter++) {
    $sum += $myArray[$counter]
}

PS > $sum
6
```

## 讨论

PowerShell 提供了三种主要的替代方法来处理数组中的元素。`ForEach-Object` cmdlet 和 `foreach` 脚本关键字技术逐个访问数组中的项目，而 `for` 循环（及相关的循环构造）则允许您以一种结构化程度较低的方式处理数组中的项目。

有关 `ForEach-Object` cmdlet 的更多信息，请参阅 食谱 2.5。

有关 `foreach` 脚本关键字、`for` 关键字和其他循环构造的更多信息，请参阅 食谱 4.4。

## 另请参阅

- 食谱 2.5，“处理列表或命令输出中的每个项目”
- 食谱 4.4，“使用循环重复操作”

# 7.5 对数组或项目列表进行排序

## 问题

您想要对数组或列表的元素进行排序。

## 解决方案

要对项目列表进行排序，请使用 `Sort-Object` cmdlet：

```
PS > Get-ChildItem | Sort-Object -Descending Length | Select Name,Length

Name                       Length
----                       ------
Convert-TextObject.ps1       6868
Select-FilteredObject.ps1    3252
Get-PageUrls.ps1             2878
Get-Characteristics.ps1      2515
Get-Answer.ps1               1890
New-GenericObject.ps1        1490
Invoke-CmdScript.ps1         1313
```

## 讨论

`Sort-Object` cmdlet 提供了一种便捷的方式，让您按指定的属性对项目进行排序。如果您不指定属性，`Sort-Object` cmdlet 将遵循这些项目定义的排序规则（如果有）。

`Sort-Object` cmdlet 还支持自定义排序表达式，而不仅仅是按现有属性排序。要按您自己的逻辑排序，请使用脚本块作为排序表达式。此示例按第二个字符排序：

```
PS > "Hello","World","And","PowerShell" | Sort-Object { $_.Substring(1,1) }
Hello
And
PowerShell
World
```

如果您想对保存在变量中的列表进行排序，可以将结果存回该变量，或使用 .NET Framework 的 `[Array]::Sort()` 方法：

```
PS > $list = "Hello","World","And","PowerShell"
PS > $list = $list | Sort-Object
PS > $list
And
Hello
PowerShell
World
PS > $list = "Hello","World","And","PowerShell"
PS > [Array]::Sort($list)
PS > $list
And
Hello
PowerShell
World
```

除了按升序或降序按属性或表达式排序外，`Sort-Object` cmdlet 的 `-Unique` 开关还允许您从排序后的集合中删除重复项。

有关 `Sort-Object` cmdlet 的更多信息，请输入 **`Get-Help Sort-Object`**。

# 7.6 确定数组是否包含某个项目

## 问题

您想要确定数组或列表是否包含特定项目。

## 解决方案

要确定列表是否包含特定项目，请使用 `-contains` 运算符：

```
PS > "Hello","World" -contains "Hello"
True
PS > "Hello","World" -contains "There"
False
```

或者，使用 `-in` 运算符，其作用类似于操作数颠倒的 `-contains` 运算符：

```
PS > "Hello" -in "Hello","World"
True
PS > "There" -in "Hello","World"
False
```

## 讨论

`-contains` 和 `-in` 运算符是快速确定列表是否包含特定元素的有用方法。要在列表中搜索匹配模式的项目，请使用 `-match` 或 `-like` 运算符。

有关 `-contains`、`-in`、`-match` 和 `-like` 运算符的更多信息，请参阅 “比较运算符”。

## 另请参阅

- “比较运算符”

# 7.7 合并两个数组

## 问题

您有两个数组，想要将它们合并为一个。

## 解决方案

要合并 PowerShell 数组，请使用加法运算符（`+`）：

```
PS > $firstArray = "Element 1","Element 2","Element 3","Element 4"
PS > $secondArray = 1,2,3,4
PS >
PS > $result = $firstArray + $secondArray
PS > $result
Element 1
Element 2
Element 3
Element 4
1
2
3
4
```

## 讨论

合并两个数组的一个常见原因是当您想要将数据添加到其中一个数组的末尾时。例如：

```
PS > $array = 1,2
PS > $array = $array + 3,4
PS > $array
1
2
3
4
```

您可以更清晰地写成：

```
PS > $array = 1,2
PS > $array += 3,4
PS > $array
1
2
3
4
```

然而，当以第二种形式编写时，您可能会认为 PowerShell 只是将项目添加到数组末尾，同时保持数组本身完好无损。事实并非如此，因为 PowerShell 中的数组（像大多数其他语言一样）一旦创建，长度保持不变。要合并两个数组，PowerShell 会创建一个足够大的新数组来容纳两个数组的内容，然后将两个数组都复制到目标数组中。

如果您计划频繁地向数组添加和删除数据，`System.Collections.ArrayList` 类提供了一个更动态的替代方案。有关使用 `ArrayList` 类的更多信息，请参阅 食谱 7.12。

## 另请参阅

- 食谱 7.12，“使用 ArrayList 类进行高级数组任务”

# 7.8 查找数组中与值匹配的项目

## 问题

您有一个数组，想要查找所有与给定项目或术语匹配的元素——无论是精确匹配、模式匹配还是正则表达式匹配。

## 解决方案

要查找所有与项目匹配的元素，请使用 `-eq`、`-like` 和 `-match` 比较运算符：

```
PS > $array = "Item 1","Item 2","Item 3","Item 1","Item 12"
PS > $array -eq "Item 1"
Item 1
Item 1
PS > $array -like "*1*"
Item 1
Item 1
Item 12
PS > $array -match "Item .."
Item 12
```

## 讨论

`-eq`、`-like` 和 `-match` 运算符是查找集合中与给定术语匹配的元素的有用方法。`-eq` 运算符返回所有等于您术语的元素，`-like` 运算符返回所有与您模式中给定的通配符匹配的元素，`-match` 运算符返回所有与您模式中给定的正则表达式匹配的元素。

对于更复杂的比较条件，`Where-Object` cmdlet 允许您查找满足更复杂条件的列表中的元素：

```
PS > $array = "Item 1","Item 2","Item 3","Item 1","Item 12"
PS > $array | Where-Object { $_.Length -gt 6 }
Item 12
```

有关筛选列表中项目的更多信息，请参阅 食谱 2.2。

有关 `-eq`、`-like` 和 `-match` 运算符的更多信息，请参阅 “比较运算符”。

## 另请参阅

- 食谱 2.2，“筛选列表或命令输出中的项目”
- “比较运算符”

# 7.9 比较两个列表

## 问题

您有两个列表，想要查找仅存在于其中一个列表中的项目。

## 解决方案

要比较两个列表，请使用 `Compare-Object` cmdlet：

```
PS > $array1 = "Item 1","Item 2","Item 3","Item 12"
PS > $array2 = "Item 1","Item 8","Item 3","Item 9","Item 12"
PS > Compare-Object $array1 $array2 -IncludeEqual

InputObject SideIndicator
----------- -------------
Item 1      ==
Item 3      ==
Item 12     ==
Item 8      =>
Item 9      =>
Item 2      <=
```

## 讨论

`Compare-Object` cmdlet 允许您比较两个列表。默认情况下，它仅显示仅存在于其中一个列表中的项目，尽管其 `-IncludeEqual` 参数允许您包含同时存在于两个列表中的项目。如果它不返回任何结果，则这两个列表相等。

有关比较数据的更多信息，请参阅 第 22 章。

## 另请参阅

- 第 22 章

# 7.10 从数组中删除元素

## 问题




你想要从数组中移除所有与给定项或术语匹配的元素——无论是完全匹配、模式匹配还是正则表达式匹配。

## 解决方案

要移除数组中与模式匹配的所有元素，请使用 `-ne`、`-notlike` 和 `-notmatch` 比较运算符，如 示例 7-2 所示。

### 示例 7-2. 使用 -ne、-notlike 和 -notmatch 运算符从数组中移除元素

```
PS > $array = "Item 1","Item 2","Item 3","Item 1","Item 12"
PS > $array -ne "Item 1"
Item 2
Item 3
Item 12
PS > $array -notlike "*1*"
Item 2
Item 3
PS > $array -notmatch "Item .."
Item 1
Item 2
Item 3
Item 1
```

要真正从数组中移除项目，请将结果存回数组：

```
PS > $array = "Item 1","Item 2","Item 3","Item 1","Item 12"
PS > $array = $array -ne "Item 1"
PS > $array
Item 2
Item 3
Item 12
```

## 讨论

`-eq`、`-like` 和 `-match` 运算符是查找集合中与给定术语匹配的元素的有用方法。它们的对立面，即 `-ne`、`-notlike` 和 `-notmatch` 运算符，返回所有与该给定术语不匹配的元素。

要移除数组中与给定模式匹配的所有元素，你可以保存所有*不*匹配该模式的元素。

有关 `-ne`、`-notlike` 和 `-notmatch` 运算符的更多信息，请参阅 “比较运算符”。

## 另请参阅

“比较运算符”

# 7.11 在数组中查找大于或小于指定值的项目

## 问题

你有一个数组，想要查找所有大于或小于给定项或值的元素。

## 解决方案

要查找所有大于或小于给定值的元素，请使用 `-gt`、`-ge`、`-lt` 和 `-le` 比较运算符：

```
PS > $array = "Item 1","Item 2","Item 3","Item 1","Item 12"
PS > $array -ge "Item 3"
Item 3
PS > $array -lt "Item 3"
Item 1
Item 2
Item 1
Item 12
```

## 讨论

`-gt`、`-ge`、`-lt` 和 `-le` 运算符是查找集合中大于或小于给定值的元素的有用方法。与所有其他 PowerShell 比较运算符一样，这些运算符使用集合中项目的比较规则。由于解决方案中的数组是字符串数组，此结果很容易让你感到惊讶：

```
PS > $array -lt "Item 2"
Item 1
Item 1
Item 12
```

当你查看排序后的数组时，原因就变得清晰了——`Item 12` 在字母顺序上排在 `Item 2` *之前*，这是 PowerShell 比较字符串数组的方式：

```
PS > $array | Sort-Object
Item 1
Item 1
Item 12
Item 2
Item 3
```

有关 `-gt`、`-ge`、`-lt` 和 `-le` 运算符的更多信息，请参阅 “比较运算符”。

## 另请参阅

“比较运算符”

# 7.12 使用 ArrayList 类执行高级数组任务

## 问题

你有一个数组，想要频繁地向其中添加元素、移除元素、搜索和修改。

## 解决方案

要在定义数组后频繁地处理它，请使用 `System.Collections.ArrayList` 类：

```
PS > $myArray = New-Object System.Collections.ArrayList
PS > [void] $myArray.Add("Hello")
PS > [void] $myArray.AddRange( ("World","How","Are","You") )
PS > $myArray
Hello
World
How
Are
You
PS > $myArray.RemoveAt(1)
PS > $myArray
Hello
How
Are
You
```

## 讨论

与大多数其他语言一样，PowerShell 中的数组一旦创建，其长度就保持不变。PowerShell 允许你在数组中添加项目、移除项目和搜索项目，但当你处理大量数据时，这些操作可能会很耗时。例如，要合并两个数组，PowerShell 会创建一个足够大的新数组来容纳两个数组的内容，然后将两个数组都复制到目标数组中。

相比之下，`ArrayList` 类旨在让你轻松地在集合中添加、移除和搜索项目。

### 注意

PowerShell 会传递脚本生成的任何数据，除非你捕获它或将其转换为 `[void]`。由于 `System.Collections.ArrayList` 类主要设计用于编程语言，它会产生输出，即使你可能不期望它这样做。为了防止它将数据发送到输出管道，请捕获数据或将其转换为 `[void]`：

```
PS > $collection = New-Object System.Collections.ArrayList
PS > $collection.Add("Hello")
0
PS > [void] $collection.Add("World")
```

如果你计划频繁地向数组添加和移除数据，`System.Collections.ArrayList` 类提供了一种更动态的替代方案。

有关使用 .NET Framework 类的更多信息，请参阅 配方 3.8。

## 另请参阅

配方 3.8，“使用 .NET 对象”

# 7.13 创建哈希表或关联数组

## 问题

你有一组项目，想要通过你提供的标签来访问它们。

## 解决方案

要定义标签和项目之间的映射，请使用哈希表（关联数组）：

```
PS > $myHashtable = @{ Key1 = "Value1"; "Key 2" = 1,2,3 }
PS > $myHashtable["New Item"] = 5
PS >
PS > $myHashTable

Name                           Value
----                           -----
Key 2                          {1, 2, 3}
New Item                       5
Key1                           Value1
```

## 讨论

哈希表非常类似于数组，但它们允许你通过任何想要的标签访问项目，而不仅仅是通过数组中的索引。由于这种自由度，它们构成了大量脚本技术的基石。因为它们允许你将名称映射到值，所以它们自然地构成了查找表的基础，例如邮政编码和区号查找表。因为它们允许你将名称映射到功能完整的对象和脚本块，所以它们通常可以替代自定义对象。而且因为你可以将丰富的对象映射到其他丰富的对象，它们甚至可以构成更高级数据结构（如缓存和对象图）的基础。

该解决方案演示了如何同时创建和初始化哈希表，但你也可以创建一个哈希表并逐步使用它：

```
PS > $myHashtable = @{}
PS > $myHashtable["Hello"] = "World"
PS > $myHashtable.AnotherHello = "AnotherWorld"
PS > $myHashtable

Name                           Value
----                           -----
AnotherHello                   AnotherWorld
Hello                          World
```

在使用哈希表时，你可能会注意到它们通常会按乱序列出元素——或者至少与插入顺序不同。要创建一个保留插入顺序的哈希表，请使用 `[ordered]` 类型转换，如 配方 7.14 所述。

将标签映射到结构化值的这种能力在与支持高级配置参数的 cmdlet 交互时也很有帮助，例如 `Format-Table` 和 `Select-Object` cmdlet 上可用的计算属性参数。有关此用法的示例，请参阅 配方 3.2。

有关使用哈希表的更多信息，请参阅 “哈希表（关联数组）”。

## 另请参阅

配方 3.2，“将项目的属性显示为表”

配方 7.14，“按键或值对哈希表进行排序”

“哈希表（关联数组）”

# 7.14 按键或值对哈希表进行排序

## 问题

你有一个包含键和值的哈希表，想要获取按键排序后生成的值列表。

## 解决方案

要对哈希表进行排序，请对哈希表使用 `GetEnumerator()` 方法以访问其各个元素。然后使用 `Sort-Object` cmdlet 按 `Name` 或 `Value` 排序：



```
foreach($item in $myHashtable.GetEnumerator() | Sort-Object Name)
{
    $item.Value
}
```

如果您控制哈希表的定义，请在定义哈希表时使用 `[Ordered]` 类型转换，以使其保留定义中提供的顺序。

```
$orderedHashtable = [Ordered] @{ Item1 = "Hello"; Item2 = "World" }
```

## 讨论

由于哈希表的主要焦点只是将键映射到值，因此它通常完全不保留任何顺序——例如您添加项的顺序、键的排序顺序或值的排序顺序。这在 示例 7-3 中变得清晰。

### 示例 7-3. 哈希表项不保留其顺序的演示

```
PS > $myHashtable = @{}
PS > $myHashtable["Hello"] = 3
PS > $myHashtable["Ali"] = 2
PS > $myHashtable["Alien"] = 4
PS > $myHashtable["Duck"] = 1
PS > $myHashtable["Hectic"] = 11
PS > $myHashtable

Name                           Value
----                           -----
Hectic                         11
Duck                           1
Alien                          4
Hello                          3
Ali                            2
```

然而，哈希表对象支持 `GetEnumerator()` 方法，该方法允许您处理单个哈希表条目——所有这些条目都具有 `Name` 和 `Value` 属性。一旦您拥有这些，我们可以像排序任何其他 PowerShell 数据一样轻松地按它们进行排序。示例 7-4 演示了此技术。

### 示例 7-4. 按名称和值对哈希表进行排序

```
PS > $myHashtable.GetEnumerator() | Sort-Object Name

Name                           Value
----                           -----
Ali                            2
Alien                          4
Duck                           1
Hectic                         11
Hello                          3

PS > $myHashtable.GetEnumerator() | Sort-Object Value

Name                           Value
----                           -----
Duck                           1
Ali                            2
Hello                          3
Alien                          4
Hectic                         11
```

通过使用 `[Ordered]` 类型转换，您可以创建一个保留您定义和添加项顺序的哈希表：

```
PS > $myHashtable = [Ordered] @{
    Duck = 1;
    Ali = 2;
    Hectic = 11;
    Alien = 4;
 }

PS > $myHashtable["Hello"] = 3
PS > $myHashtable

Name                           Value
----                           -----
Duck                           1
Ali                            2
Hectic                         11
Alien                          4
Hello                          3
```

有关使用哈希表的更多信息，请参阅 “哈希表（关联数组）”。

## 另请参阅

“哈希表（关联数组）”
