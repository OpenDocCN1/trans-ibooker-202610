
## 从基数到序数转换数字

用于计数的数字称为 **cardinal** 数字，例如 1、2 和 3，而用于排序的数字是 **ordinal** 数字，例如 1st、2nd 和 3rd。让我们创建一个函数来将基数转换为序数：

- 1. 编写一个名为 CardinalToOrdinal 的函数，将基数 int 值转换为序数字符串值；例如，它将 1 转换为 1st，2 转换为 2nd，等等，如以下代码所示：

```
static string CardinalToOrdinal(int number)
{
    switch (number)
    {
        case 11: // special cases for 11th to 13th
        case 12:
        case 13:
            return $"{number}th";
        default:
            int lastDigit = number % 10;

            string suffix = lastDigit switch
            {
                1 => "st",
                2 => "nd",
                3 => "rd",
                _ => "th"
            };
            return $"{number}{suffix}";
    }
}
```

从前面的代码中，请注意以下几点：

- CardinalToOrdinal 有一个输入：名为 number 的 int 类型参数，以及一个输出：string 类型的返回值。
- 使用 switch 语句处理 11、12 和 13 的特殊情况。
- 然后 switch 表达式处理所有其他情况：如果最后一位数字是 1，则使用 st 作为后缀；如果最后一位数字是 2，则使用 nd 作为后缀；如果最后一位数字是 3，则使用 rd 作为后缀；如果最后一位数字是其他任何数字，则使用 th 作为后缀。

- 2. 编写一个名为 RunCardinalToOrdinal 的函数，使用 for 语句从 1 循环到 40，对每个数字调用 CardinalToOrdinal 函数并将返回的字符串写入控制台，用空格字符分隔，如以下代码所示：

```
static void RunCardinalToOrdinal()
{
    for (int number = 1; number <= 40; number++)
    {
        Write($"{CardinalToOrdinal(number)} ");
    }
    WriteLine();
}
```

3. 注释掉 CalculateTax 语句，并调用 RunCardinalToOrdinal 方法，如以下代码所示：

```
// TimesTable(6);

// decimal taxToPay = CalculateTax(amount: 149, twoLetterRegionCode: "FR");
// WriteLine($"You must pay {taxToPay} in tax.");

RunCardinalToOrdinal();
```

4. 运行代码并查看结果，如以下输出所示：

```
1st 2nd 3rd 4th 5th 6th 7th 8th 9th 10th 11th 12th 13th 14th 15th 16th
17th 18th 19th 20th 21st 22nd 23rd 24th 25th 26th 27th 28th 29th 30th 31st
32nd 33rd 34th 35th 36th 37th 38th 39th 40th
```

## 使用递归计算阶乘

5 的阶乘是 120，因为阶乘是通过将起始数字乘以比它小 1 的数，然后再乘以更小的数，等等，直到数字减少到 1 来计算的。这里有一个例子可以看出：5 x 4 x 3 x 2 x 1 = 120。

阶乘是这样写的：5!，其中感叹号读作 bang，所以 5! = 120，即五 bang 等于一百二十。Bang 是一个好名字，因为它们的大小增长非常迅速，就像爆炸一样。

我们将编写一个名为 Factorial 的函数；它将计算作为参数传递给它的 int 的阶乘。我们将使用一种称为递归的巧妙技术，这意味着一个函数在其实现内部调用自身，可以是直接的也可以是间接的：

1. 添加一个名为 Factorial 的函数，以及一个调用它的函数，如以下代码所示：

```
static int Factorial(int number)
{
    if (number < 1)
    {
        return 0;
    }
    else if (number == 1)
    {
        return 1;
    }
    else
    {
        return number * Factorial(number - 1);
    }
}
```

如前所述，前面的代码有几个值得注意的元素，包括以下内容：

- 如果输入参数 number 为零或负数，Factorial 返回 0。
- 如果输入参数 number 为 1，Factorial 返回 1，因此停止调用自身。
- 如果输入参数 number 大于一，在所有其他情况下，Factorial 将 number 乘以调用自身并传递比 number 少 1 的结果。这使得函数具有递归性。

**更多信息：** 递归很巧妙，但它可能导致问题，例如由于每次函数调用都使用内存来存储数据而导致堆栈溢出，因为函数调用太多，最终使用太多。迭代是在 C# 等语言中更实用、如果不太简洁的解决方案。你可以在以下链接阅读更多相关信息：https://en.wikipedia.org/wiki/Recursion_(computer_science)#Recursion_versus_iteration.

