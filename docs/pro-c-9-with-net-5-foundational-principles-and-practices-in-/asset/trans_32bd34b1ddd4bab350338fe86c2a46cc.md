

如果查看此应用程序的输出，你可能会惊讶地发现 sum 包含的值为 94（而不是预期的 350）。原因很简单。鉴于 System.Byte 只能保存 0 到 255 之间（包含）的值（总共 256 个槽），sum 现在包含溢出值（350 – 256 = 94）。默认情况下，如果你不采取任何纠正措施，溢出/下溢条件会在没有错误的情况下发生。

要在应用程序中处理溢出或下溢条件，你有两个选择。你的第一个选择是利用你的智慧和编程技能手动处理所有溢出/下溢条件。当然，这种技术的问题在于简单的事实是你是一个人类，即使你最好的尝试也可能导致错误而逃脱你的视线。

值得庆幸的是，C# 提供了 checked 关键字。当你将语句（或语句块）包装在 checked 关键字的范围内时，C# 编译器会发出额外的 CIL 指令来测试在添加、乘法、减法或除法两个数值数据类型时可能发生的溢出条件。

如果发生了溢出，你将收到一个运行时异常：System.OverflowException。第 7 章将检查结构化异常处理以及 try 和 catch 关键字的使用的所有细节。在不在此刻过于纠结于细节的情况下，请观察以下更新：

```
static void ProcessBytes()
{
    byte b1 = 100;
    byte b2 = 250;

    // This time, tell the compiler to add CIL code
    // to throw an exception if overflow/underflow
    // takes place.
    try
    {
        byte sum = checked((byte)Add(b1, b2));
        Console.WriteLine("sum = {0}", sum);
    }
    catch (OverflowException ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

请注意，Add() 的返回值已被包装在 checked 关键字的范围内。因为总和大于一个字节，所以这会触发运行时异常。请注意通过 Message 属性打印出的错误消息。

Arithmetic operation resulted in an overflow.

如果你想强制对代码语句块进行溢出检查，可以通过定义“checked 范围”来做到这一点，如下所示：

```
try
{
    checked
    {
        byte sum = (byte)Add(b1, b2);
        Console.WriteLine("sum = {0}", sum);
    }
}
catch (OverflowException ex)
{
    Console.WriteLine(ex.Message);
}
```

在任何一种情况下，所讨论的代码都将被自动评估为可能的溢出条件，如果遇到则会触发溢出异常。

## 设置项目范围的溢出检查

如果你正在创建一个应用程序，该应用程序绝不允许静默溢出发生，你可能会发现自己处于令人厌烦的位置，即将大量代码行包装在 checked 关键字的范围内。作为替代方案，C# 编译器支持 /checked 标志。当启用时，你的所有算术运算都将被评估溢出，而无需使用 C# checked 关键字。如果发现溢出，你仍然会收到运行时异常。要为整个项目设置它，请在项目文件中输入以下内容：

```
<PropertyGroup>
    <CheckForOverflowUnderflow>true</CheckForOverflowUnderflow>
</PropertyGroup>
```

## 设置项目范围的溢出检查（Visual Studio）

要使用 Visual Studio 启用此标志，请打开项目的属性页。选择所有配置，然后单击构建选项卡上的高级按钮。从 resulting 对话框中，选择“检查算术溢出/下溢”复选框（见图 3-3）。在创建调试构建时启用此设置可能有所帮助。所有溢出异常一旦从代码库中消除，你就可以为后续构建禁用 /checked 标志（这可以提高应用程序的运行时性能）。

![](img/587021f4d5ba039be70a11107130377b_139_0.png)

图 3-3. 启用项目范围的溢出/下溢数据检查

为了说明隐式类型的角色，创建一个名为 ImplicitlyTypedLocalVars 的新控制台应用程序项目并将其添加到你的解决方案中。将 Program.cs 中的代码更新为以下内容：

```
using System;
using System.Linq;
Console.WriteLine("***** Fun with Implicit Typing *****");
```

添加以下函数来演示隐式声明：

```
static void DeclareImplicitVars()
{
    // Implicitly typed local variables
    // are declared as follows:
    // var variableName = initialValue;
    var myInt = 0;
    var myBool = true;
    var myString = "Time, marches on...";
}
```

注意 严格来说，var 不是 C# 关键字。允许声明名为 var 的变量、参数和字段而不会出现编译时错误。但是，当 var 标记用作数据类型时，编译器在上下文中将其视为关键字。

在这种情况下，编译器能够根据最初分配的值推断出 myInt 实际上是 System.Int32，myBool 是 System.Boolean，而 myString 确实是 System.String 类型。你可以通过反射打印类型名称来验证这一点。正如你在第 17 章中将更详细地看到的那样，反射是在运行时确定类型的组成的行为。例如，使用反射，你可以确定隐式类型局部变量的数据类型。用以下代码语句更新你的方法：

```
static void DeclareImplicitVars()
{
    // Implicitly typed local variables.
    var myInt = 0;
    var myBool = true;
    var myString = "Time, marches on...";

    // Print out the underlying type.
    Console.WriteLine("myInt is a: {0}", myInt.GetType().Name);
    Console.WriteLine("myBool is a: {0}", myBool.GetType().Name);
    Console.WriteLine("myString is a: {0}", myString.GetType().Name);
}
```

注意 请注意，你可以使用此隐式类型用于任何类型，包括数组、泛型类型（见第 10 章）和你自己的自定义类型。你将在本书的后续部分看到隐式类型的其他示例。

如果你从顶层语句调用 DeclareImplicitVars() 方法，你会发现输出如下所示：

```
**** Fun with Implicit Typing ****

