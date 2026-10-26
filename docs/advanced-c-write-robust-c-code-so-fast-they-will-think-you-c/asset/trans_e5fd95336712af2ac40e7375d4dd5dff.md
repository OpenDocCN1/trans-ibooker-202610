

6. 现在，进入主函数并创建并初始化名称为 track_001 的 Track 类型的实例。最后，使用 std::cout 打印 track_001 的值：

```
int main()
{
    Track track_001("Summer night city",
                    "ABBA",
                    "1979",
                    213);

    std::cout << track_001 << std::endl;

    return 0;
}
```

7. 编译并执行应用程序。运行它。你将得到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_421_0.png)

图 6.4：执行练习 1 的结果

做得好。在这里，我们考虑了使用预定义的标准流对象，并学习了如何为用户定义的类型编写自己的重载移位运算符。让我们继续并检查使用 C++ 标准 IO 库进行文件读写。

## 文件 I/O 实现类

文件流管理对文件的输入和输出。它们提供了一个接口，该接口实现了资源获取即初始化（RAII）——文件在流构造时打开，并在其销毁时自动关闭。在标准库中，文件流由以下类表示：basic_ifstream 用于输入操作，basic_ofstream 用于输出操作，basic_fstream 用于输入和输出操作，以及 basic_filebuf 用于原始文件设备的实现。它们都定义在 <fstream> 头文件中。标准库还为 char 和 wchar_t 类型提供了 typedef，即 ifstream、fstream 和 ofstream，以及用于宽字符的带 "w" 前缀的相同名称。

我们可以以两种方式创建文件流。第一种方式是一行完成，即通过将文件名传递给构造函数来打开文件并将流连接到文件：

```
std::ofstream outFile(filename);
std::ifstream outFile(filename);
std::fstream outFile(filename);
```

另一种方式是创建对象然后调用 open() 函数：

```
std::ofstream outFile;
outFile.open(filename);
```

> **注意**
> 
> IO 流具有 bool 变量：**goodbit**、**eofbit**、**failbit** 和 **badbit**。
> 它们用于在每次操作后检查流的状态并指示流上发生的错误。

在对象创建后，我们可以通过检查 failbit 或检查与打开文件关联的流来检查流状态。要检查 failbit，请在文件流上调用 fail() 函数：

```
if (outFile.fail())
{
    std::cerr << filename << " file couldn't be opened"<< std::endl;
}
```

要检查流是否与打开的文件关联，请调用 is_open() 函数：

```
if (!outFile.is_open())
{
    std::cerr << filename << " file couldn't be opened"<< std::endl;
}
```

输入、输出和双向文件流也可以通过使用标志以不同的模式打开。它们在 ios_base 命名空间中声明。除了 ios_base::in 和 ios_base::out 标志外，我们还有 ios_base::ate、ios_base::app、ios_base::trunc 和 ios_base::binary 标志。ios_base::trunc 标志会删除文件的内容。ios_base::app 标志始终将输出写入文件的末尾。即使你决定更改文件中的位置，也无法做到这一点。ios_base::ate 标志将文件描述符的位置设置为文件末尾，但允许你稍后修改位置。最后，ios_base::binary 标志会抑制数据的任何格式化，以便以“原始”格式读取或写入。让我们考虑所有可能的打开模式组合。

默认情况下，std::ofstream 以 ios_base::out 模式打开，std::ifstream 以 ios_base::in 模式打开，std::fstream 以 ios_base::in|ios_base::out 模式打开。ios_base::out|ios_base::trunc 模式如果文件不存在则创建文件，或从现有文件中删除所有内容。ios_base::out|ios_base::app 模式如果文件不存在则创建文件，或打开现有文件并只允许你在文件末尾写入。上述两种模式都可以与 ios_base::in 标志组合，因此文件将同时以读写模式打开。

以下是如何使用上述模式打开文件的示例：

```
std::ofstream outFile(filename, std::ios_base::out|std::ios_base::trunc);
```

