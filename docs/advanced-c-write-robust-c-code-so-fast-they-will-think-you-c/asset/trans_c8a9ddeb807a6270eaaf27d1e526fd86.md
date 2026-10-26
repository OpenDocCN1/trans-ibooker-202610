

我们可以为它编写以下测试用例：

+   - 只有额外字符（"  "），返回空字符串（）。
+   - 只有开头为空字符("  AA") 返回末尾字符的字符串 ("AA")
+   - 只有末尾为空字符("AA  ")，应返回开头字符的字符串("AA")
+   - 中间有字符 ("  AA  ") 返回字符的字符串 ("AA")
+   - 中间有空格 ("AA  BB")，返回相同的字符串 ("AA BB")
+   - 所有步骤 3,4,5 使用单个字符。应返回单个字符的字符串。

要创建测试用例，请检查文件 **ParseFileTestCases.cpp**，`trim` 函数的测试用例已写入测试套件 `trim`。现在编写 **Util.cpp** 文件（所有杂项函数都写在 **Util.cpp** 中）。在文件中编写签名如上所示的 `trim` 函数。执行 `trim` 函数的测试用例并检查是否通过。它没有适当更改函数并再次测试它。重复直到所有测试用例都通过。

现在我们有信心在项目中使用 `trim` 函数。对其余的常用函数（`isDigit`、`isNumeric`、`parseHeader` 等）重复类似步骤。请参考 **Util.cpp** 文件和 **ParseFiletestCases.cpp** 进行操作并测试所有常用函数。

完成常用函数后，我们可以分别编写解析每个文件的函数。这里要理解和学习的主要内容是如何将模块分解为小函数。找到小的重复任务并为每个任务创建小函数以便重构。理解这些小函数的详细功能并创建适当的单元测试用例。

完成单个函数并彻底测试它，如果失败，则更新函数直到通过所有测试用例。同样，完成其他函数。然后为更大的函数编写并执行测试用例，这应该相对容易，因为我们在这些更大的函数中调用了上面测试过的小函数。

在实施上述步骤后，我们将得到以下输出：

```
00:35@/media/sf_packt/Advance_C++/StartTask/Activity/unitTesting$ ./ParseFileTestCases
Running main() from src/gtest_main.cc
========== Running 32 tests from 8 test suites.
[----------] Global test environment set-up.
[----------] 8 tests from trim
[ RUN      ] trim.empty
[       OK ] trim.empty (1 ms)
[ RUN      ] trim.start_space
[       OK ] trim.start_space (0 ms)
[ RUN      ] trim.end_space
[       OK ] trim.end_space (0 ms)
[ RUN      ] trim.string_middle
[       OK ] trim.string_middle (0 ms)
[ RUN      ] trim.space_middle
[       OK ] trim.space_middle (0 ms)
[ RUN      ] trim.single_char_start
[       OK ] trim.single_char_start (0 ms)
[ RUN      ] trim.single_char_end
[       OK ] trim.single_char_end (0 ms)
[ RUN      ] trim.single_char_middle
[       OK ] trim.single_char_middle (0 ms)
[----------] 8 tests from trim (1 ms total)

[----------] 5 tests from isNumber
[ RUN      ] isNumber.alphabets_present
[       OK ] isNumber.alphabets_present (0 ms)
[ RUN      ] isNumber.special_character_present
[       OK ] isNumber.special_character_present (0 ms)
[ RUN      ] isNumber.correct_number
[       OK ] isNumber.correct_number (0 ms)
[ RUN      ] isNumber.decimal_begin
[       OK ] isNumber.decimal_begin (0 ms)
[ RUN      ] isNumber.decimal_end
[       OK ] isNumber.decimal_end (0 ms)
[----------] 5 tests from isNumber (0 ms total)

[----------] 3 tests from isDigit
[ RUN      ] isDigit.alphabet_present
[       OK ] isDigit.alphabet_present (1 ms)
[ RUN      ] isDigit.decimal_present
[       OK ] isDigit.decimal_present (0 ms)
[ RUN      ] isDigit.correct_digit
[       OK ] isDigit.correct_digit (0 ms)
[----------] 3 tests from isDigit (1 ms total)

[----------] 2 tests from CurrencyParameters
[ RUN      ] CurrencyParameters.extra_currency_chararcters
[       OK ] CurrencyParameters.extra_currency_chararcters (0 ms)
[ RUN      ] CurrencyParameters.correct_parameters
[       OK ] CurrencyParameters.correct_parameters (0 ms)
[----------] 2 tests from CurrencyParameters (0 ms total)
```

