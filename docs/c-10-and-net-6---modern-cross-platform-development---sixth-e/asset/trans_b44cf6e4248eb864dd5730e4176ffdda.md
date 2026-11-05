
## 练习 3.4 – 练习异常处理

在 Chapter03 中创建一个名为 Exercise04 的控制台应用程序，要求用户输入两个范围在 0-255 之间的数字，然后用第二个数字除第一个数字：

```
Enter a number between 0 and 255: 100
Enter another number between 0 and 255: 8
100 divided by 8 is 12
```

编写异常处理程序以捕获所有抛出的错误，如以下输出所示：

```
Enter a number between 0 and 255: apples
Enter another number between 0 and 255: bananas
FormatException: Input string was not in a correct format.
```

## 练习 3.5 – 测试你对运算符的了解

执行以下语句后 x 和 y 的值是多少？

- 1. 自增和加法运算符：
```
x = 3;
y = 2 + ++x;
```
- 2. 二进制移位运算符：
```
x = 3 << 2;
y = 10 >> 1;
```
- 3. 按位运算符：
```
x = 10 & 8;
y = 10 | 7;
```

## 练习 3.6 – 探索主题

使用以下页面上的链接了解本章涵盖主题的更多详细信息：

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-3---controlling-flow-and-converting-types

## 摘要

在本章中，你练习了一些运算符，学习了如何分支和循环，如何在类型之间转换，以及如何捕获异常。

你现在准备好学习如何通过定义函数来重用代码块，如何向它们传递值并获取返回值，以及如何在代码中追踪错误并将其消除！

## 04
### 编写、调试和测试函数

本章介绍如何编写函数以重用代码，在开发期间调试逻辑错误，在运行时记录异常，通过单元测试你的代码以消除错误，并确保稳定性和可靠性。

本章涵盖以下主题：

- 编写函数
- 开发期间的调试
- 运行时日志记录
- 单元测试
- 在函数中抛出和捕获异常

### 编写函数

编程的一个基本原则是不要重复自己（DRY）。

在编程时，如果你发现自己一遍又一遍地编写相同的语句，那么就将这些语句变成一个函数。函数就像完成一个小任务的微型程序。例如，你可能会编写一个函数来计算销售税，然后在财务应用程序的许多地方重用该函数。

与程序一样，函数通常具有输入和输出。它们有时被描述为黑盒，你将一些原材料输入一端，制造好的物品从另一端出现。一旦创建，你就不需要考虑它们的工作原理。

### 乘法表示例

假设你想帮助你的孩子学习乘法表，所以你想让生成一个数字的乘法表变得容易，例如 12 的乘法表：

```
1 x 12 = 12
2 x 12 = 24
...
12 x 12 = 144
```

你之前在本书中学习过 for 语句，所以你知道它可用于在存在规律模式时生成重复的行输出，例如 12 的乘法表，如以下代码所示：

```
for (int row = 1; row <= 12; row++)
{
    Console.WriteLine($"{row} x 12 = {row * 12}");
}
```

然而，与其输出 12 的乘法表，我们希望使其更灵活，以便它可以输出任何数字的乘法表。我们可以通过创建函数来实现这一点。

### 编写乘法表函数

让我们通过创建一个函数来输出 0 到 255 的数字乘以 1 到 12 的任意乘法表来探索函数：

- 1. 使用你喜欢的编码工具创建新的控制台应用，如以下列表中定义：
  - 1. 项目模板：Console Application / console
  - 2. 工作区/解决方案文件和文件夹：Chapter04
  - 3. 项目文件和文件夹：WritingFunctions
- 2. 静态导入 System.Console。
- 3. 在 Program.cs 中，编写语句来定义一个名为 TimesTable 的函数，如以下代码所示：

```
static void TimesTable(byte number)
{
    WriteLine($"This is the {number} times table:");

    for (int row = 1; row <= 12; row++)
    {
        WriteLine($"{row} x {number} = {row * number}");
    }
    WriteLine();
}
```

在前面的代码中，请注意以下几点：

- TimesTable 必须有一个作为参数名为 number 的 byte 值传递给它。
- TimesTable 是一个静态方法，因为它将由静态方法 Main 调用。
- TimesTable 不向调用者返回值，因此在它名称之前使用 void 关键字声明。
- TimesTable 使用 for 语句输出传递给它的数字的乘法表。

