
## 求和函数

```cpp
Sum sumAOS(vector<Data1> &aos)
{
    TIME_IT;
    Sum ret;
    for(int i = 0; i < N; ++i)
    {
        ret.field1 += aos[i].field1;
        ret.field2 += aos[i].field2;
        ret.field3 += aos[i].field3;
    }
    return ret;
}

Sum sumSOA(Data2 &soa)
{
    TIME_IT;
    Sum ret;
    for(int i = 0; i < N; ++i)
    {
        ret.field1 += soa.field1[i];
        ret.field2 += soa.field2[i];
        ret.field3 += soa.field3[i];
    }
    return ret;
}
```

## 主函数实现

5. 在 `main` 函数内编写以下代码：

```cpp
int main()
{
    vector<Data1> arrOfStruct;
    Data2 structOfArr;
    // 预留空间
    structOfArr.field1.reserve(N);
    structOfArr.field2.reserve(N);
    structOfArr.field3.reserve(N);
    arrOfStruct.reserve(N);
    // 填充随机值
    for(int i = 0; i < N; ++i)
    {
        Data1 temp;
        temp.field1 = dist(rng);
        temp.field2 = dist(rng);
        temp.field3 = dist(rng);
        arrOfStruct.push_back(temp);
        structOfArr.field1.push_back(temp.field1);
        structOfArr.field2.push_back(temp.field2);
        structOfArr.field3.push_back(temp.field3);
    }
    Sum s1 = sumAOS(arrOfStruct);
    Sum s2 = sumSOA(structOfArr);
    Timer::dump();
}
```

## 运行程序

6. 保存程序并打开终端。通过添加以下命令运行程序以计时：

```bash
$ g++ -O3 Snippet12.cpp Timer.cpp
$ ./a.out
```

前面的代码生成以下输出：

![图 8.40：Snippet12.cpp 中代码的输出，对比 AOS 和 SOA 模式的计时](image/C14583_08_40.jpg)

**图 8.40：Snippet12.cpp 中代码的输出，对比 AOS 和 SOA 模式的计时**

结构体数组（SoA）方法比数组结构体（AoS）方法快两倍。考虑到结构体中各向量的地址相距甚远，我们可能会好奇为什么 SoA 情况下的缓存表现更好。原因在于缓存的设计方式——缓存不是作为一个单一的整体块来处理，而是被划分为多个缓存行，正如我们之前讨论的那样。当访问内存地址时，32 位或 64 位地址会被转换成几位的“标签”，并使用与该标签关联的缓存行。非常接近的内存地址将获得相同的标签并到达同一个缓存行。如果访问差异很大的地址，它将到达不同的缓存行。这种基于行的缓存设计对我们的测试程序的影响是，就好像我们为每个向量都有独立的缓存一样。

前面关于缓存行的解释非常简化，但缓存行的基本概念适用。这种结构体数组模式的代码可读性可能看起来稍差，但考虑到性能的提升，这是非常值得的。随着结构体尺寸的增大，这种特定的优化会变得更加有效。此外，请记住，如果字段大小各异，填充结构体可能会使其尺寸膨胀很大倍数。我们已经探讨了内存延迟的性能影响，并学习了一些帮助处理器缓存高效工作的方法。在编写性能关键型程序时，我们应牢记缓存效应。有时，从一开始就采用更友好的缓存架构是合理的。一如既往，在尝试对数据结构进行激进更改之前，我们应始终先测量代码性能。优化应集中在程序中最耗时的区域，而不是每个部分。

## 算法优化

算法优化最简单的形式是寻找能完成你任务的库——最流行的库都经过高度优化且编写精良。例如，`Boost` 库提供了许多实用的库，在许多项目中派上用场，如 `Boost.Geometry`、`Boost.Graph`、`Boost.Interval` 和 `Boost.Multiprecision`，仅举几例。使用专业编写的库比自己尝试创建要容易且明智得多。例如，`Boost.Graph` 实现了十几种处理拓扑图的算法，每一个都经过高度优化。

许多计算可以归结为一系列组合在一起的标准算法——如果做得正确，这些可以产生极其高效的代码——并且通常甚至可以由编译器并行化，以利用多核或 SIMD。在本节的其余部分，我们将采用一个单一程序，并尝试以各种方式对其进行优化——这将是一个具有以下规格的单词计数程序：

*   为隔离磁盘 I/O 所花费的时间，我们将在处理前读取整个文件到内存。
*   将忽略 Unicode 支持，并假设为 ASCII 编码的英文文本。
*   我们将使用网上可用的大型公共领域文学文本作为测试数据。

## 练习 14：优化单词计数程序

在这个漫长的练习中，我们将使用各种优化技术来优化一个程序。我们将对实际程序进行增量优化。我们将使用的测试数据由名为《双城记》的书组成，该书被追加了 512 次。

> **注意**
> 本练习使用的数据集可在此处获得：<https://github.com/TrainingByPackt/Advanced-CPlusPlus/blob/master/Lesson8/Exercise14/data.7z>。你需要解压这个 7zip 压缩包，并将生成的名为 `data.txt` 的文件复制到你进行此练习的文件夹中。

执行以下步骤以完成此练习：

### 步骤 1：样板代码

1. 编写读取文件的基本样板代码（完整代码可在 `SnippetWC.cpp` 中找到）。这将作为我们编写的所有单词计数版本的通用驱动程序：

```cpp
int wordCount(const string &s);

int main(int argc, char **argv)
{
    if(argc > 1)
    {
        TIME_IT;
        string sContent;
        ostringstream buf;
        ifstream ifs(argv[1]);
        {
            Timer t("Read file");
            buf << ifs.rdbuf();
            sContent = buf.str();
            sContent.push_back(' ');
        }
        cerr << wordCount(sContent) << endl;
    }
    Timer::dump();
}
```

我们将使用一个虚拟代码块来分离读取文件的代码计时，并对 `main()` 本身计时以获得总执行时间。

注意 `push_back` 在末尾添加了一个空格——这确保数据以空白字符结尾，简化了我们使用的算法。

### 步骤 2：基本单词计数函数

2. 编写一个基本的单词计数函数。逻辑非常简单——对于字符串中的每个字符，如果该字符不是空白字符而下一个字符是，则它是单词的结尾，应被计数。由于我们的样板代码在末尾添加了一个空格，任何最后的单词都会被计数。该函数定义在 `Snippet13.cpp` 中：

```cpp
int wordCount(const std::string &s)
{
    int count = 0;
    for(int i = 0, j = 1; i < s.size() - 1; ++i, ++j)
    {
        if(!isspace(s[i]) && isspace(s[j]))
        {
            ++count;
        }
    }
    return count;
}
```

### 步骤 3：编译和运行

3. 让我们编译、运行，并了解一下性能。我们将通过将我们代码的结果与标准 `wc` 程序提供的结果进行比较，来验证其是否正常工作：

```bash
$ g++ -O3 Snippet13.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

我们收到以下输出：

![图 8.41：Snippet13.cpp 中代码的输出，带有基准单词计数实现](image/C14583_08_41.jpg)

**图 8.41：Snippet13.cpp 中代码的输出，带有基准单词计数实现**

让我们给 `wc` 程序计时：
