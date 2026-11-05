
1. 使用您首选的编码工具在 Chapter04 工作区/解决方案中添加一个新的类库，命名为 CallStackExceptionHandlingLib。
2. 将 Class1.cs 文件重命名为 Calculator.cs。
3. 打开 Calculator.cs 并修改其内容，如以下代码所示：
4. 使用您首选的编码工具在 Chapter04 工作区/解决方案中添加一个新的控制台应用程序，命名为 CallStackExceptionHandling。
5. 在 Visual Studio Code 中，将 CallStackExceptionHandling 选择为活动的 OmniSharp 项目。当您看到弹出警告消息提示缺少所需资源时，点击“是”以添加它们。
6. 在 CallStackExceptionHandling 项目中，为 CallStackExceptionHandlingLib 项目添加引用。
7. 在 Program.cs 中，添加语句以定义两个方法并链式调用它们，以及类库中的方法，如以下代码所示：

```csharp
using static System.Console;

namespace Packt;

public class Calculator
{
    public static void Gamma() // public so it can be called from outside
    {
        WriteLine("In Gamma");
        Delta();
    }

    private static void Delta() // private so it can only be called internally
    {
        WriteLine("In Delta");
        File.OpenText("bad file path");
    }
}
```

```csharp
using Packt;

using static System.Console;

WriteLine("In Main");
Alpha();

static void Alpha()
{
    WriteLine("In Alpha");
    Beta();
}
```

```csharp
static void Beta()
{
    WriteLine("In Beta");
    Calculator.Gamma();
}
```

8. 运行控制台应用程序，并注意结果，如以下部分输出所示：

```
In Main
In Alpha
In Beta
In Gamma
In Delta
Unhandled exception. System.IO.FileNotFoundException: Could not find file 'C:\Code\Chapter04\CallStackExceptionHandling\bin\Debug\net6.0\bad file path'.
  at Microsoft.Win32.SafeHandles.SafeFileHandle.CreateFile(...
  at Microsoft.Win32.SafeHandles.SafeFileHandle.Open(...
  at System.IO.Strategies.OSFileStreamStrategy..ctor(...
  at System.IO.Strategies.FileStreamHelpers.ChooseStrategyCore(...
  at System.IO.Strategies.FileStreamHelpers.ChooseStrategy(...
  at System.IO.StreamReader.ValidateArgsAndOpenPath(...
  at System.IO.File.OpenText(String path) in ...
  at Packt.Calculator.Delta() in C:\Code\Chapter04\CallStackExceptionHandlingLib\Calculator.cs:line 16
  at Packt.Calculator.Gamma() in C:\Code\Chapter04\CallStackExceptionHandlingLib\Calculator.cs:line 10
  at <Program>.$.<Main>$g__Beta|0_1() in C:\Code\Chapter04\CallStackExceptionHandling\Program.cs:line 16
  at <Program>.$.<Main>$g__Alpha|0_0() in C:\Code\Chapter04\CallStackExceptionHandling\Program.cs:line 10
  at <Program>.$.<Main>$(String[] args) in C:\Code\Chapter04\CallStackExceptionHandling\Program.cs:line 5
```

请注意以下内容：

- 调用堆栈是倒置的。从底部开始，您可以看到：
  - 第一次调用是自动生成的 Program 类中的 Main 入口点函数。这里将参数作为字符串数组传入。
  - 第二次调用是 Alpha 函数。
  - 第三次调用是 Beta 函数。
  - 第四次调用是 Gamma 函数。
  - 第五次调用是 Delta 函数。该函数尝试通过传递错误的文件路径来打开文件。这会导致抛出异常。任何带有 try-catch 语句的函数都可以捕获此异常。如果它们不捕获，则异常会自动沿调用堆栈向上传递，直到到达顶部，此时 .NET 会输出异常（以及此调用堆栈的详细信息）。

## 在何处捕获异常

程序员可以决定是在故障点附近捕获异常，还是在调用堆栈更高处集中捕获。这可以使您的代码简化并标准化。您可能知道调用可能会抛出一种或多种类型的异常，但您不需要在调用堆栈的当前点处理它们。

## 重新抛出异常

有时您想要捕获异常、记录它，然后重新抛出它。在 catch 块中重新抛出异常有三种方法，如下列表所示：