你也可以这样做：

```
std::ofstream outFile;

outFile.open(filename, std::ios_base::out|std::ios_base::trunc);
```

在我们以所需模式打开文件流后，我们可以开始向文件读写。文件流允许我们更改文件中的位置。让我们考虑如何做到这一点。要获取当前文件的位置，我们可以在 ios_base::out 模式下调用 tellp() 函数，在 ios_base::in 模式下调用 tellg() 函数。它可以在以后使用，以便在需要时返回此位置。我们还可以使用 ios_base::out 模式下的 seekp() 函数和 ios_base::in 模式下的 seekg() 函数来查找文件中的确切位置。它接受两个参数：要移动的字符数以及应从哪个文件位置开始计数。允许有三种可查找的位置类型：std::ios_base::beg，即文件开头，std::ios_base::end，即文件末尾，以及 std::ios_base::cur，即当前位置。以下是调用 seekp() 函数的示例：

```
outFile.seekp(-5, std::ios_base::end);
```

正如你所见，我们要求将当前文件的位置设置为文件末尾的第五个字符处。

要写入文件，我们可以使用重载的左移运算符 `<<` 进行通用格式化输出，使用 **put()** 函数写入单个字符，或使用 **write()** 函数写入字符块。使用左移运算符是将数据写入文件的最方便方式，因为你可以将任何内置类型作为参数传递：

```
outFile << "This is line No " << 1 << std::endl;
```

**put()** 和 **write()** 函数只能与字符值一起使用。

要从文件读取，我们可以使用重载的右移运算符 `>>`，或使用用于读取字符的一组函数，例如 **read()**、**get()** 和 **getline()**。右移运算符针对所有内置类型进行了重载，我们可以这样使用它：

```
std::ifstream inFile(filename);
```

```
std::string str;
```

```
int num;
```

```
float floatNum;
```

```
// for data: "book 3 24.5"
```

```
inFile >> str >> num >> floatNum;
```

最后，当执行离开可见性范围时，文件流将被关闭，因此我们不需要执行任何额外操作来关闭文件。

> **注意**
> 
> 在从文件读取数据时要小心。右移运算符 `>>` 仅在遇到空白或换行字符时读取字符串。要读取完整字符串，你可以使用循环或像我们在 *练习 1，重载左移运算符 << 用于用户定义类型* 中那样将每个单词读入单独的变量。

现在，让我们练习使用 C++ IO 标准库读写数据到文件。

## 练习 2：向文件读写用户定义数据类型

在这个练习中，我们将为一家书店编写一段代码。我们需要在文件中存储有关图书价格的信息，并在需要时从文件中读取这些信息。为此，我们将创建一个表示图书的类，该类具有名称、作者、出版年份和价格。接下来，我们将创建该类的一个实例并将其写入文件。稍后，我们将从文件中读取有关图书的信息并将其读入图书类的实例。执行以下步骤以完成此练习：

- 1. 包含所需的头文件：**`<iostream>`** 用于向控制台输出，**`<string>`** 用于字符串支持，以及 **`<fstream>`** 用于 I/O 文件库支持：

```
```cpp
#include <fstream>
#include <iostream>
#include <string>
```
```

- 2. 实现 **Book** 类，表示书店中的图书。在私有部分，定义四个具有自解释名称的变量：**m_Name**、**m_Author**、**m_Year** 和 **m_Price**。在公共部分，定义一个带有参数的构造函数，该构造函数初始化所有类成员。此外，在 **public** 部分，为所有类成员定义 getter：

```
```cpp
class Book
{
public:
    Book(const std::string& name,
         const std::string& author,
         const int year,
         const float price)
        : m_Name(name)
        , m_Author(author)
        , m_Year(year)
        , m_Price(price) {}

    std::string getName() const { return m_Name; }
    std::string getAuthor() const { return m_Author; }
    int getYear() const { return m_Year; }
```
