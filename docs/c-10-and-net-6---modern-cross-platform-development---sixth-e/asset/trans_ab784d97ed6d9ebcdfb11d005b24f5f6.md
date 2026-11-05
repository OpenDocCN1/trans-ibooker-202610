
## 03

## 控制流、类型转换和异常处理

本章全部关于编写执行变量上的简单操作、做出决策、执行模式匹配、重复语句或代码块、将变量或表达式值从一种类型转换为另一种类型、处理异常以及检查数字变量溢出的代码。

本章涵盖以下主题：

- 操作变量
- 理解选择语句
- 理解迭代语句
- 在类型之间进行强制转换和转换
- 处理异常
- 检查溢出

### 操作变量

**运算符** 对诸如变量和字面值之类的 **操作数** 应用诸如加法和乘法之类的简单操作。它们通常返回操作结果的新值，该值可以分配给变量。

大多数运算符是二元的，这意味着它们作用于两个操作数，如以下伪代码所示：

```
var resultOfOperation = firstOperand operator secondOperand;
```

二元运算符的示例包括加法和乘法，如以下代码所示：

```
int x = 5;
int y = 3;
int resultOfAdding = x + y;
int resultOfMultiplying = x * y;
```

一些运算符是一元的，这意味着它们作用于单个操作数，并且可以在操作数之前或之后应用，如以下伪代码所示：

```
var resultOfOperation = onlyOperand operator;
var resultOfOperation2 = operator onlyOperand;
```

一元运算符的示例包括增量器和检索类型或其以字节为单位的大小，如以下代码所示：

```
int x = 5;
int postfixIncrement = x++;
int prefixIncrement = ++x;
Type theTypeOfAnInteger = typeof(int);
int howManyBytesInAnInteger = sizeof(int);
```

三元运算符作用于三个操作数，如以下伪代码所示：

```
var resultOfOperation = firstOperand ? secondOperand : thirdOperand;
```

### 探索一元运算符

两个常见的一元运算符用于递增 ++ 和递减 -- 一个数字。让我们编写一些示例代码来展示它们的工作方式：

1. 如果您已完成前面的章节，那么您已经拥有一个 Code 文件夹。如果没有，则需要创建它。
2. 使用您首选的编码工具创建新的控制台应用程序，如以下列表中定义：
   1. 项目模板：控制台应用程序 / console
   2. 工作区/解决方案文件和文件夹：Chapter03
   3. 项目文件和文件夹：Operators
3. 在 Program.cs 顶部，静态导入 System.Console。
4. 在 Program.cs 中，声明两个名为 a 和 b 的整数变量，将 a 设置为 3，在分配结果的同时递增 a，然后输出它们的值，如以下代码所示：

```
int a = 3;
int b = a++;
WriteLine($"a is {a}, b is {b}");
```

5. 在运行控制台应用程序之前，请问自己一个问题：您认为输出时 b 的值会是多少？思考之后，运行代码，并将您的预测与实际结果进行比较，如以下输出所示：

```
a is 4, b is 3
```

变量 b 的值为 3，因为 ++ 运算符在赋值之后执行；这被称为后缀运算符。如果需要在赋值之前递增，则使用前缀运算符。

6. 复制并粘贴语句，然后修改它们以重命名变量并使用前缀运算符，如以下代码所示：

```
int c = 3;
int d = ++c; // increment c before assigning it
WriteLine($"c is {c}, d is {d}");
```

7. 重新运行代码并注意结果，如以下输出所示：

