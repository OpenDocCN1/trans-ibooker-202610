

## 第4章
### 使用变量

在本章中，你将全面了解变量。*变量*是计算机内存中的一个具名位置，程序可以在其中存储内容。它可以是任何你想要的东西。事实上，你可以在程序中拥有任意数量的变量，只要需要。

本章将从一些简单的示例开始，但最终你会看到变量对于编程来说是绝对基础的。

### 存储文本

第一个任务将向你介绍变量。你将学习如何使用变量执行一些基本操作。

### 任务

你将创建一个名为 `message` 的变量。随后，你将在其中存储一些文本。最后，你将向用户显示该变量的值。

### 解决方案

以下是代码：

```
static void Main(string[] args)
{
    // Declaration of a variable to store text
    string message;

    // Storing a value in prepared variable (assignment statement)
    message = "I can't live with you.";
```
```
```csharp
// Another variable (initialized with some value)
string anotherMessage = "I can't live without you.";

// Output of variables
Console.WriteLine(message);
Console.WriteLine(anotherMessage);

// Waiting for Enter
Console.ReadLine();
}
```
```

### 讨论

现在让我们来讨论这个解决方案。

### 变量声明

如果要使用变量，你需要首先*声明*（创建）它。

变量声明语句的一般语法如下：

*类型名*`[space]`*变量名*`[semicolon]`.

在这种情况下，其写法如下：

```
```csharp
string message;
```
```

### 另一种方式

还有一种编写变量声明语句的方式。在分号之前，你可以使用等号和变量的初始值。

以下是这种语法的一个示例：

```
```csharp
string anotherMessage = "I can't live without you.";
```
```

```csharp
// Waiting for Enter
Console.ReadLine();
}
```

不要忘记，数字输入时*不带引号*。

### 1加1相加

什么？又是1加1？你可能以为我疯了！

### 任务

在上一章中，我告诉过你，将数字与文本组合时，变量可以为你提供更高的确定性。现在我要回到那个建议。

### 解决方案

以下是代码：

```
```csharp
static void Main(string[] args)
{
    // Precalculation of result (into a variable)
    int sum = 1 + 1;
    // Output to the user
    Console.WriteLine(
"Answer to Senior math test
==========================
One and one is: " + sum);

    // Waiting for Enter
    Console.ReadLine();
}
}
```
```

程序运行的结果见图4-1。

![](img/32b055c352c70d22a656320349173d95_75_0.png)

图4-1. 1加1程序的结果

### 讨论

请将1加1的计算与上一章中的计算进行比较。在这里，你显式地将结果存储在变量中。这样可以避免求值顺序可能带来的问题，以及得到错误答案11。

### 使用变量进行计算

在下一个任务中，你将学习如何同时使用多个变量。

### 任务

你将把一些数字存储在两个变量中。然后，你将它们的总和计算到第三个变量中。

### 解决方案

以下是代码：

```
static void Main(string[] args)
{
    // 1. SOLUTION
    // Values to be summed
```
```
int firstNumber = 42;
int secondNumber = 11;

// Calculating
int sum = firstNumber + secondNumber;

// Output
Console.WriteLine("Sum is: " + sum);

// 2. SOLUTION
// Declaring all variables at once
int thirdNumber, fourthNumber, newSum;

// Values to be summed
thirdNumber = 42;
fourthNumber = 11;

// Calculating
newSum = thirdNumber + fourthNumber;

// Output
Console.WriteLine("Calculated another way: " + newSum);

// Waiting for Enter
Console.ReadLine();
}
```

### 讨论

这两个（可选的）解决方案展示了你会经常遇到的两种情况：

- 你声明一个变量并*立即*将值存储在其中。
- 你先声明一个变量，*稍后*再将值存储在其中。

### 组装宏大组合

通常，你需要从几个值组装你的输出。在这个任务中，你将学习如何操作。

### 任务

我将通过一个足球比赛结果的示例（图4-2）向你展示如何组装复杂文本。

![](img/32b055c352c70d22a656320349173d95_77_0.png)

图4-2. 宏大组合程序

在示例中，你有一些固定文本、一些（可能）可变的文本，以及一些（可能）可变的数字。这是典型的现实世界情况。

### 解决方案

要存储（可能）可变的值，你可以使用变量。当然，在这个简单的程序中，这些值实际上是固定的，但通常你会从其他地方（如用户、文件、数据库或Web服务）获取它们。在本书后面，你将学习如何从用户那里获取输入。

```
static void Main(string[] args)
{
    // Data in variables
    string club1 = "FC Liverpool";
    string club2 = "Manchester United";
    int goals1 = 3;
    int goals2 = 2;

    // Output of match result
    Console.WriteLine(
        "Match " + club1 + " - " + club2 +
        " ended with result " +
        goals1 + ":" + goals2 + ".");
```

```
// Waiting for Enter
Console.ReadLine();
}
```

### 讨论

在解决方案中，你应该特别注意以下几点：

- 你使用了不同数据类型的变量来存储不同类型的值。
- 你构建的显示消息由九个部分组成，并由八个加号连接。消息的某些部分是固定的，而另一些是可变的。

### 使用小数

在编程中，你需要仔细区分整数和小数。你已经知道如何处理整数，现在你将要了解小数。

### 任务

在这个任务中，我将向你展示一些处理小数的示例。

### 解决方案

在C#中，有一种用于小数的类型叫做 `double`。以下是代码：

```
static void Main(string[] args)
{
    // IN CODE, decimal separator is always DOT regardless of computer language settings
    double piApproximately = 3.14;

    // Pi is already available in C#
    double piMorePrecisely = Math.PI;
```

```
```csharp
// Decimal numbers have always limited precision
double notCompletelyOne = 0.99999999999999999;

// Outputs
Console.WriteLine("Pi value from our code: " + piApproximately);
Console.WriteLine("Pi value from C#: " + piMorePrecisely);
Console.WriteLine("This should not be exact one: " + notCompletelyOne);

// Waiting for Enter
Console.ReadLine();
}
```
```

### 讨论

请注意以下几点：

- 在代码中，你始终需要使用一个小数*点*作为数字整数部分和小数部分之间的分隔符。
- 但是，输出取决于你的Windows设置。如图4-3所示，我的计算机输出使用逗号作为小数点分隔符，因为我的计算机设置为捷克语。
- 你还可以看到，小数没有无限精度。它们在大约15位有效数字后会被四舍五入。

![](img/32b055c352c70d22a656320349173d95_79_0.png)

*图4-3.* 小数程序的结果

## 第4章   使用变量

### 使用逻辑值

在编程中，你经常处理*逻辑值*，即“是”和“否”的值。

### 任务

在这个任务中，我将向你展示如何处理逻辑值。

### 解决方案

在C#中，逻辑值的类型叫做 `bool`。“是”的值写成 `true`，“否”的值写成 `false`。以下是代码：

```
```csharp
static void Main(string[] args)
{
    // Two logical (Boolean) variables
    bool thePrettiestGirlLovesMe = true;
    bool iAmHungry = false;

    // Use exclamation mark to negate logical value
    bool iAmNotHungry = !iAmHungry;

    // Output
    Console.WriteLine("She loves me: " + thePrettiestGirlLovesMe);
    Console.WriteLine("I am hungry: " + iAmHungry);
    Console.WriteLine("I am not hungry: " + iAmNotHungry);

    // Waiting for Enter
    Console.ReadLine();
}
```
```

### 讨论

请注意，当你需要对逻辑值取反（将其从“是”翻转为“否”，反之亦然）时，使用感叹号。

