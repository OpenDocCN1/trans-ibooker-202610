
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言 zend_write 函数会影响使用此 C 语言函数的所有 PHP 原生函数。这包括 print()、printf()（及其变体）等等。

这结束了我们关于在 PHP 应用程序中使用 FFI 扩展的讨论。你现在知道如何从外部共享库中合并原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

在本章中，你了解了 FFI、其历史以及它如何用于促进快速 PHP 扩展原型制作。你还了解到，尽管 FFI 扩展不应被用于提高速度，它也用于允许你的 PHP 应用程序直接调用来自外部 C 库的原生 C 函数。这种能力的强大之处通过一个从外部 C 库调用冒泡排序函数的示例得以证明。这种相同的能力可以扩展到涵盖数千个可用的 C 库，包括机器学习、光学字符识别、通信、加密；无穷无尽。

在本章中，你对 PHP 本身在 C 语言级别的运行方式有了更深入的理解。你学习了如何创建和直接使用 C 语言数据结构，从而能够与 PHP 语言交互，甚至覆盖它。此外，你现在也有了如何将任何 C 语言库的功能直接合并到 PHP 应用程序中的想法。这一知识的进一步好处是，如果你在一家计划开发或已经开发了自己的自定义 PHP 扩展的公司找到工作，它有助于增强你的职业前景。

下一章标志着本书新部分 PHP 8 Tricks 的开始。在下一部分，你将了解升级到 PHP 8 时的向后兼容性问题。下一章专门针对与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

在本部分，你将进入 PHP 8 的黑暗角落：存在向后兼容性破坏的地方。本部分指导你完成将现有应用程序迁移到 PHP 8 的关键过程。

以下章节包含在此部分：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章标志着本书第 2 部分 *PHP 8 Tricks* 的开始。在本部分，你将发现 PHP 8 的黑暗角落：存在 **backward-compatibility breaks** 的地方。本部分为你在将现有应用程序迁移到 PHP 8 之前避免问题提供了见解。你将学习在现有代码中寻找什么内容，这些内容可能导致在 PHP 8 升级后停止工作。一旦你掌握了本书本部分介绍的主题，你将能够很好地修改现有代码，使其在 PHP 8 升级后继续正常运行。

在本章中，你将[content]
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言 zend_write 函数会影响使用此 C 语言函数的所有 PHP 原生函数。这包括 print()、printf()（及其变体）等等。

这结束了我们关于在 PHP 应用程序中使用 FFI 扩展的讨论。你现在知道如何从外部共享库中合并原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

在本章中，你了解了 FFI、其历史以及它如何用于促进快速 PHP 扩展原型制作。你还了解到，尽管 FFI 扩展不应被用于提高速度，它也用于允许你的 PHP 应用程序直接调用来自外部 C 库的原生 C 函数。这种能力的强大之处通过一个从外部 C 库调用冒泡排序函数的示例得以证明。这种相同的能力可以扩展到涵盖数千个可用的 C 库，包括机器学习、光学字符识别、通信、加密；无穷无尽。

在本章中，你对 PHP 本身在 C 语言级别的运行方式有了更深入的理解。你学习了如何创建和直接使用 C 语言数据结构，从而能够与 PHP 语言交互，甚至覆盖它。此外，你现在也有了如何将任何 C 语言库的功能直接合并到 PHP 应用程序中的想法。这一知识的进一步好处是，如果你在一家计划开发或已经开发了自己的自定义 PHP 扩展的公司找到工作，它有助于增强你的职业前景。

下一章标志着本书新部分 PHP 8 Tricks 的开始。在下一部分，你将了解升级到 PHP 8 时的向后兼容性问题。下一章专门针对与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

在本部分，你将进入 PHP 8 的黑暗角落：存在向后兼容性破坏的地方。本部分指导你完成将现有应用程序迁移到 PHP 8 的关键过程。

以下章节包含在此部分：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章标志着本书第 2 部分 *PHP 8 Tricks* 的开始。在本部分，你将发现 PHP 8 的黑暗角落：存在 **backward-compatibility breaks** 的地方。本部分为你在将现有应用程序迁移到 PHP 8 之前避免问题提供了见解。你将学习在现有代码中寻找什么内容，这些内容可能导致在 PHP 8 升级后停止工作。一旦你掌握了本书本部分介绍的主题，你将能够很好地修改现有代码，使其在 PHP 8 升级后继续正常运行。

