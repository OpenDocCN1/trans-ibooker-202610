
// 这次使用 typeof 来提取一个 Type。
Console.WriteLine("EmpTypeEnum uses a {0} for storage", Enum.GetUnderlyingType(typeof(EmpTypeEnum)));

## 动态发现枚举的名称-值对

除了 Enum.GetUnderlyingType() 方法之外，所有 C# 枚举都支持一个名为 ToString() 的方法，该方法返回当前枚举值的字符串名称。以下代码是一个示例：

```
EmpTypeEnum emp = EmpTypeEnum.Contractor;
...
// 打印出 "emp is a Contractor"。
Console.WriteLine("emp is a {0}.", emp.ToString());
Console.ReadLine();
```

如果您有兴趣发现给定枚举变量的值，而不是其名称，则可以简单地将枚举变量强制转换为底层存储类型。以下是一个示例：

```
Console.WriteLine("**** Fun with Enums ****");
EmpTypeEnum emp = EmpTypeEnum.Contractor;
...
// 打印出 "Contractor = 100"。
Console.WriteLine("{0} = {1}", emp.ToString(), (byte)emp);
Console.ReadLine();
```

注意 静态 Enum.Format() 方法通过指定期望的格式标志提供了更精细的格式化选项。请查阅文档以获取完整的格式标志列表。

System.Enum 还定义了另一个名为 GetValues() 的静态方法。此方法返回 System.Array 的实例。数组中的每个项对应于指定枚举的一个成员。考虑以下方法，该方法将打印出您作为参数传入的任何枚举中的每个名称-值对：