2. 添加一个名为 RunFactorial 的函数，使用 for 语句输出 1 到 14 的数字的阶乘，在其循环中调用 Factorial 函数，然后输出结果，使用代码 N0 格式化，这意味着数字格式使用千位分隔符且小数位数为零，如以下代码所示：

```
static void RunFactorial()
{
    for (int i = 1; i < 15; i++)
    {
        WriteLine($"{i}! = {Factorial(i):N0}");
    }
}
```

3. 注释掉 RunCardinalToOrdinal 方法调用并调用 RunFactorial 方法。

4. 运行代码并查看结果，如以下输出所示：

```
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5,040
8! = 40,320
9! = 362,880
10! = 3,628,800
11! = 39,916,800
12! = 479,001,600
13! = 1,932,053,504
14! = 1,278,945,280
```

在之前的输出中并不明显，但 13 及以上的阶乘会溢出 int 类型，因为它们太大了。12! 是 479,001,600，大约是五亿。int 变量中可以存储的最大正值约为二十亿。13! 是 6,227,020,800，大约是六十亿，当存储在 32 位整数中时，它会静默溢出而不会显示任何问题。

你还记得我们可以做什么来收到数值溢出的通知吗？

当发生溢出时你应该做什么？当然，我们可以通过使用 long（64 位整数）而不是 int（32 位整数）来解决 13! 和 14! 的问题，但我们很快就会再次达到溢出限制。

本节的重点是理解数字可能会溢出以及如何显示它而不是忽略它，而不是专门如何计算大于 12! 的阶乘。

1. 修改 Factorial 函数以检查溢出，如以下代码中突出显示所示：

```
checked // for overflow
{
    return number * Factorial(number - 1);
}
```

2. 修改 RunFactorial 函数以处理调用 Factorial 函数时发生的溢出异常，如以下代码中突出显示所示：

```
try
{
    WriteLine($"{i}! = {Factorial(i):N0}");
}
catch (System.OverflowException)
{
    WriteLine($"{i}! is too big for a 32-bit integer.");
}
```

3. 运行代码并查看结果，如以下输出所示：

```
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5,040
8! = 40,320
9! = 362,880
10! = 3,628,800
11! = 39,916,800
12! = 479,001,600
13! is too big for a 32-bit integer.
14! is too big for a 32-bit integer.
```

## 使用 XML 注释记录函数

默认情况下，调用 CardinalToOrdinal 等函数时，代码编辑器会显示一个包含基本信息的工具提示，如图 4.1 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_170_0.png)

图 4.1：显示默认简单方法签名的工具提示

让我们通过添加额外信息来改进工具提示：

- 1. 如果你使用的是带有 C# 扩展的 Visual Studio Code，你应该导航到 View | Command Palette | Preferences: Open Settings (UI)，然后搜索 formatOnType 并确保它已启用。C# XML 文档注释是 Visual Studio 2022 的内置功能。
- 2. 在 CardinalToOrdinal 函数上方的行输入三个正斜杠 ///，并注意它们会扩展为 XML 注释，该注释识别该函数有一个名为 number 的单个参数。

3. 输入适合 XML 文档注释的摘要信息以及描述 CardinalToOrdinal 函数的输入参数和返回值的说明，如以下代码所示：

```
/// <summary>
/// Pass a 32-bit integer and it will be converted into its ordinal equivalent.
/// </summary>
/// <param name="number">Number is a cardinal value e.g. 1, 2, 3, and so on.</param>
/// <returns>Number as an ordinal value e.g. 1st, 2nd, 3rd, and so on.</returns>
```

4. 现在，调用该函数时，你将看到更多详细信息，如图 4.2 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_171_0.png)

图 4.2：显示更详细方法签名的工具提示

在撰写第六版时，C# XML 文档注释在 .NET Interactive 笔记本中不起作用。

> 良好实践：为所有函数添加 XML 文档注释。

## 在函数实现中使用 lambda

F# 是微软的强类型函数优先编程语言，与 C# 一样，编译为 IL 由 .NET 执行。函数式语言源于 lambda 演算；一种仅基于函数的计算系统。代码看起来更像数学函数而不是食谱中的步骤。

函数式语言的一些重要属性在以下列表中定义：

- **模块化**：在 C# 中定义函数的相同好处适用于函数式语言。将大型复杂的代码库分解为更小的部分。
- **不可变性**：C# 意义上的变量不存在。函数内部的任何数据值都不能更改。相反，可以从现有值创建新数据值。这减少了错误。