在本章中，你将[content]
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言 zend_write 函数会影响使用此 C 语言函数的所有 PHP 原生函数。这包括 print()、printf()（及其变体）等等。

这结束了我们关于在 PHP 应用程序中使用 FFI 扩展的讨论。你现在知道如何从外部共享库中合并原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

在本章中，你了解了 FFI、其历史以及它如何用于促进快速 PHP 扩展原型制作。你还了解到，尽管 FFI 扩展不应被用于提高速度，它也用于允许你的 PHP 应用程序直接调用来自外部 C 库的原生 C 函数。这种能力的强大之处通过一个从外部 C 库调用冒泡排序函数的示例得以证明。这种相同的能力可以扩展到涵盖数千个可用的 C 库，包括机器学习、光学字符识别、通信、加密；无穷无尽。

在本章中，你对 PHP 本身在 C 语言级别的运行方式有了更深入的理解。你学习了如何创建和直接使用 C 语言数据结构，从而能够与 PHP 语言交互，甚至覆盖它。此外，你现在也有了如何将任何 C 语言库的功能直接合并到 PHP 应用程序中的想法。这一知识的进一步好处是，如果你在一家计划开发或已经开发了自己的自定义 PHP 扩展的公司找到工作，它有助于增强你的职业前景。

下一章标志着本书新部分 PHP 8 Tricks 的开始。在下一部分，你将了解升级到 PHP 8 时的向后兼容性问题。下一章专门针对与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

在本部分，你将进入 PHP 8 的黑暗角落：存在向后兼容性破坏的地方。本部分指导你完成将现有应用程序迁移到 PHP 8 的关键过程。

以下章节包含在此部分：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章标志着本书第 2 部分 *PHP 8 Tricks* 的开始。在本部分，你将发现 PHP 8 的黑暗角落：存在 **backward-compatibility breaks** 的地方。本部分为你在将现有应用程序迁移到 PHP 8 之前避免问题提供了见解。你将学习在现有代码中寻找什么内容，这些内容可能导致在 PHP 8 升级后停止工作。一旦你掌握了本书本部分介绍的主题，你将能够很好地修改现有代码，使其在 PHP 8 升级后继续正常运行。

在本章中，你将[content]
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言 zend_write 函数会影响使用此 C 语言函数的所有 PHP 原生函数。这包括 print()、printf()（及其变体）等等。

这结束了我们关于在 PHP 应用程序中使用 FFI 扩展的讨论。你现在知道如何从外部共享库中合并原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

在本章中，你了解了 FFI、其历史以及它如何用于促进快速 PHP 扩展原型制作。你还了解到，尽管 FFI 扩展不应被用于提高速度，它也用于允许你的 PHP 应用程序直接调用来自外部 C 库的原生 C 函数。这种能力的强大之处通过一个从外部 C 库调用冒泡排序函数的示例得以证明。这种相同的能力可以扩展到涵盖数千个可用的 C 库，包括机器学习、光学字符识别、通信、加密；无穷无尽。

在本章中，你对 PHP 本身在 C 语言级别的运行方式有了更深入的理解。你学习了如何创建和直接使用 C 语言数据结构，从而能够与 PHP 语言交互，甚至覆盖它。此外，你现在也有了如何将任何 C 语言库的功能直接合并到 PHP 应用程序中的想法。这一知识的进一步好处是，如果你在一家计划开发或已经开发了自己的自定义 PHP 扩展的公司找到工作，它有助于增强你的职业前景。

下一章标志着本书新部分 PHP 8 Tricks 的开始。在下一部分，你将了解升级到 PHP 8 时的向后兼容性问题。下一章专门针对与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

在本部分，你将进入 PHP 8 的黑暗角落：存在向后兼容性破坏的地方。本部分指导你完成将现有应用程序迁移到 PHP 8 的关键过程。

以下章节包含在此部分：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章标志着本书第 2 部分 *PHP 8 Tricks* 的开始。在本部分，你将发现 PHP 8 的黑暗角落：存在 **backward-compatibility breaks** 的地方。本部分为你在将现有应用程序迁移到 PHP 8 之前避免问题提供了见解。你将学习在现有代码中寻找什么内容，这些内容可能导致在 PHP 8 升级后停止工作。一旦你掌握了本书本部分介绍的主题，你将能够很好地修改现有代码，使其在 PHP 8 升级后继续正常运行。

