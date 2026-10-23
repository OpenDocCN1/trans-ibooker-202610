# CHAPTER 4

## Large Integer Arithmetic

本章将介绍算术运算并解释如何处理大整数。某些密码算法需要的大整数无法容纳在普通变量的大小中，例如 int。我们将快速概述大整数以及用于处理它们的库。

在实现复杂的密码算法时，使用大整数进行运算可能非常困难。限制可能源于硬件设备，例如处理器、RAM 内存，或编程语言。

在 C/C++ 中，整数表示为 32 位。在 32 位中，只有 31 位可用于表示正整数算术。在密码学中，我们可以处理高达二十亿的数字，2 · 10⁹。

一些编译器，例如 GNU C++ 或 g++，提供 long long 类型。这提供了表示约 9 quintillion，即 9 · 10¹⁸ 的整数的能力。对于大多数简单的密码运算来说，这已经足够好，但某些密码算法需要在其整数表示中包含更多位数。让我们以 RSA Rivest-Shamir-Adleman 公钥加密密码系统为例，它需要大约 300 位数。如果我们处理特定的真实事件及其概率，计算通常会涉及非常大的数字。输出和实现主要结果可能适合 C/C++。与其它复杂计算相比，我们将会有非常大的数字。

作为一个有趣的例子，考虑用一张彩票中头奖的几率。组合是从 50 中取 6 个，“50 choose 6” 是 50! / ((50 − 6)! 6!)。

所得数字是 15.890.700，因此中奖几率是 1/15.890.700。使用 C/C++ 编程语言，数字 15.890.700 可以很容易表示。然而，这可能很棘手，我们在实现 50! 时很容易陷入天真，使用 Windows 计算器计算，它是 3.041409320171e+64 或 30,414,093,201,713,378,043,612,608,166,064,768,844,377,641,568,960,512,000,000,000,000

```
BigNo[k++] = number % bitSign;

//** we don't need the least significant bit
number /= BASE;
}

//** complete the remain of the array with zeroes
while (k < N)
    BigNo[k++] = 0;
}
```

Listing 4-1 的算法具有 O(N) 的空间和时间。

让我们继续研究将 1 加到 big int 的可能性。这是一个非常有用的运算，在密码学中经常使用。优点是它比完整加法容易得多。见 Listing 4-2。

### Listing 4-2. Adding 1 to a big int²

```
void increment (int BigNo [])
{
    Int i;
    int N;
    int BASE;

    /* start indexing with least significant digit

    i = 0;
    while (i < N)
    {

        /* increment the digit
        BigNo[i]++;

        /** if it overflows
        if (BigNo[i] == BASE)
```

²The code is meant to be a sketch of a function that will transform a simple integer to a big integer. The source code will not compile without a proper adjustment for a real cryptographic application.

```
{
    //** make it zero and move the index to next
    BigNo[i] = 0;
    i++;
}
else
    //** else, we are done!
    break;
}
}
```

Listing 4-2 所示的算法在最坏情况下需要 O(n) 时间，例如想象 9999999999999999999999....，而在最好情况下需要 Ω(1)。最好情况是在最低有效位上没有溢出时。

继续前进，让我们看一下加两个大整数的方法。在这种情况下，我们想在两个不同的数组 BigNo1[0,..., N-1] 和 BigNo2[0,...,N-1] 中相加两个大整数。输出结果将保存在另一个数组 BigNo3[0,...,N-1] 中。该算法非常基础；没有什么花哨的。见 Listing 4-3。

### Listing 4-3. Addition Algorithm³

```
void addition(int BigNo1[], int BigNo2[], int BigNo3[])
{
    Int j, overflowCarry, sum;
    int carry, N, BASE;

    //** There is no need to carry yet
    carry = 0;

    //** move from the least to the most significant digit
    for (j=0; j<N; j++)
```

³The code is meant to be a sketch of a function that will transform a simple integer to a big integer. The source code will not compile without a proper adjustment for a real cryptographic application.

### Listing 4-5. Multiplying Using a Single Digit⁵

