

- **7.** 编译并执行应用程序。现在，转到项目文件夹，找到 `prices.txt` 文件所在位置。在下面的截图中，你可以看到创建的文件在项目目录中的位置：

  | 名称 | 修改日期 | 大小 |
  | --- | --- | --- |
  | `.settings` | 01.09.2019 17:05 | |
  | `build` | 01.09.2019 17:02 | |
  | `src` | 01.09.2019 17:02 | |
  | `.cproject` | 01.09.2019 17:05 | 2 КБ |
  | `.project` | 01.09.2019 17:02 | 1 КБ |
  | `Makefile` | 01.09.2019 17:03 | 1 КБ |
  | `prices` | 02.09.2019 12:19 | 1 КБ |

  *图 6.5：所创建文件的位置*

- **8.** 在记事本中打开它。在下面的截图中，你可以看到输出到文件的内容：

  ![](img/6507d435e2edfa3c00c220d0ccfcaf94_427_0.png)

  *图 6.6：用户自定义类型输出到文件的结果*

- **9.** 现在，让我们将这些数据读取到变量中。创建一个 `std::ifstream` 类的实例。打开名为 `pricesFile` 的文件。检查流是否已成功打开，如果没有则打印错误消息：

  ```cpp
  std::ifstream inFile(pricesFile);
  if (inFile.fail())
  {
      std::cerr << "Failed to open file " << pricesFile << std::endl;
      return 1;
  }
  ```

- **10.** 创建将从文件输入时使用的局部变量，即 `name`、`authorName`、`authorSurname`、`year` 和 `price`。它们的名称不言自明：

  ```cpp
  std::string name;
  std::string authorName;
  std::string authorSurname;
  int year;
  float price;
  ```

- **11.** 现在，按照文件中的顺序将数据从文件读取到变量中：

  ```cpp
  inFile >> name >> authorName >> authorSurname >> year >> price;
  ```

- **12.** 创建一个名为 `book_002` 的 `Book` 实例，并用这些读取到的值对其进行初始化：

  ```cpp
  Book book_002(name, std::string(authorName + " " + authorSurname), year,
  price);
  ```

- **13.** 为了检查读取操作是否成功执行，将 `book_002` 变量打印到控制台：

  ```cpp
  std::cout << "Book name: " << book_002.getName() << std::endl
          << "Author name: " << book_002.getAuthor() << std::endl
          << "Year: " << book_002.getYear() << std::endl
          << "Price: " << book_002.getPrice() << std::endl;
  ```

- **14.** 再次编译并执行应用程序。在控制台中，你将看到以下输出：

  ![](img/6507d435e2edfa3c00c220d0ccfcaf94_428_0.png)

  *图 6.7：执行练习 2 的结果*

正如你所见，我们轻松地从一个文件中读写自定义格式的数据。我们创建了自己的自定义类型，使用 `std::ofstream` 类将其写入文件，并检查所有内容都已成功写入。然后，我们使用 `std::ifstream` 类将这些数据从文件读取到自定义变量中，将其输出到控制台，并确保所有内容都被正确读取。通过这样做，我们学会了如何使用 I/O 标准库向文件读写数据。现在，让我们继续学习 I/O 库的内存部分。

## 字符串 I/O 实现

I/O 标准库不仅允许与文件等设备进行输入输出，还允许与内存进行输入输出，特别是与 `std::string` 对象。在这种情况下，字符串既可以是输入操作的源，也可以是输出操作的目标。在 `<sstream>` 头文件中，声明了管理字符串输入输出的流类。与文件流一样，它们也提供了实现 RAII 的接口——字符串在流创建时打开以供读取或写入，并在流销毁时关闭。它们在标准库中由以下类表示：`basic_stringbuf`（实现原始字符串接口）、`basic_istringstream`（用于输入操作）、`basic_ostringstream`（用于输出操作）以及 `basic_stringstream`（用于输入和输出操作）。标准库还为 `char` 和 `wchar_t` 类型提供了 typedef：`istringstream`、`ostringstream` 和 `stringstream`，以及带有 `w` 前缀的宽字符版本。

要创建 `std::istringstream` 类的对象，我们应该将初始字符串作为构造函数参数传递，或者稍后使用 `str()` 函数设置它：

```cpp
std::string track("ABBA 1967 Vule");
std::istringstream iss(track);
```

或者，我们可以这样做：

