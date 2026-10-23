
- **L3HARRIS Geospatial Solutions** 提供了 Big Integer 类 [2]，这是另一个计算速度很快的库。⁹
- **Boost 库**¹⁰ 是另一个强大的库，用于完成基于线性代数、伪随机数生成、多线程、图像处理、正则表达式和单元测试的任务。该库拥有一组令人印象深刻、相互独立的库（约 160 个），并且文档结构清晰，非常易于学习和使用 [3]。
- **GMP 库**（GNU 多重精度算术库）是另一个可用于任意精度算术的免费库。¹¹ 它支持基于有符号整数、有理数和浮点数的运算（更多细节请参见第6章）。该库唯一的限制在于可用内存。在 32 位系统上，上限为 2³² − 1 位；在 64 位系统上，上限为 2³⁷ − 1 位。其主要接口面向 C/C++，但也支持 C#、.NET 和 OCaml。（它也可以轻松移植到 Haskell。更多细节请参阅 [4]、[5] 和 [11]。）此外，它还提供对 Ruby、PHP、Python、R、Perl 和 Wolfram 语言的重要支持。该库的主要目标和用途是密码学软件应用、互联网安全和代数系统。
- **LibBF 库 [8]** 用于处理以 2 为基数表示的浮点数。该库基于 IEEE 754 标准 [7] 实现。库网页上提供的示例 TinyPI 是展示其强大功能的绝佳示例。该库将在第6章进一步讨论。
- **Bignum C++ 库 [9]**（TTMath）允许个人和商业用户执行算术运算。支持的整数类型包括大无符号整数、大有符号整数和大浮点数。支持加、减、除、乘等数学运算。参见清单4-7。

⁹L3HARRIS Geospatial Solutions, www.harrisgeospatial.com
¹⁰Boost Library, www.boost.org
¹¹GMP Library, http://gmplib.org

## 第4章 大整数算术

### 清单4-7 使用 tmath::UInt<>

```cpp
#include <ttmath/ttmath.h>
#include <iostream>

int main()
{
    ttmath::UInt<2> firstA, secondB, thirdC;

    a = "8765";
    b = 3456;
    c = a*b;

    std::cout << thirdC << std::endl;
}
```

## 结论

本章讨论了大整数的通用表示及其运算。我们分析了最重要的大整数库，重点介绍了这些库在您搭建密码算法开发环境时的优势。

## 参考文献

+ 1. C++ 大整数库。可在线访问：https://mattmccutchen.net/bigint/。最后访问日期：2020年5月16日。
+ 2. BigInteger 类 L3HARRIS Geospatial。可在线访问：www.harrisgeospatial.com/docs/BIGINTEGER.html。
+ 3. Boost 库文档。可在线访问：www.boost.org/doc/libs/1_72_0/。
+ 4. S.L. Nita 和 M. Mihailescu，*《实用并发 Haskell：大数据应用》*。Apress。2017年。
+ 5. S.L. Nita 和 M. Mihailescu，*《Haskell 快速语法参考》*。Apress。2019年。
+ 6. Bellard。可在线访问：https://bellard.org/libbf/。
+ 7. IEEE 754-2019 – 浮点运算标准。可在线访问：https://standards.ieee.org/content/ieee-standards/en/standard/754-2019.html。
+ 8. LibBF 库。可在线访问：https://bellard.org/libbf/。
+ 9. 大数库。可在线访问：www.ttmath.org/。
+ 10. TTMath 示例。可在线访问：www.ttmath.org/samples。
+ 11. A.S. Mena，*《实用 Haskell：真实世界的编程指南》*。Apress。2019年。

```
int a{4};      //** 4 is an integer
double b{3.0};   //** 3.0 represents a floating point (with no
                 //** suffix - double type by default)
float c{6.0f};   //** 6.0 represents a floating point (f is the
                 //** suffix which means a float type)
```

## 显示浮点数

让我们考虑清单5-1中的示例。

