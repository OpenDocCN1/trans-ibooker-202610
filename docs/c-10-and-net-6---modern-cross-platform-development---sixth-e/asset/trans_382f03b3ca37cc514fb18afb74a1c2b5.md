
```
WriteLine("{0} + {1}i added to {2} + {3}i is {4} + {5}i",
  c1.Real, c1.Imaginary,
  c2.Real, c2.Imaginary,
  c3.Real, c3.Imaginary);
```

2. 运行代码并查看结果，如以下输出所示：

```
Working with complex numbers:
(4, 2) added to (3, 7) is (7, 9)
4 + 2i added to 3 + 7i is 7 + 9i
```

## 理解四元数

四元数是一种扩展复数的数系。它们构成了实数上的四维结合赋范除代数，因此也是一个域。

哈？是的，我知道。我也不懂。别担心；我们不会用它们写任何代码！就说它们擅长描述空间旋转，所以视频游戏引擎使用它们，许多计算机模拟和飞行控制系统也是如此。

## 处理文本

其中一种最常见的数据类型是文本。在 .NET 中用于处理文本的最常见类型如下表所示：

| 命名空间 | 类型 | 描述 |
|---|---|---|
| System | Char | 存储单个文本字符 |
| System | String | 存储多个文本字符 |
| System.Text | StringBuilder | 高效操作字符串 |
| System.Text.RegularExpressions | Regex | 高效模式匹配字符串 |

## 获取字符串的长度

让我们探索处理文本时的一些常见任务；例如，有时你需要找出存储在字符串变量中的一段文本的长度：

- 1. 使用你偏好的代码编辑器在 Chapter08 解决方案/工作区中添加一个名为 WorkingWithText 的新控制台应用：
  - 1. 在 Visual Studio 中，将解决方案的启动项目设置为当前选择。
  - 2. 在 Visual Studio Code 中，选择 WorkingWithText 作为活动的 OmniSharp 项目。

2. 在 WorkingWithText 项目中，在 Program.cs 中，添加语句定义一个变量来存储城市伦敦的名称，然后将其名称和长度写入控制台，如以下代码所示：

```
string city = "London";
WriteLine($"{city} is {city.Length} characters long.");
```

3. 运行代码并查看结果，如以下输出所示：

```
London is 6 characters long.
```

## 获取字符串的字符

string 类在内部使用 char 数组来存储文本。它还有一个索引器，这意味着我们可以使用数组语法读取其字符。数组索引从零开始，因此第三个字符位于索引 2。

让我们看看实际效果：

1. 添加一条语句来写入字符串变量中第一个和第三个位置的字符，如以下代码所示：

```
WriteLine($"First char is {city[0]} and third is {city[2]}.");
```

2. 运行代码并查看结果，如以下输出所示：

```
First char is L and third is n.
```

## 分割字符串

有时，你需要在出现某个字符的地方分割一些文本，例如逗号：

1. 添加语句定义一个包含逗号分隔城市名称的单个字符串变量，然后使用 Split 方法并指定要将逗号视为分隔符，然后枚举返回的字符串值数组，如以下代码所示：

```
string cities = "Paris,Tehran,Chennai,Sydney,New York,Medellin";

string[] citiesArray = cities.Split(',');

WriteLine($"There are {citiesArray.Length} items in the array.");
foreach (string item in citiesArray)
{
    WriteLine(item);
}
```

2. 运行代码并查看结果，如以下输出所示：

```
There are 6 items in the array.
Paris
Tehran
Chennai
Sydney
New York
Medellin
```

本章后面你将学习如何处理更复杂的场景。

## 获取字符串的一部分

有时，你需要获取部分文本。IndexOf 方法有九个重载，返回字符串中指定 char 或字符串的索引位置。Substring 方法有两个重载，如下列表所示：

- Substring(startIndex, length)：返回从 startIndex 开始并包含接下来 length 个字符的子字符串。
- Substring(startIndex)：返回从 startIndex 开始并包含到字符串末尾所有字符的子字符串。

让我们探索一个简单示例：

1. 添加语句将一个人的全名存储在字符串变量中，名字和姓氏之间有一个空格字符，找到空格的位置，然后将名字和姓氏作为两个部分提取出来，以便以不同的顺序重新组合，如以下代码所示：