```cpp
std::string track("ABBA 1967 Vule");
std::istringstream iss;
iss.str(track);
```

接下来，要从流中读取值，请使用右移运算符 `>>`，它为所有内置类型重载：

```cpp
std::string group;
std::string name;
int year;

iss >> group >> year >> name;
```

要创建 `std::ostringstream` 类的对象，我们只需声明一个该类型的变量：

```cpp
std::ostringstream oss;
```

接下来，要向字符串写入数据，请使用左移运算符 `<<`，它为所有内置类型重载：

```cpp
std::string group("ABBA");
std::string name("Vule");
int year = 1967;
```

```cpp
oss << group << std::endl
    << name << std::endl
    << year << std::endl;
```

要获取结果字符串，请使用 `str()` 函数：

```cpp
std::cout << oss.str();
```

`std::stringstream` 对象是双向工作的，因此它既有默认构造函数，也有接受字符串的构造函数。我们可以通过声明该类型的变量来创建默认的 `std::stringstream` 对象，然后将其用于读取和写入：

```cpp
std::stringstream ss;
ss << "45";
int count;
ss >> count;
```

此外，我们可以使用带有字符串参数的构造函数创建 `std::stringstream`。然后，我们可以像平常一样将其用于读取和写入：

```cpp
std::string employee("Alex Ismailow 26");
std::stringstream ss(employee);
```

或者，我们可以创建一个默认的 `std::stringstream` 对象，并通过 `str()` 函数设置字符串来初始化它：

```cpp
std::string employee("Charlz Buttler 26");
std::stringstream ss;
ss.str(employee);
```

接下来，我们可以使用 `ss` 对象进行读取和写入：

```cpp
std::string name;
std::string surname;
int age;
```

```cpp
ss >> name >> surname >> age;
```

我们还可以为这些类型的流应用打开模式。它们的功能与文件流类似，但略有不同。在处理字符串流时，`ios_base::binary` 无关紧要，而 `ios_base::trunc` 会被忽略。因此，我们可以以四种模式打开任何字符串流：`ios_base::app`、`ios_base::ate`、`ios_base::in` 和 `ios_base::out`。

### 练习 3：创建替换字符串中单词的函数

在这个练习中，我们将实现一个函数，该函数解析给定的字符串并将给定的单词替换为另一个单词。为了完成此练习，我们创建一个可调用类，它接受三个参数：原始字符串、要替换的单词和用于替换的单词。结果应返回新字符串。执行以下步骤以完成此练习：

- **1.** 包含所需的头文件：用于终端输出的 `<iostream>` 和用于字符串 I/O 支持的 `<sstream>`：

  ```cpp
  #include <sstream>
  #include <iostream>
  ```

- **2.** 实现名为 `Replacer` 的可调用类。它只有一个函数——一个重载的括号运算符 `()`，它返回一个字符串并接受三个参数：原始字符串、要替换的单词和用于替换的单词。函数声明如下所示：

  ```cpp
  std::string operator()(const std::string& originalString,
      const std::string& wordToBeReplaced,
      const std::string& wordReplaceBy);
  ```

- **3.** 接下来，创建 `istringstream` 对象，即 `iss`，并将 `originalString` 变量设置为输入源：

  ```cpp
  std::istringstream iss(originalString);
  ```

- **4.** 创建 `ostringstream` 对象，即 `oss`，它将保存转换后的字符串：

  ```cpp
  std::ostringstream oss;
  ```

- **5.** 然后，在循环中，只要还能输入，就将下一个单词读入 `word` 变量。检查该单词是否等于 `wordToBeReplaced` 变量。如果是，则将其替换为 `wordReplaceBy` 变量并写入 `oss` 流。如果不等，则将原始单词写入 `oss` 流。在每个单词后添加一个空格字符，因为 `iss` 流会截断它们。最后，返回结果。完整的类如下：

  ```cpp
  class Replacer
  {
  public:
      std::string operator()(const std::string& originalString,
                              const std::string& wordToBeReplaced,
                              const std::string& wordReplaceBy)
      {
          std::istringstream iss(originalString);
          std::ostringstream oss;

          std::string word;
          while (iss >> word)
          {
              if (0 == word.compare(wordToBeReplaced))
              {
                  oss << wordReplaceBy << " ";
              }
              else
              {
                  oss << word << " ";
              }
          }

          return oss.str();
      }
  };
  ```