在本章中，你将[content]
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言 zend_write 函数会影响使用此 C 语言函数的所有 PHP 原生函数。这包括 print()、printf()（及其变体）等等。

这结束了我们关于在 PHP 应用程序中使用 FFI 扩展的讨论。你现在知道如何从外部共享库中合并原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

在本章中，你了解了 FFI、其历史以及它如何用于促进快速 PHP 扩展原型制作。你还了解到，尽管 FFI 扩展不应被用于提高速度，它也用于允许你的 PHP 应用程序直接调用来自外部 C 库的原生 C 函数。这种能力的强大之处通过一个从外部 C 库调用冒泡排序函数的示例得以证明。这种相同的能力可以扩展到涵盖数千个可用的 C 库，包括机器学习、光学字符识别、通信、加密；无穷无尽。

在本章中，你对 PHP 本身在 C 语言级别的运行方式有了更深入的理解。你学习了如何创建和直接使用 C 语言数据结构，从而能够与 PHP 语言交互，甚至覆盖它。此外，你现在也有了如何将任何 C 语言库的功能直接合并到 PHP 应用程序中的想法。这一知识的进一步好处是，如果你在一家计划开发或已经开发了自己的自定义 PHP 扩展的公司找到工作，它有助于增强你的职业前景。

下一章标志着本书新部分 PHP 8 Tricks 的开始。在下一部分，你将了解升级到 PHP 8 时的向后兼容性问题。下一章专门针对与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

在本部分，你将进入 PHP 8 的黑暗角落：存在向后兼容性破坏的地方。本部分指导你完成将现有应用程序迁移到 PHP 8 的关键过程。

以下章节包含在此部分：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章标志着本书第 2 部分 *PHP 8 Tricks* 的开始。在本部分，你将发现 PHP 8 的黑暗角落：存在 **backward-compatibility breaks** 的地方。本部分为你在将现有应用程序迁移到 PHP 8 之前避免问题提供了见解。你将学习在现有代码中寻找什么内容，这些内容可能导致在 PHP 8 升级后停止工作。一旦你掌握了本书本部分介绍的主题，你将能够很好地修改现有代码，使其在 PHP 8 升级后继续正常运行。

在本章中，你将[content]
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言 zend_write 函数会影响使用此 C 语言函数的所有 PHP 原生函数。这包括 print()、printf()（及其变体）等等。

这结束了我们关于在 PHP 应用程序中使用 FFI 扩展的讨论。你现在知道如何从外部共享库中合并原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

在本章中，你了解了 FFI、其历史以及它如何用于促进快速 PHP 扩展原型制作。你还了解到，尽管 FFI 扩展不应被用于提高速度，它也用于允许你的 PHP 应用程序直接调用来自外部 C 库的原生 C 函数。这种能力的强大之处通过一个从外部 C 库调用冒泡排序函数的示例得以证明。这种相同的能力可以扩展到涵盖数千个可用的 C 库，包括机器学习、光学字符识别、通信、加密；无穷无尽。

在本章中，你对 PHP 本身在 C 语言级别的运行方式有了更深入的理解。你学习了如何创建和直接使用 C 语言数据结构，从而能够与 PHP 语言交互，甚至覆盖它。此外，你现在也有了如何将任何 C 语言库的功能直接合并到 PHP 应用程序中的想法。这一知识的进一步好处是，如果你在一家计划开发或已经开发了自己的自定义 PHP 扩展的公司找到工作，它有助于增强你的职业前景。

下一章标志着本书新部分 PHP 8 Tricks 的开始。在下一部分，你将了解升级到 PHP 8 时的向后兼容性问题。下一章专门针对与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

在本部分，你将进入 PHP 8 的黑暗角落：存在向后兼容性破坏的地方。本部分指导你完成将现有应用程序迁移到 PHP 8 的关键过程。

以下章节包含在此部分：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章标志着本书第 2 部分 *PHP 8 Tricks* 的开始。在本部分，你将发现 PHP 8 的黑暗角落：存在 **backward-compatibility breaks** 的地方。本部分为你在将现有应用程序迁移到 PHP 8 之前避免问题提供了见解。你将学习在现有代码中寻找什么内容，这些内容可能导致在 PHP 8 升级后停止工作。一旦你掌握了本书本部分介绍的主题，你将能够很好地修改现有代码，使其在 PHP 8 升级后继续正常运行。