### 清单5-1 显示常见浮点数

```
#include <iostream>

using namespace std;

int main()
{
    cout << 5.0 << endl;
    cout << 6.7f << endl;
    cout << 9876543.21 << endl;

    return 0;
}
```

输出如图5-1所示。

![](img/c3d9424f64735276d661e7aba725b2a2_128_0.png)

图5-1 常见浮点数的输出

通过观察程序的输出，您可以发现：在第一种情况下，输出是 5，但源代码显示的是 5.0。这是因为小数部分等于 0。在第二种情况下，打印出的数字与源代码中的完全相同。在第三种情况下，数字以科学计数法显示，这对密码学算法而言是一项重要特性。

## 浮点数的范围

让我们看看 IEEE 754 表示，并考虑以下大小及其范围和精度。见表5-1。

### 表5-1 IEEE 754 标准表示

| 尺寸 | 范围 | 精度 |
|---|---|---|
| 4 字节 | ±1.18 × 10⁻³⁸ 至 ± 3.4 × 10³⁸ | 6-9 是最重要的数字。通常约为 7 位数字。 |
| 8 字节 | ±2.23 × 10⁻³⁰⁸ 至 ± 1.80 × 10³⁰⁸ | 15-18 是最重要的数字。通常约为 16 位数字。 |
| 80 位（通常使用 12 或 16 字节） | ±3.36 × 10⁻⁴⁹³² 至 ± 1.18 × 10⁴⁹³² | 18-21 是最重要的数字。 |
| 16 字节 | ±3.36 × 10⁻⁴⁹³² 至 ± 1.18 × 10⁴⁹³² | 33-36 是最重要的数字。 |

当今处理器上的 80 位浮点数使用 12 或 16 字节实现。处理器可以轻松处理该大小。

## 浮点精度

让我们考虑用分数 1/3 表示的示例。其十进制表示为 0.3333333…，3 无限循环。

使用计算机时，无限长度的数字需要无限内存来存储。这种内存限制将浮点数的存储限制在特定数量的有效数字。浮点数及其精度定义了在不丢失任何信息的情况下可以表示多少位有效数字。

在密码学中，如果我们输出一个浮点数，cout 具有 6 位的隐式精度。在图5-2中，您可以看到清单5-2中的 cout 如何将值截断为六位数字。

### 清单5-2 浮点精度的表示

```
#include <iostream>

using namespace std;

int main()
{
    cout << 7.56756767f << endl;
    cout << 765.657667f << endl;
    cout << 345543.564f << endl;
    cout << 9976544.43f << endl;
    cout << 0.00043534345f << endl;

    return 0;
}
```

![](img/c3d9424f64735276d661e7aba725b2a2_130_0.png)

图5-2 浮点精度的输出

请记住，上述每种情况都只有六位有效数字。

请注意，在某些情况下，使用 std::cout 的输出将采用科学计数法表示。根据所使用的编译器，指数通常会被填充到最少位数。所显示的指数位数取决于所使用的编译器。例如，Visual Studio 使用三位，其他编译器使用两位（它们是根据 C99 指令和标准实现的）。

表示浮点数及其精度的位数取决于存储的大小和具体值。float 值以 6 到 9 位数字作为精度表示，大多数值至少有 7 位有效数字。double 值以 15 到 18 位数字作为精度表示。long double 值至少以 15 或 33 位有效数字的精度表示，具体取决于字节的占用方式。

清单5-3中的代码通过使用 setprecision() 函数覆盖了 cout 或 std::cout 显示的默认精度。setprecision() 函数定义在 iomanip 头文件中。输出见图5-3。

### 清单5-3 默认精度

```
#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
    std::cout << std::setprecision(16);
    std::cout << 3.333333333333333333333333333333333f << endl;
    std::cout << 3.333333333333333333333333333333333 << endl;

    return 0;
}
```

![](img/c3d9424f64735276d661e7aba725b2a2_131_0.png)

图5-3 覆盖默认精度
