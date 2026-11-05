

4. 运行代码并注意结果，如以下输出所示：

```
Your password is too short. Use at least 8 characters.
```

## 为什么应该始终在使用 if 语句时使用大括号

由于每个块内只有一个语句，前面的代码可以不使用大括号编写，如以下代码所示：

```
if (password.Length < 8)
    WriteLine("Your password is too short. Use at least 8 characters.");
else
    WriteLine("Your password is strong.");
```

这种 if 语句的写法应避免，因为它可能引入严重的错误，例如苹果 iPhone iOS 操作系统中臭名昭著的 #gotofail 错误。

在 2012 年 9 月发布 Apple 的 iOS 6 之后 18 个月，其 Secure Sockets Layer (SSL) 加密代码中存在一个错误，这意味着任何运行 Safari（设备的网络浏览器）的用户尝试连接安全网站，例如他们的银行，都无法得到适当的保护，因为一个重要的检查被意外跳过。

仅仅因为你可以省略大括号并不意味着你应该这样做。没有它们，你的代码并不是“更高效”；相反，它的可维护性更差，并且可能更危险。

## 使用 if 语句进行模式匹配

从 C# 7.0 及更高版本引入的一项功能是模式匹配。if 语句可以结合使用 is 关键字和声明局部变量来使代码更安全：

- 1. 添加语句，以便如果变量名为 o 中存储的值是 int，则该值会被赋给局部变量名为 i，然后可以在 if 语句内部使用。这比使用变量名为 o 更安全，因为我们确信 i 是一个 int 变量而不是其他东西，如以下代码所示：

```
// add and remove the """ to change the behavior
object o = "3";
int j = 4;

if (o is int i)
{
    WriteLine($"{i} x {j} = {i * j}");
}
else
{
    WriteLine("o is not an int so it cannot multiply!");
}
```

2. 运行代码并查看结果，如以下输出所示：

```
o is not an int so it cannot multiply!
```

3. 删除 "3" 值周围的双引号字符，以便变量名为 o 中存储的值是 int 类型而不是字符串类型。

4. 重新运行代码以查看结果，如以下输出所示：

```
3 x 4 = 12
```

## 使用 switch 语句进行分支

switch 语句与 if 语句不同，因为 switch 将单个表达式与多个可能的 case 语句列表进行比较。每个 case 语句都与单个表达式相关。每个 case 部分必须以以下内容结束：

- - break 关键字（如以下代码中的 case 1）
- - 或 goto case 关键字（如以下代码中的 case 2）
- - 或它们不应该有语句（如以下代码中的 case 3）
- - 或引用命名标签的 goto 关键字（如以下代码中的 case 5）
- - 或 return 关键字以离开当前函数（代码中未显示）

让我们编写一些代码来探索 switch 语句：

1. 输入 switch 语句的语句。你应该注意倒数第二个语句是一个可以跳转到的标签，并且第一个语句生成 1 到 6 之间的随机数（代码中的数字 7 是排他性的上界）。switch 语句的分支基于此随机数的值，如以下代码所示：

```
int number = (new Random()).Next(1, 7);
WriteLine($"My random number is {number}");

switch (number)
{
    case 1:
        WriteLine("One");
        break; // jumps to end of switch statement
    case 2:
        WriteLine("Two");
        goto case 1;
    case 3: // multiple case section
    case 4:
        WriteLine("Three or four");
        goto case 1;
    case 5:
        goto A_label;
    default:
        WriteLine("Default");
        break;
} // end of switch statement

WriteLine("After end of switch");
A_label:
WriteLine($"After A_label");
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_137_0.png)

**良好实践：** 你可以使用 goto 关键字跳转到另一个 case 或标签。大多数程序员都不赞成使用 goto 关键字，但在某些场景下它可以成为代码逻辑的良好解决方案。然而，你应该谨慎使用它。

- 2. 多次运行代码以查看随机数各种情况下会发生什么，如以下示例输出所示：

```
// first random run
My random number is 4
Three or four
One
After end of switch
After A_label

// second random run
My random number is 2
Two
One
After end of switch
After A_label

// third random run
My random number is 6
Default
After end of switch
After A_label

// fourth random run
My random number is 1
One
After end of switch
After A_label

// fifth random run
My random number is 5
After A_label
```

## 使用 switch 语句进行模式匹配

与 if 语句一样，switch 语句在 C# 7.0 及更高版本中支持模式匹配。case 值不再需要是字面值；它们可以是模式。

让我们看一个使用文件夹路径的 switch 语句模式匹配的示例。如果你使用的是 macOS，请交换设置 path 变量的注释语句并用你的用户文件夹名称替换我的用户名：

- 1. 添加语句以声明一个指向文件的字符串路径，将其打开为只读或可写流，然后根据流的类型和功能显示一条消息，如以下代码所示：

```csharp
// string path = "/Users/markjprice/Code/Chapter03";
string path = @"C:\Code\Chapter03";

Write("Press R for read-only or W for writeable: ");
ConsoleKeyInfo key = ReadKey();
WriteLine();

Stream? s;

if (key.Key == ConsoleKey.R)
{
    s = File.Open(
        Path.Combine(path, "file.txt"),
        FileMode.OpenOrCreate,
        FileAccess.Read);
}
else
{
    s = File.Open(
        Path.Combine(path, "file.txt"),
        FileMode.OpenOrCreate,
        FileAccess.Write);
}

string message;

switch (s)
{
    case FileStream writeableFile when s.CanWrite:
        message = "The stream is a file that I can write to.";
        break;
    case FileStream readOnlyFile:
        message = "The stream is a read-only file.";
        break;
    case MemoryStream ms:
        message = "The stream is a memory address.";
        break;
    default: // always evaluated last despite its current position
        message = "The stream is some other type.";
        break;
    case null:
        message = "The stream is null.";
        break;
}

WriteLine(message);
```

2. 运行代码并注意变量名为 s 被声明为 Stream 类型，因此它可以是任何子类型的流，例如内存流或文件流。在此代码中，流是使用 File.Open 方法创建的，该方法返回一个文件流，并且根据你的按键，它将是可写或只读的，因此结果将是一条描述情况的消息，如以下输出所示：

```
The stream is a file that I can write to.
```

在 .NET 中，有多个 Stream 的子类型，包括 FileStream 和 MemoryStream。在 C# 7.0 及更高版本中，你的代码可以根据流的子类型更简洁地分支，并声明和分配局部变量以安全地使用它。你将在第 9 章《使用文件、流和序列化》中了解更多关于 System.IO 命名空间和 Stream 类型的内容。

此外，case 语句可以包含 when 关键字以执行更具体的模式匹配。在前述代码中的第一个 case 语句中，s 只有在流是 FileStream 并且其 CanWrite 属性为 true 时才匹配。

## 使用 switch 表达式简化 switch 语句

在 C# 8.0 或更高版本中，你可以使用 switch 表达式简化 switch 语句。

大多数 switch 语句非常简单，但需要大量输入。switch 表达式旨在简化你需要输入的代码，同时在所有 case 都返回一个值以设置单个变量的场景中仍然表达相同的意图。switch 表达式使用 lambda，=>，来指示返回值。

让我们使用 switch 表达式实现之前使用 switch 语句的代码，以便你可以比较这两种风格：

- 1. 输入语句以使用 switch 表达式根据流的类型和功能设置 message，如以下代码所示：

```
message = s switch
{
```
