

# 表 12-1。System.MulticastDelegate/System.Delegate 的部分成员

| 成员 | 实际含义 |
|---|---|
| Method | 此属性返回一个 `System.Reflection.MethodInfo` 对象，该对象表示委托所维护的静态方法的详细信息。 |
| Target | 如果要调用的方法是在对象级别定义的（而不是静态方法），Target 将返回一个对象，该对象表示委托所维护的方法。如果 Target 返回的值等于 `null`，则要调用的方法是静态成员。 |
| Combine() | 此静态方法将一个方法添加到委托所维护的列表中。在 C# 中，可以使用重载的 `+=` 运算符来触发此方法，这是一种简写形式。 |
| GetInvocationList() | 此方法返回一个 `System.Delegate` 对象数组，其中每个对象都表示一个可能被调用的方法。 |
| Remove() / RemoveAll() | 这些静态方法从委托的调用列表中移除一个方法（或所有方法）。在 C# 中，可以通过重载的 `-=` 运算符间接调用 `Remove()` 方法。 |

## 最简单的委托示例

当然，委托在初次接触时可能会造成一些困惑。因此，为了让大家开始了解委托，我们来看一个简单的控制台应用程序（名为 SimpleDelegate）。该程序使用了之前见过的 BinaryOp 委托类型。下面是完整代码，随后将进行分析：

```
//SimpleMath.cs
namespace SimpleDelegate
{
    // This class contains methods BinaryOp will
    // point to.
    public class SimpleMath
    {
        public static int Add(int x, int y) => x + y;
        public static int Subtract(int x, int y) => x - y;
    }
}

//Program.cs
using System;
using SimpleDelegate;

Console.WriteLine("***** Simple Delegate Example *****");

// Create a BinaryOp delegate object that
// "points to" SimpleMath.Add().
BinaryOp b = new BinaryOp(SimpleMath.Add);

// Invoke Add() method indirectly using delegate object.
Console.WriteLine("10 + 10 is {0}", b(10, 10));
Console.ReadLine();
```

```
//Additional type definitions must be placed at the end of the
// top-level statements
// This delegate can point to any method,
// taking two integers and returning an integer.
public delegate int BinaryOp(int x, int y);
```

> 注意：回顾第 3 章，其他类型声明（本例中的 BinaryOp 委托）必须放在所有顶级语句之后。

再次注意 BinaryOp 委托类型的声明格式：它指定 BinaryOp 委托对象可以指向任何接受两个整数并返回一个整数的方法（被指向的方法的实际名称并不重要）。这里创建了一个名为 SimpleMath 的类，该类定义了两个符合 BinaryOp 委托所定义模式的静态方法。

当你想要将目标方法分配给某个委托对象时，只需将方法名称传递给委托的构造函数即可。

```
// Create a BinaryOp delegate object that
// "points to" SimpleMath.Add().
BinaryOp b = new BinaryOp(SimpleMath.Add);
```

此时，你可以使用类似直接调用函数的语法，调用委托所指向的成员。

```
// Invoke() is really called here!
Console.WriteLine("10 + 10 is {0}", b(10, 10));
```

在底层，运行时会调用派生自 MulticastDelegate 的类中由编译器生成的 Invoke() 方法。如果在 il dasm.exe 中打开程序集，并检查 Main() 方法中的 CIL 代码，就可以自行验证这一点。

```
.method private hidebysig static void Main(string[] args) cil managed
{
...
    callvirt   instance int32 BinaryOp::Invoke(int32, int32)
}
```

C# 不要求你在代码中显式调用 Invoke()。由于 BinaryOp 可以指向接受两个参数的方法，因此下面的代码语句同样是允许的：

```
Console.WriteLine("10 + 10 is {0}", b.Invoke(10, 10));
```

请回想一下，.NET Core 委托是类型安全的。因此，如果你尝试创建一个指向不符合该模式的方法的委托对象，就会收到编译时错误。为了说明这一点，假设 SimpleMath 类现在定义了一个名为 SquareNumber() 的附加方法，该方法接受一个整数作为输入。

```
public class SimpleMath
{
    public static int SquareNumber(int a) => a * a;
}
```

由于 BinaryOp 委托只能指向接受两个整数并返回一个整数的方法，所以下面的代码是非法的，无法编译：

```
// Compiler error! Method does not match delegate pattern!
BinaryOp b2 = new BinaryOp(SimpleMath.SquareNumber);
```

## 调查委托对象

让我们通过在 Program 类中创建一个静态方法（名为 DisplayDelegateInfo()）来扩展当前示例。此方法将打印委托对象所维护的方法名称，以及定义该方法的类的名称。为此，你将遍历由 GetInvocationList() 返回的 System.Delegate 数组，并调用每个对象的 Target 和 Method 属性。

```
static void DisplayDelegateInfo(Delegate delObj)
{
    // Print the names of each member in the
    // delegate's invocation list.
    foreach (Delegate d in delObj.GetInvocationList())
    {
        Console.WriteLine("Method Name: {0}", d.Method);
        Console.WriteLine("Type Name: {0}", d.Target);
    }
}
```

假设你已经更新 Main() 方法，以调用这个新的辅助方法，如下所示：

```
BinaryOp b = new BinaryOp(SimpleMath.Add);
DisplayDelegateInfo(b);
```

你将得到如下输出：

```
***** Simple Delegate Example *****
Method Name: Int32 Add(Int32, Int32)
Type Name:
10 + 10 is 20
```

注意，在调用 Target 属性时，当前并没有显示目标类（SimpleMath）的名称。原因在于 BinaryOp 委托指向的是静态方法，因此没有可供引用的对象！不过，如果将 Add() 和 Subtract() 方法更新为非静态方法（只需删除 static 关键字），就可以创建 SimpleMath 类的实例，并使用对象引用指定要调用的方法。

```
using System;
using SimpleDelegate;
```

```
Console.WriteLine("***** Simple Delegate Example *****");
```

```
// Delegates can also point to instance methods as well.
SimpleMath m = new SimpleMath();
BinaryOp b = new BinaryOp(m.Add);
```

```
// Show information about this object.
DisplayDelegateInfo(b);

Console.WriteLine("10 + 10 is {0}", b(10, 10));
Console.ReadLine();
```

在这种情况下，你将得到如下输出：

```
**** Simple Delegate Example ****
Method Name: Int32 Add(Int32, Int32)
Type Name: SimpleDelegate.SimpleMath
10 + 10 is 20
```

## 使用委托发送对象状态通知

显然，前面的 SimpleDelegate 示例纯粹是为了说明问题，因为没有充分理由仅仅为了实现两个数字相加而定义一个委托。为了展示委托类型更实际的用途，我们将使用委托定义一个 Car 类，使其能够向外部实体通知当前的引擎状态。为此，需要执行以下步骤：

- 1. 定义一个新的委托类型，用于向调用方发送通知。
- 2. 在 Car 类中声明一个该委托类型的成员变量。
- 3. 在 Car 类中创建一个辅助函数，使调用方能够指定要回调的方法。
- 4. 实现 Accelerate() 方法，使其在适当的情况下调用委托的调用列表。

首先，创建一个名为 CarDelegate 的新控制台应用程序项目。现在，定义一个新的 Car 类，初始代码如下：

```
using System;
using System.Linq;

namespace CarDelegate
{
public class Car
{
    // Internal state data.
    public int CurrentSpeed { get; set; }
    public int MaxSpeed { get; set; } = 100;
    public string PetName { get; set; }

    // Is the car alive or dead?
    private bool _carIsDead;

    // Class constructors.
    public Car() {}
    public Car(string name, int maxSp, int currSp)
```