在本章中，你将[content]
7. 以下输出演示了 PHP echo 命令现在在每个命令末尾都会产生一个 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需要注意的是，修改 PHP 库 C 语言的 zend_write 函数会影响所有使用该 C 语言函数 的 PHP 原生函数。这包括 print()、printf() 及其变体等。

至此，我们关于在 PHP 应用程序中使用 FFI 扩展的讨论结束。你现在知道如何从外部共享库中引入原生 C 函数。你也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身的行为。

## Summary

本章介绍了 FFI、其历史以及如何利用它进行快速 PHP 扩展原型开发。你还了解到，虽然 FFI 扩展不应用于提升速度，它也能让 PHP 应用直接调用外部 C 库中的原生 C 函数。这种能力通过调用外部 C 库中的冒泡排序函数示例得以体现。同样的方法可扩展到数千个 C 库，包括机器学习、光学字符识别、通信、加密等，无穷无尽。

本章加深了你对 PHP 在 C 语言层面运行方式的理解。你学会了如何创建和直接使用 C 语言数据结构，从而能与 PHP 语言交互甚至覆盖它。此外，你现在也知道如何将任意 C 语言库的功能直接集成到 PHP 应用中。掌握这些知识还能提升你在有自定义 PHP 扩展开发需求的公司中的职业前景。

下一章开启本书新部分 PHP 8 Tricks。在下一部分，你将学习升级到 PHP 8 时的向后兼容性问题。下一章专门讨论与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

本部分带你进入 PHP 8 的暗角：存在向后兼容性破坏的地方。本部分指导你完成将现有应用迁移到 PHP 8 的关键流程。

本部分包含以下章节：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章开启本书第 2 部分 *PHP 8 Tricks*。在本部分，你将发现 PHP 8 的暗角：存在 **backward-compatibility breaks** 的地方。本部分帮助你在迁移现有应用到 PHP 8 之前规避问题。你将学习在现有代码中寻找可能导致 PHP 8 升级后失效的内容。掌握本部分主题后，你将能够修改现有代码，使其在 PHP 8 升级后仍能正常运行。

本章将介绍特定于 **object-oriented programming (OOP)** 的新 PHP 8 功能。本章提供大量简短代码示例，清晰展示新特性和概念。本章有助于你快速利用 PHP 8 的强大功能，将示例适配到自己的实践中。本章重点关注对象导向代码在 PHP 8 迁移后可能破坏的情况。

本章涵盖的主题包括：

- Discovering core OOP coding differences
- Navigating changes in magic methods
- Taking control of serialization
- Understanding expanded PHP 8 variance support
- Handling **Standard PHP Library (SPL)** changes

## Technical requirements

为检查和运行本章提供的代码示例，推荐的最低硬件如下：

- x86_64 based desktop PC or laptop
- 1 gigabyte (GB) of free disk space
- 4 GB of RAM
- A 500 kilobits per second (Kbps) or faster internet connection

此外，你需要安装以下软件：

- Docker
- Docker Compose

请参阅 *Chapter 1, Introducing New PHP 8 OOP Features* 中 *Technical requirements* 部分，了解 Docker 和 Docker Compose 的安装信息，以及如何构建本书中用于演示代码的 Docker 容器。本书中，我们将样本代码恢复的目录称为 `/repo`。

本章的源代码位于此处：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。

我们现在可以开始讨论，通过检查核心 OOP 编码差异来展开。

## Discovering core OOP coding differences

PHP 8 中编写 OOP 代码的方式存在多项重大变化。本节聚焦于三个可能带来向后兼容性破坏的关键领域。我们在本节讨论的领域是与进行静态方法调用、处理对象属性和 PHP 自动加载相关的常见不良实践。

阅读本节并[content]
7. 以下输出演示 PHP echo 命令现在在每个命令末尾产生 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需注意，修改 PHP 库 C 语言 zend_write 函数会影响使用该 C 函数的所有 PHP 原生函数。包括 print()、printf() 及其变体等。

至此结束关于在 PHP 应用中使用 FFI 扩展的讨论。你现在知道如何从外部共享库引入原生 C 函数。也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身行为。