```
void multiplyUsingOneDigit (int BigOne1[], int BigOne2[],
    int number) {
    int k, carryOverflow;
    int N, BASE;
    //** there is nothing related to
    //** extra overflow to be added at this moment
    carryOverflow = 0;
    //** for each digit, starting with least significant...
    for (k=0; k<N; k++){
        //** multiply the digit by number,
        //** putting the result in BigOne2
        BigOne2[k] = number * BigOne1[k];
        //** adding extra any overflow that is taking
        //** place starting with the last digit
        BigOne2[k] += carryOverflow;
        //** product is too big to fit in a digit
        if (BigOne2[k] >= BASE) {
            //** handle the overflow
            carryOverflow = BigOne2[k] / BASE;
            BigOne2[k] %= BASE;
        }
        else
            //** no overflow
            carryOverflow = 0;
    }
    if (carryOverflow)
        printf ("During the multiplication
            we experienced an overflow!\n");
}
```

⁵The code is meant to be a sketch of a function that will transform a simple integer to a big integer. The source code will not compile without a proper adjustment for a real cryptographic application.

我们将继续使用一个功能，将特定数量的空格向左移动，如 Listing 4-6 所示。

### Listing 4-6. Shifting to the Left a Specific Number of Spaces⁶

```
void leftShifting (int BigInt1[], int number) {
    int i;

    //** moving starting from left to right,
    //** we will move anything with left n spaces
    for (i=N-1; i>= number; i--)
        BigInt1[i] = BigInt1[i- number];

    //** complete the last n digits with zeros
    while (i >= 0) BigInt1[i--] = 0;
}
```

### Big Integer Libraries

有几个库和框架处理大数。对于其中一些，开发过程已暂停，但它们仍在密码学应用中使用。

用于处理大整数的库如下：

- **Matt McCutchen⁷** 提出了一个非常易于使用的 C++ 库，用于大整数计算 [1]。代码有很好的解释，易于理解。在对称和非对称密码算法中获得的结果很有希望。大多数结果都与其他工具进行了比较以供参考和检查，例如 CryptTool。⁸

⁶The code is meant to be a sketch of a function that will transform a simple integer to a big integer. The source code will not compile without a proper adjustment for a real cryptographic application.

⁷Matt McCutchen’s web site, https://mattmccutchen.net/

⁸CrypTool, www.cryptool.org/en/




- **L3HARRIS Geospatial Solutions** 提供了 Big Integer 类 [2]，这是另一个计算速度很快的库。⁹
- **Boost 库**¹⁰ 是另一个强大的库，用于完成基于线性代数、伪随机数生成、多线程、图像处理、正则表达式和单元测试的任务。该库拥有一组令人印象深刻、相互独立的库（约 160 个），并且文档结构清晰，非常易于学习和使用 [3]。
- **GMP 库**（GNU 多重精度算术库）是另一个可用于任意精度算术的免费库。¹¹ 它支持基于有符号整数、有理数和浮点数的运算（更多细节请参见第6章）。该库唯一的限制在于可用内存。在 32 位系统上，上限为 2³² − 1 位；在 64 位系统上，上限为 2³⁷ − 1 位。其主要接口面向 C/C++，但也支持 C#、.NET 和 OCaml。（它也可以轻松移植到 Haskell。更多细节请参阅 [4]、[5] 和 [11]。）此外，它还提供对 Ruby、PHP、Python、R、Perl 和 Wolfram 语言的重要支持。该库的主要目标和用途是密码学软件应用、互联网安全和代数系统。
- **LibBF 库 [8]** 用于处理以 2 为基数表示的浮点数。该库基于 IEEE 754 标准 [7] 实现。库网页上提供的示例 TinyPI 是展示其强大功能的绝佳示例。该库将在第6章进一步讨论。
- **Bignum C++ 库 [9]**（TTMath）允许个人和商业用户执行算术运算。支持的整数类型包括大无符号整数、大有符号整数和大浮点数。支持加、减、除、乘等数学运算。参见清单4-7。

⁹L3HARRIS Geospatial Solutions, www.harrisgeospatial.com
¹⁰Boost Library, www.boost.org
¹¹GMP Library, http://gmplib.org
