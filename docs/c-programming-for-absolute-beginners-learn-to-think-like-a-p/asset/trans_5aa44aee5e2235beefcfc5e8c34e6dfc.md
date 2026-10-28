

## 它是什么？

到目前为止，你总是需要编写一个包含多个语句的程序，然后运行它才能看到实际效果。交互模式允许你输入单条 C# 语句并立即运行。通过这种方式，你可以更快地探索某些 C# 特性。

## 如何启动它？

你可以通过选择“视图 ➤ 其他窗口 ➤ C# 交互”来启动交互模式（见图 14-15）。你甚至不需要创建或打开项目。

![](img/32b055c352c70d22a656320349173d95_215_0.png)

*图 14-15.* 切换到交互模式

图 14-16 显示了一个交互式会话示例——我声明了一个数值变量，将其增加 10，并显示其值。

![](img/32b055c352c70d22a656320349173d95_216_0.png)

图 14-16. 交互式会话示例

## 注释

请注意以下事项：

- 如果需要输入多行语句，可以用 Shift+Enter 换行，而不是直接按 Enter。
- 使用 `#help` 可以显示有关如何使用交互模式的简明信息。

![](img/32b055c352c70d22a656320349173d95_219_0.png)

**图 15-1.** 密码错误

![](img/32b055c352c70d22a656320349173d95_219_1.png)

**图 15-2.** 密码正确

## 分析

让我们更详细地看一下这个程序。

### 程序

在这个程序中，当两个密码（输入的密码和存储的密码）一致时执行某种操作，当它们不一致时执行另一种操作。在这种情况下，你要么允许用户进入，要么拒绝用户，并显示相应的消息（见图 15-3）。

![](img/32b055c352c70d22a656320349173d95_220_0.png)

图 15-3. 程序流程

### 程序分支

一般来说，程序分支意味着根据条件是否成立而走不同的路径（见图 15-4）。

![](img/32b055c352c70d22a656320349173d95_220_1.png)

图 15-4. 分支

### 语法

对于分支，C# 使用如下所示的 if-else 结构：

```csharp
if (condition)
{
    Statements to perform when the condition holds
}
else
{
    Statements to perform otherwise
}
```

### 解决方案

代码如下：

```csharp
static void Main(string[] args)
{
    // Input
    Console.Write("Enter password: ");
    string enteredPassword = Console.ReadLine();

    // Password check
    if (enteredPassword == "friend")
    {
        Console.WriteLine("Password is OK, please enter");
    }
    else
    {
        Console.WriteLine("Incorrect password");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

### 讨论

为了构造条件，我使用了相等性测试，它由两个等号组成。如果比较的值相同，测试结果为 true，条件被视为成立，执行 if 分支中的语句。如果比较的值不同，测试结果为 false，条件被视为不成立，执行 else 分支中的语句。

### 测试

现在你可以检查程序的执行情况了！除了正常运行程序外，你还可以像上一章学到的那样逐行执行代码。

## 反向条件

为了让你更熟悉条件，从不同角度来认识它们是很有帮助的。继续以密码问题为例，让我们换一种方式来思考。

### 任务

现在的任务是用另一种方式解决前面的练习，即使用反向条件。换句话说，你将测试不相等而不是相等。

### 解决方案

代码如下：

```csharp
static void Main(string[] args)
{
    // Correct password
    string correctPassword = "friend";

    // Input
    Console.Write("Enter password: ");
    string enteredPassword = Console.ReadLine();

    // Password check
    if (enteredPassword != correctPassword)
    {
        Console.WriteLine("Incorrect password");
    }
    else
    {
        Console.WriteLine("Password is OK, please enter");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

### 讨论

在这个练习中，我使用了不相等性测试，它由一个感叹号后跟一个等号组成。当比较的值**不相同**时，测试返回 true。

## 长度检查

两段文本只能比较是相同还是不同，而两个数字还可以比较哪个更大（或更小）。让我们来看一看。

### 任务

在本节中，你将学习在程序中进行数字比较，该程序会判断输入的文本是否最多为四个字符（见图 15-5 和图 15-6）。

![](img/32b055c352c70d22a656320349173d95_224_0.png)

图 15-5. 短文本

![](img/32b055c352c70d22a656320349173d95_224_1.png)

图 15-6. 长文本

### 解决方案

想必你应该先确定输入文本的字符数，然后将其与数字四进行比较。你在第 7 章中学习了如何确定文本的字符数——即使用其 `Length` 属性（那个程序是“文本作为对象”）。无论如何，如果你不记得该属性的名称，可以在文本变量末尾添加一个点，然后浏览 IntelliSense 中的选项，看看哪些可能是合适的，正如上一章所介绍的那样。

代码如下：

```csharp
static void Main(string[] args)
{
    // Input
    Console.Write("Enter a word: ");
    string word = Console.ReadLine();

    // Determining length
    int wordLength = word.Length;

    // Checking length
    if (wordLength <= 4)
    {
        Console.WriteLine("Word is short (at most 4 characters)");
    }
    else
    {
        Console.WriteLine("Word is long (more than 4 characters)");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

### 注释

在这个解决方案中，我使用了小于或等于运算符，它看起来像这样：`<=`。

## 正数

在本节中，你将获得更多关于数字比较的练习。

### 任务

你将编写一个程序，判断用户输入的数字是否为正数（见图 15-7 和图 15-8）。

### 解决方案

```csharp
// Evaluation
if (number > 0)
{
    Console.WriteLine("The number is positive");
}
else
{
    Console.WriteLine("The number is NOT positive");
}

// Waiting for Enter
Console.ReadLine();
```

### 讨论

我使用了大于运算符来将输入的数字与零进行比较。
你认为当用户输入零时程序会做什么？它会检查条件 `0 > 0`，并发现该条件不成立。因此，它会显示该数字不是正数。这就是为什么消息措辞相当不寻常（“……不是正数”），如图 15-9 所示。我没有使用“……是负数”。

![](img/32b055c352c70d22a656320349173d95_227_0.png)

图 15-9. 零时的结果

## 奇数和偶数

让我们继续看另一个数字比较。

### 任务

你现在的任务是编写一个程序，判断用户输入的数字是奇数还是偶数（见图 15-10 和图 15-11）。

![](img/32b055c352c70d22a656320349173d95_228_0.png)

**图 15-10.** 判断偶数

![](img/32b055c352c70d22a656320349173d95_228_1.png)

**图 15-11.** 判断奇数

### 解决方案

解决方案的核心是确定输入数字除以二的余数。如果余数为零，则该数字是偶数。如果存在余数，则该数字是奇数。

代码如下：

```csharp
static void Main(string[] args)
{
    // Input
    Console.Write("Enter a number: ");
    string input = Console.ReadLine();
    int number = Convert.ToInt32(input);

    // Remainder calculation
    int remainderAfterDivisionByTwo = number % 2;

    // Evaluation
    if (remainderAfterDivisionByTwo == 0)
    {
        Console.WriteLine("The number is even");
    }
    else
    {
        Console.WriteLine("The number is odd");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

## 不区分大小写

你已经知道，可以对两段文本进行比较，看它们是否相等或不相等。这种比较是区分大小写的。换句话说，`hobbit` 和 `Hobbit` 被认为是不同的单词。然而，你经常需要进行不区分大小写的比较，我现在就来向你展示如何操作。

