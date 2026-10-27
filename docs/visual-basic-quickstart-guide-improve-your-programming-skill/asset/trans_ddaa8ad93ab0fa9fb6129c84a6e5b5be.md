
提供了短路求值，这意味着如果 `condition1` 为 false，则 `condition2` 不会被求值。在 `condition2` 代价高昂或耗时的情况下，短路可以提高性能。

下面是一个演示 **AndAlso** 运算符用法的示例：

```
Dim var1 As Integer = 4
Dim var2 As Integer = 8
If var1 >= 4 AndAlso var2 < 8 Then
    ' 此代码块不会执行。
End If
If var1 = 4 AndAlso var2 <= 8 Then
    ' 此代码块会执行。
End If
```

在此示例中，第一个 `If` 语句不会执行代码块，因为 *condition2*（`var2 < 8`）为 false。第二个 `If` 语句会执行代码块，因为两个条件（`var1 >= 4` 和 `var2 <= 8`）都为 true。由于两个 `If` 语句都使用了 **AndAlso** 运算符，因此如果 *condition1* 为 false，则不会对 *condition2* 求值，这样可以提高性能。

### GoTo 语句

在 VB 中，**GoTo** 语句将控制转移到代码中的特定行或标签。虽然现代编程实践通常不鼓励使用 **GoTo** 语句，但 Visual Basic 在某些特定场景下仍支持该语句。

Visual Basic 中 **GoTo** 语句的基本语法如下：

```
goto line_or_label
```

这里，`line_or_label` 指的是你希望将控制转移到的行号或标签。要了解 **GoTo** 的用法，请看以下示例：

```
Sub GotoProc()
    Dim num1 As Integer = 20
    If num1 > 10 Then
        GoTo Label1
    Else
        GoTo Label2
    End If
Label1:
    Console.WriteLine("The number is above 10.")
    GoTo EndLabel
Label2:
    Console.WriteLine("The number is equal or under 10.")
    GoTo EndLabel
EndLabel:
    Console.WriteLine("Program execution finished.")
End Sub
```

在此示例中，我们有一个名为 **GotoProc** 的子程序，它根据 `num1` 变量的值使用 **GoTo** 语句转移控制。如果 `num1` 大于 10，控制转移到 **Label1**，并显示一条消息。否则，控制转移到 **Label2**，并显示另一条消息。最终控制到达 **EndLabel**，在两种情况下都会显示一条完成消息。

**GoTo** 语句的历史与决策分支密切相关。**GoTo** 语句是一种控制转移语句，它允许程序执行跳转到代码的不同部分。它是 **FORTRAN** 和 **BASIC** 等早期编程语言中的基本构造。

然而，**GoTo** 语句的广泛使用导致了非结构化和难以维护的代码。结果，程序员经常使用 **GoTo** 语句来创建“意大利面条式代码”，使得程序流程变得不可预测且难以理解。这种做法后来被称为“**GoTo** 意大利面条”。

在 20 世纪 60 年代末和 70 年代初，结构化编程作为一门学科出现，以解决不受控制地使用 **GoTo** 语句所导致的问题。结构化编程强调使用循环和条件语句等结构化控制流结构，以提高代码的可读性和可维护性。

结构化编程的引入引发了一场反对 **GoTo** 语句的运动。Edsger Dijkstra 等有影响力的计算机科学家认为，应避免使用 **GoTo** 语句，因为它们可能导致无法管理的代码。Dijkstra 于 1968 年发表的著名论文 *《GoTo 语句有害》* 极大地改变了人们对 **GoTo** 语句的看法。

因此，现代编程语言和编码实践不鼓励甚至彻底消除 **GoTo** 语句的使用。相反，使用条件语句、循环和子程序等结构化控制流结构，以更有条理、更易管理的方式实现决策分支。此外，这些结构提供了更高的清晰度和可维护性，使程序员更容易理解其代码的行为。

总的来说，决策分支是编程中的一个基本概念，它允许根据条件执行不同的代码路径。然而，**GoTo** 语句的历史表明，编程语言和实践是如何演进的，以促进结构化控制流，并阻止不受控制地使用 **GoTo** 语句。

### 小结

本章介绍了使用 **If** 语句、**case** 语句和三元运算符进行决策分支。决策分支用于根据条件测试执行代码块。这允许根据输入或外部条件在代码中产生不同的路径。在下一章中，我们将继续学习迭代。

## 迭代

在编程中，迭代是指多次重复执行一组指令的过程。支持迭代的编程语言提供了一种重复执行代码块的方式，无论是固定次数还是直到满足特定条件。

大多数编程语言都提供某种形式的迭代控制结构，例如 for 循环、while 循环、do-while 循环或 foreach 循环，使程序员能够以结构化的方式执行迭代。

VB 是一种支持多种迭代形式的编程语言，包括 For 循环、While 循环和 Do 循环。这些循环允许你重复执行代码块特定次数，或直到满足特定条件。VB 还支持在迭代过程中改变循环流程的语句。

在本章中，我们将介绍以下主要内容：

- For 循环
- While 循环
- Do 循环
- Exit 和 Continue 语句
- VB.NET 与其他家族成员之间在循环中的类型安全差异
- 无限循环

### For 循环

For 循环是一种控制结构，允许你以固定次数重复执行代码块。VB 中 For 循环的基本语法如下：

```
For cntr As Integer = start To end Step stepValue
    '要执行的代码
Next
```

在此语法中，cntr 是用于控制循环的循环变量。循环从 start 的值开始，到 end 的值结束，每次循环执行时变量递增 stepValue。stepValue 是可选的，如果未指定，则默认为 1。

以下是一个 VB.NET 代码示例，它将在一定范围的数字上循环并将每个数字打印到控制台：

```
For x As Integer = 1 To 7
    Console.WriteLine(x)
Next
```

此代码会将数字 1 到 7 赋给变量 x，然后将其打印到控制台。

For 循环也可以有自定义步长值，例如：

```
For cntr As Integer = 1 To 20 Step 2
    Console.WriteLine(cntr)
Next
```

此代码将通过每次循环时将循环变量 cntr 递增 2 来打印从 1 到 20 的奇数。这些数字包括 1、3、5、7、9、11、13、15、17 和 19。

接下来，我们将了解基于条件测试执行零次或多次的循环。

### While 循环

在计算机编程中，带有前置条件的循环是一种控制流结构，它会在执行循环体之前，根据特定条件是否为 true 来重复执行一组语句。这种循环类型称为 while 循环，因为条件是在运行循环体之前检查的。由于循环可能永远不会执行，我们说前置条件循环总是执行零次或多次。

当你希望重复执行一段代码直到某个特定条件不再为 true 时，带有前置条件的 While 循环非常有用。While 循环通常用于无法预先知道需要重复循环次数的情况。通常，这是因为代码依赖于接收特定输入。

VB 中 while 循环的基本语法如下：

```
While test_condition
    '要运行的代码
End While
```

这里，test_condition 是在每次循环迭代之前求值的布尔表达式。如果 test_condition 为 true，循环将继续执行代码块。当 test_condition 变为 false 时，循环终止，并继续执行循环之后的语句。

以下是 VB 中 while 循环的一个示例。此示例迭代数字 1 到 20，并将每个数字打印到控制台：

```
Dim x As Integer = 1
While x <= 20
    Console.WriteLine(x)
```