## Summary

本章介绍了 FFI、历史及如何用于快速 PHP 扩展原型。你还了解到 FFI 不应用于提升速度，但可让 PHP 应用直接调用外部 C 库的原生 C 函数。该能力通过调用外部 C 库的冒泡排序函数示例得到体现。同样可扩展到数千个 C 库，包括机器学习、光学字符识别、通信、加密等。

本章加深了你对 PHP 在 C 语言层运行方式的理解。你学会创建和直接使用 C 语言数据结构，可与 PHP 语言交互甚至覆盖它。你也知道如何将任意 C 语言库功能直接集成到 PHP 应用。掌握此知识有助于你在开发自定义 PHP 扩展的公司提升职业前景。

下一章开启本书新部分 PHP 8 Tricks。下一部分将介绍升级到 PHP 8 的向后兼容性问题。下一章专门讨论与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

本部分带你进入 PHP 8 的暗角：存在向后兼容性破坏之处。本部分指导将现有应用迁移到 PHP 8 的关键流程。

本部分包含以下章节：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章开启本书第 2 部分 *PHP 8 Tricks*。本部分让你发现 PHP 8 的暗角：存在 **backward-compatibility breaks** 之处。本部分帮助你在迁移现有应用到 PHP 8 前规避问题。你将学习现有代码中可能导致 PHP 8 升级后失效的内容。掌握本部分主题后，你可修改现有代码使其在 PHP 8 升级后仍正常运行。

本章将介绍特定于 **object-oriented programming (OOP)** 的新 PHP 8 功能。本章提供大量简短代码示例，清晰展示新特性和概念。本章有助于你快速利用 PHP 8 强大功能，将示例适配到自己的实践。本章重点关注对象导向代码在 PHP 8 迁移后可能破坏的情况。

本章涵盖主题包括：

- Discovering core OOP coding differences
- Navigating changes in magic methods
- Taking control of serialization
- Understanding expanded PHP 8 variance support
- Handling **Standard PHP Library (SPL)** changes

## Technical requirements

检查和运行本章代码示例的最低推荐硬件如下：

- x86_64 based desktop PC or laptop
- 1 gigabyte (GB) of free disk space
- 4 GB of RAM
- A 500 kilobits per second (Kbps) or faster internet connection

此外需安装以下软件：

- Docker
- Docker Compose

请参阅 *Chapter 1, Introducing New PHP 8 OOP Features* 中 *Technical requirements* 部分，了解 Docker 和 Docker Compose 安装及如何构建本书演示代码的 Docker 容器。本书中，样本代码恢复目录称为 `/repo`。

本章源代码位于：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。

我们现在可开始讨论，通过检查核心 OOP 编码差异展开。

## Discovering core OOP coding differences

PHP 8 中编写 OOP 代码的方式存在多项重大变化。本节聚焦三个可能带来向后兼容性破坏的关键领域。所讨论领域是与进行静态方法调用、处理对象属性和 PHP 自动加载相关的常见不良实践。

阅读本节并[content]
7. 以下输出演示 PHP echo 命令现在在每个命令末尾产生 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需注意，修改 PHP 库 C 语言 zend_write 函数会影响使用该 C 函数的所有 PHP 原生函数。包括 print()、printf() 及其变体等。

至此结束关于在 PHP 应用中使用 FFI 扩展的讨论。你现在知道如何从外部共享库引入原生 C 函数。也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而有潜力改变 PHP 语言本身行为。

## Summary

本章介绍了 FFI、历史及如何用于快速 PHP 扩展原型。你还了解到 FFI 不应用于提升速度，但可让 PHP 应用直接调用外部 C 库的原生 C 函数。该能力通过调用外部 C 库的冒泡排序函数示例得到体现。同样可扩展到数千个 C 库，包括机器学习、光学字符识别、通信、加密等。

本章加深了你对 PHP 在 C 语言层运行方式的理解。你学会创建和直接使用 C 语言数据结构，可与 PHP 语言交互甚至覆盖它。你也知道如何将任意 C 语言库功能直接集成到 PHP 应用。掌握此知识有助于你在开发自定义 PHP 扩展的公司提升职业前景。

下一章开启本书新部分 PHP 8 Tricks。下一部分将介绍升级到 PHP 8 的向后兼容性问题。下一章专门讨论与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

