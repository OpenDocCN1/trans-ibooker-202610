

### 表 19-3. 将 .NET 基类类型映射到 C# 关键字以及 C# 关键字映射到 CIL

| .NET Core 基类类型 | C# 关键字 | CIL 表示 | CIL 常量表示法 |
|---|---|---|---|
| System.SByte | sbyte | int8 | I1 |
| System.Byte | byte | unsigned int8 | U1 |
| System.Int16 | short | int16 | I2 |
| System.UInt16 | ushort | unsigned int16 | U2 |
| System.Int32 | int | int32 | I4 |
| System.UInt32 | uint | unsigned int32 | U4 |
| System.Int64 | long | int64 | I8 |
| System.UInt64 | ulong | unsigned int64 | U8 |
| System.Char | char | char | CHAR |
| System.Single | float | float32 | R4 |
| System.Double | double | float64 | R8 |
| System.Boolean | bool | bool | BOOLEAN |
| System.String | string | string | N/A |
| System.Object | object | object | N/A |
| System.Void | void | void | VOID |

> **注意** System.IntPtr 和 System.UIntPtr 类型映射到 native int 和 native unsigned int（了解这一点很有用，因为许多 COM 互操作和 P/Invoke 场景都大量使用它们）。

## 在 CIL 中定义类型成员

正如你已经了解的，.NET Core 类型可以支持各种成员。枚举有一组名称-值对。结构和类可能具有构造函数、字段、方法、属性、静态成员等。在本书的前 18 章中，你已经看到了之前提到的项目的部分 CIL 定义，但无论如何，这里快速回顾一下各种成员如何映射到 CIL 原语。

## 在 CIL 中定义字段数据

枚举、结构和类都可以支持字段数据。在每种情况下，都会使用 .field 指令。例如，让我们为骨架 MyEnum 枚举注入一些生命，并定义以下三个名称-值对（注意值在括号内指定）：

```
.class public sealed enum MyEnum
{
    .field public static literal valuetype
    MyNamespace.MyEnum A = int32(0)
    .field public static literal valuetype
    MyNamespace.MyEnum B = int32(1)
    .field public static literal valuetype
    MyNamespace.MyEnum C = int32(2)
}
```

位于 .NET Core System.Enum 派生类型的范围内的字段使用 static 和 literal 属性限定。正如你所猜测的，这些属性将字段数据设置为可从类型本身访问的固定值（例如 MyEnum.A）。

> **注意** 分配给枚举值的数值也可以使用 0x 前缀的十六进制形式。

当然，当你想在类或结构中定义一个字段数据点时，你并不局限于 public static literal 数据点。例如，你可以更新 MyBaseClass 以支持两个私有、实例级字段数据点，设置为默认值。

```
.class public MyBaseClass
{
    .field private string stringField = "hello!"
    .field private int32 intField = int32(42)
}
```

与 C# 一样，类字段数据将自动初始化为适当默认值。如果你希望允许对象使用者在创建时为这些私有字段数据点提供自定义值，你（当然）需要创建自定义构造函数。

## 在 CIL 中定义类型构造函数

CTS 同时支持实例级和类级（静态）构造函数。就 CIL 而言，实例级构造函数使用 .ctor 标记表示，而静态级构造函数则通过 .cctor（类构造函数）表示。两个 CIL 标记都必须使用 rtspecialname（返回类型特殊名称）和 specialname 属性限定。简单来说，这些属性用于标识一个特定的 CIL 标记，该标记可以由给定的 .NET Core 语言以独特方式处理。例如，在 C# 中，构造函数不定义返回类型；然而，就 CIL 而言，构造函数的返回值确实是 void。

```
.class public MyBaseClass
{
    .field private string stringField
    .field private int32 intField

    .method public hidebysig specialname rtspecialname
        instance void .ctor(string s, int32 i) cil managed
    {
        // TODO: Add implementation code...
    }
}
```

