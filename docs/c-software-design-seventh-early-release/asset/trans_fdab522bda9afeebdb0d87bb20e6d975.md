

## GUIDELINE 14: 使用设计模式的名称来传达意图

+   - 使用设计模式的名称来传达解决方案的意图；
+   - 使用设计模式的名称来提高可读性。

1 Gang of Four，或简称为 GoF，是对四位作者 Erich Gamma、Richard Helm、Ralph E. Johnson 和 John Vlissides 及其关于设计模式的书籍“Design Patterns, Elements of Reusable Object-Oriented Software”，Prentice Hall，1994 年 10 月，ISBN: 978-0201633610 的常用引用。GoF 书籍在数十年后仍然是设计模式的参考资料。在本书的其余部分，我将提及 GoF 书籍、GoF 模式，或具有特征性的面向对象的 GoF 风格。

2 如果你还不了解 Visitor 设计模式，不用担心。我将在第 4 章介绍该模式。

3 Strategy 设计模式将在第 5 章详细解释，Decorator 设计模式在第 9 章。

4 我只提及将在后续章节中解释的设计模式（参见第 5 章中的 Strategy 和 Command 设计模式，以及“Guideline 28: Build Bridges to Remove Physical Dependencies”中的 Bridge 设计模式）。还有一些设计模式具有相同的结构。

5 如果你对 Strategy 设计模式不熟悉，请放心，第 5 章将提供更多信息，包括多个代码示例。

6 这可能是一个有争议的例子。由于我了解 C++ 社区，我知道你可能有不同的看法。然而，我坚持我的观点：由于其定义，std::make_unique() 无法解耦软件实体，因此在软件设计层面不起作用。它只是一个实现细节（但是一个有价值且有用的细节）。

7 Venkat Subramaniam 和 Andrew Hunt: “Practices of an Agile Developer”，The Pragmatic Programmers, LLC，2017 年 7 月，ISBN: 978-0974514086。

8 好吧，它能工作，在某种“工作”的定义下。

9 我知道你在想什么：“你不能是认真的！外面有那么多有趣的例子，但你却选了书中最老旧、最无聊的例子！”。好吧，我承认，这可能不是最令人兴奋的例子选择。但，仍然，我有使用这个例子的两个好理由。首先，这个场景如此为人所知，以至于我可以假设没有人会难以理解它。这意味着每个人都应该能够跟上我关于

软件设计的论点。其次：让我们同意在计算机科学中以形状或另外动物示例开始已经成为一种传统。而当然我不想让传统主义者失望。

10 第 5 章将提供 *Strategy* 设计模式的完整和彻底的介绍。

11 你可能会（正确地）观察到，即使没有第四个参数，你也可以通过为给定类型提供自定义加法运算符（即 `operator+()`）来改变累加的工作方式。然而，这只具有有限的用途。虽然你可以为用户定义的类型提供自定义加法运算符，但你不能为基本类型（例如示例中的 `int`）提供自定义加法运算符。此外，为除加法操作（或与之相关的操作，如字符串的连接）之外的任何东西定义 `operator+()` 都是非常可疑的。因此依赖加法运算符在技术上和语义上都是有限制的。

12 在他的 CppCon 2016 演讲“std::accumulate: Exploring an Algorithmic Empire”中，Ben Deane 令人印象深刻地展示了 `std::accumulate()` 得益于该第四参数是多么强大（YouTube）。

13 有关 STL 算法及其函数式编程传承的更多信息，请参阅 Ivan Cukic 的优秀介绍“Functional Programming in C++”，Manning，2018 年，ISBN: 978-1617293818。

14 这种形式的 *Strategy* 设计模式的另一个常用名称是 *Policy-Based Design*；参见“Guideline 19: Use Strategy to Isolate How Things are Done”

15 我将在第 4 章解释 *Visitor* 设计模式，并在“Guideline 30: Apply Prototype for Abstract Copy Operations”中介绍 *Prototype* 设计模式。

16 同样，我将你引向 Ivan Cukic 的“Functional Programming in C++”介绍。*CRTP* 设计模式将是“Guideline 26: Use CRTP to Introduce Static Type Categories”的主题。而有关 *Expression Templates*，一种基于模板的模式的信息，请参阅 C++ 模板参考：David Vandevoorde, Nicolai Josuttis, Douglas Gregor: “C++ Templates: The Complete Guide”，Addison Wesley，2017 年 12 月，ISBN: 978-0321714121。

17 我认为 C++ 自 1989 年首次将模板实现添加到语言中以来就一直是多范式编程语言。模板对语言的影响随着 1994 年将标准模板库（STL）的一部分添加到标准库而变得清晰。至少从那时起，C++ 就提供了面向对象、函数式以及泛型能力。

18 *Small String Optimization (SSO)* 是小型字符串的常见优化。不是通过提供的分配器在堆上分配动态内存，字符串会将少量字符直接存储在字符串的栈部分。由于字符串通常在栈上占用 24 到 32 字节（这不是 C++ 标准要求，而是 `std::string` 常见实现的特性），超过 32 字节的任何内容都需要堆分配。这就是三个给定字符串的情况。

19 *Singleton* 是原始 23 个 GoF 设计模式之一。但我将在“Guideline 37: Treat Singleton as an Implementation Pattern, Not a Design Pattern”中尽力说服你 *Singleton* 并不是真正的设计模式，而是一个实现细节。出于这个原因，我将把 *Singleton* 称为不是设计模式，而是简单地称为实现模式。

20 不幸的是，我不会在本書中涵蓋 *Template Method* 设计模式。这不是因为它不重要，而是仅仅由于可用页数不足。请参阅 GoF 书籍以获取更多细节。

21 我将在第 9 章给出 *Decorator* 设计模式的完整介绍。

22 *Adapter* 设计模式将在“Guideline 24: Use Adapters to Standardize Interfaces”中作为主题。

23 好的名称总是承载意义。这就是为什么它们如此根本重要。

24 我将在第 5 章与 *Strategy* 设计模式一起解释 *Command* 设计模式。

25 *Visitor* 设计模式，包括使用 `std::variant` 的现代实现，将是我们第 4 章的重点。

26 命名是困难的，正如 Kate Gregory 在她在 CppCon 2019 强烈推荐的演讲“Naming is Hard: Let’s Do Better”（YouTube）中所恰当地指出的那样。

## Chapter 4. The Visitor Design Pattern

### A NOTE FOR EARLY RELEASE READERS

使用 Early Release 电子书，你可以在这些技术正式发布之前很久就获得书籍的早期形式——作者在写作时的原始和未编辑内容。

这将是最终书籍的第 4 章。请注意 GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系编辑 sevans@oreilly.com。

本章的全部内容都集中在 *Visitor* 设计模式上。如果你已经听说过 *Visitor* 设计模式甚至在自己的设计中使用过它，你可能会想知道为什么我选择 *Visitor* 作为第一个详细解释的设计模式。是的，*Visitor* 绝对不是最光鲜的设计模式。然而，它将成为一个很好的例子来展示你在实现设计模式时拥有的许多选择，以及这些实现可以有多么不同。它也将成为宣传现代 C++ 优势的有效例子。

在“Guideline 15: Design for the Addition of Types or Operations”中，我们首先必须讨论你在动态多态的领域中行走时需要做出的基本设计决策：专注于类型或操作。在该准则中，我们还将讨论编程范式的内在优势和弱点。

