
## 表19-4（续）

| 操作码 | 含义 |
|---|---|
| box, unbox | 这些CIL操作码用于在引用类型和值类型之间进行转换。 |
| Ret | 该CIL操作码用于退出方法并（如有必要）向调用方返回一个值。 |
| beq, bgt, ble, blt, switch | 这些CIL操作码（以及许多其他相关操作码）用于控制方法内的分支逻辑。下面是一些示例：<br>beq：如果相等则跳转到代码标签<br>bgt：如果大于则跳转到代码标签<br>ble：如果小于或等于则跳转到代码标签<br>blt：如果小于则跳转到代码标签<br>所有以分支为中心的操作码都要求你指定一个CIL代码标签，以便在测试结果为true时跳转到该标签。 |
| Call | 该CIL操作码用于调用给定类型上的成员。 |
| newarr, newobj | 这些CIL操作码允许你（分别）在内存中分配一个新的数组或新的对象类型。 |

下一大类CIL操作码（表19-5中展示了其中一部分）用于将参数加载（推入）到虚拟执行栈上。请注意，这些特定于加载的操作码都有一个ld（load，加载）前缀。

## 表19-5. CIL的主要以栈为中心的操作码

| 操作码 | 含义 |
|---|---|
| ldarg（有多种变体） | 将方法的一个参数加载到栈上。除了通用的ldarg（它与标识参数的给定索引配合使用）之外，还有许多其他变体。例如，带有数值后缀的ldarg操作码（ldarg.0）会硬编码要加载的参数。此外，ldarg操作码的一些变体允许你使用表19-4中所示的CIL常量表示法来硬编码数据类型（ldarg_I4，用于int32），以及数据类型和值（ldarg_I4_5，用于加载值为5的int32）。 |
| ldc（有多种变体） | 将一个常量值加载到栈上。 |
| ldfld（有多种变体） | 将实例级字段的值加载到栈上。 |
| ldloc（有多种变体） | 将局部变量的值加载到栈上。 |
| ldobj | 获取基于堆的对象所收集的所有值，并将它们放到栈上。 |
| ldstr | 将一个字符串值加载到栈上。 |

除了这组特定于加载的操作码之外，CIL还提供了大量显式将栈顶值弹出的操作码。正如本章前几个示例所示，从栈中弹出一个值通常涉及将该值存储到临时本地存储器中以备进一步使用（例如作为即将进行的方法调用的参数）。因此，请注意，许多从虚拟执行栈中弹出当前值的操作码都带有st（store，存储）前缀。表19-6列出了其中的重点。

## 表19-6. 各种以弹出为中心的操作码

| 操作码 | 含义 |
|---|---|
| Pop | 移除当前位于计算栈顶部的值，但不进行存储。 |
| Starg | 将栈顶的值存储到指定索引处的方法参数中。 |
| stloc（有多种变体） | 从计算栈顶部弹出当前值，并将其存储到指定索引处的局部变量列表中。 |
| Stobj | 将指定类型的值从计算栈复制到所提供的内存地址中。 |
| Stsfld | 用计算栈中的值替换静态字段的值。 |

请注意，各种CIL操作码会隐式地从栈中弹出值以完成当前任务。例如，如果你尝试使用sub操作码来减去两个数字，那么应该清楚的是，sub必须先弹出接下来两个可用的值，然后才能执行计算。计算完成后，其结果（不出所料）会再次被推入栈中。

## .maxstack指令

当你使用原始CIL编写方法实现时，需要注意一个名为.maxstack的特殊指令。顾名思义，.maxstack规定了在执行方法期间，任意时刻可以推入栈中的变量最大数量。好消息是，.maxstack指令有一个默认值（8），对于你可能编写的大多数方法来说，这个值应该都是安全的。但是，如果你想显式指定，也可以手动计算栈上局部变量的数量并显式定义该值，如下所示：

```
.method public hidebysig instance void
    Speak() cil managed
{
    // During the scope of this method, exactly
    // 1 value (the string literal) is on the stack.
    .maxstack 1
    ldstr "Hello there..."
    call void [mscorlib]System.Console::WriteLine(string)
    ret
}
```