Figure 7.23: All tests running properly

下面是下一步的截图：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_553_0.png)

Figure 7.24: All tests running properly

# Note

本活动的解决方案可在第 706 页找到。

## Summary

在本章中，我们研究了各种方法，通过使用断言在编译时和运行时捕获可执行文件抛出的错误。我们还了解了静态断言。我们理解了异常是如何生成的以及如何在代码中处理它们。我们还看到了单元测试如何成为开发者的救星，因为他们能够在开始时识别代码中的任何问题。我们利用模拟对象来用于测试用例中需要使用的类。然后我们学习了调试器、断点、观察点和可视化数据。我们能够使用调试器定位代码中的问题并修复它们。我们还解决了一个活动，其中我们编写了必要的测试用例来检查用于解析文件的函数的准确性。

在下一章中，我们将学习如何优化我们的代码。我们将回顾处理器如何执行代码并访问内存。我们还将学习如何确定软件执行所需的多余时间。最后，我们将学习内存对齐和缓存访问。

## 8

## Need for Speed – Performance and Optimization

## Learning Objectives

本章结束时，你将能够：

+   - 手动计时代码性能
+   - 使用源代码检测来测量代码执行时间
+   - 使用 perf 工具分析程序性能
+   - 使用 godbolt 编译器资源管理器工具分析编译器生成的机器代码
+   - 使用编译器标志生成更好的代码
+   - 应用导致性能的代码惯用法
+   - 编写缓存友好的代码
+   - 对现实世界问题应用算法级优化

在本章中，我们将探索概念，这些概念将使我们能够在一般情况下编写快速代码，以及特别适用于 C++ 的几种实用技术。

## Introduction

在当今极其庞大和复杂的软件系统世界中，**稳定性**和**可维护性**通常被认为是对大多数软件项目的主要目标，而自 2000 年代以来，优化并没有被广泛视为一个值得的目标。这是因为硬件技术的快速加速定期超过了软件需求。

多年来，似乎硬件改进将继续跟上软件性能需求，但应用程序继续变得更大、更复杂。与 C 和 C++ 等低级原生编译语言相比，**Python** 或 **Ruby** 等性能较低但更易于使用的解释型语言的流行度下降。

然而，到 2000 年代末，CPU 晶体管数量（和性能）每 18 个月翻一番的趋势（**摩尔定律**的结果）已经停止，性能改进趋于平稳。由于物理和制造成本的限制，广泛提供 5 到 10 GHz 处理器的预期从未实现。然而，移动设备的快速采用和数据科学和机器学习的高性能计算应用的兴起，突然使对快速高效代码的需求复苏。每瓦性能已成为新的目标指标，因为大型数据中心消耗了巨大的电力。例如，2017 年美国的 Google 服务器使用的电力超过了整个英国国家。

到目前为止，在本书中，我们了解到 C++ 语言在易用性方面是如何发展的，而不会牺牲相对于传统语言如 C 的任何性能潜力。这意味着我们可以在不 necessarily 牺牲可读性或稳定性的情况下用 C++ 编写快速代码。在下一节中，我们将学习性能测量的概念。

## Performance Measurement

优化最重要的方面是**代码执行时间的测量**。除非我们使用各种输入数据集测量应用程序的性能，否则我们将不知道哪一部分花费了最多时间，我们的优化工作将盲目进行，没有保证结果。有几种测量方法，其中一些列在这里：

+   - 运行时检测或分析
+   - 源代码检测