```
a is 4, b is 3
c is 4, d is 4
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_127_0.png)

**良好实践：** 由于在与赋值结合使用时，递增和递减运算符的前缀和后缀形式会造成混淆，Swift 编程语言设计者在第 3 版中决定放弃对该运算符的支持。我在 C# 中的使用建议是永远不要将 ++ 和 -- 运算符的使用与赋值运算符 = 结合使用。将操作作为单独的语句执行。

### 探索二元算术运算符

递增和递减是一元算术运算符。其他算术运算符通常是二元的，允许您对两个数字执行算术运算，如下所示：

1. 添加语句以声明并为两个名为 e 和 f 的整数变量分配值，然后将五个常见的二元算术运算符应用于这两个数字，如以下代码所示：

```
int e = 11;
int f = 3;
WriteLine($"e is {e}, f is {f}");
WriteLine($"e + f = {e + f}");
WriteLine($"e - f = {e - f}");
WriteLine($"e * f = {e * f}");
WriteLine($"e / f = {e / f}");
WriteLine($"e % f = {e % f}");
```

2. 运行代码并注意结果，如以下输出所示：

```
e is 11, f is 3
e + f = 14
e - f = 8
e * f = 33
e / f = 3
e % f = 2
```

要理解将除法 / 和取模 % 运算符应用于整数时的工作原理，您需要回想小学知识。想象您有十一个糖果和三个朋友。

您如何将糖果分给朋友？您可以给每个朋友三个糖果，还会剩下两个。这些两个糖果就是模数，也称为除法后的余数。如果您有十二个糖果，那么每个朋友得到四个，没有剩余，因此余数将为 0。

3. 添加语句以声明并为名为 g 的 double 变量分配值，以显示整数除法和实数除法的区别，如以下代码所示：

```
double g = 11.0;
WriteLine($"g is {g:N1}, f is {f}");
WriteLine($"g / f = {g / f}");
```

4. 运行代码并注意结果，如以下输出所示：

```
g is 11.0, f is 3
g / f = 3.6666666666666665
```

如果第一个操作数是浮点数，例如值为 11.0 的 g，则除法运算符返回浮点值，例如 3.66666666666665，而不是整数。

### 赋值运算符

您已经在使用最常见的赋值运算符 =。

为了使代码更简洁，您可以将赋值运算符与其他运算符（如算术运算符）结合使用，如以下代码所示：

```
int p = 6;
p += 3; // equivalent to p = p + 3;
p -= 3; // equivalent to p = p - 3;
p *= 3; // equivalent to p = p * 3;
p /= 3; // equivalent to p = p / 3;
```

### 探索逻辑运算符

逻辑运算符作用于布尔值，因此返回 true 或 false。让我们探索作用于两个布尔值的二元逻辑运算符：

1. 使用您首选的编码工具向 Chapter03 工作区/解决方案添加一个新的控制台应用程序，命名为 BooleanOperators。
   1. 在 Visual Studio Code 中，选择 BooleanOperators 作为活动的 OmniSharp 项目。当您看到弹出警告消息说缺少必需的资源时，单击“是”以添加它们。
   2. 在 Visual Studio 中，将解决方案的启动项目设置为当前选择。

> **良好实践：** 请记住静态导入 System.Console 类型以简化语句。

2. 在 Program.cs 中，添加语句以声明两个布尔变量，其值分别为 true 和 false，然后输出显示应用 AND、OR 和 XOR（异或）逻辑运算符结果的真值表，如以下代码所示：

```
bool a = true;
bool b = false;

WriteLine($"AND  | a      | b      ");
WriteLine($"a    | {a & a,-5} | {a & b,-5} ");
WriteLine($"b    | {b & a,-5} | {b & b,-5} ");
WriteLine();
WriteLine($"OR   | a      | b      ");
WriteLine($"a    | {a | a,-5} | {a | b,-5} ");
WriteLine($"b    | {b | a,-5} | {b | b,-5} ");
WriteLine();
WriteLine($"XOR  | a      | b      ");
WriteLine($"a    | {a ^ a,-5} | {a ^ b,-5} ");
WriteLine($"b    | {b ^ a,-5} | {b ^ b,-5} ");
```

3. 运行代码并注意结果，如以下输出所示：

```
AND  | a      | b
 a    | True   | False
 b    | False  | False

OR   | a      | b
 a    | True   | True
 b    | True   | False

XOR  | a      | b
 a    | False  | True
 b    | True   | False
```
