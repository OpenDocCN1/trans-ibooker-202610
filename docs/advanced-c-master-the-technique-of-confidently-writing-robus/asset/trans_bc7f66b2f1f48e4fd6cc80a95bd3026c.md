

## 12. 并行化标准库函数

这段代码不如我们在 `Snippet15.cpp` 中基于循环的版本快。原因在于：在循环版本中，我们将 2 个字节组合读取为 `short` 以获取索引，这不需要计算；但在这里，我们通过位运算将 2 个字节读入 `short`。

现在，大部分工作由标准库函数完成，我们可以免费获得自动并行化 —— 按如下方式编译并测试：

```bash
$ g++ -O3 -fopenmp -D_GLIBCXX_PARALLEL Snippet17.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

输出如下：

![图 8.47：Snippet17.cpp 中并行化标准库代码的输出](image/C14583_08_47.jpg)

**图 8.47：Snippet17.cpp 中并行化标准库代码的输出**

显然，它无法完全并行化，因此速度仅提升约 2.5 倍，但这无需修改代码即可实现。我们能否以同样的方式使基于循环的代码并行化？理论上可以 —— 我们可以手动使用 `OpenMP` 指令实现；但这需要修改代码并了解 `OpenMP` 的用法。`Snippet16.cpp` 版本呢？

```bash
$ g++ -O3 -fopenmp -D_GLIBCXX_PARALLEL Snippet16.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

输出如下：

![图 8.48：Snippet16.cpp 中并行化标准库代码的输出](image/C14583_08_48.jpg)

**图 8.48：Snippet16.cpp 中并行化标准库代码的输出**

该版本也有类似的提升。我们完成了吗？还能更快吗？著名游戏程序员 **Michael Abrash** 创造了首字母缩略词 **TANSTATFC** —— 代表 "There ain't no such thing as the fastest code"（不存在最快的代码）。他的意思是：只要投入足够的精力，总能让代码跑得更快。这看似不可能，但一次又一次，人们发现了执行计算的更快方法 —— 我们的代码也不例外，还能再进一步。优化的一个权衡是降低代码的通用性 —— 我们已经给代码加了一些约束，例如只处理 **ASCII** 英文文本。通过给输入数据添加更多约束，我们可以做得更好。假设文件中没有不可打印字符。这对我们的输入数据是合理的假设。如果假设成立，则可以简化空格检测条件 —— 因为所有空白字符的 ASCII 码都大于等于 32，我们可以完全省去查找表。

## 13. 实现简化逻辑的代码

基于上述思路实现代码（完整代码见 `Snippet18.cpp`）：

```cpp
int wordCount(const std::string &s)
{
    auto isWordEnd = [&](char a, char b)
    {
        return a > 32 & b < 33;
    };
    return std::inner_product(s.begin(), s.end()-1, s.begin()+1, 0, std::plus<int>(), isWordEnd);
}
```

## 14. 编译运行并对比并行化效果

```bash
$ g++ -O3 Snippet18.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

输出如下：

![图 8.49：Snippet18.cpp 中用于检测空格的简化逻辑代码输出](image/C14583_08_49.jpg)

**图 8.49：Snippet18.cpp 中用于检测空格的简化逻辑代码输出**

该版本比并行化版本快一倍，且仅需几行代码。并行化能否让它更快？

```bash
$ g++ -O3 -fopenmp -D_GLIBCXX_PARALLEL Snippet18.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

输出如下：

![图 8.50：Snippet18.cpp 中并行化标准库代码的输出](image/C14583_08_50.jpg)

**图 8.50：Snippet18.cpp 中并行化标准库代码的输出**

不幸的是并非如此 —— 反而变慢了。管理多线程和线程竞争的开销有时比多线程代码的收益更大。此时可以看到，文件读取代码占用了大部分时间 —— 我们能优化这部分吗？

## 15. 修改 `main()` 函数分别计时各部分

修改 `main()` 函数以单独计时各个部分（完整代码见 `SnippetWC2.cpp`）：

```cpp
{
    Timer t("File read");
    buf << ifs.rdbuf();
}
{
    Timer t("String copy");
    sContent = buf.str();
}
{
    Timer t("String push");
    sContent.push_back(' ');
}
int wc;
{
    Timer t("Word count");
    wc = wordCount(sContent);
}
```

## 16. 编译运行并分析耗时分布

```bash
$ g++ -O3 Snippet18.cpp SnippetWC2.cpp Timer.cpp
$ ./a.out data.txt
```

输出如下：

![图 8.51：Snippet18.cpp 中所有操作计时的输出](image/C14583_08_51.jpg)

**图 8.51：Snippet18.cpp 中所有操作计时的输出**

大部分时间花在 `push_back()` 和字符串复制上。因为字符串大小正好等于文件大小，`push_back()` 最终会为字符串分配新缓冲区并复制内容。如何消除这个 `push_back()` 调用？我们在末尾追加空格是为了能统一计算最后一个单词（如果有），因为我们的算法统计单词的结尾。有三种方法可以避免：统计单词的开头而非结尾；单独统计最后一个单词（如果有）；使用 `c_str()` 函数，这样末尾会有 `NUL` 字符。下面依次尝试这三种方法。

## 17. 方案一：去掉 `push_back` 的 `main()` 函数

编写不带 `push_back` 的主函数（完整代码见 `SnippetWC3.cpp`）：

```cpp
{
    Timer t("File read");
    buf << ifs.rdbuf();
}
{
    Timer t("String copy");
    sContent = buf.str();
}
int wc;
{
    Timer t("Word count");
    wc = wordCount(sContent);
}
```

## 18. 方案一对应的 `wordCount()` 修改

将 `isWordEnd()` 重命名为 `isWordStart()` 并反转逻辑。若当前字符是空格且后继字符不是，则视为单词开始。若字符串以非空格开头，额外计数一个单词（完整代码见 `Snippet19.cpp`）：

```cpp
int wordCount(const std::string &s)
{
    auto isWordStart = [&](char a, char b)
    {
        return a < 33 & b > 32;
    };
    // Count the first word if any
    int count = s[0] > 32;
    // count the remaining
    return std::inner_product(s.begin(), s.end()-1, s.begin()+1, count, std::plus<int>(), isWordStart);
}
```

## 19. 方案二：单独统计最后一个单词

代码与 `Snippet18.cpp` 版本几乎相同，只是额外检查最后一个单词（完整代码见 `Snippet20.cpp`）：

```cpp
int count = std::inner_product(s.begin(), s.end()-1, s.begin()+1, 0, std::plus<int>(), isWordEnd);
// count the last word if any
if(s.back() > 32)
{
    ++count;
}
return count;
```

## 20. 方案三：使用 `c_str()`

只需修改 `inner_product()` 的参数（完整代码见 `Snippet21.cpp`）：

```cpp
int wordCount(const std::string &s)
{
    auto isWordEnd = [&](char a, char b)
    {
        return a > 32 & b < 33;
    };
    const char *p = s.c_str();
    return std::inner_product(p, p + s.size(), p+1, 0, std::plus<int>(), isWordEnd);
}
```

由于 `c_str()` 末尾有 `NUL`，其行为与之前一致。

## 21. 编译并计时所有三个版本

