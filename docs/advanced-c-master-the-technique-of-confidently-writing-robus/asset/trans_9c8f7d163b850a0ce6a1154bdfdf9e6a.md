
在过去，代码优化涉及尝试最小化代码中的机器指令数量，使用更高效的指令，甚至重新排序指令以使流水线保持满载。到了今天，编译器能够将上述所有优化执行到大多数程序员难以企及的水平——特别是考虑到编译器可以跨越数亿条指令的整个程序进行优化。即使在现在，优化数据访问模式以利用缓存，依然坚定地是程序员的责任。

这项任务非常简单——确保内存访问靠近之前访问过的内存——但实现这一目标的方法可能需要付出大量努力。

## 注意

著名游戏程序员兼代码优化大师 Terje Mathisen 据称在 90 年代说过：“所有编程本质上都是在练习缓存。” 时至 2019 年，这句话在试图编写高性能代码这一子领域中比以往任何时候都更适用。

## 缓存友好性法则

有一些增加缓存友好性的基本经验法则：

* 栈始终是 `hot`（热点）的，因此我们应尽可能使用局部变量。
* 动态分配的对象彼此之间很少具有数据局部性——应避免使用它们，或使用预分配的对象池，使它们在内存中顺序排列。
* 基于指针的数据结构（如树——尤其是链表）由堆上分配的多个节点组成，非常不利于缓存。
* 面向对象代码中虚函数的运行时分发会使指令缓存失效——在性能关键代码中应避免动态分发。

在下一节中，我们将探讨堆分配的成本。

## 练习 12：探究堆分配的成本

在本练习中，我们将检查动态分配内存的性能影响，并考察堆内存如何影响代码性能。执行以下步骤完成此练习：

1. 创建一个名为 `Snippet11.cpp` 的文件。

2. 添加以下代码以包含必要的库：

```
#include <string>
#include <iostream>
#include <random>
#include "Timer.h"
using std::string;
using std::cerr;
using std::endl;
```

3. 声明一个常量变量 `N` 和一个名为 `fruits` 的字符数组。为它们赋值：

```
const int N = 10'000'000;
const char* fruits[] = {"apple", "banana", "cherry", "durian", "guava", "jackfruit", "kumquat", "mango", "orange", "pear"};
```

4. 创建一个名为 `fun1()` 的函数，它仅遍历 `fruits` 中的每个字符串，将其复制到 `string` 中，并对该字符串的字符求和：

```
uint64_t fun1()
{
  TIME_IT;
  uint64_t sum = 0;
  string s1;
  for(uint64_t i = 0; i < N; ++i)
  {
    s1 = fruits[i % 10];
    for(int k = 0; k < s1.size(); ++k) sum += s1[k];
  }
  return sum;
}
```

5. 创建另一个名为 `fun2()` 的函数，它使用局部声明的字符数组代替 `string`，并用循环进行复制：

```
uint64_t fun2()
{
  TIME_IT;
  uint64_t sum = 0;
  char s1[32];
  for(uint64_t i = 0; i < N; ++i)
  {
    char *ps1 = s1;
    const char *p1 = fruits[i % 10];
    do { *ps1++ = *p1; } while(*p1++);
    for(ps1 = s1; *ps1; ++ps1) sum += *ps1;
  }
  return sum;
}
```

6. 在 `main()` 函数中编写以下代码：

```
int main()
{
  for(int i = 0; i < 10; ++i)
  {
    volatile uint64_t asum1 = fun1();
    volatile uint64_t asum2 = fun2();  
  }
  Timer::dump();
}
```

7. 保存文件并打开终端。编译并运行程序：

```
$ g++ -O3 Snippet11.cpp Timer.cpp
$ ./a.out
```

前述命令生成以下输出：

![Figure 8.37: Output of the code in Snippet11.cpp showing the effect of heap allocation on the timing](image/C14583_08_37.jpg)

**图 8.37：Snippet11.cpp 代码的输出，展示了堆分配对计时的影响**

从前述输出中注意到，`fun2()` 的速度几乎是 `fun1()` 的两倍。

8. 现在，使用 `perf` 命令进行性能分析：

```
$ perf record ./a.out
```

前述命令生成以下输出：

![Figure 8.38: Output of the perf command profiling the code in Snippet11.cpp](image/C14583_08_38.jpg)

**图 8.38：perf 命令对 Snippet11.cpp 代码进行性能分析的输出**

9. 现在，我们可以使用以下代码查看性能报告：

```
$ perf report
```

我们收到以下输出：

![Figure 8.39: Output of the perf command's timing report for the code in Snippet11.cpp](image/C14583_08_39.jpg)

**图 8.39：perf 命令对 Snippet11.cpp 代码的计时报告输出**

在前述输出中注意到，约 33% 的执行时间被 `std::string` 构造函数、`strlen()` 和 `memmove()` 占用。这些都与 `fun1()` 中使用的 `std::string` 有关。特别是堆分配是最慢的操作。

## 结构体数组模式

在许多程序中，我们经常使用同类型对象的数组——这些可能代表数据库记录、游戏实体等。一种常见模式是遍历一个大型结构体数组并对某些字段执行操作。即使结构体在内存中是连续的，如果我们只访问其中几个字段，较大的结构体尺寸会使缓存效果降低。

处理器可能会将多个结构体预取到缓存中，但程序只访问了这些缓存数据的一小部分。由于没有使用每个结构体的每个字段，大部分缓存数据被丢弃。为了避免这种情况，可以使用另一种数据布局——不使用 `结构体数组` (`AoS`) 模式，而是使用 `数组结构体` (`SoA`) 模式。在下一节中，我们将通过一个练习来考察使用 `SoA` 模式相对于 `AoS` 模式的性能优势。

## 练习 13：使用数组结构体模式

在本练习中，我们将考察使用 `SoA` 与 `AoS` 模式的性能优势。执行以下步骤完成此练习：

1. 创建一个名为 `Snippet12.cpp` 的文件。

2. 包含必要的库，以及 `Timer.h` 头文件。初始化随机数生成器，并创建一个从 1 到 N-1 的分布范围。创建一个常量整数变量 `N`，并将其初始化为 100,000,000。添加以下代码以实现此功能：

```
#include <vector>
#include <list>
#include <algorithm>
#include <string>
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

3. 以两种不同方式表示数据——一个 `结构体数组` 和一个 `数组结构体`。使用六个 `uint64_t` 字段，以便模拟大尺寸结构体，这更能代表真实世界的程序：

```
struct Data1
{
  uint64_t field1;
  uint64_t field2;
  uint64_t field3;
  uint64_t field4;
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

struct Sum
{
  uint64_t field1;
  uint64_t field2;
  uint64_t field3;
  Sum(): field1(), field2(), field3() {}
};
```

4. 定义两个函数，分别名为 `sumAOS` 和 `sumSOA`，它们对前述两种数据结构中的 `field1`、`field2` 和 `field3` 的值求和。编写以下代码以实现此功能：

