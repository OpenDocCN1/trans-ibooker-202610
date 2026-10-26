

从前面的输出可以看出，处理器对分支预测能力更好，使得 sum2 函数大约有 2% 的小幅但显著的加速。在下一节中，我们将探索更多优化技术。

## 进一步优化

在编码时可以实现多种其他技术；其中有些并不能保证产生更好的代码，但只需付出很少的努力就能改变编码习惯，使其成为本能。它们几乎不花费成本，但可能带来收益。其中一些技术如下：

- 在可能的情况下，将非原始类型的参数按 const 引用传递。即使移动构造函数可以使复制变得廉价，但它们仍然比使用 const 引用涉及更多的开销。
- 使用前置递增 (++i) 或前置递减 (--i) 运算符，而不是后缀版本。对于简单的类型如整数，这通常没有用处，但对于具有自定义递增运算符的复杂类型可能有用。养成写 ++i 而不是 i++ 的习惯是良好的做法，除非后缀递增确实是期望的行为。除了性能优势外，这种代码通过使用正确的运算符更清晰地声明了意图。
- 尽可能晚地声明变量——在 C 中在函数顶部声明所有变量很常见，但在 C++ 中，由于变量可能具有非平凡的构造函数，因此最好只在实际使用它们的块中声明它们。
- 就循环提升而言，如果循环中存在不随循环迭代而改变的代码或计算，则最好将其移出循环之外。这包括在循环体中创建对象。通常，更有效的做法是在循环外部一次性声明它们。现代编译器会自动执行此操作，但自己动手也不需要额外精力。
- 尽可能使用 const。它不会改变代码的含义，但它使编译器能够对代码做出更强的假设，从而可能带来更好的优化。除此之外，使用 const 使代码更具可读性和合理性。
- 整数除法、取模和乘法（尤其是乘以非 2 的幂的数）在 X86 硬件上是一些最慢的操作之一。如果需要在循环中执行此类操作，也许可以通过一些代数运算来消除它们。

- 3. 创建一个常量整数变量 N，并将其值设为 100000：

    ```
    const int N = 100000;
    ```

- 4. 初始化一个随机数生成器并创建一个从 0 到 N 的分布范围。添加以下代码来实现此功能：

    ```
    std::random_device dev;
    std::mt19937 rng(dev());
    std::uniform_int_distribution<std::mt19937::result_type> dist(0,N);
    ```

- 5. 创建一个名为 insertRandom() 的方法，并以随机位置将元素从 0 到 N 插入到容器中。添加以下代码来实现此功能：

    ```
    template<class C> void insertRandom(C &l)
    {
        // insert one element to initialize
        l.insert(l.end(), 0);
        for(int i = 0; i < N; ++i)
        {
            int pos = dist(rng) % l.size();
            auto it = l.begin();
            advance(it, pos);
            l.insert(it, i);
        }
    }
    ```

- 6. 创建一个名为 insertStart() 的方法，并将元素从 0 到 N 插入到容器的开头。添加以下代码来实现此功能：

    ```
    template<class C> void insertStart(C &l)
    {
        for(int i = 0; i < N; ++i)
        {
            l.insert(l.begin(), i);
        }
    }
    ```

- 7. 创建一个名为 **insertEnd()** 的方法，并将元素从 **0** 到 **N** 插入到容器的末尾。添加以下代码来实现此功能：

    ```
    template<class C> void insertEnd(C &l)
    {
        for(int i = 0; i < N; ++i)
        {
            l.insert(l.end(), i);
        }
    }
    ```

- 8. 在 **main** 方法中编写以下代码：

    ```
    int main()
    {
        std::list<int> l;
        std::vector<int> v;
        // list
        {
            Timer t("list random");
            insertRandom(l);
        }

        {
            Timer t("list end");
            insertEnd(l);
        }
        {
            Timer t("list start");
            insertStart(l);
        }
        // vector
        {
            Timer t("vect random");
            insertRandom(v);
        }

        {
            Timer t("vect end");
            insertEnd(v);
        }
        {
            Timer t("vect start");
    ```