```csharp
string fullName = "Alan Jones";
int indexOfTheSpace = fullName.IndexOf(' ');

string firstName = fullName.Substring(
    startIndex: 0, length: indexOfTheSpace);

string lastName = fullName.Substring(
    startIndex: indexOfTheSpace + 1);

WriteLine($"Original: {fullName}");
WriteLine($"Swapped: {lastName}, {firstName}");
```

2. 运行代码并查看结果，如以下输出所示：

```
Original: Alan Jones
Swapped: Jones, Alan
```

如果初始全名的格式不同，例如 "LastName, FirstName"，则代码需要不同。作为可选练习，尝试编写一些语句将输入 "Jones, Alan" 更改为 "Alan Jones"。

## 检查字符串内容

有时，你需要检查一段文本是否以某些字符开头或结尾，或是否包含某些字符。你可以使用名为 StartsWith、EndsWith 和 Contains 的方法来实现：

- 1. 添加语句存储一个字符串值，然后检查它是否以或包含几个不同的字符串值开头，如以下代码所示：
- 2. 运行代码并查看结果，如以下输出所示：

```
string company = "Microsoft";
bool startsWithM = company.StartsWith("M");
bool containsN = company.Contains("N");
WriteLine($"Text: {company}");
WriteLine($"Starts with M: {startsWithM}, contains an N: {containsN}");
```

```
Text: Microsoft
Starts with M: True, contains an N: False
```

## 连接、格式化和其他字符串成员

还有许多其他字符串成员，如下表所示：

| 成员 | 描述 |
|---|---|
| Trim, TrimStart, TrimEnd | 这些方法从开头和/或结尾修剪空白字符，如空格、制表符和回车符。 |
| ToUpper, ToLower | 这些将所有字符转换为大写或小写。 |
| Insert, Remove | 这些方法插入或删除一些文本。 |
| Replace | 这用其他文本替换一些文本。 |
| string.Empty | 这可以代替每次使用空的一对双引号 (") 作为字面字符串值时分配内存。 |
| string.Concat | 这连接两个字符串变量。+ 运算符在字符串操作数之间使用时执行等效操作。 |
| string.Join | 这用一个字符在每个之间连接一个或多个字符串变量。 |
| string.IsNullOrEmpty | 这检查字符串变量是否为空或空。 |
| string.IsNullOrWhitespace | 这检查字符串变量是否为空或空白；即任意数量的水平和垂直间距字符的混合，例如制表符、空格、回车符、换行符等。 |
| string.Format | 这是字符串插值的替代方法，用于输出格式化的字符串值，使用位置参数而不是命名参数。 |

其中一些前面提到的方法是静态方法。这意味着该方法只能从类型调用，不能从变量实例调用。在前面的表中，我通过在它们前面加上 string. 来表示静态方法，如 string.Format。

让我们探索其中一些方法：

1. 添加语句使用 Join 方法将字符串值数组重新组合成一个带分隔符的单个字符串变量，如以下代码所示：

```
string recombined = string.Join(" => ", citiesArray);
WriteLine(recombined);
```

2. 运行代码并查看结果，如以下输出所示：

```
Paris => Tehran => Chennai => Sydney => New York => Medellín
```

3. 添加语句使用位置参数和插值字符串格式化语法输出相同的三个变量两次，如以下代码所示：

```
string fruit = "Apples";
decimal price = 0.39M;
DateTime when = DateTime.Today;

WriteLine($"Interpolated: {fruit} cost {price:C} on {when:dddd}.");

WriteLine(string.Format("string.Format: {0} cost {1:C} on {2:dddd}.",
    arg0: fruit, arg1: price, arg2: when));
```

4. 运行代码并查看结果，如以下输出所示：

```
Interpolated:  Apples cost £0.39 on Thursday.
string.Format: Apples cost £0.39 on Thursday.
```

请注意，我们本可以简化第二个语句，因为 WriteLine 支持与 string.Format 相同的格式代码，如以下代码所示：

```
WriteLine("WriteLine: {0} cost {1:C} on {2:dddd}.",
    arg0: fruit, arg1: price, arg2: when);
```

## 高效构建字符串

你可以使用 String.Concat 方法或简单地使用 + 运算符将两个字符串连接起来形成一个新字符串。但这两种选择都是不良做法，因为 .NET 必须在内存中创建一个完全新的字符串。

如果只是添加两个字符串值，可能不会明显，但如果在具有多次迭代的循环中连接，则会对性能和内存使用产生显著的负面影响。在第 12 章《使用多任务提高性能和可扩展性》中，你将学习如何使用 StringBuilder 类型高效地连接字符串变量。
