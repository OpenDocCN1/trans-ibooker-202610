

对于这些结构（上一节和下一节中的结构），允许不使用大括号。换句话说，下面的代码与之前的示例完全相同：

```
for(int i = 0; i < 4; i++)
    Console.WriteLine("Number is: {0} ", i);
```

虽然这是允许的，但通常并不是一个好主意。问题不在于单行语句，而在于从一行扩展到多行以上的语句。如果没有大括号，在迭代/决策结构中扩展代码时可能会出错。例如，下面两个示例并不相同：

```
for(int i = 0; i < 4; i++)
{
    Console.WriteLine("Number is: {0} ", i);
    Console.WriteLine("Number plus 1 is: {0} ", i+1)
}
for(int i = 0; i < 4; i++)
    Console.WriteLine("Number is: {0} ", i);
    Console.WriteLine("Number plus 1 is: {0} ", i+1)
```

如果你很幸运（像这个例子一样），额外的代码行会产生编译错误，因为变量 i 仅在 for 循环的作用域内定义。如果你不幸，你执行的代码不会被标记为编译器错误，但会是一个逻辑错误，这更难找到和调试。

## 使用决策结构和关系/相等运算符

现在你可以在一组语句上进行迭代，下一个相关概念是如何控制程序执行的流程。C# 定义了两个简单的结构来根据各种情况改变程序的流程：

- if/else 语句
- switch 语句

> 注意 C# 7 扩展了 is 表达式和 switch 语句，使用了一种称为模式匹配的技术。这里出于完整性考虑展示了这些扩展如何影响 if/else 和 switch 语句的基础。这些扩展在阅读第 6 章后会更有意义，第 6 章涵盖了基类/派生类规则、强制转换和标准的 is 运算符。

## 使用 if/else 语句

首先是 if/else 语句。与 C 和 C++ 不同，C# 中的 if/else 语句仅对布尔表达式操作，而不是 -1 或 0 这样的临时值。

## 使用相等和关系运算符

C# if/else 语句通常涉及使用表 3-8 中所示的 C# 运算符来获得字面布尔值。

表 3-8. C# 关系和相等运算符

| C# 相等/关系运算符 | 示例用法 | 含义 |
|---|---|---|
| == | if(age == 30) | 仅当每个表达式相同时返回 true |
| != | if("Foo" != myStr) | 仅当每个表达式不同时返回 true |
| < | if(bonus < 2000) | 如果表达式 A（bonus）小于表达式 B（2000）则返回 true |
| > | if(bonus > 2000) | 如果表达式 A（bonus）大于表达式 B（2000）则返回 true |
| <= | if(bonus <= 2000) | 如果表达式 A（bonus）小于或等于表达式 B（2000）则返回 true |
| >= | if(bonus >= 2000) | 如果表达式 A（bonus）大于或等于表达式 B（2000）则返回 true |

同样，C 和 C++ 程序员需要注意，测试条件是否不等于零的旧技巧在 C# 中不起作用。假设你想查看你正在处理的字符串是否长于零个字符。你可能会被诱惑写成这样：

```
static void IfElseExample()
{
    // This is illegal, given that Length returns an int, not a bool.
    string stringData = "My textual data";
    if(stringData.Length)
    {
        Console.WriteLine("string is greater than 0 characters");
    }
    else
    {
        Console.WriteLine("string is not greater than 0 characters");
    }
    Console.WriteLine();
}
```

如果你想使用 String.Length 属性来确定真假，你需要修改条件表达式使其解析为布尔值。

```
// Legal, as this resolves to either true or false.
If (stringData.Length > 0)
{
    Console.WriteLine("string is greater than 0 characters");
}
```

## 使用 if/else 进行模式匹配（新 7.0）

C# 7.0 中新增，*模式匹配* 允许在 if/else 语句中使用。模式匹配允许代码检查对象是否具有某些特征和属性，并根据这些属性和特征的（非）存在做出决策。如果你是面向对象编程的新手，不用担心；上一句话将在后面的章节中详细解释。只需知道（暂时）你可以使用 is 关键字检查对象的类型，如果模式匹配则将该对象分配给变量，然后使用该变量。

IfElsePatternMatching 方法检查两个对象变量并确定它们是字符串还是 int，然后将结果打印到控制台：