## 在CIL中声明局部变量

让我们首先看看如何声明一个局部变量。假设你想用CIL构建一个名为MyLocalVariables()的方法，该方法不接收参数并返回void。在该方法中，你想定义三个局部变量，类型分别为System.String、System.Int32和System.Object。在C#中，该成员将如下所示（请记住，局部作用域的变量不会获得默认值，在进一步使用之前应先将其设置为初始状态）：

```
public static void MyLocalVariables()
{
    string myStr = "CIL code is fun!";
    int myInt = 33;
    object myObj = new object();
}
```

如果你直接用CIL来构造MyLocalVariables()，可以编写如下代码：

```
.method public hidebysig static void
    MyLocalVariables() cil managed
{
    .maxstack 8
    // Define three local variables.
    .locals init (string myStr, int32 myInt, object myObj)
    // Load a string onto the virtual execution stack.
    ldstr "CIL code is fun!"
    // Pop off current value and store in local variable [0].
    stloc.0

    // Load a constant of type "i4"
    // (shorthand for int32) set to the value 33.
    ldc.i4.s 33
    // Pop off current value and store in local variable [1].
    stloc.1

    // Create a new object and place on stack.
    newobj instance void [mscorlib]System.Object::.ctor()
    // Pop off current value and store in local variable [2].
    stloc.2
    ret
}
```

在原始CIL中分配局部变量的第一步是使用.locals指令，该指令与init属性配合使用。每个变量通过其数据类型和一个可选变量名来标识。局部变量定义好之后，你可以将一个值加载到栈上（使用各种以加载为中心的操作码），然后将该值存储到局部变量中（使用各种以存储为中心的操作码）。

## 在CIL中将参数映射到局部变量

你已经看到了如何使用.locals init指令在原始CIL中声明局部变量；但是，你还没有看到如何将传入参数映射到局部方法。考虑下面这个静态C#方法：

```
public static int Add(int a, int b)
{
    return a + b;
}
```

这个看起来无害的方法在CIL方面有很多值得说明的地方。首先，必须使用ldarg（load argument，加载参数）操作码将传入参数（a和b）推入虚拟执行栈。接下来，add操作码将弹出栈中的下两个值，计算它们的和，然后将结果再次存储到栈上。最后，这个和会被弹出栈，并通过ret操作码返回给调用方。如果你使用ildasm.exe反汇编这个C#方法，会发现构建过程注入了大量额外的标记，但CIL代码的核心其实非常简单。

```
.method public hidebysig static int32 Add(int32 a,
    int32 b) cil managed
{
    .maxstack 2
    ldarg.0 // Load "a" onto the stack.
    ldarg.1 // Load "b" onto the stack.
    add     // Add both values.
    ret
}
```

## 隐藏的this引用

请注意，由于虚拟执行栈从位置0开始索引，因此两个传入参数（a和b）在CIL代码中使用其索引位置（索引0和索引1）来引用。

在检查或编写CIL代码时，有一件事需要注意：每个接收传入参数的非静态方法都会自动获得一个隐式的额外参数，该参数是对当前对象的引用（就像C#中的this关键字）。因此，如果Add()方法被定义为非静态方法，如下所示：

```
// No longer static!
public int Add(int a, int b)
{
    return a + b;
}
```

那么传入的参数a和b将使用ldarg.1和ldarg.2来加载（而不是预期的ldarg.0和ldarg.1操作码）。原因还是一样的：槽位0包含隐式的this引用。考虑下面的伪代码：

```
// This is JUST pseudocode!
.method public hidebysig static int32 AddTwoIntParams(
    MyClass_HiddenThisPointer this, int32 a, int32 b) cil managed
{
    ldarg.0 // Load MyClass_HiddenThisPointer onto the stack.
    ldarg.1 // Load "a" onto the stack.
    ldarg.2 // Load "b" onto the stack.
    ...
}
```

