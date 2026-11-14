
## 文件 I/O 操作

### 写入文件

要写入文件，我们可以使用重载的左移运算符 `<<` 进行通用格式化输出，使用 `put()` 函数写入单个字符，或使用 `write()` 函数写入一块字符。使用左移运算符是将数据写入文件最方便的方式，因为你可以将任何内置类型作为参数传递：

```cpp
outFile << "This is line No " << 1 << std::endl;
```

`put()` 和 `write()` 函数只能用于字符值。

### 从文件读取

要从文件读取，我们可以使用重载的右移运算符 `>>`，或使用一组读取字符的函数，如 `read()`、`get()` 和 `getline()`。右移运算符为所有内置类型重载，我们可以这样使用它：

```cpp
std::ifstream inFile(filename);

std::string str;

int num;

float floatNum;

// for data: "book 3 24.5"

inFile >> str >> num >> floatNum;
```

最后，当执行离开可见作用域时，文件流会自动关闭，因此我们不需要执行任何额外操作来关闭文件。

### 注意

从文件读取数据时要小心。右移运算符 `>>` 只会读取字符串直到遇到空白字符或换行符。要读取完整字符串，你可以使用循环，或将每个词读取到单独的变量中，就像我们在*练习 1*、*为用户定义类型重载左移运算符 ,<<,* 中所做的那样。

## 练习 2：将用户定义的数据类型读写到文件

在本练习中，我们将为一家书店编写一段代码。我们需要将书籍价格信息存储在文件中，并在需要时从文件中读取该信息。为了实现这一点，我们将创建一个表示书籍的类，包含名称、作者、出版年份和价格。接下来，我们将创建该类的实例并将其写入文件。稍后，我们将从文件中读取书籍信息到书籍类的实例中。执行以下步骤完成此练习：

### 步骤

1.  包含所需的头文件：`<iostream>` 用于控制台输出，`<string>` 用于字符串支持，`<fstream>` 用于 I/O 文件库支持：

    ```cpp
    #include <fstream>
    #include <iostream>
    #include <string>
    ```

2.  实现 `Book` 类，表示书店中的书籍。在私有部分，定义四个名称不言自明的变量：`m_Name`、`m_Author`、`m_Year` 和 `m_Price`。在公有部分，定义一个带参数的构造函数，用于初始化所有类成员。此外，在 `public` 部分，为所有类成员定义获取器：

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
        float getPrice() const { return m_Price; }

    private:
        std::string m_Name;
        std::string m_Author;
        int m_Year;
        float m_Price;
    };
    ```

3.  进入 `main` 函数并声明 `pricesFile` 变量，该变量保存文件名：

    ```cpp
    std::string pricesFile("prices.txt");
    ```

4.  接下来，创建 `Book` 类的实例并用 `book name`、`author name`、`year` 和 `price` 初始化它：

    ```cpp
    Book book_001("Brave", "Olena Lizina", 2017, 33.57);
    ```

5.  将此类实例写入文件。创建 `std::ofstream` 类的实例。使用 `pricesFile` 变量名打开文件。检查流是否成功打开，如果未成功则打印错误消息：

    ```cpp
    std::ofstream outFile(pricesFile);
    if (outFile.fail())
    {
        std::cerr << "Failed to open file " << pricesFile << std::endl;
        return 1;
    }
    ```

6.  然后，使用获取器将 `book_001` 书籍的所有信息写入文件，每项之间用空格分隔，末尾加换行符：

    ```cpp
    outFile << book_001.getName() << " "
            << book_001.getAuthor() << " "
            << book_001.getYear() << " "
            << book_001.getPrice() << std::endl;
    ```

7.  编译并执行应用程序。现在，转到项目文件夹并找到 `prices.txt` 文件的位置。在下面的屏幕截图中，你可以看到项目目录中创建的文件位置：

    ![](image/C14583_06_05.jpg)

    **图 6.5：创建的文件位置**

8.  在**记事本**中打开它。在下面的屏幕截图中，你可以看到输出到文件的内容：

    ![](image/C14583_06_06.jpg)

    **图 6.6：用户定义类型输出到文件的结果**

9.  现在，让我们将这些数据读取到变量中。创建 `std::ifstream` 类的实例。打开名为 `pricesFile` 的文件。检查流是否成功打开，如果未成功则打印错误消息：

    ```cpp
    std::ifstream inFile(pricesFile);
    if (inFile.fail())
    {
        std::cerr << "Failed to open file " << pricesFile << std::endl;
        return 1;
    }
    ```

10. 创建将用于从文件输入的局部变量，即 `name`、`authorName`、`authorSurname`、`year` 和 `price`。它们的名称不言自明：

    ```cpp
    std::string name;
    std::string authorName;
    std::string authorSurname;
    int year;
    float price;
    ```

11. 现在，按文件中的顺序将数据从文件读取到变量中：

    ```cpp
    inFile >> name >> authorName >> authorSurname >> year >> price;
    ```

12. 创建一个名为 `book_002` 的 `Book` 实例，并用读取的值初始化它：

    ```cpp
    Book book_002(name, std::string(authorName + " " + authorSurname), year, price);
    ```

13. 为了检查读取操作是否成功执行，将 `book_002` 变量打印到控制台：

    ```cpp
    std::cout  << "Book name: " << book_002.getName() << std::endl
               << "Author name: " << book_002.getAuthor() << std::endl
               << "Year: " << book_002.getYear() << std::endl
               << "Price: " << book_002.getPrice() << std::endl;
    ```

### 结果

14. 再次编译并执行应用程序。在控制台中，你将看到以下输出：

    ![](image/C14583_06_07.jpg)

    **图 6.7：执行练习 2 的结果**

正如你所见，我们毫无困难地将自定义格式数据写入文件并从中读取。我们创建了自己的自定义类型，使用 `std::ofstream` 类将其写入文件，并检查一切是否成功写入。然后，我们使用 `std::ifstream` 类从文件中将这些数据读取到自定义变量中，输出到控制台，并确保一切读取正确。通过这样做，我们学习了如何使用 I/O 标准库读写文件数据。现在，让我们继续学习 I/O 库的内存部分。

## 字符串 I/O 实现