myInt is a: Int32
myBool is a: Boolean
myString is a: String
```

## 隐式声明数值

如前所述，整数默认是整数，浮点数默认是双精度数。创建一个名为 DeclareImplicitNumerics 的新方法，并添加以下代码来演示数值的隐式声明：

```
static void DeclareImplicitNumerics()
{
    // Implicitly typed numeric variables.
    var myUInt = 0u;
    var myInt = 0;
    var myLong = 0L;
    var myDouble = 0.5;
    var myFloat = 0.5F;
    var myDecimal = 0.5M;

    // Print out the underlying type.
    Console.WriteLine("myUInt is a: {0}", myUInt.GetType().Name);
    Console.WriteLine("myInt is a: {0}", myInt.GetType().Name);
    Console.WriteLine("myLong is a: {0}", myLong.GetType().Name);
    Console.WriteLine("myDouble is a: {0}", myDouble.GetType().Name);
    Console.WriteLine("myFloat is a: {0}", myFloat.GetType().Name);
    Console.WriteLine("myDecimal is a: {0}", myDecimal.GetType().Name);
}
```

## 了解隐式类型变量的限制

关于 var 关键字的用法有各种限制。首先，隐式类型仅适用于方法或属性范围中的局部变量。使用 var 关键字定义自定义类型的返回值、参数或字段数据是非法的。例如，以下类定义将导致各种编译时错误：

```
class ThisWillNeverCompile
{
    // Error! var cannot be used as field data!
    private var myInt = 10;

    // Error! var cannot be used as a return value
    // or parameter type!
    public var MyMethod(var x, var y){}
}
```

此外，使用 var 关键字声明的局部变量必须在声明的确切时间分配初始值，并且不能分配初始值为 null。最后这个限制是有道理的，因为编译器仅根据 null 无法推断变量在内存中将指向的类型。

```
// Error! Must assign a value!
var myData;
```

```
// Error! Must assign value at exact time of declaration!
var myInt;
myInt = 0;
```

```
// Error! Can't assign null as initial value!
var myObj = null;
```

但是，允许在初始分配后将推断的局部变量分配为 null（前提是它是引用类型）。

```
// OK, if SportsCar is a reference type!
var myCar = new SportsCar();
myCar = null;
```

此外，允许将隐式类型局部变量的值分配给其他变量的值，无论是否隐式类型。

```
// Also OK!
var myInt = 0;
var anotherInt = myInt;
```

```
string myString = "Wake up!";
var myData = myString;
```

此外，允许将隐式类型局部变量返回给调用者，前提是方法返回类型与 var 定义的数据点的底层类型相同。

```
static int GetAnInt()
{
    var retVal = 9;
    return retVal;
}
```

## 隐式类型数据是强类型数据

请注意，局部变量的隐式类型化会产生强类型数据。因此，使用 var 关键字并不是与脚本语言（如 JavaScript 或 Perl）或 COM Variant 数据类型中使用的技术相同，在这些技术中，变量在其在程序中的生命周期内可以保存不同类型的值（通常称为动态类型）。

> 注意 C# 使用一个名为——惊喜，惊喜——dynamic 的关键字允许在 C# 中进行动态类型。你将在第 18 章学习语言的这一方面。