本部分带你进入 PHP 8 的暗角：存在向后兼容性破坏之处。本部分指导将现有应用迁移到 PHP 8 的关键流程。

本部分包含以下章节：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章开启本书第 2 部分 *PHP 8 Tricks*。本部分让你发现 PHP 8 的暗角：存在 **backward-compatibility breaks** 之处。本部分帮助你在迁移现有应用到 PHP 8 前规避问题。你将学习现有代码中可能导致 PHP 8 升级后失效的内容。掌握本部分主题后，你可修改现有代码使其在 PHP 8 升级后仍正常运行。

本章将介绍特定于 **object-oriented programming (OOP)** 的新 PHP 8 功能。本章提供大量简短代码示例，清晰展示新特性和概念。本章有助于你快速利用 PHP 8 强大功能，将示例适配到自己的实践。本章重点关注对象导向代码在 PHP 8 迁移后可能破坏的情况。

本章涵盖主题包括：

- Discovering core OOP coding differences
- Navigating changes in magic methods
- Taking control of serialization
- Understanding expanded PHP 8 variance support
- Handling **Standard PHP Library (SPL)** changes

## Technical requirements

检查和运行本章代码示例的最低推荐硬件如下：

- x86_64 based desktop PC or laptop
- 1 gigabyte (GB) of free disk space
- 4 GB of RAM
- A 500 kilobits per second (Kbps) or faster internet connection

此外需安装以下软件：

- Docker
- Docker Compose

请参阅 *Chapter 1, Introducing New PHP 8 OOP Features* 中 *Technical requirements* 部分，了解 Docker 和 Docker Compose 安装及如何构建本书演示代码的 Docker 容器。本书中，样本代码恢复目录称为 `/repo`。

本章源代码位于：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。

我们现在可开始讨论，通过检查核心 OOP 编码差异展开。

## Discovering core OOP coding differences

PHP 8 中编写 OOP 代码的方式存在多项重大变化。本节聚焦三个可能带来向后兼容性破坏的关键领域。所讨论领域是与进行静态方法调用、处理对象属性和 PHP 自动加载相关的常见不良实践。

在阅读本节并[content]
7. 以下输出演示 PHP echo 命令现在在每个命令末尾产生 LF：

| 修订后的 echo 命令添加 LF： |
|---|
| A |
| B |
| C |

还需注意，修改 PHP 库 C 语言 zend_write 函数会影响使用该 C 函数的所有 PHP 原生函数。包括 print()、printf() 及其变体等。

至此结束关于在 PHP 应用中使用 FFI 扩展的讨论。你现在知道如何从外部共享库引入原生 C 函数。也知道如何用 PHP 回调替换原生 PHP 核心或扩展共享库，从而可改变 PHP 语言本身行为。

## Summary

本章介绍了 FFI、历史及如何用于快速 PHP 扩展原型。你还了解到 FFI 不应用于提升速度，但可让 PHP 应用直接调用外部 C 库的原生 C 函数。该能力通过调用外部 C 库的冒泡排序函数示例得到体现。同样可扩展到数千个 C 库，包括机器学习、光学字符识别、通信、加密等。

本章加深了你对 PHP 在 C 语言层运行方式的理解。你学会创建和直接使用 C 语言数据结构，可与 PHP 语言交互甚至覆盖它。你也知道如何将任意 C 语言库功能直接集成到 PHP 应用。掌握此知识有助于你在开发自定义 PHP 扩展的公司提升职业前景。

下一章开启本书新部分 PHP 8 Tricks。下一部分将介绍升级到 PHP 8 的向后兼容性问题。下一章专门讨论与 OOP 相关的向后兼容性问题。

## Section 2: PHP 8 Tricks

本部分带你进入 PHP 8 的暗角：存在向后兼容性破坏之处。本部分指导将现有应用迁移到 PHP 8 的关键流程。

本部分包含以下章节：

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

本章开启本书第 2 部分 *PHP 8 Tricks*。本部分让你发现 PHP 8 的暗角：存在 **backward-compatibility breaks** 之处。本部分帮助你在迁移现有应用到 PHP 8 前规避问题。你将学习现有代码中可能导致 PHP 8 升级后失效的内容。掌握本部分主题后，你可修改现有代码使其在 PHP 8 升级后仍正常运行。

