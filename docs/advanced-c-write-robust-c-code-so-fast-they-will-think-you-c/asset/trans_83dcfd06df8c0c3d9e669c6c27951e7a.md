

## 追求速度——性能与优化

让我们使用标准算法重写使用 **isspace** 查找表的程序版本。如果观察统计单词数量的主循环，我们会取出两个字符，并根据某些逻辑将 1 或 0 累加到 **count** 变量中。这是许多代码中常见的模式：

```
X OP (a[0] OP2 b[0]) OP (a[1] OP2 b[1]) OP (a[2] OP2 b[2]) ... OP (a[N] OP2 b[N])
```

这里，**a** 和 **b** 是大小为 **N** 的数组，**X** 是初始值，**OP** 和 **OP2** 是运算符。有一个封装了这种模式的标准算法，名为 **std::inner_product**——它接收两个序列，对每对元素之间应用一个运算符（OP2），并在这些结果之上应用另一个运算符（OP），起始值为初始值 X。

- 8. 我们可以按如下方式编写该函数（完整代码可在 **Snippet16.cpp** 中找到）：

```cpp
int wordCount(const std::string &s)
{
    // Create a lookup table for every char
    bool table[256];
    for(int i = 0; i < 256; ++i)
    {
        table[i] = isspace((unsigned char)i) ? 1 : 0;
    }

    auto isWordEnd = [&](char a, char b)
    {
        return !table[a] & table[b];
    };

    return std::inner_product(s.begin(), s.end()-1, s.begin()+1, 0,
    std::plus<int>(), isWordEnd);
}
```

这个 **inner_product()** 调用会对每对 **s[n]** 和 **s[n+1]** 应用 **isWordEnd()** lambda，并在这些结果之间应用标准加法函数。实际上，当 **s[n]** 和 **s[n+1]** 构成单词结尾时，我们就会向总数加 1。

> **注意**
>
> 尽管这看起来像是许多嵌套函数调用，但编译器会内联所有内容，不会产生任何开销。

- 11. 编译并计时运行代码：

```bash
$ g++ -O3 Snippet17.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

我们得到以下输出：

| 名称 | 次数 | 时间(ms) | 平均(ms) |
|---|---:|---:|---:|
| 文件读取 | 1 | 1016 | 1016 |
| 单词统计 | 1 | 251 | 251 |
| 总时间 | 1 | 1308 | 1308 |

图 8.46：Snippet17.cpp 中代码的输出

这段代码不如我们在 **Snippet15.cpp** 中基于 loop() 的版本那么快。原因在于，在循环版本中，我们将 2 个字节合并为一个 **short** 来获取索引，这不需要任何计算；但在这里，我们通过位运算将 2 个字节读入一个 **short**。

- 12. 现在我们的代码中大部分工作由标准库函数完成，我们可以免费获得自动并行化——编译并测试如下：

```bash
$ g++ -O3 -fopenmp -D_GLIBCXX_PARALLEL Snippet17.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

我们得到以下输出：

| 名称 | 次数 | 时间(ms) | 平均(ms) |
|---|---:|---:|---:|
| 文件读取 | 1 | 1032 | 1032 |
| 单词统计 | 1 | 89 | 89 |
| 总时间 | 1 | 1166 | 1166 |

图 8.47：Snippet17.cpp 中代码在并行化标准库下的输出

显然，它无法完全并行化，因此我们在速度上只获得了大约 2.5 倍的提升，而且无需对代码做任何修改。我们能否以同样的方式让基于循环的代码并行化？理论上可以——我们可以手动使用 **OpenMP** 指令来实现；然而，这需要修改代码并了解如何使用 OpenMP。那么 **Snippet16.cpp** 中的版本呢？

```bash
$ g++ -O3 -fopenmp -D_GLIBCXX_PARALLEL Snippet16.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

我们得到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_629_0.png)

图 8.48：Snippet16.cpp 中代码在并行化标准库下的输出

这个版本也能看到类似的改进。我们完成了吗？还是可以更快？著名游戏程序员 Michael Abrash 提出了缩写词 TANSTAAFC——它代表"There ain't no such thing as the fastest code"（不存在最快的代码）。他的意思是，只要付出足够的努力，代码总是有可能变得更快。这似乎不可能，但人们一次又一次地找到了越来越快的计算方法——我们的代码也不例外，我们还可以更进一步。我们可以为优化做出的一种权衡是降低代码的通用性——我们已经对代码施加了一些约束——例如，我们只处理 ASCII 英文文本。通过对输入数据添加更多约束，我们可以做得更好。假设文件中没有不可打印字符。对于我们的输入数据来说，这是一个合理的假设。如果我们这样假设，那么就可以简化检测空白的条件——由于所有空白字符都大于或等于 ASCII 32，我们可以完全省去查找表。

- 13. 让我们基于之前的想法实现代码（完整代码可在 Snippet18.cpp 中找到）：

```cpp
int wordCount(const std::string &s)
{
    auto isWordEnd = [&](char a, char b)
    {
        return a > 32 & b < 33;
    };
    return std::inner_product(s.begin(), s.end()-1, s.begin()+1, 0,
    std::plus<int>(), isWordEnd);
}
```

- 14. 编译并运行程序：

```bash
$ g++ -O3 Snippet18.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

```cpp
int wc;
{
    Timer t("Word count");
    wc = wordCount(sContent);
}
```

- 16. 编译并运行上述代码：

```bash
$ g++ -O3 Snippet18.cpp SnippetWC2.cpp Timer.cpp
$ ./a.out data.txt
```

我们得到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_631_0.png)

图 8.51：Snippet18.cpp 中代码对所有操作计时后的输出

大部分时间都花在了 push_back() 和复制字符串上。由于字符串的大小恰好等于文件的大小，push_back() 最终会为字符串分配一个新的缓冲区并复制内容。我们如何消除这个 push_back() 调用？我们在末尾附加了一个空格，以便能够一致地统计最后一个单词（如果有的话），因为我们的算法统计的是单词的结尾。有三种方法可以避免这种情况：统计单词的开头而不是结尾；单独统计最后一个单词（如果有的话）；以及使用 c_str() 函数，这样末尾就会有一个 NUL 字符。现在让我们依次尝试每一种方法。

- 17. 首先，编写不使用 push_back 的 main 函数（完整代码可在 SnippetWC3.cpp 中找到）：

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

- 18. 修改 wordCount() 中的代码，将 **isWordEnd()** 重命名为 **isWordStart()** 并反转逻辑。如果当前字符是空格而下一个字符不是，则认为一个单词开始。此外，如果字符串以非空格字符开头，则额外计数一个单词（完整代码可在 **Snippet19.cpp** 中找到）：

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
    return std::inner_product(s.begin(), s.end()-1, s.begin()+1, count,
        std::plus<int>(), isWordStart);
}
```

- 19. 现在编写第二种替代方案——统计最后一个单词（如果有的话）。除了检查最后一个单词之外，代码与 **Snippet18.cpp** 版本几乎相同（完整代码可在 **Snippet20.cpp** 中找到）：

```cpp
int count = std::inner_product(s.begin(), s.end()-1, s.begin()+1, 0,
    std::plus<int>(), isWordEnd);

// count the last word if any
if(s.back() > 32)
{
    ++count;
}

return count;
```

- 20. 编写使用 **c_str()** 的第三个版本——我们只需要更改 **inner_product()** 的参数（完整代码可在 **Snippet21.cpp** 中找到）：

```cpp
int wordCount(const std::string &s)
{
    auto isWordEnd = [&](char a, char b)
    {
        return a > 32 & b < 33;
    };
    const char *p = s.c_str();
```