// 此方法将打印出任何枚举的详细信息。
static void EvaluateEnum(System.Enum e)
{
    Console.WriteLine("=> Information about {0}", e.GetType().Name);

    Console.WriteLine("Underlying storage type: {0}", Enum.GetUnderlyingType(e.GetType()));

    // 获取传入参数的所有名称-值对。
    Array enumData = Enum.GetValues(e.GetType());
    Console.WriteLine("This enum has {0} members.", enumData.Length);

    // 现在显示字符串名称和相关联的值，使用 D 格式标志（参见第 3 章）。
    for(int i = 0; i < enumData.Length; i++)
    {
Console.WriteLine("Name: {0}, Value: {0:D}",
    enumData.GetValue(i));
}
```

要测试此新方法，请更新您的代码以创建 System 命名空间中声明的几种枚举类型的变量（以及为了好玩而添加一个 EmpTypeEnum 枚举）。以下代码是一个示例：

```
Console.WriteLine("**** Fun with Enums *****");
...
EmpTypeEnum e2 = EmpTypeEnum.Contractor;

// 这些类型是 System 命名空间中的枚举。
DayOfWeek day = DayOfWeek.Monday;
ConsoleColor cc = ConsoleColor.Gray;

EvaluateEnum(e2);
EvaluateEnum(day);
EvaluateEnum(cc);
Console.ReadLine();
```

这里显示了部分输出：

```
=> Information about DayOfWeek

Underlying storage type: System.Int32
This enum has 7 members.
Name: Sunday, Value: 0
Name: Monday, Value: 1
Name: Tuesday, Value: 2
Name: Wednesday, Value: 3
Name: Thursday, Value: 4
Name: Friday, Value: 5
Name: Saturday, Value: 6
```

正如您将在本书后续内容中看到的那样，枚举在 .NET Core 基类库中被广泛使用。当您使用任何枚举时，请始终记住您可以使用 System.Enum 的成员与名称-值对进行交互。

## 使用枚举、标志和按位运算

按位运算提供了一种快速机制，可在位级别上对二进制数进行操作。表 4-3 包含 C# 按位运算符、它们的作用以及每个运算符的示例。

### 表 4-3. 按位运算

| 运算符 | 操作 | 示例 |
|---|---|---|
| & (AND) | 如果位同时存在于两个操作数中则复制该位 | 0110 & 0100 = 0100 (4) |
| | (OR) | 如果位存在于任一操作数中则复制该位 | 0110 | 0100 = 0110 (6) |
| ^ (XOR) | 如果位仅存在于一个操作数中则复制该位 | 0110 ^ 0100 = 0010 (2) |
| ~ (按位取反) | 翻转位 | ~0110 = -7 (由于溢出) |
| << (左移) | 将位左移 | 0110 << 1 = 1100 (12) |
| >> (右移) | 将位右移 | 0110 >> 1 = 0011 (3) |

为了展示这些操作，创建一个名为 FunWithBitwiseOperations 的新控制台应用程序项目。将 Program.cs 文件更新为以下代码：

```
using System;
using FunWithBitwiseOperations;
Console.WriteLine("==== Fun with Bitwise Operations");
Console.WriteLine("6 & 4 = {0} | {1}", 6 & 4, Convert.ToString((6 & 4),2));
Console.WriteLine("6 | 4 = {0} | {1}", 6 | 4, Convert.ToString((6 | 4),2));
Console.WriteLine("6 ^ 4 = {0} | {1}", 6 ^ 4, Convert.ToString((6 ^ 4),2));
Console.WriteLine("6 << 1 = {0} | {1}", 6 << 1, Convert.ToString((6 << 1),2));
Console.WriteLine("6 >> 1 = {0} | {1}", 6 >> 1, Convert.ToString((6 >> 1),2));
Console.WriteLine("~6 = {0} | {1}", ~6, Convert.ToString(~(short)6,2));
Console.WriteLine("Int.MaxValue {0}", Convert.ToString((int.MaxValue),2));
Console.ReadLine();
```

执行代码后，您将看到以下结果：

```
==== Fun with Bitwise Operations

6 & 4 = 4 | 100
6 | 4 = 6 | 110
6 ^ 4 = 2 | 10
6 << 1 = 12 | 1100
6 >> 1 = 3 | 11
~6 = -7 | 1111111111111111111111111111001
Int.MaxValue 1111111111111111111111111111111
```

现在您已经了解了按位运算的基础知识，是时候将它们应用于枚举了。添加一个名为 ContactPreferenceEnum.cs 的新文件并将代码更新为以下内容：

```
using System;
namespace FunWithBitwiseOperations
{
    [Flags]
    public enum ContactPreferenceEnum
    {
        None = 1,
Email = 2,
Phone = 4,
Ponyexpress = 6
}
}
```

注意 Flags 属性。这允许将枚举的多个值组合到单个变量中。例如，Email 和 Phone 可以这样组合：

```
ContactPreferenceEnum emailAndPhone = ContactPreferenceEnum.Email | ContactPreferenceEnum.Phone;
```

这允许您检查组合值中是否存在其中一个值。例如，如果您想检查 emailAndPhone 变量中是否存在 ContactPreference 值，可以使用以下代码：

```
Console.WriteLine("None? {0}", (emailAndPhone | ContactPreferenceEnum.None) == emailAndPhone);
Console.WriteLine("Email? {0}", (emailAndPhone | ContactPreferenceEnum.Email) == emailAndPhone);
Console.WriteLine("Phone? {0}", (emailAndPhone | ContactPreferenceEnum.Phone) == emailAndPhone);
Console.WriteLine("Text? {0}", (emailAndPhone | ContactPreferenceEnum.Text) == emailAndPhone);
```

执行后，控制台窗口会显示以下内容：

```
None? False
Email? True
Phone? True
Text? False
```

## 理解结构（又名值类型）

现在您已经了解了枚举类型的角色，让我们来研究 .NET Core 结构（或简称为结构体）的使用。结构类型非常适合在应用程序中建模数学、几何和其他“原子”实体。结构（例如枚举）是一种用户定义的类型；然而，结构不仅仅是一组名称-值对。相反，结构是可包含任意数量数据字段和操作这些字段的成员的类型。

> 注意 如果您有面向对象编程的背景，可以将结构视为“轻量级类类型”，因为结构提供了一种定义支持封装的类型的方式，但不能用于构建相关类型的族。当您需要通过继承构建相关类型的族时，您需要使用类类型。

表面上，定义和使用结构的过程很简单，但正如他们所说，魔鬼在细节中。为了开始了解结构类型的基础知识，创建一个名为 FunWithStructures 的新项目。在 C# 中，结构使用 struct 关键字定义。定义一个名为 Point 的新结构，该结构定义了两个 int 类型的成员变量以及一组用于交互数据的操作方法。

```
struct Point
{
    // 结构的字段。
    public int X;
    public int Y;

    // 将 (X, Y) 位置加 1。
    public void Increment()
    {
        X++; Y++;
    }

    // 将 (X, Y) 位置减 1。
    public void Decrement()
    {
        X--; Y--;
    }

    // 显示当前位置。
    public void Display()
    {
        Console.WriteLine("X = {0}, Y = {1}", X, Y);
    }
}
```

这里，您使用 public 关键字定义了两个整数字段（X 和 Y），public 关键字是访问控制修饰符（第 5 章将继续讨论此内容）。使用 public 关键字声明数据可确保调用者可以通过点运算符直接从给定的 Point 变量访问数据。

注意 通常认为在类或结构中定义公共数据是不好的风格。相反，您希望定义私有数据，可以通过公共属性访问和更改。这些细节将在第 5 章中讨论。

以下是对 Point 类型进行测试的代码：

```
Console.WriteLine("***** A First Look at Structures ****\n");

// 创建一个初始 Point。
Point myPoint;
myPoint.X = 349;
myPoint.Y = 76;
myPoint.Display();

// 调整 X 和 Y 值。
myPoint.Increment();
myPoint.Display();
Console.ReadLine();
```
