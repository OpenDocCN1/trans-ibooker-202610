
## 使用正则表达式进行模式匹配

正则表达式对于验证用户输入非常有用。它们非常强大，也可能变得非常复杂。几乎所有编程语言都支持正则表达式，并使用一组通用的特殊字符来定义它们。

让我们尝试一些正则表达式示例：

- 1. 使用你喜欢的代码编辑器，在 Chapter08 解决方案/工作区中添加一个名为 WorkingWithRegularExpressions 的新控制台应用。
- 2. 在 Visual Studio Code 中，将 WorkingWithRegularExpressions 选择为活动的 OmniSharp 项目。
- 3. 在 Program.cs 中，导入以下命名空间：

```
using System.Text.RegularExpressions;
```

### 检查作为文本输入的数字

我们将从实现验证数字输入的常见示例开始：

- 1. 添加语句提示用户输入年龄，然后使用查找数字字符的正则表达式检查其是否有效，如以下代码所示：

```
Write("Enter your age: ");
string? input = ReadLine();

Regex ageChecker = new(@"\d");

if (ageChecker.IsMatch(input))
{
    WriteLine("Thank you!");
}
else
{
    WriteLine($"This is not a valid age: {input}");
}
```

请注意代码的以下内容：

- @ 字符会关闭在字符串中使用转义字符的功能。转义字符以反斜杠为前缀。例如，\t 表示制表符，\n 表示换行符。编写正则表达式时，我们需要禁用此功能。借用电视剧《白宫风云》的话来说，“Let backslash be backslash.”

使用 @ 禁用转义字符后，它们就可以由正则表达式解释。例如，\d 表示数字。你将在本主题后面学习更多以反斜杠为前缀的正则表达式。

- 2. 运行代码，输入例如 34 这样的整数作为年龄，并查看结果，如以下输出所示：

```
Enter your age: 34
Thank you!
```

- 3. 再次运行代码，输入 carrots，并查看结果，如以下输出所示：

```
Enter your age: carrots
This is not a valid age: carrots
```

- 4. 再次运行代码，输入 bob30smith，并查看结果，如以下输出所示：

```
Enter your age: bob30smith
Thank you!
```

我们使用的正则表达式是 \d，表示一个数字。然而，它没有指定该数字前后可以输入什么。这个正则表达式可以用英语描述为“Enter any characters you want as long as you enter at least one digit character.”

在正则表达式中，你使用插入符号 ^ 表示输入的开始，使用美元符号 $ 表示输入的结束。让我们使用这些符号来表示我们期望输入的开始和结束之间除了数字外没有其他内容。

- 5. 将正则表达式更改为 ^\d$，如以下代码中突出显示的那样：

```
Regex ageChecker = new(@"^\d$");
```

- 6. 再次运行代码，注意它会拒绝除单个数字之外的任何输入。我们希望允许一个或多个数字。为此，我们在 \d 表达式后添加一个 +，将其含义修改为一个或多个。

- 7. 更改正则表达式，如以下代码中突出显示的那样：

```
Regex ageChecker = new(@"^\d+$");
```

- 8. 再次运行代码，注意正则表达式只允许任意长度的零或正整数。

### 正则表达式性能改进

用于处理正则表达式的 .NET 类型在整个 .NET 平台以及使用它构建的许多应用程序中都被使用。因此，它们对性能有重大影响，但直到现在，微软对其优化关注度并不高。

从 .NET 5 及更高版本开始，System.Text.RegularExpressions 命名空间已重写内部实现以获得最大性能。使用 IsMatch 等方法进行的常见正则表达式基准测试现在快了五倍。最好的事情是，你无需更改代码即可获得这些好处！

### 理解正则表达式的语法

以下是可在正则表达式中使用的一些常见正则表达式符号：

| Symbol | Meaning | Symbol | Meaning |
|---|---|---|---|
| ^ | 输入开始 | $ | 输入结束 |
| \d | 一个数字 | \D | 一个非数字 |
| \s | 空白字符 | \S | 非空白字符 |
| \w | 单词字符 | \W | 非单词字符 |
| [A-Za-z0-9] | 字符范围 | \^ | ^（插入符号）字符 |
| [aeiou] | 字符集合 | [^aeiou] | 不在字符集合中 |
| . | 任意单个字符 | \. | .（点）字符 |

