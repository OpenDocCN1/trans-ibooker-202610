

### 解决方案

以下是代码：

```csharp
static void Main(string[] args)
{
    // Date input
    Console.Write("Enter a date: ");
    string input = Console.ReadLine();
    DateTime enteredDate = Convert.ToDateTime(input);

    // Calculations
    int enteredYear = enteredDate.Year;
    int enteredMonth = enteredDate.Month;

    int numberOfQuarter = (enteredMonth + 2) / 3;
    int monthOfQuarterStart = 3 * numberOfQuarter - 2;
    DateTime firstDayOfQuarter = new DateTime(enteredYear,
    monthOfQuarterStart, 1);
    DateTime lastDayOfQuarter = firstDayOfQuarter.AddMonths(3).AddDays(-1);

    // Outputs
    Console.WriteLine();
    Console.WriteLine("Corresponding quarter: " +
        "number-" + numberOfQuarter +
        ", from " + firstDayOfQuarter.ToShortDateString() +
        " to " + lastDayOfQuarter.ToShortDateString());

    // Waiting for Enter
    Console.ReadLine();
}
```

## 日期差

你经常需要计算两个特定日期之间的时间跨度，换句话说，即输入日期之间经过了多少天或多少年。这就是你现在要学习的内容。

### 任务

用户输入自己的出生日期。程序显示出这个世界为拥有他们而感到高兴的天数（见图 11-5）。

![](img/32b055c352c70d22a656320349173d95_167_0.png)

*图 11-5.* 计算存活了多少天

### 解决方案

如你所见，你需要从今天的日期中减去出生日期。当你相减日期时，结果是一个 `TimeSpan` 对象。有了这个对象，你可以使用它的众多属性之一。在本练习中，你将使用 `Days` 属性。

以下是代码：

```csharp
static void Main(string[] args)
{
    // Input
    Console.Write("Enter your date of birth: ");
    string input = Console.ReadLine();
    DateTime dateOfBirth = Convert.ToDateTime(input);

    // Today
    DateTime today = DateTime.Today;

    // Date difference
    TimeSpan difference = today - dateOfBirth;
    int numberOfDays = difference.Days;

    // Output
    Console.WriteLine();
    Console.WriteLine("Today is: " + today.ToShortDateString());
    Console.WriteLine("The world likes you for this number of days: " +
    numberOfDays.ToString("N0"));

    // Waiting for Enter
    Console.ReadLine();
}
```

## 时区与 UTC

如果你想存储某个事件发生的时间（例如记录订单、问题等），你可能会对夏令时感到意外。或者，更重要的是，假设你正在创建一个将在全球运行的程序。你将不得不处理不同的时区。

为了处理这些情况，了解如何操作协调世界时（UTC）是很有用的。UTC 是零度经线处的时间，不受食品添加剂的影响……抱歉，我的意思是它不受夏令时的影响。UTC 简单来说就是与时区无关。

同时，熟悉 `DateTimeOffset` 对象也是不错的，它除了包含日期和时间外，还包含时区信息。

### 任务

在本练习中，我将向你展示如何使用 UTC 以及 `DateTimeOffset` 对象中包含的时区。你将创建一个处理当前时间的程序（见图 11-6）。

![](img/32b055c352c70d22a656320349173d95_169_0.png)

**图 11-6.** *DateTimeOffset 对象*

### 解决方案

以下是代码：

```csharp
static void Main(string[] args)
{
    // Current time serves as input
    DateTime now = DateTime.Now;
    DateTime utcNow = DateTime.UtcNow;
    DateTimeOffset completeInstant = DateTimeOffset.Now;
    DateTimeOffset utcCompleteInstant = DateTimeOffset.UtcNow;

    // Outputs
    Console.WriteLine("Now: " + now);
    Console.WriteLine("UTC now: " + utcNow);
    Console.WriteLine("Now (including time zone): " + completeInstant);
    Console.WriteLine("Time zone (offset against UTC): " + completeInstant.Offset.TotalHours);
    Console.WriteLine("UTC now (including time zone): " + utcCompleteInstant);

    // Waiting for Enter
    Console.ReadLine();
}
```

请注意，有些变量是 `DateTime` 类型，而另一些则是 `DateTimeOffset` 类型。

