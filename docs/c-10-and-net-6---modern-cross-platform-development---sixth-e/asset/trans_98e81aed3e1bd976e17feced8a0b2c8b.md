
```
FileStream writableFile when s.CanWrite
    => "The stream is a file that I can write to.",
FileStream readOnlyFile
    => "The stream is a read-only file.",
MemoryStream ms
    => "The stream is a memory stream.",
null
    => "The stream is null.",
_
    => "The stream is some other type."
};

WriteLine(message);
```

主要区别在于移除了 case 和 break 关键字。下划线字符 _ 用于表示默认返回值。

2. 运行代码，并注意结果与之前相同。

## 理解迭代语句

迭代语句在条件为真时重复执行一段语句，或对集合中的每个项执行。选择使用哪种语句基于解决逻辑问题的易理解性以及个人偏好的结合。

## 使用 while 语句循环

The while 语句评估布尔表达式，并在其为真时继续循环。让我们探索迭代语句：

- 1. 使用你偏好的编码工具在 Chapter03 工作区/解决方案中添加一个新的控制台应用程序，命名为 IterationStatements。
- 2. 在 Visual Studio Code 中，将 IterationStatements 选择为活动的 OmniSharp 项目。
- 3. 在 Program.cs 中，输入语句定义一个 while 语句，在整数变量的值小于 10 时循环，如以下代码所示：

```
int x = 0;

while (x < 10)
{
    WriteLine(x);
    x++;
}
```

4. 运行代码并查看结果，应为数字 0 到 9，如以下输出所示：

```
0
1
2
3
4
5
6
7
8
9
```

## 使用 do 语句循环

do 语句与 while 类似，只是布尔表达式在块底部而不是顶部检查，这意味着块至少执行一次，如下所示：

1. 输入语句定义一个 do 循环，如以下代码所示：

```
string? password;

do
{
    Write("Enter your password: ");
    password = ReadLine();
}
while (password != "Pa$$w0rd");

WriteLine("Correct!");
```

2. 运行代码，注意系统会反复提示你输入密码，直到输入正确，如以下输出所示：

```
Enter your password: password
Enter your password: 12345678
Enter your password: ninja
Enter your password: correct horse battery staple
Enter your password: Pa$$w0rd
Correct!
```

3. 作为可选挑战，添加语句使用户最多只能尝试十次，然后显示错误消息。

## 使用 for 语句循环

for 语句与 while 类似，只是更简洁。它结合了：

- 一个 **初始化表达式**，在循环开始时执行一次。
- 一个 **条件表达式**，在每次迭代开始时执行，以检查是否应继续循环。
- 一个 **迭代器表达式**，在语句底部每次循环时执行。

for 语句通常与整数计数器一起使用。让我们探索一些代码：

1. 输入一个 for 语句输出数字 1 到 10，如以下代码所示：

```
for (int y = 1; y <= 10; y++)
{
    WriteLine(y);
}
```

2. 运行代码查看结果，应为数字 1 到 10。

## 使用 foreach 语句循环

foreach 语句与之前的三个迭代语句略有不同。

它用于对序列中的每个项执行一段语句，例如数组或集合。每个项通常是只读的，如果在迭代期间修改序列结构，例如通过添加或删除项，则会抛出异常。

尝试以下示例：

1. 输入语句创建一个字符串变量数组，然后输出每个的长度，如以下代码所示：

```
string[] names = { "Adam", "Barry", "Charlie" };

foreach (string name in names)
{
    WriteLine($"{name} has {name.Length} characters.");
}
```

2. 运行代码并查看结果，如以下输出所示：

```
Adam has 4 characters.
Barry has 5 characters.
Charlie has 7 characters.
```

## 理解 foreach 内部的工作原理

表示多个项的任何类型的创建者，如数组或集合，应确保程序员可以使用 foreach 语句枚举该类型项。

从技术上讲，foreach 语句可用于遵循以下规则的任何类型：

- 1. 该类型必须有一个名为 GetEnumerator 的方法，返回一个对象。
- 2. 返回的对象必须具有名为 Current 的属性和名为 MoveNext 的方法。
- 3. MoveNext 方法必须更改 Current 的值，并在仍有项可枚举时返回 true，或在没有更多项时返回 false。

存在名为 IEnumerable 和 IEnumerable<T> 的接口正式定义了这些规则，但从技术上讲，编译器并不要求类型实现这些接口。

编译器将前述示例中的 foreach 语句转换为类似以下伪代码的内容：

```
IEnumerator e = names.GetEnumerator();

while (e.MoveNext())
{
    string name = (string)e.Current; // Current is read-only!
    WriteLine($"{name} has {name.Length} characters.");
}
```

由于使用了迭代器，foreach 语句中声明的变量不能用于修改当前项的值。

## 在类型之间进行转换和转换

你经常需要在不同类型之间转换变量的值。例如，数据输入通常以文本形式在控制台输入，因此最初存储在 string 类型的变量中，但随后需要根据存储和处理方式将其转换为日期/时间、数字或其他数据类型。

有时在执行计算之前，你需要在数字类型之间转换，例如在整数和浮点数之间。

转换也称为强制转换，有两种类型：隐式和显式。隐式转换会自动发生，并且是安全的，意味着不会丢失任何信息。

显式转换必须手动执行，因为它可能会丢失信息，例如数字的精度。通过显式转换，你是在告诉 C# 编译器你理解并接受风险。

## 隐式和显式地转换数字

将 int 变量隐式转换为 double 变量是安全的，因为不会丢失信息，如下所示：

- 1. 使用你偏好的编码工具在 Chapter03 工作区/解决方案中添加一个新的控制台应用程序，命名为 CastingConverting。
- 2. 在 Visual Studio Code 中，将 CastingConverting 选择为活动的 OmniSharp 项目。
- 3. 在 Program.cs 中，输入语句声明并赋值一个 int 变量和一个 double 变量，然后在将其赋值给 double 变量时隐式转换整数的值，如以下代码所示：
- 4. 输入语句声明并赋值一个 double 变量和一个 int 变量，然后在将其赋值给 int 变量时隐式转换 double 值，如以下代码所示：
- 5. 运行代码并注意错误消息，如以下输出所示：
- 6. 修改 d 变量的赋值语句，如以下代码所示：
- 7. 运行代码查看结果，如以下输出所示：

```
int a = 10;
double b = a; // an int can be safely cast into a double
WriteLine(b);
```

```
double c = 9.8;
int d = c; // compiler gives an error for this line
WriteLine(d);
```

```
Error: (6,9): error CS0266: Cannot implicitly convert type 'double' to 'int'. An explicit conversion exists (are you missing a cast?)
```

此错误消息也会出现在 Visual Studio 错误列表或 Visual Studio Code PROBLEMS 窗口中。

你不能将 double 变量隐式转换为 int 变量，因为它可能不安全且可能丢失数据，例如小数点后的值。你必须使用一对圆括号围绕要转换 double 类型成的类型，显式地将 double 变量转换为 int 变量。这对圆括号是强制转换运算符。即便如此，你也必须注意，小数点后的部分将被无警告地截断，因为你选择执行显式转换，因此理解其后果。

```
int d = (int)c;
WriteLine(d); // d is 9 losing the .8 part
```

```
10
9
```