此外，这里还有一些正则表达式量词会影响正则表达式中前面的符号：

| Symbol | Meaning | Symbol | Meaning |
|---|---|---|---|
| + | 一个或多个 | ? | 一个或无 |
| {3} | 恰好三个 | {3,5} | 三到五个 |
| {3,} | 至少三个 | {,3} | 最多三个 |

### 正则表达式示例

以下是一些正则表达式示例及其含义的描述：

| Expression | Meaning |
|---|---|
| \d | 输入中某处的单个数字 |
| a | 输入中某处的字符 a |
| Bob | 输入中某处的单词 Bob |
| ^Bob | 输入开始处的单词 Bob |
| Bob$ | 输入末尾的单词 Bob |
| ^\d{2}$ | 恰好两位数字 |
| ^[0-9]{2}$ | 恰好两位数字 |
| ^[A-Z]{4,}$ | 仅 ASCII 字符集中的至少四个大写英文字母 |
| ^[A-Za-z]{4,}$ | 仅 ASCII 字符集中的至少四个大写或小写英文字母 |
| ^[A-Z]{2}\d{3}$ | 仅 ASCII 字符集中的两个大写英文字母和三位数字 |
| ^[A-Za-z\u00c0-\u017e]+$ | 至少一个 ASCII 字符集中的大写或小写英文字母，或 Unicode 字符集中的欧洲字母，如以下列表所示：
ĀÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜÝ
ßàáâãäåæçèéêëìíîïðñòóôõö÷øùúûüýÿĈĖŠŜŸ Žž |
| ^d.g$ | 字母 d，然后是任意字符，然后是字母 g，因此它会匹配 dig 和 dog 或 d 和 g 之间的任何单个字符 |
| ^d\.g$ | 字母 d，然后是一个点 (.)，然后是字母 g，因此它只会匹配 d.g |

![](img/c63b00dca7192cc58cfb9e1f1e64f483_363_0.png)

**Good Practice:** 使用正则表达式验证用户输入。相同的正则表达式可以在 JavaScript 和 Python 等其他语言中重复使用。

### 分割复杂的逗号分隔字符串

本章前面，你学习了如何分割简单的逗号分隔字符串变量。但对于以下电影标题示例呢？

```
"Monsters, Inc.","I, Tonya","Lock, Stock and Two Smoking Barrels"
```

字符串值在每个电影标题周围使用双引号。我们可以利用这些引号来确定是否需要在逗号处分割（或不分割）。Split 方法不够强大，因此我们可以使用正则表达式代替。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_363_1.png)

**Good Practice:** 你可以在以下链接中阅读启发此任务的 Stack Overflow 文章中的更完整解释：https://stackoverflow.com/questions/18144431/regex-to-split-a-csv

要在字符串值中包含双引号，我们在它们前面加上反斜杠：

- 1. 添加语句存储一个复杂的逗号分隔字符串变量，然后使用 Split 方法以愚蠢的方式分割它，如以下代码所示：

```
string films = "\"Monsters, Inc.\" ,\"I, Tonya\" ,\"Lock, Stock and Two Smoking Barrels\"";

WriteLine($"Films to split: {films}");

string[] filmsDumb = films.Split(',');

WriteLine("Splitting with string.Split method:");
```

```
foreach (string film in filmsDumb)
{
    WriteLine(film);
}
```

- 2. 添加语句定义正则表达式以智能方式分割并写入电影标题，如以下代码所示：

```
WriteLine();

Regex csv = new(
    "(?:^|,)(?:[^\"]|\"(?:[^\"]*|[^",])*\")?(?:,|$)");

MatchCollection filmsSmart = csv.Matches(films);

WriteLine("Splitting with regular expression:");
foreach (Match film in filmsSmart)
{
    WriteLine(film.Groups[2].Value);
}
```

- 3. 运行代码并查看结果，如以下输出所示：

```
Splitting with string.Split method:
"Monsters
Inc."
"I
Tonya"
"Lock
Stock and Two Smoking Barrels"

Splitting with regular expression:
Monsters, Inc.
I, Tonya
Lock, Stock and Two Smoking Barrels
```

### 在集合中存储多个对象

另一种最常见的数据类型是集合。如果需要在变量中存储多个值，则可以使用集合。

集合是内存中的数据结构，可以以不同的方式管理多个项目，尽管所有集合都有一些共享功能。
