

这些旧技术明显的缺点是缺乏对称性。每个方法或多或少都是针对特定的技术、特定的语言，甚至可能是特定的项目量身定制的。为了结束这种疯狂，.NET 平台提供了一种标准的发送和捕获运行时错误的技术：结构化异常处理。这种方法的美妙之处在于，开发人员现在拥有了一种统一的错误处理方法，该方法适用于所有针对 .NET 平台的语言。因此，C# 程序员处理错误的方式在语法上与 VB 程序员或使用 C++/CLI 的 C++ 程序员相似。

作为额外的好处，用于跨程序集和机器边界抛出和捕获异常的语法是相同的。例如，如果你使用 C# 构建 ASP.NET Core RESTful 服务，你可以使用与在同一应用程序中方法之间抛出异常相同的关键字，向远程调用方抛出 JSON 错误。

.NET 异常的另一个好处是，与其接收晦涩的数值，异常是包含问题的人类可读描述的对象，以及触发异常的调用堆栈的详细快照。此外，你能够为最终用户提供帮助链接信息，该信息指向提供错误详细信息的 URL，以及自定义程序员定义的数据。

## .NET 异常处理的构建块

使用结构化异常处理涉及使用四个相互关联的实体。

- 表示异常详细信息的类类型
- 在正确情况下向调用方抛出异常类实例的成员
- 调用方上调用易发生异常成员的代码块
- 调用方上将处理（或捕获）异常的代码块，如果发生异常的话

C# 编程语言提供了五个关键字（`try`、`catch`、`throw`、`finally` 和 `when`）来让你抛出和处理异常。表示当前问题的对象是一个扩展 `System.Exception`（或其后代）的类。鉴于此，让我们来看看这个以异常为中心的基类的角色。

## System.Exception 基类

所有异常最终都派生自 `System.Exception` 基类，而它又派生自 `System.Object`。以下是该类的核心（请注意，其中一些成员是虚拟的，因此可能被派生类重写）：

```csharp
public class Exception : ISerializable
{
    // Public constructors
    public Exception(string message, Exception innerException);
    public Exception(string message);
    public Exception();
    ...
    // Methods
    public virtual Exception GetBaseException();
    public virtual void GetObjectData(SerializationInfo info,
        StreamingContext context);
    // Properties
    public virtual IDictionary Data { get; }
    public virtual string HelpLink { get; set; }
    public int HResult { get; set; }
    public Exception InnerException { get; }
    public virtual string Message { get; }
    public virtual string Source { get; set; }
    public virtual string StackTrace { get; }
    public MethodBase TargetSite { get; }
}
```

正如你所见，`System.Exception` 定义的许多属性本质上是只读的。这是因为派生类型通常会为每个属性提供默认值。例如，`IndexOutOfRangeException` 类型的默认消息是“Index was outside the bounds of the array。”

表 7-1 描述了 `System.Exception` 的最重要的成员。

## 表 7-1. System.Exception 类型的核心成员

| System.Exception 属性 | 意义 |
|---|---|
| Data | 此只读属性检索一个键值对集合（由实现 `IDictionary` 的对象表示），提供有关异常的额外、程序员定义的信息。默认情况下，此集合为空。 |
| HelpLink | 此属性获取或设置指向描述错误的帮助文件或网站的 URL。 |
| InnerException | 此只读属性可用于获取有关导致当前异常发生的先前异常的信息。先前异常是通过将它们传递给当前异常的构造函数来记录的。 |
| Message | 此只读属性返回给定错误的文本描述。错误消息本身作为构造函数参数设置。 |
| Source | 此属性获取或设置抛出当前异常的程序集或对象的名称。 |
| StackTrace | 此只读属性包含一个字符串，标识触发异常的调用序列。正如你可能猜到的，此属性在调试期间很有用，或者如果你想将错误转储到外部错误日志中。 |
| TargetSite | 此只读属性返回一个 `MethodBase` 对象，该对象描述了抛出异常的方法的众多细节（调用 `ToString()` 将按名称标识该方法）。 |

## 最简单的可能示例

为了说明结构化异常处理的用处，你需要创建一个类，该类在正确（或可以说异常）的情况下抛出异常。假设你创建了一个新的 C# 控制台应用程序项目（名为 `SimpleException`），它定义了两个相关联的类类型（`Car` 和 `Radio`），关联方式为“has-a”关系。`Radio` 类型定义了单个方法，用于打开或关闭收音机的电源。

```csharp
using System;

namespace SimpleException
{
    class Radio
    {
        public void TurnOn(bool on)
        {
            Console.WriteLine(on ? "Jamming..." : "Quiet time...");
        }
    }
}
```

除了通过包含/委托利用 `Radio` 类之外，`Car` 类（如下所示）还定义为：如果用户将 `Car` 对象的加速超过预定义的最高速度（使用名为 `MaxSpeed` 的常量成员变量指定），其发动机就会爆炸，使 `Car` 无法使用（由名为 `_carIsDead` 的私有 `bool` 成员变量捕获）。

除此之外，`Car` 类型还有一些属性来表示当前速度和用户提供的“昵称”，以及各种构造函数来设置新 `Car` 对象的状态。以下是完整的定义（带代码注释）：

```csharp
using System;

namespace SimpleException
{
    class Car
    {
        // Constant for maximum speed.
        public const int MaxSpeed = 100;

        // Car properties.
        public int CurrentSpeed { get; set; } = 0;
        public string PetName { get; set; } = "";

        // Is the car still operational?
        private bool _carIsDead;

        // A car has-a radio.
        private readonly Radio _theMusicBox = new Radio();

        // Constructors.
        public Car() { }
        public Car(string name, int speed)
        {
            CurrentSpeed = speed;
            PetName = name;
        }

        public void CrankTunes(bool state)
        {
            // Delegate request to inner object.
            _theMusicBox.TurnOn(state);
        }
    }
}
```

```csharp
// See if Car has overheated.
public void Accelerate(int delta)
{
    if (_carIsDead)
    {
        Console.WriteLine("{0} is out of order...", PetName);
    }
    else
    {
        CurrentSpeed += delta;
        if (CurrentSpeed > MaxSpeed)
        {
            Console.WriteLine("{0} has overheated!", PetName);
            CurrentSpeed = 0;
            _carIsDead = true;
        }
        else
        {
            Console.WriteLine("=> CurrentSpeed = {0}",
                CurrentSpeed);
        }
    }
}
```

接下来，更新你的 `Program.cs` 代码以强制 `Car` 对象超过预定义的最高速度（在 `Car` 类中设置为 100），如下所示：

```csharp
using System;
using System.Collections;
using SimpleException;

Console.WriteLine("***** Simple Exception Example *****");
Console.WriteLine("=> Creating a car and stepping on it!");
Car myCar = new Car("Zippy", 20);
myCar.CrankTunes(true);

for (int i = 0; i < 10; i++)
{
    myCar.Accelerate(10);
}
Console.ReadLine();
```

执行代码时，你会看到以下输出：

```text
***** Simple Exception Example *****
=> Creating a car and stepping on it!
Jamming...
=> CurrentSpeed = 30
=> CurrentSpeed = 40
```