请注意，.ctor 指令已使用 instance 属性限定（因为它不是静态构造函数）。cil managed 属性表示此方法的作用域包含 CIL 代码，而不是在平台调用请求期间可能使用的非托管代码。

## 在 CIL 中定义属性

属性和方法也有特定的 CIL 表示。作为示例，如果 MyBaseClass 被更新以支持名为 TheString 的公共属性，你将编写以下 CIL（再次注意 specialname 属性的使用）：

```
.class public MyBaseClass
{
...
    .method public hidebysig specialname
        instance string get_TheString() cil managed
    {
        // TODO: Add implementation code...
    }

    .method public hidebysig specialname
        instance void set_TheString(string 'value') cil managed
    {
        // TODO: Add implementation code...
    }

    .property instance string TheString()
    {
        .get instance string
            MyNamespace.MyBaseClass::get_TheString()
        .set instance void
            MyNamespace.MyBaseClass::set_TheString(string)
    }
}
```

就 CIL 而言，属性映射到一对以 get_ 和 set_ 为前缀的方法。.property 指令使用相关的 .get 和 .set 指令将属性语法映射到正确的“特殊命名”方法。

> **注意** 请注意，属性的 set 方法的传入参数放在单引号中，这表示在方法作用域内赋值运算符右侧使用的标记名称。

## 定义成员参数

简而言之，在 CIL 中指定参数（或多或少）与在 C# 中做此操作相同。例如，每个参数都是通过指定其数据类型，后跟参数名称来定义的。此外，与 C# 一样，CIL 提供了定义输入、输出和按引用传递参数的方法。同样，CIL 允许你定义参数数组参数（即 C# 的 params 关键字），以及可选参数。

为了说明在原始 CIL 中定义参数的过程，假设你想构建一个方法，该方法接受一个 int32（按值传递）、一个 int32（按引用传递）、一个 [mscorlib]System.Collections.ArrayList，以及一个 int32 类型的单个输出参数。就 C# 而言，此方法看起来像这样：

```
public static void MyMethod(int inputInt,
    ref int refInt, ArrayList ar, out int outputInt)
{
    outputInt = 0; // Just to satisfy the C# compiler...
}
```

如果你将此方法映射到 CIL 术语，你会发现 C# 引用参数用与参数底层数据类型后缀的与号（&）标记。输出参数也使用 & 后缀，但它们进一步使用 CIL [out] 标记限定。还请注意，如果参数是引用类型（在本例中为 [mscorlib]System.Collections.ArrayList 类型），则 class 标记前缀于数据类型（不要与 .class 指令混淆！）。

```
.method public hidebysig static void MyMethod(int32 inputInt,
    int32& refInt,
    class [System.Runtime.Extensions]System.Collections.ArrayList ar,
    [out] int32& outputInt) cil managed
{
    ...
}
```

## 检查 CIL 操作码

本章中你将检查的 CIL 代码的最后一个方面与各种操作码的作用有关。回想一下，操作码只是用于构建给定成员实现逻辑的 CIL 标记。完整的 CIL 操作码集合（数量很大）可以分为以下几个大类：

- 控制程序流的操作码
- 评估表达式的操作码
- 访问内存中值的操作码（通过参数、局部变量等）

为了提供有关通过 CIL 进行成员实现的见解，表 19-4 定义了一些与成员实现逻辑相关的更有用的操作码，按相关功能分组。

### 表 19-4. 各种实现特定的 CIL 操作码

| 操作码 | 意义 |
|---|---|
| add, sub, mul, div, rem | 这些 CIL 操作码允许你对两个值进行加、减、乘和除运算（rem 返回除法运算的余数）。 |
| and, or, not, xor | 这些 CIL 操作码允许你对两个值执行按位操作。 |
| ceq, cgt, clt | 这些 CIL 操作码允许你以各种方式比较栈上的两个值。以下是一些示例：<br> ceq: 比较相等<br> cgt: 比较大于<br> clt: 比较小于 |