历史上，处理器在 1980 年到 2010 年间速度提高了约 16,000 倍。与此同时，RAM 速度的提升要小一个数量级——不到 100 倍。因此，指令对 RAM 的单次访问可能导致 CPU 等待大量时钟周期。这将是不可接受的性能下降，因此出现了许多技术来缓解此问题。在我们探索这个问题之前，让我们在下一个练习中测量内存访问的影响。

## 练习 11：测量内存访问的影响

在本练习中，我们将检查随机访问内存的性能影响。执行以下步骤来完成此练习：

- 1. 创建一个名为 Snippet9.cpp 的新文件。

- 2. 包含必要的库以及 Timer.h 头文件。创建两个常量整数变量 SIZE 和 N，并将其值设为 100000000。另创建一个随机数生成器和一个从 0 到 N-1 的分布范围。编写以下代码来实现此功能：

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

    const int SIZE = 100'000'000;
    const int N = 100'000'000;

    std::random_device dev;
    std::mt19937 rng(dev());

    std::uniform_int_distribution<std::mt19937::result_type> dist(0,SIZE-1);
    ```

- 3. 创建 getPRIIndex() 函数，该函数返回介于 0 和 SIZE-1 之间的伪随机索引，其中 SIZE 是数组中的元素数。编写以下代码来实现此功能：

    > Note
    > 我们稍后会讨论为什么使用随机数。

    ```
    uint64_t getPRIIndex(uint64_t i)
    {
        return (15485863 * i) % SIZE;
    }
    ```

- 4. 编写一个名为 sum1() 的函数，该函数随机访问大型数据数组并对这些元素求和：

    ```
    uint64_t sum1(vector<int> &v)
    {
        TIME_IT;
        uint64_t sum = 0;
        for(int i = 0; i < N; ++i)
        {
            sum += v[getPRIIndex(i)];
        }
        return sum;
    }
    ```

- 5. 编写一个名为 sum2() 的函数，该函数对随机数求和而没有任何内存访问：

    ```
    uint64_t sum2()
    {
        TIME_IT;
        uint64_t sum = 0;
        for(int i = 0; i < N; ++i)
        {
            sum += getPRIIndex(i);
        }
        return sum;
    }
    ```

## 缓存

现代处理器在处理器寄存器和 RAM 之间具有多层缓存内存。这些缓存被标记为 L1、L2、L3、L4 等，其中 L1 最接近处理器，L4 最远。每层缓存都比其下层更快（通常也更小）。以下是 Haswell 系列处理器缓存/内存大小和延迟的示例：

| Cache/Memory | Size | Latency |
| --- | --- | --- |
| L1 | 32 KB | 4 cycles |
| L2 | 256 KB | 12 cycles |
| L3 | 6 MB | 20 cycles |
| L4 | 128 MB | 58 cycles |
| RAM | many GB | 115 cycles |

缓存工作以帮助性能的简单模型如下：当访问内存地址时，会在 L1 缓存中查找——如果找到，则从那里检索。如果没有找到，则在 L2 缓存中查找，如果仍未找到，则查找 L3 缓存，依此类推——如果在任何缓存中都未找到，则从内存中获取。当从内存中获取时，它会被存储在每一层缓存中以便以后更快访问。这种方法本身几乎没用，因为它只有在我们一遍又一遍地访问同一个内存地址时才会提高性能。第二方面，称为预取，是使缓存真正发挥作用的机制。

### 预取

预取是一个过程，即在执行内存访问时，附近的数据也会被提取到缓存中，即使它没有被直接访问。预取的第一方面与内存总线粒度有关——可以认为“RAM 子系统可以向处理器发送的最小数据量是多少？”。在大多数现代处理器中，这是 64 位——换句话说，无论你从内存中请求单个字节还是 64 位值，包含该地址的整个 64 位机器字都会从 RAM 中读取。这些数据存储在每一层缓存中以便以后更快访问。显然，这会立即改善内存性能——假设我们读取地址 0x1000 处的内存字节；我们还将该地址之后的其他 7 个字节带入缓存。如果我们随后访问地址 0x1001 处的字节，它将来自缓存，避免了昂贵的 RAM 访问。