```
static void IfElsePatternMatching()
{
    Console.WriteLine("===If Else Pattern Matching ===/n");
    object testItem1 = 123;
    object testItem2 = "Hello";
    if (testItem1 is string myStringValue1)
    {
        Console.WriteLine($"{myStringValue1} is a string");
    }
    if (testItem1 is int myValue1)
    {
        Console.WriteLine($"{myValue1} is an int");
    }
    if (testItem2 is string myStringValue2)
    {
        Console.WriteLine($"{myStringValue2} is a string");
    }
    if (testItem2 is int myValue2)
    {
        Console.WriteLine($"{myValue2} is an int");
    }
    Console.WriteLine();
}
```

## 改进模式匹配（新 9.0）

C# 9.0 引入了对模式匹配的一系列改进，如表 3-9 所示。

*表 3-9. 模式匹配改进*

| 模式 | 含义 |
|---|---|
| 类型模式 | 检查变量是否为某种类型 |
| 带括号的模式 | 强制或强调模式组合的优先级 |
| 合取（and）模式 | 要求两个模式都匹配 |
| 析取（or）模式 | 要求任一模式匹配 |
| 否定（not）模式 | 要求模式不匹配 |
| 关系模式 | 要求输入小于、小于或等于、大于或大于或等于 |

更新后的 IfElsePatternMatchingUpdatedInCSharp9() 展示了这些新模式的实际应用：

```
static void IfElsePatternMatchingUpdatedInCSharp9()
{
    Console.WriteLine("============== C# 9 If Else Pattern Matching Improvements
=================/n");
    object testItem1 = 123;
    Type t = typeof(string);
    char c = 'f';

    //Type patterns
    if (t is Type)
    {
        Console.WriteLine($"{t} is a Type");
    }

    //Relational, Conjuctive, and Disjunctive patterns
    if (c is >= 'a' and <= 'z' or >= 'A' and <= 'Z')
    {
        Console.WriteLine($"{c} is a character");
    };

    //Parenthesized patterns
    if (c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z') or '.' or ',')
    {
        Console.WriteLine($"{c} is a character or separator");
    };

    //Negative patterns
    if (testItem1 is not string)
    {
        Console.WriteLine($"{testItem1} is not a string");
    }
    if (testItem1 is not null)
    {
        Console.WriteLine($"{testItem1} is not null");
    }
    Console.WriteLine();
}
```

## 使用条件运算符（更新 7.2、9.0）

条件运算符 (?:)，也称为三元条件运算符，是编写简单 if/else 语句的简写方法。语法如下：

```
condition ? first_expression : second_expression;
```

condition 是条件测试（if/else 语句的 if 部分）。如果测试通过，则在问号 (?) 之后立即执行的代码将被执行。如果测试未评估为 true，则在冒号之后的代码

else 部分的 if/else 语句）将被执行。之前的代码示例可以用条件运算符写成如下：

```
static void ExecuteIfElseUsingConditionalOperator()
{
    string stringData = "My textual data";
    Console.WriteLine(stringData.Length > 0
        ? "string is greater than 0 characters"
        : "string is not greater than 0 characters");
    Console.WriteLine();
}
```

条件运算符有一些限制。首先，first_expression 和 second_expression 的两种类型必须具有相互的隐式转换，或者在 C# 9.0 中新增的，每个都必须具有到目标类型的隐式转换。其次，只能在赋值语句中使用条件运算符。以下代码将导致编译器错误“Only assignment, call, increment, decrement, and new object expressions can be used as a statement”：

```
stringData.Length > 0
    ? Console.WriteLine("string is greater than 0 characters")
    : Console.WriteLine("string is not greater than 0 characters");
```

C# 7.2 中新增，条件运算符可用于返回条件结果的引用。以下示例使用两种形式的 by ref 条件运算符：

```
static void ConditionalRefExample()
{
    var smallArray = new int[] { 1, 2, 3, 4, 5 };
    var largeArray = new int[] { 10, 20, 30, 40, 50 };

    int index = 7;
    ref int refValue = ref ((index < 5)
        ? ref smallArray[index]
        : ref largeArray[index - 5]);
    refValue = 0;

    index = 2;
    ((index < 5)
        ? ref smallArray[index]
        : ref largeArray[index - 5]) = 100;

    Console.WriteLine(string.Join(" ", smallArray));
    Console.WriteLine(string.Join(" ", largeArray));
}
```

如果你不熟悉 ref 关键字，现在不用担心太多，因为它将在下一章中深入介绍。总结一下，第一个示例返回条件检查的数组位置的引用，并将 refValue 变量分配给该引用。概念上将引用视为指向数组中位置的指针，而不是数组位置的实际值。这允许通过更改分配给变量的值直接更改数组中该位置的值。将 refValue 变量的值设置为零的结果会将第二个数组的值更改为 10,20,0,40,50。第二个示例将第一个数组的第二个值更新为 100，导致 1,2,100,4,5。