## 小结

在本章中，你已经相当深入地学习了如何进行日期计算。

你首先使用 `Convert.ToDateTime` 方法从用户那里获取一个日期，然后通过调用 `DateTime` 的构造函数（`new DateTime...`）从指定的年、月、日获取一个日期。

在计算过程中，你恰当地使用了 `DateTime` 对象的各个属性，如 `Day`、`Month` 或 `Year`，以及它的方法，如 `AddDays`。此外，为了计算季度号，你巧妙地利用了整数除法。

进一步地，你熟悉了如何计算任意两个给定日期之间的差值以及如何处理结果。具体来说，你使用了 `TimeSpan` 对象。

最后，我们讨论了 UTC 和时区，以帮助程序跨越多个时区运行，并正确处理夏令时导致的时间跳变。具体来说，你了解了 `DateTimeOffset` 对象。

```csharp
Console.WriteLine("Unsigned whole numbers");
Console.WriteLine("----------------------");
Console.WriteLine("byte:   " + byte.MinValue + " to " + byte.MaxValue);
Console.WriteLine("ushort: " + ushort.MinValue + " to " + ushort.MaxValue);
Console.WriteLine("uint:   " + uint.MinValue + " to " + uint.MaxValue);
Console.WriteLine("ulong:  " + ulong.MinValue + " to " + ulong.MaxValue);
Console.WriteLine();
Console.WriteLine("Basic decimal numbers");
Console.WriteLine("---------------------");
Console.WriteLine("float:  " + float.MinValue + " to " + float.MaxValue);
Console.WriteLine("double: " + double.MinValue + " to " + double.MaxValue);
Console.WriteLine();

Console.WriteLine("Exact decimal numbers");
Console.WriteLine("---------------------");
Console.WriteLine("decimal: " + decimal.MinValue + " to " + decimal.MaxValue);

// Waiting for Enter
Console.ReadLine();
}
```

### 注意

为了显示这些范围，我使用了所有数值数据类型的 `MinValue` 和 `MaxValue` 属性。

### 讨论

以下各节讨论这个程序。

### 无符号数

程序打印的结果表明，某些数据类型不允许存储负数！然而，这些*无符号数*很少使用，但 `byte` 类型除外。当从文件、数据库或 Web 服务读取二进制数据时，你会用到 `byte` 类型。

## 内存占用

如果你了解一些关于位和字节的知识，你可能会想到，类型范围不同是因为相应类型可用的内存空间不同。这正是原因所在，你将在本小节中了解更多。

### 任务

在本小节中，你将编写一个程序，告诉你每种类型使用多少字节的内存（见图 12-2）。

![](img/32b055c352c70d22a656320349173d95_175_0.png)

*图 12-2.* 显示每种类型使用的字节数

### 解决方案

以下是代码：

```csharp
static void Main(string[] args)
{
    // Outputs
    Console.WriteLine("Whole numbers");
    Console.WriteLine("-------------");
    Console.WriteLine("byte:    " + sizeof(byte));
    Console.WriteLine("sbyte:   " + sizeof(sbyte));
    Console.WriteLine();
    Console.WriteLine("short:   " + sizeof(short));
    Console.WriteLine("ushort:  " + sizeof(ushort));
    Console.WriteLine();
    Console.WriteLine("int:     " + sizeof(int));
    Console.WriteLine("uint:    " + sizeof(uint));
    Console.WriteLine();
    Console.WriteLine("long:    " + sizeof(long));
    Console.WriteLine("ulong:   " + sizeof(ulong));
    Console.WriteLine();
    Console.WriteLine("Decimal numbers");
    Console.WriteLine("---------------");
    Console.WriteLine("float:   " + sizeof(float));
    Console.WriteLine("double:  " + sizeof(double));
    Console.WriteLine("decimal: " + sizeof(decimal));
    Console.WriteLine();

    // Waiting for Enter
    Console.ReadLine();
}
```

### 联系

将当前程序和之前程序的结果联系起来是可能的。例如，让我们讨论重要的 `int` 类型。它使用 4 字节，即 32 位内存。这意味着可能的值有 2 的 32 次方个，超过四十亿。`int` 是一种有符号

