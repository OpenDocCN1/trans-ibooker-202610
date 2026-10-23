

```
struct CryptographyOps {}; // empty class

struct Encryption {
    int i;
    CryptographyOps CryptoOps;
};

struct Decryption {
    int i;
    [[no_unique_address]] CryptographyOps CryptoOps;
};
```

## C++20 中的新头文件

在本节中，我们将介绍 C++20 中最重要的头文件。它们旨在帮助改进密码学领域的开发过程。

### `<concepts>` 头文件

这个新的 C++20 概念是概念库的一部分。概念库包含基本概念，可用于执行编译时验证。验证是针对模板和类的参数进行的。其第二个目的是实现基于类型属性的分派过程。

### 核心语言概念

大多数与核心语言相关的概念都与对象和类型有关。在密码学中，这些概念包括 std::floating_point、std::destructible、std::integral、std::signed_integral 和 std::unsigned_integral。在接下来的小节中，我们将通过一些示例来展示如何在密码学中使用这些概念。大多数此类概念可用于密码学实现中，以检查类型，并确保你在类、对象、结构体、变量等类型上与预期保持一致。

### std::floating_point

让我们考虑以下类声明：

```
//** the class is a general class for working with cryptographic
//** operations related to floating numbers.
class FloatingClassExample {};
```

如果我们调用

```
std::cout << std::floating_point<FloatingClassExample>::value;
```

输出将为 false。但如果我们有

```
std::cout << std::floating_point<float>::value;
```

输出将为 true。

### std::destructible

在密码学中，一旦某个操作（例如加密或解密）结束，建议销毁所有函数、结构体、类、对象和类型。

要通过 std::destructible 实现销毁过程，我们只需将类型传递给 destructible<>，如下所示：

```
class AESClass {};
std::cout << std::destructible<AESClass>::value;
```

输出将为 true。

### std::integral, std::signed_integral, std::unsigned_integral

这些概念的使用方式与上面列出的示例相同。让我们考虑以下类型测试：

```
std::cout << std::integral<AESClass>::value;
std::cout << std::integral<int>::value;
std::cout << std::integral<float>::value;
```

输出将分别为 false、true 和 false。对于 signed_integral 和 unsigned_integral，情况也如此。

### 比较概念

在 C++20 中，有四个比较概念：equality_comparable、equality_comparable_with、totally_ordered 和 totally_ordered_with [4]。它们通过 ==、!=、<、>、<=、>= 等比较运算符对指定类型进行操作。尽管这些比较概念都使用上述运算符，但它们之间存在细微差别。totally_ordered 概念侧重于对类型产生严格全序的比较结果 [3]，而 totally_ordered_with<operand1, operand2> 则侧重于混合操作数，其结果也产生严格全序。

### 对象概念

对象概念涉及对象的主要操作，例如移动、交换和复制。有四个对象概念用于处理对象操作：movable、copyable、semiregular 和 regular。这些概念在密码学中非常有用，尤其是在处理复杂的密码学对象时，这些对象在不同的软件基础设施、操作系统和软件应用程序之间进行交换的过程中需要被移动、复制或交换。通过这种方式，我们可以提高应用程序互操作性的安全性。

作为示例，让我们快速查看以下内容：

```
template <class AESCrypto>
concept movable =
    std::is_object_v<AESCrypto> &&
    std::move_constructible<AESCrypto> &&
    std::assignable_from<AESCrypto&, AESCrypto> &&
    std::swappable<AESCrypto>;
```

### 可调用概念

首先，让我们澄清一下“可调用”这个术语的含义，并将可调用概念定义为可以被调用的“某种东西”，例如函数。通常，它是 object() 或 object(arguments)。一个对象之所以可调用，是因为重载了 operator() 函数。

在 C++20 中，涵盖了以下概念：invocable/regular_invocable、predicate、relation、equivalence_relation 和 strict_weak_order。在密码学中，我们更多会用到 predicate 和 relation。一旦充分理解了这两个概念，其余概念就简单了。

使用 predicate 概念时，提供的参数将产生布尔结果。相较于 relation 概念，我们有如下示例：

```
template<class AESCrypto, class Encryption, class Decryption>
concept relation =
    std::predicate<AESCrypto, Encryption, Encryption> &&
    std::predicate<AESCrypto, Encryption, Decryption> &&
    std::predicate<AESCrypto, Decryption, Encryption> &&
    std::predicate<AESCrypto, Decryption, Decryption>;
```

正如你所观察到的，根据 relation<AESCrypto, Encryption, Decryption>，AESCrypto 将根据由 Encryption 或 Decryption 编码的类型和值所表征的一组表达式来定义二元关系。

### `<compare>` 头文件

`<compare>` 头文件 [5] 处理比较运算符，它是通用工具库的一个组件。

从 C++20 开始，通过该头文件我们可以使用强大的概念（three_way_comparable 和 three_way_comparable_with）、类（partial_ordering、weak_ordering 和 strong_ordering）以及定制点对象（strong_order、weak_order 和 partial_order）。

如你在“比较概念”一节中所见，其机制是相同的。如果你正在使用线程处理复杂应用程序，并且这些线程上使用和传输的数据是敏感的，那么发送方和接收方可以通过比较函数和概念来验证数据是否被正确发送和接收，完整性是否得到保证。这些函数扩展了密码学算法的能力，并让你能够自由地创建复杂的机制，在两端比较和处理敏感数据。

### `<format>` 头文件

在密码学中，我们处理各种规则。我们有数字表示规则、字符串规则、字符规则、表达式上的数学规则等等。

`<format>` 头文件在 C++20 中扩展了其能力，为格式化规则的定义和实现带来了新的见解。尽管在撰写本书时，format 头文件尚未包含在 C++ 标准库中，但它在密码学应用中的潜力值得关注。C++ 库和功能的更新可在 [6] 中找到。

作为示例，请考虑清单 6-1 中的程序。

**清单 6-1. `<format>` 头文件**

```
#include <format>
#include <iostream>

//** let's define a wrapper for class AESCryptography
template<class AESCryptography>
struct Encryption {
    AESCryptography value;
};

template<class AESCryptography, class CharAESCryptography>
struct std::formatter<Encryption<AESCryptography>,
    CharAESCryptography>:
std::formatter<AESCryptography, CharAESCryptography>
{
    template<class FormatContext>
    auto format(Encryption<AESCryptography>
        encAESCrypto, FormatContext& theFormatContext)
    {
        return std::formatter<AESCryptography,
            CharAESCryptography>::format
            (encAESCrypto.value, theFormatContext);
    }
};

int main()
{
    Encryption<int> encrypted = { 32 };
    std::cout << std::format("{:#x}", encrypted);
}
```

### 结论

在本章中，我们讨论了 C++20 中最重要的特性，以及它们如何帮助应用密码学领域的专业人员。我们重点关注了密码学算法实现过程中的关键点。本章收录的这些特性旨在覆盖与内存和类型比较相关的主要密码学操作：

- 测试 C++20 语言预处理器的特性
- 内存洞察力，以及如何更专业、更优雅地操作内存
- carries_dependency 和 no_unique_address 概念，这些概念对内存操作非常有用
- `<concepts>`、`<compare>` 和 `<format>` 头文件及其最强大的类和函数，可用于处理密码学机制，例如密钥生成操作、加密和解密函数，以及类型测试和验证

### 参考文献

- [1] std::memory_order。在线获取：https://en.cppreference.com/w/cpp/atomic/memory_order.
- [2] C++20 – no_unique_address。在线获取：https://en.cppreference.com/w/cpp/language/attributes/no_unique_address.

