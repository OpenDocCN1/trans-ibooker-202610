

### 用于字段宽度和填充控制的 I/O 操纵符

在标准库中，也有用于操纵输出字段宽度的函数，即当宽度大于输出数据时应使用哪些字符，以及这些填充字符应插入在何处。当你想将输出向左或向右对齐，或者想用其他符号替换空格时，这些函数将非常有用。例如，假设你需要在两列中打印价格。如果你使用标准格式化，将得到以下输出：

```
2.33 3.45

2.2 4.55

3.67 3.02
```

这看起来不太好，而且难以阅读。如果我们应用格式化，输出将如下所示：

```
2.33   3.45

2.2    4.55

3.67   3.02
```

这看起来好多了。同样，你可能想检查哪些字符被用来填充空白处，哪些是你实际插在数字之间的空格。让我们将填充字符设置为 "`*`"，例如。你将得到以下输出：

```
2.33* 3.45*

2.2** 4.55**

3.67* 3.02**
```

现在，你可以看到空白处被星号填充了。既然我们已经了解了在哪里格式化宽度和填充输出会很有用，现在让我们看看如何使用 I/O 操纵符来实现这一点。`std::setw` 和 `std::setfill` 函数声明在 `<iomanip>` 头文件中。`std::setw` 接受一个整数值作为参数，并将流的宽度设置为精确的 n 个字符。有几种情况会将宽度设置为 0。它们如下：

*   当移位运算符与 `std::string` 或 `char` 一起调用时
*   当调用 `std::put_money()` 函数时
*   当调用 `std::quoted()` 函数时

在 `<ios>` 头文件中，声明了用于更改填充字符插入位置的函数：`std::internal`、`std::left` 和 `std::right`。它们仅用于输出操作，且仅影响整数、浮点数和货币值。

现在，让我们看一个将它们一起使用的示例。让我们以宽度 10 输出正数、负数、浮点数和十六进制值，并将填充字符替换为 "`#`"：

```
std::cout << "Internal fill: " << std::endl
          << std::setfill('#')
          << std::internal
          << std::setw(10) << -2.38 << std::endl
          << std::setw(10) << 2.38 << std::endl
          << std::setw(10) << std::hex << std::showbase << 0x4b << std::endl;

std::cout << "Left fill: " << std::endl
          << std::left
          << std::setw(10) << -2.38 << std::endl
          << std::setw(10) << 2.38 << std::endl
          << std::setw(10) << std::hex << std::showbase << 0x4b << std::endl;

std::cout << "Right fill: " << std::endl
          << std::right
          << std::setw(10) << -2.38 << std::endl
          << std::setw(10) << 2.38 << std::endl
          << std::setw(10) << std::hex << std::showbase << 0x4b << std::endl;
```

构建并运行此示例后，你将得到以下输出：

```
Internal fill:

-#####2.38

######2.38

0x######4b

Left fill:

-2.38#####

2.38######

0x4b######

Right fill:

#####-2.38

######2.38

######0x4b
```

### 用于其他数值格式化的 I/O 操纵符

如果你需要带 "`+`" 号输出正数值，可以使用来自 `<ios>` 头文件的另一个 I/O 操纵符——`std::showpos` 函数。含义相反的操纵符也存在——`std::noshowpos` 函数。它们都影响输出。它们的用法非常简单。让我们看看下面的例子：

```
std::cout << "Default formatting: " << 13 << " " << 0 << std::endl;
std::cout << "showpos flag is set: " << std::showpos << 13 << " " << 0 << std::endl;
std::cout << "noshowpos flag is set: " << std::noshowpos << 13 << " " << 0 << std::endl;
```

在这里，我们先用默认格式化输出，然后设置 `std::showpos` 标志输出，最后设置 `std::noshowpos` 标志输出。如果你构建并运行这个小例子，你会看到，默认情况下设置的是 `std::noshowpos` 标志。看执行结果：

```
Default formatting: 13 0
showpos flag is set: +13 +0
noshowpos flag is set: 13 0
```

你可能还想为浮点数或十六进制数字输出大写字符，以便可以使用来自 `<ios>` 头文件的函数：`std::uppercase` 和 `std::nouppercase`。它们仅作用于输出。让我们看一个小例子：

```
std::cout << "12345.0 in uppercase with precision 4: "
          << std::setprecision(4) << std::uppercase << 12345.0 << std::endl;
std::cout << "12345.0 in no uppercase with precision 4: "
          << std::setprecision(4) << std::nouppercase << 12345.0 << std::endl;
std::cout << "0x2a in uppercase: "
          << std::hex << std::showbase << std::uppercase << 0x2a << std::endl;
std::cout << "0x2a in nouppercase: "
          << std::hex << std::showbase << std::nouppercase << 0x2a << std::endl;
```

在这里，我们分别带有和不带有 `std::uppercase` 标志输出浮点数和十六进制数字。默认情况下，设置的是 `std::nouppercase` 标志。看执行结果：

```
12345.0 in uppercase with precision 4: 1.234E+004
12345.0 in no uppercase with precision 4: 1.234e+004
0x2a in uppercase: 0X2A
0x2a in nouppercase: 0x2a
```

### 用于空白字符处理的 I/O 操纵符

在标准库中，有用于处理空白字符的函数。来自 `<istream>` 头文件的 `std::ws` 函数仅适用于输入流，并丢弃前导空白字符。来自 `<ios>` 头文件的 `std::skipws` 和 `std::noskipws` 函数用于控制读取和写入前导空白字符。它们适用于输入和输出流。当设置了 `std::skipws` 标志时，流会忽略字符序列输入前的空白字符。默认情况下，设置了 `std::skipws` 标志。让我们看一个使用这些 I/O 操纵符的例子。首先，我们将使用默认格式化读取输入并输出读取的内容。接下来，我们将清空字符串并使用 `std::noskipws` 标志读取数据：

```
std::string name;
std::string surname;
std::istringstream("Peppy Ping") >> name >> surname;
std::cout << "Your name: " << name << ", your surname: " << surname << std::endl;
name.clear();
surname.clear();
std::istringstream("Peppy Ping") >> std::noskipws >> name >> surname;
std::cout << "Your name: " << name << ", your surname: " << surname << std::endl;
```

构建并运行此示例后，我们将得到以下输出：

```
Your name: Peppy, your surname: Ping
Your name: Peppy, your surname:
```

正如你从前面的输出中所见，如果我们设置了 `std::noskipws` 标志，我们也会读取空白字符。

在 `<iomanip>` 头文件中，声明了一个该头文件不寻常的操纵符：`std::quoted`。当此函数应用于输入时，它会用转义字符将给定字符串包裹在引号中。如果输入字符串已经包含转义引号，它也会读取它们。为了理解这一点，让我们看一个小例子。我们将用一些不带引号的文本初始化源字符串，另一个字符串用带有转义引号的文本初始化。接下来，我们将在未设置标志的情况下使用 `std::ostringstream` 读取它们，并通过 `std::cout` 提供输出。请看下面的例子：

```
std::string str1("String without quotes");
std::string str2("String with quotes \"right here\"");
std::ostringstream ss;
ss << str1;
std::cout << "[" << ss.str() << "]" << std::endl;
ss.str("");
ss << str2;
std::cout << "[" << ss.str() << "]" << std::endl;
```

结果，我们将得到以下输出：

```
[String without quotes]
[String with quotes "right here"]
```

现在，让我们做同样的输出，但带上 `std::quoted` 调用：

```
std::string str1("String without quotes");
std::string str2("String with quotes \"right here\"");
std::ostringstream ss;
ss << std::quoted(str1);
```

