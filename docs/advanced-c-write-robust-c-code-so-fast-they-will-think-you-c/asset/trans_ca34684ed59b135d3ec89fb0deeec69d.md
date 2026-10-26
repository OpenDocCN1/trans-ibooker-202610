

我们收到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_616_0.png)

图 8.39：Snippet11.cpp 中代码的 perf 命令时序报告输出

在前面的输出中，注意到约 33% 的执行时间被 std::string 构造函数、strlen() 和 memmove() 占用。所有这些都与 fun1() 中使用的 std::string 相关。尤其是堆分配是最慢的操作。

## Struct of Arrays Pattern

在许多程序中，我们经常使用相同类型的对象数组——这些可能代表数据库中的记录、游戏中的实体等等。一种常见模式是遍历一个大型结构体数组并对某些字段执行操作。即使结构体在内存中是顺序的，如果我们只访问少数几个字段，更大的结构体大小会使缓存效果变差。

处理器可能会预取多个结构体到缓存中，但程序只访问了该缓存数据的一部分。由于它没有使用每个结构体的每个字段，大部分缓存数据都被丢弃。为了避免这种情况，可以使用另一种数据布局——而不是使用数组的数组 AoS 模式，我们使用结构体数组 SoA 模式。在下一节中，我们将解决一个练习，其中我们将检查使用 SoA 模式与 AoS 模式的性能优势。

## Exercise 13: Using the Struct of Arrays Pattern

在本练习中，我们将检查使用 SoA 与 AoS 模式的性能优势。执行以下步骤来完成此练习：

- 1. 创建一个名为 Snippet12.cpp 的文件。
- 2. 包含必要的库以及 Timer.h 头文件。初始化一个随机数生成器并创建一个从 1 到 N-1 的分布范围。创建一个常量整数变量 N 并将其初始化为 100,000,000。添加以下代码来实现此功能：

```
#include <vector>
#include <list>
#include <algorithm>
#include <string>
```

```
#include <iostream>
#include <random>
#include "Timer.h"

using std::vector;
using std::list;
using std::cerr;
using std::endl;

const int N = 100'000'000;

std::random_device dev;
std::mt19937 rng(dev());

std::uniform_int_distribution<std::mt19937::result_type> dist(1,N-1);
```

3. 编写两种不同的数据表示方式——结构体数组和结构体数组。使用六个 uint64_t 字段，以便我们可以模拟一个更大的结构，这更能代表真实世界的程序：

```
struct Data1
{
    uint64_t field1;
    uint64_t field2;
    uint64_t field3;
    uint64_t field4;
    uint64_t field5;
    uint64_t field6;
};
struct Data2
{
    vector<uint64_t> field1;
    vector<uint64_t> field2;
    vector<uint64_t> field3;
    vector<uint64_t> field4;
    vector<uint64_t> field5;
    vector<uint64_t> field6;
};
```

- 5. 在 `main` 函数内编写以下代码：

```
```cpp
int main()
{
    vector<Data1> arrOfStruct;
    Data2 structOfArr;

    // Reserve space
    structOfArr.field1.reserve(N);
    structOfArr.field2.reserve(N);
    structOfArr.field3.reserve(N);
    arrOfStruct.reserve(N);
    // Fill random values
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
```

- 6. 保存程序并打开终端。通过添加以下命令来计时程序：

```
```sh
$ g++ -O3 Snippet12.cpp Timer.cpp
$ ./a.out
```
```

前面的代码生成以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_619_0.png)

图 8.40：Snippet12.cpp 中代码的输出，对比 AOS 和 SOA 模式的时序

```
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

我们将使用一个虚拟块来分隔读取文件的代码的计时，并同时计时 main() 本身以获得整体执行时间。

请注意 push_back 添加了一个空格在末尾——这确保数据以空白字符结尾，简化了我们使用的算法。

- 2. 编写一个基本的词计数函数。逻辑非常简单——对于字符串中的每个字符，如果该字符不是空白且下一个是，则它是单词的结尾，应计数。由于我们的样板代码已添加了空格，任何最终单词都将被计数。此函数定义在 Snippet13.cpp 中：

```
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

- 3. 让我们编译、运行并了解性能。我们将通过将我们的代码结果与标准 wc 程序的结果进行比较来验证其是否正确：

```
$ g++ -O3 Snippet13.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

我们收到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_623_0.png)

图 8.41：Snippet13.cpp 中代码的输出，具有基线词计数实现

让我们计时 wc 程序：

```
$ time wc -w data.txt
```

我们收到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_623_1.png)

图 8.42：计时 wc 程序的输出

wc 程序显示相同的词数，即 71108096，因此我们知道我们的代码是正确的。我们的代码花费了约 3.6 秒，包括读取文件，这比 wc 慢得多。

- 4. 我们优化的第一个策略是看看是否可以用更好的方式实现 isspace()。不用函数，我们可以使用一个查找表来判断一个字符是否为空白（此代码可在 Snippet14.cpp 中找到）：

```
int wordCount(const std::string &s)
{
    // Create a lookup table
    bool isSpace[256];
    for(int i = 0; i < 256; ++i)
    {
        isSpace[i] = isspace((unsigned char)i);
    }
    int count = 0;
    int len = s.size() - 1;
    for(int i = 0, j = 1; i < len; ++i, ++j)
    {
        count += !isSpace[s[i]] & isSpace[s[j]];
    }
    return count;
}
```

```
int idx = j + i * 256;
table[idx] = !isspace(j) && isspace(i);
}
```

用于计数单词的循环变为如下所示（完整代码可在 Snippet15.cpp 中找到）：

```
int count = 0;
for(int i = 0; i < s.size() - 1; ++i)
{
    // grab the 2 bytes at s[i] as a 16 bit value
    unsigned short idx;
    memcpy(&idx, &s[i], 2);
    count += table[idx];
}
```

我们将字符串的每个字符作为单个 16 位值读取。直接将指针从 char* 强制转换为另一种类型并解引用它是未定义行为——正确的方法是使用 memcpy()。编译器足够智能，可以使用 CPU 内存访问指令而不是实际调用 memcpy() 处理 2 字节。我们最终得到一个循环中不包含条件语句，这应该使其更快。记住 X86 架构是小端——因此从字符数组读取的 16 位值将第一个字符作为其 LSB，第二个作为 MSB。

- 7. 现在，计时我们编写的代码：

```
$ g++ -O3 Snippet15.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

![](img/6507d435e2edfa3c00c220d0ccfcaf94_625_0.png)

图 8.44：Snippet15.cpp 中代码的输出

这个更大的查找表使 wordCount() 的速度提高了 1.8 倍。让我们退一步从另一个角度来看，以便我们可以有效地使用现有的标准库。其优点是两方面的——首先，代码出错的可能性更小，其次，我们可以利用某些编译器提供的并行化。