1. 要使用其原始调用堆栈抛出捕获的异常，请调用 throw。
2. 要像在调用堆栈当前层级抛出一样抛出捕获的异常，请使用捕获的异常调用 throw，例如 throw ex。这通常是不良做法，因为您丢失了一些对调试可能有用的信息。
3. 要将捕获的异常包装在另一个异常中，该异常可以在消息中包含更多信息以帮助调用者理解问题，请抛出新的异常并将捕获的异常作为 innerException 参数传递。

如果在调用 Gamma 函数时可能发生错误，那么我们可以捕获异常，然后执行重新抛出异常的三种技术之一，如以下代码所示：

```csharp
try
{
    Gamma();
}
catch (IOException ex)
{
    LogException(ex);

    // throw the caught exception as if it happened here
    // this will lose the original call stack
    throw ex;

    // rethrow the caught exception and retain its original call stack
    throw;

    // throw a new exception with the caught exception nested within it
    throw new InvalidOperationException(
        message: "Calculation had invalid values. See inner exception for why.",
        innerException: ex);
}
```

让我们在我们的调用堆栈示例中实际操作一下：

1. 在 CallStackExceptionHandling 项目中，在 Program.cs 的 Beta 函数中，在对 Gamma 函数的调用周围添加 try-catch 语句，如以下代码中突出显示的那样：

```csharp
static void Beta()
{
    WriteLine("In Beta");

    try
    {
        Calculator.Gamma();
    }
    catch (Exception ex)
    {
        WriteLine($"Caught this: {ex.Message}");
        throw ex;
    }
}
```

2. 注意 ex 下方的绿色波浪线，以警告您将丢失调用堆栈信息。

3. 运行控制台应用程序并注意输出排除了调用堆栈的一些详细信息，如以下输出所示：

```
Caught this: Could not find file 'C:\Code\Chapter04\CallStackExceptionHandling\bin\Debug\net6.0\bad file path'.
Unhandled exception. System.IO.FileNotFoundException: Could not find file 'C:\Code\Chapter04\CallStackExceptionHandling\bin\Debug\net6.0\bad file path'.
File name: 'C:\Code\Chapter04\CallStackExceptionHandling\bin\Debug\net6.0\bad file path'
  at <Program>$.<Main>$g__Beta|0_1() in C:\Code\Chapter04\CallStackExceptionHandling\Program.cs:line 25
  at <Program>$.<Main>$g__Alpha|0_0() in C:\Code\Chapter04\CallStackExceptionHandling\Program.cs:line 11
  at <Program>$.<Main>$(String[] args) in C:\Code\Chapter04\CallStackExceptionHandling\Program.cs:line 6
```

4. 重新抛出时删除 ex。

5. 运行控制台应用程序并注意输出包含调用堆栈的所有详细信息。

## 实现测试者-执行者模式

测试者-执行者模式可以避免一些抛出的异常（但不能完全消除它们）。此模式使用成对的函数：一个执行测试，另一个执行操作，如果测试未通过则操作会失败。

.NET 本身实现了此模式。例如，在通过调用 Add 方法向集合添加项之前，您可以测试它是否为只读，这会导致 Add 失败并因此抛出异常。

例如，在从银行账户取款之前，您可能会测试账户未透支，如以下代码所示：

```
if (!bankAccount.IsOverdrawn())
{
    bankAccount.Withdraw(amount);
}
```

## 测试者-执行者模式的问题

测试者-执行者模式会增加性能开销，因此您也可以实现 try 模式，该模式实际上将测试和执行部分合并为单个函数，正如我们在 TryParse 中看到的那样。

测试者-执行者模式的另一个问题发生在您使用多个线程时。在这种情况下，一个线程可以调用测试函数并返回正常。但随后另一个线程执行并更改状态。然后原始线程继续执行，假设一切正常，但实际上并非如此。这称为竞态条件。我们将在第 12 章《使用多任务提高性能和可扩展性》中看到如何处理它。

如果您实现自己的 try 模式函数且它失败，请记住将 out 参数设置为其类型的默认值，然后返回 false，如以下代码所示：

```
static bool TryParse(string? input, out Person value)
{
    if (someFailure)
    {
        value = default(Person);
        return false;
    }

    // successfully parsed the string into a Person
    value = new Person() { ... };
    return true;
}
```