本章将介绍特定于 **object-oriented programming (OOP)** 的新 PHP 8 功能。本章提供大量简短代码示例，清晰展示新特性和概念。本章有助于你快速利用 PHP 8 强大功能，将示例适配到自己的实践。本章重点关注对象导向代码在 PHP 8 迁移后可能破坏的情况。

本章涵盖主题包括：

- Discovering core OOP coding differences
- Navigating changes in magic methods
- Taking control of serialization
- Understanding expanded PHP 8 variance support
- Handling **Standard PHP Library (SPL)** changes

## Technical requirements

检查和运行本章代码示例的最低推荐硬件如下：

- x86_64 based desktop PC or laptop
- 1 gigabyte (GB) of free disk space
- 4 GB of RAM
- A 500 kilobits per second (Kbps) or faster internet connection

此外需安装以下软件：

- Docker
- Docker Compose

请参阅 *Chapter 1, Introducing New PHP 8 OOP Features* 中 *Technical requirements* 部分，了解 Docker 和 Docker Compose 安装及如何构建本书演示代码的 Docker 容器。本书中，样本代码恢复目录称为 `/repo`。

本章源代码位于：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。

我们现在可开始讨论，通过检查核心 OOP 编码差异展开。

## Discovering core OOP coding differences

PHP 8 中编写 OOP 代码的方式存在多项重大变化。本节聚焦三个可能带来向后兼容性破坏的关键领域。所讨论领域是与进行静态方法调用、处理对象属性和 PHP 自动加载相关的常见不良实践。

阅读本节并[content]
在阅读本节并完成示例后，你能更好地发现 OOP 不良实践，并了解 PHP 8 对此类使用的限制。本章教授良好的编码实践，最终会让你成为更好的程序员。你也能处理 PHP 自动加载的变化，这些变化可能导致迁移到 PHP 8 的应用失败。

让我们先看看 PHP 8 如何收紧了静态调用。

### Handling static calls in PHP 8

令人惊讶的是，PHP 7 及以下版本允许开发者对未声明为静态的类方法进行静态调用。乍一看，任何审查你代码的未来开发者都会立即假设该方法已定义为静态。这可能导致意外行为，因为在错误假设下工作的未来开发者开始误用你的代码。

在此简单示例中，我们定义了一个 Test 类，其中包含 notStatic() 方法。在类定义之后的过程代码中，我们 echo 该方法的返回值，但在这样做的过程中我们进行了静态调用：

// /repo/ch05/php8_oop_diff_static.php
class Test {
    public function notStatic() {
        return __CLASS__ . PHP_EOL;
    }
}
echo Test::notStatic();

当我们在 PHP 7 中运行此代码时，结果如下：

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_oop_diff_static.php
```

```
PHP Deprecated:  Non-static method Test::notStatic() should not be called statically in /repo/ch05/php8_oop_diff_static.php on line 11
Test
```

正如输出所示，PHP 7 发出弃用通知，但允许调用！在 PHP 8 中，结果却是致命错误，如下所示：

```
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_oop_diff_static.php
PHP Fatal error:  Uncaught Error: Non-static method
Test::notStatic() cannot be called statically in /repo/ch05/
php8_oop_diff_static.php:11
```

使用静态方法调用语法调用非静态方法是一种不良实践，因为编写良好的代码会让代码开发者的意图非常清晰。如果你未将方法定义为静态，但后来以静态方式调用它，未来负责维护你代码的开发者可能会感到困惑，并对代码的原始意图做出错误假设。最终结果会是更多不良代码！

在 PHP 8 中，你不能再使用静态方法调用来调用非静态方法。让我们现在看看另一个涉及将对象属性视为键的不良实践。

### Dealing with object property handling changes

数组一直是 PHP 从最早版本开始的核心功能。另一方面，OOP 直到 PHP 4 才引入。在 OOP 的早期，数组函数经常被扩展以适应对象属性。这导致对象与数组之间的区别变得模糊，进而催生了许多不良实践。

为了在数组处理和对象处理之间保持清晰分离，PHP 8 现在将 array_key_exists() 函数限制为仅接受数组作为参数。为了说明这一点，请考虑以下示例：

- 1. 首先，我们定义一个只有一个属性的简单匿名类：

// /repo/ch05/php8_oop_diff_array_key_exists.php
$obj = new class () { public $var = 'OK.'; };