4. 在静态导入 Console 类的语句之后以及 TimesTable 函数之前，调用该函数并传入 number 参数的 byte 值，例如 6，如以下代码中突出显示所示：

```
using static System.Console;

TimesTable(6);
```

良好实践：如果函数有一个或多个参数，仅传递值可能不足以提供足够含义，那么你可以选择同时指定参数的名称及其值，如以下代码所示： TimesTable(number: 6)。

5. 运行代码然后查看结果，如以下输出所示：

```
This is the 6 times table:
1 x 6 = 6
2 x 6 = 12
3 x 6 = 18
4 x 6 = 24
5 x 6 = 30
6 x 6 = 36
7 x 6 = 42
8 x 6 = 48
9 x 6 = 54
10 x 6 = 60
11 x 6 = 66
12 x 6 = 72
```

6. 将传递给 TimesTable 函数的数字更改为 0 到 255 之间的其他 byte 值，并确认输出的乘法表是正确的。

7. 请注意，如果你尝试传递非 byte 数字，例如 int 或 double 或 string，将返回错误，如以下输出所示：

```
Error: (1,12): error CS1503: Argument 1: cannot convert from 'int' to 'byte'
```

### 编写返回值的函数

前一个函数执行了操作（循环和向控制台写入），但它没有返回值。假设你需要计算销售税或增值税（VAT）。在欧洲，增值税率可在瑞士的 8% 到匈牙利的 27% 之间。在美国，各州销售税可在俄勒冈州的 0% 到加利福尼亚州的 8.25% 之间。

> 税率不断变化，并且根据许多因素而变化。你不需要联系我告诉我弗吉尼亚州的税率是 6%。谢谢。

让我们实现一个函数来计算世界各地不同地区的税款：

- 1. 添加一个名为 CalculateTax 的函数，如以下代码所示：

```
static decimal CalculateTax(
    decimal amount, string twoLetterRegionCode)
{
    decimal rate = 0.0M;

    switch (twoLetterRegionCode)
    {
        case "CH": // Switzerland
            rate = 0.08M;
            break;
        case "DK": // Denmark
        case "NO": // Norway
            rate = 0.25M;
            break;
        case "GB": // United Kingdom
        case "FR": // France
            rate = 0.2M;
            break;
        case "HU": // Hungary
            rate = 0.27M;
            break;
        case "OR": // Oregon
        case "AK": // Alaska
        case "MT": // Montana
            rate = 0.0M;
            break;
        case "ND": // North Dakota
        case "WI": // Wisconsin
        case "ME": // Maine
        case "VA": // Virginia
            rate = 0.05M;
            break;
        case "CA": // California
            rate = 0.0825M;
            break;
        default: // most US states
            rate = 0.06M;
            break;
    }

    return amount * rate;
}
```

在前面的代码中，请注意以下几点：

- CalculateTax 有两个输入：一个名为 amount 的参数将是花费的金额，以及一个名为 twoLetterRegionCode 的参数将是花费金额所在的地区。
- CalculateTax 将使用 switch 语句执行计算，然后以十进制值返回金额应支付的销售税或增值税；因此，在函数名称之前，我们已将返回值的<|message|>
- 2. 注释掉 TimesTable 方法调用并调用 CalculateTax 方法，传递 amount 等于 149 等值以及 FR 等有效地区代码，如以下代码所示：

```
// TimesTable(6);

decimal taxToPay = CalculateTax(amount: 149, twoLetterRegionCode: "FR");
WriteLine($"You must pay {taxToPay} in tax.");
```

3. 运行代码并查看结果，如以下输出所示：

```
You must pay 29.8 in tax.
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_165_0.png)

> 我们可以通过使用 {taxToPay:C} 来格式化 taxToPay 输出为货币，但它将使用你的本地文化来决定如何格式化货币符号和小数。例如，对我来说在英国，我会看到 £29.80。

你能想到 CalculateTax 函数编写时存在哪些问题吗？如果用户输入的代码例如 fr 或 UK 会发生什么？你如何重写函数以改进它？使用 switch 表达式而不是 switch 语句会更清晰吗？
